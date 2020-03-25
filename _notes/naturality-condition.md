---
layout: note
title: Naturality condition
date: 2019-11-1
---

I’m currently working through Mio Alter’s post, “[Yoneda, Currying, and Fusion](https://mioalter.github.io/posts/2018-12-17-Yoneda.html)” (his other piece on [equivalence relations](https://mioalter.github.io/posts/2019-09-04-Equivalence-Relations.html) is equally stellar).

Early on, Mio mentions:

> It turns out that being a natural transformation is hard: the commutative diagram that a natural transformation has to satisfy means that the components […] fit together in the right way.

Visually, the diagram formed by functors _F_ and _G_ between _C_ and _D_ and with natural transformation _α_ must _commute_ (signaled by the rounded arrow in the center).

![](/public/images/naturality_commuting_square.png)

I’m trying to get a sense for why this condition is “hard” to meet.

What’s helped is making the commutativity explicit by drawing the diagonals and _seeing_ that they must be the equal. The four legs of the two triangles that cut the square must share a common diagonal.

![](/public/images/naturality_commuting_square_explicit.png)

Maybe that’s why natural transformations are rare? There might be many morphisms between _F(A)_ and _G(A)_ and _F(B)_ and _G(B)_, but only a select few (or none) which cause their compositions to coincide.

For more on commutative diagrams, Tai-Danae Bradley has a [post dedicated to the topic](https://www.math3ma.com/blog/commutative-diagrams-explained).