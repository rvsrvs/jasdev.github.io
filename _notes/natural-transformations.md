---
layout: note
title: What makes natural transformations, natural?
date: 2019-10-19
---

Learning category theory often involves patiently sitting with a concept until it—eventually—clicks and then considering it as a building block for the next[^1].

Grokking natural transformations went that way for me.

I still remember the team lunch last spring where I couldn’t keep my excitement for the abstraction a secret and had to share with everyone (I’m a blast at parties).

After mentioning the often-cited example of a natural transformation in engineering, [`Collection.first`](https://developer.apple.com/documentation/swift/collection/3017676-first) (a transformation between the `Collection` and `Optional` functors), a teammate asked me the question heading this note:

> What makes a natural transformation, natural?

I found an interpretation of the word.

Say we have some categories `C` and `D` and functors `F` and `G` between them, diagrammed below:

![](/public/images/natural_transformation.png)

If we wanted to move from the _image_ of `F` acting on `C` to the image of `G`, we have to find a way of moving between objects in the _same_ category.

The question, rephrased, becomes what connects objects in a category? Well, morphisms!

Now, how do we pick them? Another condition on natural transformations is that the square formed by mapping two objects, `A` and `B` connected by a morphism `f`, across two functors `F` and `G` must commute.

Let’s call our choices of morphisms between `F_A` and `G_A` and `F_B` and `G_B`, `α_A` and `α_B`, respectively.

![](/public/images/commuting_square.png)

Even if `f` flips directions across `F` and `G`—i.e. they’re contravariant functors—our choice in `α_A` and `α_B` is fixed!

The definition, the choice of morphisms,  seems to _naturally_ follow from structure at hand. It doesn’t depend on arbitrary choices.

Tangentially, a definition shaking out from some structure reminds me of how the [Curry–Howard correspondence](https://en.wikipedia.org/wiki/Curry–Howard_correspondence) causes certain functions to have a unique implementation. Brandon covered this topic in a past [Brooklyn Swift presentation](https://vimeo.com/121953811#t=1294s) (timestamped).

For more resources on natural transformations:

- [What is a Natural Transformation? Definition and Examples](https://www.math3ma.com/blog/what-is-a-natural-transformation)
- [What is a Natural Transformation? Definition and Examples, Part 2](https://www.math3ma.com/blog/what-is-a-natural-transformation-2)
- Bartosz’s [1.9.1 lecture](https://youtu.be/2LJC-XD5Ffo) on the topic.

---

[^1]: Then, [repeating further](https://twitter.com/mbrandonw/status/936376614048485385). One day, I hope to have built the machinery needed to read [Riehl](https://twitter.com/emilyriehl)’s research [and writing](http://www.math.jhu.edu/~eriehl/elements.pdf) on ∞-category theory.

