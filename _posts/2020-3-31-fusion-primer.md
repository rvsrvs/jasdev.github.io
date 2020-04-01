---
layout: post
title: An operator fusion primer
permalink: fusion-primer
type: engineering
highlighted: true
---

(Assumed audience: folks with a conceptual familiarity of ~~reactive~~ declarative programming, whether in Combine, ReactiveSwift, or other Rx flavorings.)

Nuclear physicists 🤝 Engineers at Apple

…operator fusion.

Kidding. Well, only sort of.

“Fusion” is a term I’d see in the [Reactive Java](http://akarnokd.blogspot.com/2016/03/operator-fusion-part-1.html) and [effect system](https://github.com/fused-effects/fused-effects/blame/40cf33031576f179c9e4ba3f9e8a33ba642fcc91/README.md#L98-L106) communities and struggled to build intuition around until I read through Combine’s documentation.

If anything, the goal of this primer is to be the resource younger Jasdev wish he had.

⬦

Fusion—as the word hints—_fuses_ two structures. Which, by extension, means “operator fusion” in the Combine context joins two operators to optimize away intermediary types.

Publishers in the framework bear two names:

- Their [type-erased](https://developer.apple.com/documentation/combine/publishers/output/3241611-erasetoanypublisher) nickname, `AnyPublisher<Output, Failure>`.
- Or, their fuller, non-erased names like `Publishers.Concatenate<Publishers.Map<Self, Sequence.Element>, Publishers.Sequence<Sequence, Failure>>`.

Choosing between the two is nuanced.

To illustrate the nuance, let’s walk through an operator I’m [contributing](https://github.com/CombineCommunity/CombineExt/pull/7) to CombineExt—a community overlay of Combine extensions—, `Publisher.removeKnownDuplicates`[^1].

## `Publisher.removeKnownDuplicates`

Combine ships with a three deduplicating operators:

- [`Publisher.removeDuplicates`](https://developer.apple.com/documentation/combine/publisher/3204745-removeduplicates)
- [`.removeDuplicates(by:)`](https://developer.apple.com/documentation/combine/publisher/3204746-removeduplicates)
- [`.tryRemoveDuplicates(by:)`](https://developer.apple.com/documentation/combine/publisher/3204777-tryremoveduplicates)

They all nix _pairwise_ duplicates from an upstream publisher by comparing the previous and next value events along an `Equatable` conformance or (non-)throwing predicate. If they match, the value will be dropped, if not, it’ll be published downstream.

But, what if we need distinct values across _all_ seen so far?

That’s `removeKnownDuplicates`’s goal.

Of course, this operator has a warning label. Deduplicating across known value events means the operator has to track distinct values in memory, which can be problematic for sequences that publish a high number of unique elements. The operator can only free resources after a [cancellation](https://developer.apple.com/documentation/combine/cancellable/3204391-cancel) or [completion event](https://developer.apple.com/documentation/combine/subscribers/completion).

We’ll focus in on a specific implementation constraining  `Publisher.Output` to `Hashable`[^2].

<script src="https://gist.github.com/jasdev/d241612e599842c3661bc26af28d3f3f.js"></script>

First, we need to record incoming value events and suppress those we’ve seen already. A rolling reduce[^3], in a sense. Turns out Combine has a [`scan` operator](https://developer.apple.com/documentation/combine/publisher/3229094-scan) that does just this.

The operator accepts `initialResult` and `nextPartialResult` arguments (like its [`Sequence.reduce(_:_:)`](https://developer.apple.com/documentation/swift/sequence/2907677-reduce) sibling). We’ll chisel down `nextPartialResult`’s shape to infer `initialResult`’s.

The goal is to both bookkeep values we’ve seen and determine if the next is unique. Maybe we can lean on `Output`’s `Hashable` conformance?

We could store all value events in a `Set<Output>`, say under the variable name `seen`, and determine uniqueness with [`seen.insert(incoming).inserted`](https://developer.apple.com/documentation/swift/set/3128848-insert) (for an `incoming` value).

So, `nextPartialResult` will need to bundle together `seen` and the next value to publish, if unique.

And a `(Set<Output>, Output?)` tuple can do the trick—which, then forces `initialResult`’s type and an initial value of `(Set<Output>(), Output?.none)`.

<script src="https://gist.github.com/jasdev/1c6f576a0b6ef709591c6ac96a04bb12.js"></script>

Now, we can check for `incoming`’s membership in the first component of `seenAndPrevious` (`seenAndPrevious.0`), if inserted we know it’s unique and can send it downstream, otherwise, `nil`’ing out.

<script src="https://gist.github.com/jasdev/e747a52b4a1587cf3205a3c08892bf49.js"></script>

Almost there. `removeKnownDuplicates` returns a plain ol’ `Output` publisher and that isn’t compatible with `(Set<Output>, Output?)`. We need to zoom in on the second component and drop any `nil`s.

Thankfully [`Publisher.compactMap(_:)`](https://developer.apple.com/documentation/combine/publisher/3204698-compactmap) follows [`Sequence.compactMap(_:)`](https://developer.apple.com/documentation/swift/sequence/2950916-compactmap)’s lead.

<script src="https://gist.github.com/jasdev/c11c391155764ea8c8a6052fd3a81a00.js"></script>

We’ve arrived at the “type erase?”, “fuse??”, or “[???](https://knowyourmeme.com/memes/confused-nick-young)” juncture.

If you’ve been compiling along, here’s the remaining error:

> Cannot convert return expression of type 
>
> “`Publishers.CompactMap<Publishers.Scan<Self, (Set<Output>, Output?)>, Output>`”
>
> to return type
>
> “`AnyPublisher<Self.Output, Self.Failure>`.”

## “To return the full type, erase, or be opaque? That is the question.”

— Wayne Gretzky

— — [Michael Scott](https://www.reddit.com/r/OutOfTheLoop/comments/3kyz8p/why_do_people_always_quote_things_with_michael/)

There’s two-ish[^4] routes we can take (I’ll explain the “-ish” soon).

### Return the full type

There’s no dancing around it. Returning `Publishers.CompactMap<Publishers.Scan<Self, (Set<Output>, Output?)>, Output>` is a lot to digest and seemingly leaks implementation details. But, is that such a bad thing?

The only properties on `Publishers.CompactMap` (and likewise, for other publishers) is its [upstream](https://developer.apple.com/documentation/combine/publishers/compactmap/3206087-upstream) and [transform argument](https://developer.apple.com/documentation/combine/publishers/compactmap/3206069-transform), both of which call sites can’t do much with—and if they do, they’d sidestep reaching for `removeKnownDuplicates` in the first place.

Still, is there a way to translate the shorter `AnyPublisher` placeholder to its full, underlying type? Unfortunately, the best I’ve found is letting the compiler guide me.

<blockquote class="imgur-embed-pub" lang="en" data-id="a/7JDzeI5" data-context="false" ><a href="//imgur.com/a/7JDzeI5">Converting from erased to full publisher type</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

### Maybe an opaque result type?

Revealing implementation details isn’t ideal, yet the leaked edges aren’t really that useful for consumers.

Could [SE-0244](https://github.com/apple/swift-evolution/blob/53c2e80bdede03b99aee683fe6399b6e4cf0bf95/proposals/0244-opaque-result-types.md)’s opaque result types help?

Swapping out `AnyPublisher<Output, Failure>` for `some Publisher`

…compiles? That’s fishy—and rightfully so. The issue shows up when we interface against the opaqueness.

<script src="https://gist.github.com/jasdev/620784d991fa6c9653754daae80e6f10.js"></script> 

Opaque return types are a sort of “[reverse generics](https://github.com/apple/swift-evolution/blame/53c2e80bdede03b99aee683fe6399b6e4cf0bf95/proposals/0244-opaque-result-types.md#L89-L107),” in that the _callee_ (`removeKnownDuplicate`’s implementation, in our case)—instead of the caller (`one`) determines the resulting type.

There’s a cycle though. Our operator is stating it’s in control of the resulting type, covered by `some Publisher`, yet the associated type for the `Publisher` conformance, `Output`, is chosen by the caller, causing a stalemate.

This exceptional case is [tucked away](https://github.com/apple/swift-evolution/blame/53c2e80bdede03b99aee683fe6399b6e4cf0bf95/proposals/0244-opaque-result-types.md#L387-L403) in the backing evolution proposal. And that’s the “-ish” I hinted at above.

### Erase

We could stay course with erasure. There’s a tradeoff through, and that’s fusion.

(Sorry it took almost a thousands words to get here. Nuclear physics is no joke.)

Type erasure—as the name implies—removes detail from a specific protocol conformance, leaving behind a type that only covers the protocol’s requirements. The technique is used frequently: [`AnyPublisher`](https://developer.apple.com/documentation/combine/anypublisher), [`AnySequence`](https://developer.apple.com/documentation/swift/anysequence), [`AnyIterator`](https://developer.apple.com/documentation/swift/anyiterator),[`AnyKeyPath`](https://developer.apple.com/documentation/swift/anykeypath), [`AnyView`](https://developer.apple.com/documentation/swiftui/anyview), and so on.

In Combine, we can ask if that removed detail is useful—to answer this, let’s imagine a consumer tacks on a `map` call to both non-erased and erased forms of `removeKnownDuplicates`.

<script src="https://gist.github.com/jasdev/5ed5183336a12dca288c4eeb8a4e97bd.js"></script>

Woah. `map` calls normally nest the upstream publisher a [`Publishers.Map`](https://developer.apple.com/documentation/combine/publishers/map) level. Yet, when mapping over `one`, `mappedOne` preserved its type.

Fusion!

Since `erasedRemoveKnownDuplicates` wipes the type information, Combine has no other choice than to wrap it in another intermediary. On the other hand, `Publishers.CompactMap` can specialize [its `map` implementation](https://developer.apple.com/documentation/combine/publishers/compactmap/3206027-map) by returning another instance of the same type, reducing overhead.

We can’t peek behind Apple’s closed source. But, we can guess at the implementation.

<script src="https://gist.github.com/jasdev/b5619cd756e23248b84fc12e7d92b4d2.js"></script>

## “[Tradeoffs] rule everything around me.”

Fusion is wicked. However, it’s not always needed. We have to think about our publisher’s intended use, or at least guess when exposing a public-facing API.

If the operator is meant to be used in-between other, chained operators, then it’s better to return the full type and let fusion happen.

However, if the publisher is meant to be used wholesale—i.e. its implementation covers all expected transformations—, then the aesthetic benefit of shorting to an `AnyPublisher` (or if possible, an opaque return type) is probably the way to go.

<script src="https://gist.github.com/jasdev/363cb7268e93f75d45dc09a065b76eb9.js"></script>

⬦

There you have it. We covered building our own composed operator, type erasure, opaque return types, a few memes, and nuclear physics in under 1,500 words. Fusion always felt out of reach and writing this primer distilled it from orbit and into Combine and Swift for me.

I hope it does for others, too.

In short,

- if your operator is likely an intermediary, try to return its full type to benefit from fusion’s optimizations.
- if your publisher ends a chain, lean on either an opaque return type or erasure, in that order.

■

---

Special thanks to [Peter](https://github.com/peter-tomaselli) for feedback on an early draft of this entry.

## Related reading, hat tips, and footnotes

[^1]: I also implemented a predicate-based version `Publisher.removeKnownDuplicates(by:)` for when constraining `Output` to either `Equatable` or `Hashable` isn’t feasible.

[^2]: An alternative, [`flatMap`-based implementation of `removeKnownDuplicates`](https://gist.github.com/jasdev/702d1e8fc7079ba42081b5d4f4868e20).

[^3]: [`Publisher.reduce(_:_:)`](https://developer.apple.com/documentation/combine/publisher/3204744-reduce) and [`.tryReduce(_:_:)`](https://developer.apple.com/documentation/combine/publisher/3204776-tryreduce) are also provided. But, they only emit an accumulated result after upstream completes.

[^4]: It could be argued that there are three-ish, if we consider introducing a `Publishers.RemoveKnownDuplicates` type. However, that’d preclude fusion from kicking in unless we repeated the work Apple did by extending our type with fused operators.

⇒ Thomas Visser’s “[Why Combine has so many `Publisher` types](https://www.thomasvisser.me/2019/07/04/combine-types/).”

⇒ Tim Ekl wrote a wonderful post detailing [the evolution of Swift generics](https://www.timekl.com/blog/2019/04/14/swift-generics-evolution/).

⇒ A diary entry on [type erasure with a postfix operator](/notes/postfix-erasure).

⇒ Kudos to [Adam Sharp](https://twitter.com/sharplet) for [talking through the tradeoffs](https://iosdevelopers.slack.com/archives/C0AET0JQ5/p1585060545148100?thread_ts=1582657918.079500&cid=C0AET0JQ5) between erasure and fusion with me in iOS Folks’ #reactive channel.

⇒ Point-Free subscribers might’ve noticed that [Episode #13’s mention](https://www.pointfree.co/collections/map-zip-flat-map/map/ep13-the-many-faces-of-map#t180) that “composition of maps are equivalent to the map of the composition” seems similar to fusion. And in a sense, it is! That’s because the functors we encounter in programming are strictly endofunctors.