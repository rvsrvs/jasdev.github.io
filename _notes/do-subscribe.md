---
layout: note
title: ObservableType.do‌ or .subscribe (Publisher.handleEvents or .sink)?
date: 2019-10-16
---

[`ObservableType.do`](https://github.com/ReactiveX/RxSwift/blob/9b31a15520306b073cb9d46456f64826c1d6dcab/RxSwift/Observables/Do.swift#L10-L48)‌ or [`.subscribe`](https://github.com/ReactiveX/RxSwift/blob/9b31a15520306b073cb9d46456f64826c1d6dcab/RxSwift/ObservableType%2BExtensions.swift#L29-L84)?

(The question, rephrased for Combine, involves swapping in [`Publisher.handleEvents`](https://developer.apple.com/documentation/combine/publisher/3204713-handleevents) and [`.sink`](https://developer.apple.com/documentation/combine/publisher/3343978-sink), respectively.)

`do` (`handleEvents`) gives you a hook into a sequence’s events _without_ triggering a subscription, while `subscribe` (`sink`) does the same and triggers a subscription—a way to remember this is the former returns an `Observable` (`Publisher`) and the latter returns a `Disposable` (`AnyCancellable`).

So, when should we use one over the other?

In the past, I’ve always placed _effectful_ work in the `do` block, even if meant an empty `subscribe()` call at the end of a chain.

And I’m starting to change my mind here—putting that sole `do` work in the `subscribe` block makes the chain more succinct (there are definitely cases where it’s cleaner to use both to sort of group effects, e.g. UI-related versus persistence-related).

I dug up an old bit from [Bryan Irace](https://twitter.com/irace) in an iOS Slack we’re a part of that puts it well:

> `do` is for performing a side effect inside of a bigger chain

> if all you’re doing is performing the side effect, just do that in your `subscribe` block

