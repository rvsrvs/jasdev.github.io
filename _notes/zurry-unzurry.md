---
layout: entry
title: (un)zurry
date: 2019-10-16
---

`(un)zurry` moves arguments in and out of `Void`-accepting closures in the same way `(un)curry` moves arguments in and out of tuples. Concretely, here’s how they’re defined:

<script src="https://gist.github.com/jasdev/320cd8dc45543da0f6fca4d861f61e66.js"></script>

<script src="https://gist.github.com/jasdev/31b72f034f2ba9768c905b7dad33eb00.js"></script>

The former is often helpful when staying point-free with functions that are close, but not quite the right shape you need. e.g. mapping unapplied method references:

<script src="https://gist.github.com/jasdev/ef71ee51dd159f8b0c8a8f0b660e138a.js"></script>

Eep.

Let’s try to make this work.

We’d like `[Int].sorted` to have the form `[Int] -> [Int]`, and since [SE-0042](https://github.com/apple/swift-evolution/blob/d32cf1020ee523b959ef5e0634dad197bc2e957a/proposals/0042-flatten-method-types.md) got rejected, we have to chisel it down on our own.

First, let’s flip the first two arguments and then address the rogue `()`.

<script src="https://gist.github.com/jasdev/bcd7db55446b1737d80f1fd410ff1134.js"></script>

Getting closer—this is where `zurry` can _zero_ out the initial `Void` for us.

`zurry(flip([Int].sorted)) // (Array<Int>) -> Array<Int>`

Wicked, now we can return to our original example:

`[[2, 1], [3, 1], [4, 1]].map(zurry(flip([Int].sorted))) // [[1, 2], [1, 3], [1, 4]]`.

Dually, `unzurry` shines when you’re working against a `() -> Return` interface, that isn’t `@autoclosure`’d, and you only have a `Return` in hand. Instead of opening a closure, you can pass along `unzurry(yourReturnInstance)` and call it a day.

The Point-Free folks link to Eitan Chatav’s post where he introduced the term and shows how function application is [a zurried form of function composition](https://tangledw3b.wordpress.com/2013/01/18/cartesian-closed-categories/) (!).