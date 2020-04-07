---
layout: note
title: There’s sliced bread, and then Result.publisher
date: 2020-4-7
---

Another learning from [Adam](http://twitter.com/sharplet).

A situation I often find myself in is sketching an operator chain and exercising both the value and failure paths by swapping upstream with [`Just`](https://developer.apple.com/documentation/combine/just) or [`Fail`](https://developer.apple.com/documentation/combine/fail), respectively.

And it turns out that Apple added [a Combine overlay to `Result` with the `.publisher` property](https://developer.apple.com/documentation/swift/result/3344716-publisher) that streamlines the two. That is, while all three of `Just`, `Fail`, and `Result.Publisher` have their uses, the latter might be easier to reach for in technical writing. Moreover, it’s a quick way to [`materialize` a `throw`ing function](https://github.com/antitypical/Result/blob/c0838342cedfefc25f6dd4f95344d376bed582c7/Result/Result.swift#L228-L236) and pipe it downstream.

<script src="https://gist.github.com/jasdev/7ae67582c06845e41d6007c721200971.js"></script>

([Gist permalink](https://gist.github.com/jasdev/7ae67582c06845e41d6007c721200971).)

Or, as I’ll call it going forward—“[the ol’ razzle dazzle.](https://twitter.com/jasdev/status/1247190267129745414)”

<blockquote class="twitter-tweet" data-conversation="none" data-lang="en" data-theme="light"><p lang="und" dir="ltr"><a href="https://t.co/o7EcZqNBlj">pic.twitter.com/o7EcZqNBlj</a></p>&mdash; Jasdev Singh (@jasdev) <a href="https://twitter.com/jasdev/status/1247190267129745414?ref_src=twsrc%5Etfw">April 6, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
