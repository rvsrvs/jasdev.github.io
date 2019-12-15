---
layout: note
title: Why Optics?
date: 2019-12-15
---

The better part of my weekend was spent reading Chris Penner’s incredibly well-written _[Optics by Example](https://leanpub.com/optics-by-example)_ and attempting the exercises with [BowOptics](https://bow-swift.io/docs/optics/overview/).

I’m early in my learning. Still, I wanted to note the motivation behind optics.

They seek to capture control flow, which, is usually baked into languages with `for`, `while`, `if`, `guard`, `switch`, and similar statements, _as values_.

In the same way that effect systems capture effects as values—decoupling them from execution—optics separate control flow when navigating data structures from the actions taken on them.

Sara Fransson put this well in their recent talk: _[Functional Lenses Through a Practical Lens](https://youtu.be/sFzuu676pFs?t=232)_.

■

Noticing that optics excite me much like FRP did back when I first learned about it.

And I haven’t even gotten to the category-theoretic backings yet.

(_Attempts to contain excitement._ Back to reading.)
