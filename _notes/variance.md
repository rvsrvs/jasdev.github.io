---
layout: note
title: Co-, contra-, and invariance
date: 2019-10-20
---

Folks often refer to the component of a functor’s “polarity.” “The input is in the negative position.” “The output is positive.”

And that made me wonder, is there a, well, _neutral_ polarity?

Maybe that’s when a component is in both a negative and positive position, canceling one another out.

Let’s see what happens.

`A -> …`.

`A` is in a negative position? Check. Let’s add it to the positive spot.

`A -> A`.

This is our [old friend, `Endo`](https://github.com/pointfreeco/swift-prelude/blob/b26e98e82c7598fd5f381b3a27b59e27e312eb58/Sources/Prelude/Endo.swift#L1-L7)! At the bottom of the file, I noticed `imap` and asked some folks what the “i” stood for. Turns out  it’s short for, “invariant,” which reads nicely in that both co- and contravariance net out to invariance.

Pairing functor type, variance(s), and `*map` name:

- Functor, covariant, [`map`](https://github.com/pointfreeco/swift-prelude/blob/536b8856e38853854b5c5689e5b5d06da75c992e/Sources/Prelude/Func.swift#L20-L23).
- Functor, contravariant, [~~`contramap`~~](https://github.com/pointfreeco/swift-prelude/blob/536b8856e38853854b5c5689e5b5d06da75c992e/Sources/Prelude/Func.swift#L34-L37)[`pullback`](https://www.pointfree.co/blog/posts/22-some-news-about-contramap).
- Bifunctor, covariant (and I’m guessing contra-, maybe both working in the same direction is what matters?), [`bimap`](https://github.com/pointfreeco/swift-prelude/blob/b26e98e82c7598fd5f381b3a27b59e27e312eb58/Sources/Either/Either.swift#L118-L129).
- Invariant functor, invariant (co- and contravariant in the _same_ component), [`imap`](https://github.com/pointfreeco/swift-prelude/blob/536b8856e38853854b5c5689e5b5d06da75c992e/Sources/Prelude/Endo.swift#L21-L25).
- Profunctor, co- and contravariant along two components, [`dimap`](https://github.com/pointfreeco/swift-prelude/blob/536b8856e38853854b5c5689e5b5d06da75c992e/Sources/Prelude/Func.swift#L48-L52).

 