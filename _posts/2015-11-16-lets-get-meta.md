---
layout: post
title: Let's Get Meta&#58; Swift Metatypes and Cell Reuse
permalink: lets-get-meta
---

For the past few weeks, I've been running a small project called [Public Extension](https://twitter.com/publicextension). Each weekday[^1], I come up with a handy Swift extension (to the [Standard Library](http://swiftdoc.org), UIKit, etc.) and tweet it out for everyone to follow along!

One particular extension has been on the back of my mind for a few days now. [Public Extension #9](https://twitter.com/PublicExtension/status/656489984485146624) (initial draft below)

![Reusable protocol and UITableView Extension](http://i.imgur.com/ow2GPZp.png)

The goal was to make class registration for `UITableViewCell`s safer by avoiding a `String`ly-based API that requires code to be updated in multiple places, when reuse identifiers change. To tackle this, I took the following approach:

- Define a `Reusable` protocol that requires classes to have a static `reuseIdentifier` member
- Wrap the traditional `registerClass(_:forCellReuseIdentifier:)` to accept a `Reusable.Type` metatype, which hides the previously-needed `forCellReuseIdentifier` parameter.
- In my initial extension, I omitted the wrapper for `dequeueReusableCellWithIdentifier(_:)` and `dequeueReusableCellWithIdentifier(_:forIndexPath:)`, but we'll discuss these soon!

*Note: This approach only works in code. Setting reuse identifiers in Storyboards requires you to manually enter a `String`.*

Before diving in and refining the above approach, let's take a step back and define what `Reusable.Type` and `CustomCell.self` mean in the snippet above. They're making use of Swift's [Metatype Type](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Types.html#//apple_ref/doc/uid/TP40014097-CH31-ID455).

## Swift Metatype Type

In [The Swift Programming Language](https://itunes.apple.com/us/book/swift-programming-language/id881256329?mt=11) book, the metatype type is defined as follows:

> "A metatype type refers to the type of any type, including class types, structure types, enumeration types, and protocol types."

This is exactly what we need. Since our `Resuable` protocol defines `reuseIdentifier` as a **static** member, we can access it from the metatype of any class that conforms to it! To access a type as a value, we can use a postfix `self` expression (i.e. `.self`). For example, in our `CustomCell` class above, `CustomCell.self` returns `CustomCell.Type` not an instance of `CustomCell`.

## Refining the Approach

Now that we've gone over metatypes, let's make our approach to safe cell reuse better! We'll keep the same `Reusable` protocol but redefine our `UITableView` extension as follows:

<script src="https://gist.github.com/Jasdev/efe1feb9179fa1f8dd04.js?file=UITableView%2BReusable.swift"></script>

While we could have used Swift's error handling, cell misconfiguration is tricky to recover from. If misconfigured, it might be misleading to return a plain `UITableViewCell`. So, it's probably better to lean towards a `fatalError` to catch these issues during development.

Note that the return types for the `dequeueReusable` overloads are non-optional. This is really handy because we no longer have to deal with awkward casts in `tableView(_:cellForRowAtIndexPath:)`. Here is how the call site might look:

<script src="https://gist.github.com/Jasdev/efe1feb9179fa1f8dd04.js?file=SampleTableViewController.swift"></script>

### Extending `UICollectionView`

We can apply this same pattern to `UICollectionView` as well:

<script src="https://gist.github.com/Jasdev/efe1feb9179fa1f8dd04.js?file=UICollectionView%2BReusable.swift"></script>

## A Note on Cell Subclassing

One of my friends, [Hans](https://twitter.com/Lumilux), brought up a potential gotcha with regard to subclassing cells that implement `Reusable`.

<blockquote class="twitter-tweet" data-conversation="none" lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/jasdev">@jasdev</a> <a href="https://twitter.com/PublicExtension">@PublicExtension</a> this doesn&#39;t allow for inheritance for classes that implement `Reusable`, right?</p>&mdash; Hans E Hyttinen (@Lumilux) <a href="https://twitter.com/Lumilux/status/662411481842065408">November 5, 2015</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

The `static` keyword in Swift translates to `class final`[^2], meaning that the property is attached to a class but cannot be overridden. However, when defining `Reusable`, we have to use the `static` keyword (even though we have the explicit `class` requirement on the protocol) because the class keyword is only allowed within `class` definitions. To allow for subclassing of cells that conform to `Reusable`, we can define `reuseIdentifier` as a computed[^3] `class var`.

<script src="https://gist.github.com/Jasdev/efe1feb9179fa1f8dd04.js?file=CustomCell.swift"></script>

It's a little awkward that we can conform to a protocol (constrained to `class`) with a static requirement using a `class var`. Would love it if [Chris Lattner](https://twitter.com/clattner_llvm) or [Joe Groff](https://twitter.com/jckarter) could chime in on this!

## Summary

So, there we have it! We've abstracted away a `String`ly-based API for cell reuse in table and collection views. Now, if we ever need to change reuse identifiers, all we have to do is update the the cell definition. This benefits from code locality. I had also thought about an `enum`-based approach, but this would move reuse identifiers farther away from cell definitions, as `enum`s can't be defined across multiple files. However, the `enum` approach has one advantage, which is guaranteeing unique reuse identifiers.

Hope this pattern is useful! If you have any feedback or suggestions, I'd love to [hear from you](https://twitter.com/jasdev)!

---

## Footnotes:

[^1]: Took a break between November 4th through 16th for job interviews :)

[^2]: In the case of classes

[^3]: Stored `class` properties aren't supported yet.
