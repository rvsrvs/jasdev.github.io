---
layout: post
title: Conditional flatMap’ing
permalink: conditional-flatmap
type: engineering
---

(Assumed audience: folks with a working knowledge of reactive operators and type erasure. Familiarity with Combine isn’t needed to read along.)

Let’s warm up[^1] with a shorter post, shall we?

[Shai](https://twitter.com/freak4pc) showed me how to rewrite awkward “conditional [`flatMap`](https://developer.apple.com/documentation/combine/publisher/3204712-flatmap)s”—or relatedly, “conditional [`map`](https://developer.apple.com/documentation/combine/publisher/3204718-map) and [`switchToLatest`](https://developer.apple.com/documentation/combine/publisher/3204759-switchtolatest)” sequences.

Less abstractly, when we find ourselves in situations where upstream emits `Bool`s that are switched over and only `flatMap`ed (or `map`ed and later `switchToLatest`ed) on in `true` case.

I’ve ordered the comments below to read more easily.

<script src="https://gist.github.com/jasdev/8afa204fe3a208e9bf4326473f6b8898.js"></script>

If we squint at the chain long enough—feel free to do so, if you don’t want the answer spoiled—, a reworking shakes out.

And that’s what Shai taught me.

The `else` clause is a reactive no-op we can replicate by `filter`ing out `false` value events entirely.

<script src="https://gist.github.com/jasdev/5c1316119e2f0f173dd0c7f68f3dba51.js"></script>

A small trick to add to the tool belt. If you find yourself `Empty`ing out a sequence, it might be better to filter in on the conditions you’re considering, letting downstream lean on the checked precondition.

This is the reactive parallel to `guard`ing out early.

---

## Footnotes

[^1]: It’s been some time since I’ve written here—trying not to beat myself up over the hiatus because well, a lot of Life has happened since the [last entry](https://jasdev.me/operators) and well, _gestures widely at the macro state of affairs_.