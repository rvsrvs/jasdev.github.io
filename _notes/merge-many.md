---
layout: note
title: TIL about Publishers.MergeMany.init
date: 2019-12-13
---

A few weeks back, [Jordan Morgan](https://www.twitter.com/jordanmorgan10) nerd sniped me[^1] into writing a Combine analog to [RxSwift’s `ObservableType.merge(sources:)`](https://github.com/ReactiveX/RxSwift/blob/6b2a406b928cc7970874dcaed0ab18e7265e41ef/RxSwift/Observables/Merge.swift#L84-L120)—an operator that can merge arbitrarily many observable sequences.

Here’s a rough, not-tested-in-production sketch (if you know a way too ease the `eraseToAnyPublisher` dance, let me know):

<script src="https://gist.github.com/jasdev/014f5353aa334b4e5e42f102d6276ad9.js"></script>

And in writing this entry, I decided to check and make sure I wasn’t missing something. After all, it’s odd (pun intended) that the merging operators on `Publisher` stop at arity seven.

![](/public/images/merge_overloads.png)

Then, I noticed the `Publishers.MergeMany` return value on the first and, below the (hopefully temporary) “No overview available” note on its documentation, there’s [a variadic initializer](https://developer.apple.com/documentation/combine/publishers/mergemany/3236390-init)!

So, there you have it. TIL merging a _sequence_ of publishers is as easy as `Publishers.MergeMany.init(_:)`.

---

[^1]: In [iOS Folks’ #reactive channel](https://iosdevelopers.slack.com/archives/C0AET0JQ5/p1574800569370700?thread_ts=1574786869.354400&cid=C0AET0JQ5).
