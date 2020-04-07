---
layout: post
title: setFailureType, setOutputType, and initial objects
permalink: set-output-type
type: engineering
---

(Assumed audience: folks familiar with Combine.)

[`Publisher.setFailureType(to:)`](https://developer.apple.com/documentation/combine/publisher/3204753-setfailuretype) is a head scratcher. At a glance, it seems to step on [`.mapError(_:)`](https://developer.apple.com/documentation/combine/publisher/3204719-maperror)’s toes. And, it sort of does!

The “available when `Failure` is `Never`” note in the documentation hints at why.

So, let’s spin up our favorite `Never`-erroring publisher, [`Just`](https://developer.apple.com/documentation/combine/just), and map over its error type for a closer look.

<script src="https://gist.github.com/jasdev/0f5f36ae0cbea851eb4ac37e9635e30e.js"></script>

([Gist permalink](https://gist.github.com/jasdev/0f5f36ae0cbea851eb4ac37e9635e30e).)

Hmm. We need to implement `(Never) -> OurError`, eh? Maybe we can signal an `OurError` return, ignore the input, and call it a day?

<script src="https://gist.github.com/jasdev/dc5cd15980231022e94c29a03a239039.js"></script>

([Gist permalink](https://gist.github.com/jasdev/dc5cd15980231022e94c29a03a239039).)

This compiles‽

If you scratched your head at this, don’t worry. I did, too.

That’s the magic `setFailureType` generalizes and tucks away. In fact, we can _prove_ that somewhere in Combine’s closed source, Apple calls `{ _ -> NewError in }` for some generic failure type, `NewError`.

Before we do, it’s natural to ask if `Publisher`’s other associated type, [`Output`](https://developer.apple.com/documentation/combine/publisher/3204681-output), has the same affordance.

![](/public/images/search_for_set_output_type.png)

Whelp—time to write our own (!).

## `Publisher.setOutputType(to:)`

After learning about `setOutputType` from [Adam](https://twitter.com/sharplet) and [PR’ing it](https://github.com/CombineCommunity/CombineExt/pull/15) to `CombineExt`, a couple of folks asked about its value.

> […], if the upstream is `Never`’d you’d probably `append` another publisher, which would change the output type to my understanding.

—[CombineExt/pull/15](https://github.com/CombineCommunity/CombineExt/pull/15#pullrequestreview-387675862)

This is *almost* the case! Let’s dig further.

A way to `Never` out a publisher is [ignoring its output](https://developer.apple.com/documentation/combine/publisher/3204714-ignoreoutput). Doing so turns it into a “[completable](https://github.com/ReactiveX/RxSwift/blob/002d325b0bdee94e7882e1114af5ff4fe1e96afa/Documentation/Traits.md#completable)” that only notifies downstream of `Subscribers.Completion<Failure>.finished` or `.failure` events.

<script src="https://gist.github.com/jasdev/1e7d5fa1c097e001f206ae190c20915b.js"></script>

([Gist permalink](https://gist.github.com/jasdev/1e7d5fa1c097e001f206ae190c20915b).)

We did this often at Peloton.

Our API had a notion of “workout finalization,” after which, a workout’s metrics would be min-max’d, summed, averaged, and cleaned before reporting secondary statistics like achievements and streaks.

So, we’d ignore the output of the publisher backing the finalization request and then, if successful, concatenate another publisher—of a _different_ output type—to fetch any finalization-dependent metadata.

Some issues shake out when mirroring this in the above gist by `append`ing another element.

<script src="https://gist.github.com/jasdev/d88cc0d9154a4bcc4b5cc2444d9f4418.js"></script>

([Gist permalink](https://gist.github.com/jasdev/d88cc0d9154a4bcc4b5cc2444d9f4418).)

Downstream from the third line, `Output` is fixed to `Never`, forcing the `append` method to accept either a variadic number of `Never`s, a `Sequence` of ‘em, or another `Never`-outputting publisher—each corresponding to the three available overloads:

- [`Publisher.append(_:)`](https://developer.apple.com/documentation/combine/publisher/3204683-append) (variadic).
- [`.append(_:)`](https://developer.apple.com/documentation/combine/publisher/3204684-append) (`Sequence`).
- [`.append(_:)`](https://developer.apple.com/documentation/combine/publisher/3204685-append) (`Publisher`).

The first isn’t possible by extension of `Never` being uninhibited, the second only works with an empty `Never` sequence, and the third would mean chaining another “completable” (which, does have its uses).

We need to map `Never` into another type and that’s where `setOutputType` fits.

To start, let’s scope the implementation by providing a `(Never) -> String` function like we did in our earlier `mapError`’ing.

<script src="https://gist.github.com/jasdev/eb6aed4b28648890b42bac1695eed8ca.js"></script>

([Gist permalink](https://gist.github.com/jasdev/eb6aed4b28648890b42bac1695eed8ca).)

The `{ _ -> String in }` closure is a [vacuous transformation](https://en.wikipedia.org/wiki/Vacuous_truth). In that, since it’s never called, we can claim to return any type.

Requiring folks to roll their own `{ _ -> T in }` closures for each `T` isn’t ideal and that’s the cosmetic `setOutputType(to:)` and `setFailureType(to:)` provide.

<script src="https://gist.github.com/jasdev/bdbfb245e2da525d16252310aaaab5ab.js"></script>

([Gist permalink](https://gist.github.com/jasdev/bdbfb245e2da525d16252310aaaab5ab).)

I should address the rogue type erasure after the `Just` publisher.

Turns out Apple buried some ~~Easter~~lockdown eggs there—removing the erasure and `setOutputType` call still compiles.

<script src="https://gist.github.com/jasdev/34948b0a2a1ede6356e3e96c9ff59a86.js"></script>

([Gist permalink](https://gist.github.com/jasdev/34948b0a2a1ede6356e3e96c9ff59a86).)

Combine ships with overloads of `ignoreOutput` for specific publishers to keep `Output` in tact, but that isn’t true across all publishers (and why I reached for erasure to demonstrate the subtlety).

## `setFailureType(to:)` and initial objects

You might’ve noticed—it’s okay, if you didn’t—that `setOutputType`’s implementation is kind of “forced” by the compiler to a lone `map` call (sans possibly sprinkling `print`s, `fatalError`s, or other statements around).

If we tried to implement our own `setFailureType` operator, we’d be similarly guided.

<script src="https://gist.github.com/jasdev/85b093150c8e9399da7edc5fe62b2d00.js"></script>

([Gist permalink](https://gist.github.com/jasdev/85b093150c8e9399da7edc5fe62b2d00).)

There’s no other way to implement this function (with the noted exceptions).

And if you’re thinking that’s too much of a coincidence to not scratch a larger concept, you’re right (or also guessed from the section title)!

The concept is the `(Never) -> NewOutput`, `(Never) -> NewFailure`, or more generally, `(Never) -> T` closures for an arbitrary type, `T`. No matter which `T` you have in hand, the only way to map from `Never` to it is our new friend `{ _ -> T in }`.

Put another way, there is a _unique_ `(Never) -> T` closure per `T` in Swift’s type system—making `Never` a “starting point” each type can be uniquely mapped from.

Mathematicians call such starting points [initial objects](https://en.wikipedia.org/wiki/Initial_and_terminal_objects).

In their words,

> An initial object of a category _C_ is an object _I_ in _C_ such that for every object _X_ in _C_, there exists precisely one morphism _I_ → _X_.

Replacing _C_ with Swift[^1], _I_ with `Never`, _X_ with `T`, and “morphism” with “closure,” it’s fair to say `Never` is an initial object.

Further, since there is a unique closure from `Never` to any other type, that means Apple’s implementation of `setFailureType`—somewhere underneath the [`Publishers.SetFailureType`](https://developer.apple.com/documentation/combine/publishers/setfailuretype) hood—contains that `mapError { _ -> NewFailure in }` call.

Which is wicked because even though Combine is closed-source, initiality (i.e. `Never` being an initial object) forces the implementation. 

⬦

`setOutputType`’s single expression definition packs almost a thousand words-worth of detail behind its signature and usage.

Further, `Never` is only one side of the coin. Can you think of a type in the Standard Library that has unique closures _towards_ it? That is, which type, `???`, in Swift has a unique function `(T) -> ???`, per type `T`?

It’s a fun exercise to think on.

Until next time.

■

---

## Footnotes

[^1]: Calling Swift a category [comes with asterisks](https://ro-che.info/articles/2016-08-07-hask-category). Still, it’s a trove from which we can translate decades-worth of learnings from category theory into everyday programming.
