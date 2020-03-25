---
layout: post
title: Operator etymology
permalink: operators
type: engineering
---

(_Assumed audience_: folks with a working knowledge of Swift and an openness towards functional programming.)

There’s no dancing around it. Custom operators are `#holy-war` territory in the Swift community. You see it subtly mentioned in style guides:

> [Custom operators should be avoided. Custom operators can reduce the readability of the code because there can be confusion around their functionality.](https://github.com/Lickability/swift-best-practices/blame/f52debe08841dfd00a68b8680a2ae6d879a69a7a/CustomOperators.md#L3)

sidestepped in blog posts:

> [One of the most exciting features of Swift (though also controversial) is the ability to define custom operators.](https://nshipster.com/swift-operators/#defining-custom-operators)

and even Point-Free, the functional programming in Swift series, makes sure each operator they introduce [checks three important boxes](https://www.pointfree.co/episodes/ep1-functions#t394):

> 1. We shouldn’t overload an operator with existing meaning with new meaning.
> 2. We should leverage prior art as much as possible and make sure our operator has a nice “shape” that evokes its semantics.
> 3. We shouldn’t be inventing operators to solve very domain-specific problems. We should only introduce operators that can be used and reused in very general ways.

The above concerns aside, I wonder if operator dislike stems from the fact that we rarely pause to outline their shape. Why was a pipe and right arrow, `|>`, chosen for forward function application? `flatMap` is written as `>>=` (and called `bind`) in other languages? `<$>` is `map`…wat?

It’s completely reasonable for folks to shy away from operators if they can’t build an intuition around their function without context on symbol choice and origin, or, as I’ll dive into, operator etymology.

The sections below cover what I could find about some more common operators’ roots and my interpretation of their form. I did my best to summarize the concepts backing each in-line, linking out to additional resources when I couldn’t. Feel free to skip around or even come back to this post if you stumble upon a rogue operator in the wild or for a discussion about operator ergonomics and when to introduce functional programming terminology, skip to the last section.

## Forward and backward function application: `|>`, `<|`

We’ll start with (forward) function application (and cover the backward direction in a second). You might be scratching your head right about now: “isn’t function application simply tacking on parentheses after a function’s name and calling it a day? Why’d you have to go and make things ˢᵒ ᶜᵒᵐᵖˡᶦᶜᵃᵗᵉᵈ?”

Plain ol’ function application reads like an onion. If we had a function, say `incremented`, and a value in hand, say `2` (my fave number, shoutout to the only even prime) and wanted to get `3` out—riveting, I know—we wouldn’t think twice:

`incremented(2) // ⇒ 3`

Let’s think twice though. Reading from _left to right_ we have to first mentally parse the function’s name and _then_ stumble upon the value we want to apply. The onion-ness becomes apparent with function composition—time to toss a function `squared` into the mix:

`squared(incremented(2)) // ⇒ 9`

“`squared`, okay what’re we squaring? `incremented`, huh, okay okay, what are we incrementing? Ah! I see, `2`, Jasdev’s favorite number. Sweet—an incremented `2` is `3` and a squared `3` is `9`.”

Notice how we had to read left to right and then _back_ to the left, peeling off onion rings of parentheses.

This is what our dear friend, the forward application operator, solves—that’s right, y’all are BFFs now. Most folks call it “pipe forward,” for short and maybe to sound less official. Here’s how you define it and its sibling in Swift:

<script src="https://gist.github.com/jasdev/4d8d7c5978fc048ca4de7cacea9bf448.js"></script>

Don’t fret if you haven’t stumbled upon `precedencegroup` or `associativity` in everyday Swift (most don’t)—they’re ways of helping the compiler determine an order of operations when parentheses are omitted. And ’associativity’ gives directionality to how we _associate_ multiple expressions, i.e. either from the left or from the right. And for the curious, when the association order doesn’t matter (think addition of plain ol’ `Int`s), that’s what mathematicians mean when they say an operation is ‘associative.’

Back to squaring incremented numbers.

<script src="https://gist.github.com/jasdev/705a1f3b170258923e052211ce212211.js"></script>

The above reads linearly instead of nested-ly (let’s pretend that’s a word): “got a `2`, rad, piping it forward to `incremented`, and then the result to `squared`.”

Backwards function application loses the left-to-right, top-to-bottom readability while still allowing for parentheses-free expression. Its usage is rare and usually shows up in cases where the _primary_ focus is the transformation and then the value we’re applying to it as an afterthought.

The symbol choice for `|>` has roots in F# and Scala in addition to practically self-describing its pipe forwarding role.

Now that we’re warmed up, let’s talk composition.

## Forward and backward function composition: `>>>`, `<<<`

The snippets above left repeated usages of pipe forward on the table—that is, while we’re afforded parentheses-free, linear reading, it came at the cost of two `|>`’s and, as you might have guessed since this is a post on operator etymology and not a plot-twisting thriller, composition saves the day.

Instead of applying `2` to _two_ functions, we can apply it once to the composition of `incremented` and `squared`.

<script src="https://gist.github.com/jasdev/f52012a1b60021d9654775c004b2fe55.js"></script>

Three greater-than signs is a fairly self-describing symbol choice in that the operator sequences functions from left to right and reflects well with `<<<` taking things in the opposite direction, i.e. `firstToSecond` and `secondToThird` swap spots:

<script src="https://gist.github.com/jasdev/89085277a8720f40882cd2463014a73d.js"></script>

The backwards variant is used when composition, [somewhat surprisingly](https://www.pointfree.co/episodes/ep6-functional-setters#t545), goes in the other direction.

It’s worth noting that [Haskell’s function composition operator](https://hackage.haskell.org/package/base-4.12.0.0/docs/Prelude.html#v:.), `.`, also reads right to left and closely resembles [its cousin in mathematics](https://en.wikipedia.org/wiki/Function_composition), `∘`, defined by _(g ∘ f )(x) = g(f(x))_ (verbally read “_g_ after _f_”).

The Swift community’s choice of `>>>` and `<<<` instead of `.` or `∘` is two-fold: the first being that periods are [reserved in the language’s lexical structure](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#ID418) and the second being that, well, non-Unicode keyboard layouts don’t cover `∘`[^1].

The symbol choice has the added benefit of indicating directionality and follows [PureScript’s example](https://github.com/purescript/purescript-prelude/blob/e3504b2f9f9e27a39b6e25a27eb6bb6e9045ad10/src/Control/Semigroupoid.purs#L18-L24).

## Semigroups: `<>`

Continuing on the arrow theme, we have the semigroup operator, `<>`. Don’t fret if you’re not familiar with semigroups, it’s a semi-over-the-top name for an only semi-complicated structure and I’ll more-than-semi step through its definition by your side.

Semigroups are plain ol’ sets (in our case, types) with an extra bit of structure—an associative operator that takes two instances of the type and returns another instance (operators with this property are generally called “closed”).

You’ve probably worked with semigroups without even realizing they’re your coworkers. To name a couple,

- The set of all `String`s with `<>` as [concatenation](https://developer.apple.com/documentation/swift/string/2431569).
- `Int` values (ignoring overflow crashes) under addition.
- Booleans with `<>`’s role being played by either `&&` or `||`.

Adjacent left and right arrows for a semigroup’s operator has prior art in both [Haskell](https://hackage.haskell.org/package/base-4.12.0.0/docs/Data-Semigroup.html) and [PureScript](https://github.com/purescript/purescript-prelude/blob/e3504b2f9f9e27a39b6e25a27eb6bb6e9045ad10/src/Data/Semigroup.purs#L14-L25). I couldn’t find the story behind the symbol choice, yet my intuition around it is that semigroups are the “smallest“ structures, [amongst others](https://argumatronic.com/posts/2019-06-21-algebra-cheatsheet.html), we commonly run into. Operations on more elaborate structures like functors and applicatives _nest_ symbols inside the arrows, making `<>` a kind of parenthesis-like schema for operators we’ll soon discuss.

## Functor Map: `<$>`, `<¢>`, `<^>`

_checks if the coast is clear_—“functor”…there, I said it. Phew.

Using widely accepted, colloquial terms instead of ad hoc, yet supposedly more approachable substitutes is another unfortunate holy war in the community. I’ll dig into why I’m bummed about this in the “Ergonomics and Terminology” section. For short, by making up terminology for use in a specific community, we make it harder for folks from other communities to join ours and on the flip side, we incur translation costs when reading the decades-worth of research done under the original terminology.

I’ll start with the full definition and try to bring it down from orbit, and into Swift.

A functor is a structure-preserving mapping between categories.

(Once more, functors are likely already in your Swift tool belt—`Optional`, `Sequence`, `Result`, and `Publisher` are all functors.)

Let’s break down this definition, starting with “categories.“ 

I’ve previously touched on introductory category theory [in my post on duals](/duals) (⌘ + F for “categories have a few properties” and read from there) and for a more thorough treatment, check out the lectures [1.1.1](https://youtu.be/I8LbkfSSR58)–[1.6.2](https://www.youtube.com/watch?v=EO86S2EZssc&list=PLbgaMIhjbmEnaH_LTkxLI7FMa2HsnawM_) in Bartosz Milewski’s stellar three-part Category Theory for Programmers series.

Summarizing, a category must have the following properties:

- Identity arrows for each object.
- Composition and associativity of arrows.

Swift [isn’t quite a category](http://math.andrej.com/2016/08/06/hask-is-not-a-category/)—“there are technical details which break this idea, so [we’ll] need to be a bit careful.” Still, we can use category-theoretic constructions to gain intuition around functional programming instead of blindly Memorizing Laws.

Onto the ‘mapping’ part.

You might hear folks toss around the phrase “[functor laws](https://wiki.haskell.org/Functor#Functor_Laws).” Well, they originate from the fact that a functors map categories in a _careful_ way.

What’re the categories involved when discussing functors? If we consider types in Swift—`Result`, `Publisher`, tuples, you name it—to be objects and functions between any two to be arrows, we get a sort of category we can denote __Swift__. “But, Jasdev, functors are mappings between categories and __Swift__ is only one.”

Great catch, jedi.

Turns out we can map categories into themselves (wild, I know) and such functors are called “endofunctors.”

(We usually drop the ‘endo’ prefix since functional programming exclusively deals with __Swift__ ⇒ __Swift__, __Hask__ ⇒ __Hask__, or __[Your language of choice]__ ⇒ __[Your language of choice]__.)

Now, onto “structure-preserving.”

The choice word ‘structure,’ until recently, felt out of reach. I’d wonder, “Structure? How can a bunch of intangible objects and arrows have structure?”

It clicked when I started viewing arrows as scaffolding in a category, letting you climb from one object to another. And through that perspective, functors make sure the scaffolding, the structure, the arrows of the source category, are maintained across the mapping and into the target category.

Specifically, that identity arrows are mapped to identity arrows and that composition is preserved across the functor.

`Optional<Wrapped>` is a functor in that it maps types (the objects, in the Category Theoretic and not [OOP](https://en.wikipedia.org/wiki/Object-oriented_programming)-sense) in __Swift__ into other types in __Swift__ and functions (the arrows) into functions acting on optionals.

Explicitly,

`Optional<Wrapped>` is a type-level function from `Wrapped -> Optional<Wrapped>`.

`Optional.map`, in its Standard Library form, is a _method_ and has the signature `(Optional<Wrapped>) -> ((Wrapped) -> U) -> Optional<U>`[^2]. The ordering here makes it difficult to see how the `Optional` functor _lifts_ functions into another context, so let’s flip the first two parameters: `((Wrapped) -> U) -> (Optional<Wrapped>) -> Optional<U>`.

There we go.

Shuffling makes it clearer that `Optional` takes plain ol’ `(Wrapped) -> U` transformations and makes them `(Optional<Wrapped>) -> Optional<U>`’s by way of `map`.

So, what’s the dollars, cents, and carets—`<$>`, `<¢>`, `<^>`—section heading got to do with this?

It turns out Haskell’s _generalized_ map, `fmap`, read “functor map” (across all functors, i.e. not specialized to types like `Optional`, `Sequence`, `Result`, and `Publisher`) has an operator synonym, `<$>`. And the inner `$` comes from the language’s function application operator (Haskell’s analog to `|>` we defined above)!

In the same way that Semigroup’s `<>` schema creates a context in which we can perform actions, `<$>` is function application _in_ a functorial context.

I wish someone had told me this sooner, so I’m filling you in on the secret now.

Back to Swift.

`<$>` and the back tick-escaped form aren’t valid operator definitions in Swift—and, that’s finally where `<¢>` and `<^>` come in.

The two are Swift alternatives to `<$>`.

(While `<|>>` seems to better follow suit with symbol nesting inside, the imbalanced arrows probably make chains hard to read.)

Point-Free’s [Prelude opted for `<¢>`](https://github.com/pointfreeco/swift-prelude/blob/b26e98e82c7598fd5f381b3a27b59e27e312eb58/Sources/Prelude/Operators.swift#L9) and [the TypeLift](https://github.com/typelift/Operadics/blob/ef9c00e70ac4e5fba046f4eae3bd4e4e8f058648/Sources/Operadics/Operators.swift#L81-L82) and [Thoughtbot folks](https://github.com/thoughtbot/Runes/blob/5395accc1dbe99fe457a14810c0fc20c7f298656/Sources/Runes/Runes.swift#L31-L37) opted for `<^>`—with the cents symbol being a play on the dollar sign and the caret signaling “lifting” an ordinary function into a functor’s context.

And there you have it, make some noise for functors!

I’ll touch on more specific functors below, namely applicative and monads, but, if you want a larger view of the landscape, Type Class’ “[Functortown](https://typeclasses.com/functortown)” series has you covered.

## Applicatives: `<*>`

Applicatives warrant a post of their own—so, I’ll leave you with [a talk](https://www.youtube.com/watch?v=Awva79gjoHY), [some writing](https://thoughtbot.com/blog/efficient-json-in-swift-with-functional-concepts-and-generics), and a [recent note](/notes/applicatives-monoidal).

Though, I do want to cover the etymology behind `<*>` (often called “the [tie fighter](https://en.wikipedia.org/wiki/TIE_fighter) operator,” don’t worry I haven’t seen all of Star Wars, either).

You might often hear folks call applicatives, “[monoidal functors](https://argumatronic.com/posts/2017-03-08-applicative-instances.html).” Sounds intimidating, but, if you’ve been reading a long, the intuition is an arm-length away.

Functors? Check. We went over them above. And it’s okay if they haven’t quite clicked; it took a while for me.

Monoidal? That’s the adjective form of “monoid.” They’re semigroups with an _additional_ requirement, a neutral element with respect to the binary operator. Let’s scaffold out two protocols to show the relationship here.

<script src="https://gist.github.com/jasdev/962bc702f743ad2484004ddfb8a37223.js"></script>

Don’t sweat `DefaultPrecedence` here, I used it as a placeholder to sketch things out (in a production setting, you’d need a more [explicit precedence group structure](https://github.com/pointfreeco/swift-prelude/blob/b26e98e82c7598fd5f381b3a27b59e27e312eb58/Sources/Prelude/PrecedenceGroups.swift)[^3]). Again, `<>` needs to be associative and the closed requirement is handled by the `Self` return type matching the parameters’ types.

`Monoid` inherits from `Semigroup` and tacks on the neutral element requirement, often called `empty` or `mempty`, for “monoid(al?) empty.” `.empty` must satisfy the following condition, for other instances of the conforming type (let’s call an arbitrary instance here `other`):

`(other <> .empty) == (.empty <> other) == other`

Now, if I were to guess (if you know the full backstory, please reach out), folks didn’t want to nest `<>` inside of the semigroup `<_>` format (“yo, dawg”), with `<<>>` for applicatives and instead followed math’s suit in using `*` to signal the combining operator.

That is, applicatives—monoidal functors—take two values in a context and reduce it into a single value in the same context in the same way monoids reduce two values into one. Rephrased, if you can define [`zip`](https://developer.apple.com/documentation/swift/1541125-zip) on a functor, you have an applicative in hand.

## Monadic composition and binding: `>=>`, `>>=`

We’re over two thousands into this post and I’d probably need double that to cover monads. Thankfully, there is [fantastic writing](https://broomburgo.github.io/fun-ios/post/why-monads/) (also in Swift) on the topic and, for the bird’s-eye, category theoretic view, Bartosz has you covered in [lecture 1.10.1](https://youtu.be/gHiyzctYqZ0).

Two operators you’ll come across when working with such instances are monadic compose—informally called the “fish operator,” or formally, the “Kleisli arrow”—and bind: `>=>` and `>>=`, respectively.

Let’s start with composition.

(For a more thorough introduction, the Point-Free folks step though this in their [Side Effects](https://www.pointfree.co/episodes/ep2-side-effects#t660) episode.)

The functors we mentioned earlier—`Optional`, `Sequence`, `Result`, and `Publisher`—are _also_ monads. Now, imagine we wanted to compose two functions from a generic type into an `Optional`, for sake of example.

`First -> Optional<Second>` and `Second -> Optional<Third>`.

Can we combine them to
create a `First -> Optional<Third>` transformation?

Almost! Ordinary function composition, `>>>`, doesn’t quite get us there since the output of the first function doesn’t match the input of the second.

And that’s what the swapping of the second “>” in `>>>` for “=“ in `>=>` tries to convey. To restore composition, we need to introduce an intermediary _binding_ and, in other languages, the equals sign is often read as binding some value to a named variable (in the same way that [we `if`-`let`, `guard`-`let`, or `case`-`let` bind in Swift](https://alisoftware.github.io/swift/pattern-matching/2016/03/27/pattern-matching-1/)).

To see why we need that intermediary binding when composing monads, here’s a sketch of `>=>` for `Optional`s (again, using a default precedence to get it compiling).

<script src="https://gist.github.com/jasdev/8bf1f0bacddda120cca066728c8d286f.js"></script>

The `case`-`let` on line 13 is an example of the  binding needed to complete the chain. That is, we need to bind the inner component of `firstToOptionalSecond`’s return value and pipe it into `secondToOptionalThird`.

Implementing `>=>` for `Sequence`, `Result`, and `Publisher` also takes a similar shape in that the operator will need to bind to some inner part—intermediary sequences, a success case, or an element from a publisher—to forward along to the next transformation.

Onto bind.

`>>=`’s shape is close to `|>`’s (in pseudo-Swift and where `Monad` is your monad of choice).

<script src="https://gist.github.com/jasdev/14110080702ddd226c4316de9b7d1438.js"></script>

The intuition here is that if you have a monadic instance at hand and some transformation that acts on its component, you can then retrieve another instance containing the output of the transformation.

Or, as [@morabbin put it](https://twitter.com/morabbin/status/1166630610900848641) in response to my since-deleted tweet:

> “Anyone know of (or have resources on) the story behind the `>>=` operator choice (guessing it’s a shuffling of the Kleisli arrow, `>=>`)?”

> @morabbin: “(Sequence `>>`) + (binding `=`)”

> @morabbin: “was around long before the Kleisli arrow symbol I think.”

---

## Ergonomics and Terminology

Phew. That was a lot and if you read through the above in full or skipped around (either is fine), I’m going to close with a discussion that often shakes out when considering operators in a team setting—starting with ergonomics.

It’s true, we lose Xcode’s autocomplete when using operators over named functions.

Still, the tooling limitation likely isn’t insurmountable, since both are backed by function implementations.

Then, there’s readability.

Let’s compare `>>>` and a named-version, [`compose`](https://github.com/pointfreeco/swift-overture/blob/e3e63876688ca8b6f327e5af7018f5e93e742367/Sources/Overture/Compose.swift#L2-L74), when chaining along three functions, `firstToSecond`, `secondToThird`, and `thirdToFourth`.

<script src="https://gist.github.com/jasdev/ba3d4d81a04b9274df674affe9b495ed.js"></script>

Functions—in Swift—will always require parentheses when applied. And that’s unfortunate, because the operators we’ll often use are associative, allowing us to drop them in the first place.

Moreover, even though “compose” may feel more approachable than `>>>`, we’ll likely _still_ need to look up its signature when encountering it for the first time to understand exactly what it’s doing (i.e. “compose,” as a verb, has overloaded meaning in that it works with ordinary functions, over specific monads instances, and the like).

It makes me pause that, when encountering a new word in ordinary languages, most folks would be open to—and not think twice about—looking up the definition. Then, when it comes to operators, rule them out entirely since they might not be readable at _first glance_.

And that makes me sad.

It’s limiting to only work with terms, abstractions, and functions that are within reach of one’s existing knowledge. Rephrased, how can we expect to learn anything new if we don’t open ourselves up to new ideas and terminology?

Alexis King put this succinctly in “[Climbing the infinite ladder of abstraction](https://lexi-lambda.github.io/blog/2016/08/11/climbing-the-infinite-ladder-of-abstraction/)”

> Programmers are professionals, and we work in a technical domain. I am absolutely of the belief that programming, like any other field, is not always about what comes easiest: sometimes it’s important to sit down and study for a while to grok a particularly complicated concept

Just because operators aren’t familiar, doesn’t mean we should throw them out entirely.

The same is true for terminology and the concepts behind them.

I believe functional programming (and mathematics) is best taught when terminology introduced after a structure’s shape has been observed.

Lifting a function between two types to a function between them inside another context.

The flattening of nested types.

The ability to join two instances of the same type into another instance of that type.

Once a pattern has been understood, then it can more readily be collected under an abstraction and named—in the same way generics and protocols should generally be introduced _after_ noticing repeated behavior and not the other way around.

Then, there’s the naming part.

It’s frequently argued that opting for original, mathematics-backed names for abstractions—functor, monads, et al—is a form “gatekeeping.”

And sure, when used excessively without proper introduction they can appear to be intellectual posturing.

But, that speaks more to a potential mismatch between educational material and the assumed audience or a lack of proper onboarding.

Because, [as Matt Parsons mentions](https://www.parsonsmatt.org/2019/08/30/why_functor_doesnt_matter.html):

> Functor is hard to learn. It is not hard to learn because it is named “functor.” If you renamed it to anything else, you’d have just as hard of a time, and you’d be cutting off your student from all of the resources and information currently using the word “functor” to refer to that concept.

Imagine if our community sidestepped “functor,” for example, and instead went with an alternative like “mappable.”

Two things would likely happen.

We would confuse folks by tautologically pushing the definition of “to map” even further: “things that can be mapped are ‘mappable,’ wait, what _exactly_ does mapping mean?”

Secondly,—and the counterintuitive bit—is that by not using commonplace terms, Swift would become an island relative to other communities and, in a sense, that’s gate-keeping others out while also cutting ourselves off from existing literature and adding translation overhead.

---

The next decade of engineering in the Swift ecosystem will be dense with change.

SwiftUI will require us think declaratively, instead of imperatively (à la UIKit).

In such settings, data flow can only reasonably be managed with functional reactive programming and Combine is now the provided tool of choice.

As a community, these paradigms will require open-mindedness and following precedents set by decades-worth of research in the mathematics and functional programming. Still, their previous learnings will be better appreciated if we start with the etymology.

Then, down the line, we’ll eventually contribute back with new abstractions and help tell the story of their etymology to future engineers when joining our community.

---

## Related Reading and Footnotes

[^1]: I recently came across [Ukelele](https://software.sil.org/ukelele/), a Unicode keyboard layout editor, which could help with inputting more-apt, yet physically-absent symbols.

[^2]: Ole Begemann [wrote a wonderful post](https://oleb.net/blog/2014/07/swift-instance-methods-curried-functions/) on how unapplied methods are “curried” functions in disguise.

[^3]: For another setup, here are [Rune’s precedence groups](https://github.com/thoughtbot/Runes/blob/5395accc1dbe99fe457a14810c0fc20c7f298656/Sources/Runes/Runes.swift).

⇒ “[Why ‘Functor’ Doesn’t Matter](https://www.parsonsmatt.org/2019/08/30/why_functor_doesnt_matter.html).”

⇒ “[Notation and Thought](https://github.com/hypotext/notation).”

⇒ [Point-Free’s Overture](https://github.com/pointfreeco/swift-overture), an operator-free library for function composition.

⇒ “Algebraic Roots—[Part 1](http://devlinsangle.blogspot.com/2016/04/algebraic-roots-part-1.html) and [2](https://devlinsangle.blogspot.com/2016/05/what-does-it-mean-to-do-algebra-in-part.html).”

⇒ The “A New Interface” chapter of Jeremy Kun’s “[A Programmer’s Introduction to Mathematics](https://pimbook.org).”

⇒ An operator-heavy topic is [optics](https://www.youtube.com/watch?v=sFzuu676pFs). Here are a couple of resources I’ve found that tour its history and etymology: [Haskell Lens Operator Onboarding](https://medium.com/@russmatney/haskell-lens-operator-onboarding-a235481e8fac) and [Focus’ Further Reading section](https://github.com/typelift/Focus#further-reading).
