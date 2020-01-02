---
layout: note
title: Contrasting isomorphisms, equivalencies, and adjunctions
date: 2020-1-1
---

When I was first introduced to adjunctions, 
I reacted in the way Spivak anticipated during an [applied category theory lecture](https://youtu.be/Cf3tsAeGhBg?t=203) (timestamped, transcribed below).

> …and when people see this definition [of an adjunction], they think “well, that seems…fine. I’m glad you told me about…that.”

And it wasn’t until I stumbled upon a [Catsters lecture from 2007](https://www.youtube.com/watch?v=loOJxIOmShE) (!) where Eugenia Cheng clarified the intent behind the definition (and contrasted it to isomorphisms and equivalencies).

To start, assume we have two categories `C` and `D` with functors, `F` and `G` between them (`F` moving from `C` to `D` and `G` in the other direction).

There are a few possible scenarios we could find ourselves in.

- Taking the round trip—i.e. `GF` and `FG`—are __equal__ to the identity functors on `C` and `D` (denoted with `1_C` and `1_D` below). 
- The round trip is __isomorphic__ to each identity functor.
- Or, the round trip lands us __a morphism away__ from where we started.

![](/public/images/iso-equiv-adjunction.png)

Moving between the scenarios, there’s a sort of “relaxing” of strictness:

- The round trip is the identity functor.
- […] an isomorphism away from the identity functor.
- […] a hop away from identity functor.
