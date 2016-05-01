---
layout: post
title: Swift's nonmutating Keyword
permalink: nonmutating
---

In preparing my recent "[Hidden Gems in Swift](https://speakerdeck.com/jasdev/hidden-gems-in-swift)" talk, I carefully combed through Swift's "[Lexical Structure](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/LexicalStructure.html)" section. One keyword that stuck out to me, but ultimately didn't make it into the talk was `nonmutating`. Much like the [`@nonobjc`](/when-to-use-nonobjc) attribute, it surprised me that I hadn't seen this keyword in the wild.

In searching for example uses, I stumbled upon this conversation between [Andy Matuschak](https://twitter.com/andy_matuschak/) and [Sidney San Martín](https://twitter.com/Sidnicious):

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Interesting new Swift keyword I learned about today: &quot;nonmutating.&quot;<br><br>e.g. UnsafeMutablePointer&#39;s<br><br>var memory: T { get nonmutating set }</p>&mdash; Andy Matuschak (@andy_matuschak) <a href="https://twitter.com/andy_matuschak/status/522529881717215232">October 15, 2014</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/andy_matuschak">@andy_matuschak</a> I just discovered this myself. I&#39;m using an enum to manage app defaults, and now I can…<br><br>AppDefault.Bounciness.intValue = 5</p>&mdash; Sidney (@Sidnicious) <a href="https://twitter.com/Sidnicious/status/653618390557413376">October 12, 2015</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/Sidnicious">@Sidnicious</a> Yikes. What does that mean? Have a gist?</p>&mdash; Andy Matuschak (@andy_matuschak) <a href="https://twitter.com/andy_matuschak/status/653618565271040000">October 12, 2015</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none" data-cards="hidden" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/andy_matuschak">@andy_matuschak</a> Think it&#39;s too much? <a href="https://t.co/WiK4Onj8B1">https://t.co/WiK4Onj8B1</a></p>&mdash; Sidney (@Sidnicious) <a href="https://twitter.com/Sidnicious/status/653619188494434304">October 12, 2015</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/Sidnicious">@Sidnicious</a> Yes. :)<br><br>That setter *is* mutating. This will break compilation semantics.</p>&mdash; Andy Matuschak (@andy_matuschak) <a href="https://twitter.com/andy_matuschak/status/653619522843217920">October 12, 2015</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/andy_matuschak">@andy_matuschak</a> Hah! That&#39;s fair. I&#39;d consider it nonmutating, though, because it doesn&#39;t change the instance itself, just global state. No?</p>&mdash; Sidney (@Sidnicious) <a href="https://twitter.com/Sidnicious/status/653620291629924352">October 12, 2015</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/Sidnicious">@Sidnicious</a> Hm. I think you’re right, actually!</p>&mdash; Andy Matuschak (@andy_matuschak) <a href="https://twitter.com/andy_matuschak/status/653620726801395712">October 12, 2015</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Stepping through Sidney's example, we can see how `nonmutating` signals that a setter doesn't modify the containing instance, but instead has global side effects.

<script src="https://gist.github.com/Jasdev/cd96d61277a8b1c1e6f4b413d9ccc645.js"></script>

Whether or not this is good practice is a larger question. But, it's definitely worth adding to your tool belt!
