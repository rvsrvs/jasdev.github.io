---
layout: note
title: Why applicatives are monoidal
date: 2019-10-27
---

“[Applicative functors are […] lax monoidal functors with tensorial strength.](https://en.wikipedia.org/wiki/Applicative_functor)”

I still don’t quite have the foundation to grok the “lax” and “tensorial strength” bits (I will, some day). Yet, [seeing applicatives as monoidal](https://argumatronic.com/posts/2017-03-08-applicative-instances.html) always felt out of reach.

They’re often introduced with the `pure` and `apply` duo (sketched out below for `Optional`).

<script src="https://gist.github.com/jasdev/9ead264c0366521bea98a1baf750eaa2.js"></script>

(An aside, it finally clicked that the choice of “applicative” is, because, well, it supports function application inside of the functor’s context.)

And then, coincidentally, I recently asked:

> is there any special terminology around types that support a `zip` in the same way functors have a `map` and monads have a `flatMap`?

To which, [Matthew Johnson](https://twitter.com/anandabits) let me in on the secret.

> `zip` is an alternate way to formulate applicative

!!!.

That makes way more sense and sheds light on why Point-Free treated `map`, `flatMap`, and `zip` as their big three instead of introducing `pure` and `apply`.

I can only _sort of_ see that `apply` implies monoidal-ness (pardon the formality) in that it reduces an `Optional<(A) -> B>` and `Optional<A>` into a single `Optional<B>`. However, the fact that they contained different shapes always made me wonder.

`zip` relays the ability to combine more readily. “Hand me an `Optional<A>` and an `Optional<B>` and I’ll give you an `Optional<(A, B)>`.”

Yesterday, I finally got around to defining `apply` in terms of `zip` to see the equivalence.

<script src="https://gist.github.com/jasdev/12cf95ea1c511fa74eff67a019a13fd6.js"></script>

Funnily enough, Brandon pointed me to [exercise 25.2](https://www.pointfree.co/episodes/ep25-the-many-faces-of-zip-part-3#exercises) which asks exactly this.

In short,

- Functors allow for a `map`.
- Applicatives, a `zip`.
- Monads, a `flatMap`.
