---
layout: post
title: TIL About @nonobjc
permalink: when-to-use-nonobjc
type: interlude
---

In my free time, I run a Twitter account called [Public Extension](https://twitter.com/PublicExtension), where I share handy Swift extensions I’ve found or come up with. A recent extension involved adding [custom subscripts](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Subscripts.html) to [`NSUserDefaults`](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/).

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">4️⃣6️⃣: Make NSUserDefaults a bit easier to work with 😃<a href="https://t.co/1eULFysnXb">https://t.co/1eULFysnXb</a> <a href="https://t.co/7127Y6PCB1">pic.twitter.com/7127Y6PCB1</a></p>&mdash; Public Extension (@PublicExtension) <a href="https://twitter.com/PublicExtension/status/702543100095426560">February 24, 2016</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

One attribute that sticks out is `@nonobjc`. Often times, you’ll need to use `@objc`[^1] to expose your Swift classes that don’t derive from an Objective-C class to the Objective-C runtime. It’s rare to _explicitly_ cut that interoperability. Let’s dig into why this attribute is necessitated here by dropping it and deconstructing the compilation error:

![](/public/images/nonobjc_error.png)

The full error reads “Subscript getter with Objective-C selector ’`objectForKeyedSubscript:`’ conflicts with previous declaration with the same Objective-C selector” (and the analog for the setter). Turns out that Objective-C also supports [custom subscripting](http://nshipster.com/object-subscripting/) and Swift will automatically bridge them back! However, in our example, we’re overloading the subscript with the same key and different return types (also referred to as return type polymorphism), which Objective-C doesn’t support 😔. To fix this, we simply prevent the default bridging via `@nonobjc`.

So, there you have it! A real example of the `@nonobjc` attribute in action. Hope this post sheds some light on why it exists 😃

Shout-out to [Joe Groff](https://twitter.com/jckarter) for the help and [Garric Nahapetian](https://twitter.com/garricn) for motivating me to write this post!

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/jasdev">@jasdev</a> Overloading will work if you mark one or both subscripts as <a href="https://twitter.com/nonobjc">@nonobjc</a>.</p>&mdash; Joe Groff (@jckarter) <a href="https://twitter.com/jckarter/status/701536484873019392">February 21, 2016</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/PublicExtension">@PublicExtension</a> This looks really fun. I would love to hear more about this solution. Is there a blog post about it?</p>&mdash; Garric G. Nahapetian (@garricn) <a href="https://twitter.com/garricn/status/702979513970327552">February 25, 2016</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

---

## Footnotes:

[^1]: Swift also allows for the `@objc(name)` variant, which uses `name` when exposing the target class to the Objective-C runtime.
