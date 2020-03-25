---
layout: note
title: Parity and arity
date: 2020-3-17
---

Two tucked-away, somewhat-related terms I enjoy: [parity](https://en.wikipedia.org/wiki/Parity_(mathematics)) and [arity](https://en.wikipedia.org/wiki/Arity).

The former is the odd or even-ness of an integer.

The latter describes the number of arguments a function accepts.

## Example usage of parity:

[Today I learned about the Handshaking Lemma](https://twitter.com/_substrate/status/1239971265185607682). It states that any finite undirected graph, will have an _even_ number of vertices with an _odd_ [degree](https://en.wikipedia.org/wiki/Degree_(graph_theory)).

The [proof](https://en.wikipedia.org/wiki/Handshaking_lemma#Proof) rests on parity. Specifically, if you sum the degrees of every vertex in a graph, you’ll double count each edge. And that double counting implies the sum is even, and even parity is only maintained if there is an even—including zero—number of vertices with an odd degree.

Put arithmetically, a sum can only be even if its components contain an even number of odd terms.

## Examples of arity:

- Swift’s tuple comparison operators [topping out at arity six](https://github.com/apple/swift-evolution/blame/63b11d5454ce2bda65594a7879b456e33d470176/proposals/0015-tuple-comparison-operators.md#L48).
- `Publisher.zip` is only overloaded to [arity three](https://developer.apple.com/documentation/combine/publisher/3333687-zip).
	- I PR’d a [variadic overload](https://github.com/CombineCommunity/CombineExt/pull/6) to `CombineCommunity/CombineExt` yesterday and have a post walking through it in the works.
