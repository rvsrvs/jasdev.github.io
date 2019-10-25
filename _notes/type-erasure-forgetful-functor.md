---
layout: note
title: Type erasure and forgetful functors
date: 2019-10-24
---

Type erasure and [forgetful functors](https://en.wikipedia.org/wiki/Forgetful_functor), at least in terms of intuition, feel very similar.

One removes detail (e.g. [`Publishers.Zip`](https://developer.apple.com/documentation/combine/publishers/zip) -> [`AnyPublisher`](https://developer.apple.com/documentation/combine/anypublisher)) and the other strips structure, leaving the underlying set behind (a monoid or group being mapped onto its base set).

Wonder if there’s a way to visualize this by considering [`eraseToAnyPublisher`](https://developer.apple.com/documentation/combine/anypublisher/3241539-erasetoanypublisher) as a sort of forgetful endofunctor into a subcategory of __Swift__ (hand waving) that only contains `AnyPublisher`s?
