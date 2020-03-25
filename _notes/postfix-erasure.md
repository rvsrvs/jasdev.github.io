---
layout: note
title: Postfix type erasure
date: 2020-3-25
---

A belated entry on an operator [I posted](https://twitter.com/jasdev/status/1223316966829699072) before…all of this (_gestures wildly_) started.

⬦

There’s nuance in determining whether or not to type erase a publisher—my next longer-form post will cover this—but when you need to, [`eraseToAnyPublisher()`](https://developer.apple.com/documentation/combine/publishers/output/3241611-erasetoanypublisher)’s ergonomics aren’t great.

It requires 22 characters (including a dot for the method call and `Void` argument) to apply a rather one-character concept.

And I know operators are borderline #holy-war—still, if you’re open to them, I’ve borrowed prior art from Bow and Point-Free by using a `^` postfix operator.

It passes the three checks any operator should.

- Does the operator overload an existing one in Swift? Nope ([bitwise XOR](https://docs.swift.org/swift-book/LanguageGuide/AdvancedOperators.html#ID33) is infix).
- Does its shape convey its intent? To an extent! The caret hints at a sort of “lifting” and that’s what erasure is after all. Removing specific details, leaving behind a more general shape.
- Does it have prior art? Yep.
	- Bow defines one to [ease higher-kinded type emulation](https://bow-swift.io/docs/fp-concepts/higher-kinded-types/#casting-and-the--operator).
	- Point-Free [defined a prefix variant](https://github.com/pointfreeco/swift-prelude/blob/c61b49392768a6fa90fe9508774cf90a80061c8b/Sources/Prelude/KeyPath.swift#L25-L31) to lift key path expressions into function form before [SE-0249](https://github.com/apple/swift-evolution/blob/53c2e80bdede03b99aee683fe6399b6e4cf0bf95/proposals/0249-key-path-literal-function-expressions.md) landed in Swift 5.2

The operator has tidied the Combine I’ve written so far. Here’s [a gist](https://gist.github.com/jasdev/0cf0663c4f54446b046f946b1f7af5a9#file-postfix_type_erasure-swift) with its definition.

<script src="https://gist.github.com/jasdev/0cf0663c4f54446b046f946b1f7af5a9.js"></script>
