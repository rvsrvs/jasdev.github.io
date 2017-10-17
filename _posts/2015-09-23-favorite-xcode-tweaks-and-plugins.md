---
layout: post
title: My Favorite Xcode Plugins and Tweaks
permalink: xcode-plugins-and-tweaks
---

_Updated 12/29/2015_

Eight months ago, I switched to focusing on iOS and Swift development full-time. I’ve spent a lot of time trying out different plugins and tweaks to make Xcode feel like home. Below are a handful that I can’t live without:

## Polychromatic

[Polychromatic](https://github.com/kolinkrewinkel/Polychromatic), by [@kkrewink](https://twitter.com/kkrewink), takes Xcode coloring to the next logical level. Instead of assigning each token type with a fixed color, it “gives properties, ivars, and local variables each a unique, dynamic color, stripping away emphasis from types which don’t need it.”

*Before*

![](/public/images/wo_polychromatic.png)

*After*

![](/public/images/w_polychromatic.png)

## eppz! Theme

My favorite theme by far has to be [eppz](https://github.com/eppz/iOS.Library.eppz_xCode). It is easy on the eyes and looks beautiful (used in the “*Before*” screenshot above).

Tweaking eppz with Polychromatic was a bit tricky for me to get right. If you’re interested, [here](/public/eppz!.dvtcolortheme) is my `.dvtcolortheme` file that you can import into Xcode by copying it to `~/Library/Developer/Xcode/UserData/FontAndColorThemes`.

## Replacement for the Default Xcode I-Beam Cursor

One of the problems with using darker Xcode themes is that the I-beam cursor is hard to find. [@stumbling_eric](https://twitter.com/stumbling_eric) solved this. He made a [replacement cursor](https://github.com/egold/better-xcode-ibeam-cursor) that is much easier to spot!

*Before*

![](/public/images/dark_ibeam.png)

*After*

![](/public/images/light_ibeam.png)

## Xcode TODO highlighting

In-between commits, I often leave `TODO`s sprinkled in my code. To avoid letting them slip into the repo’s history, I prefer to have Xcode make them as noticeable as possible. Wouldn’t it be nice if Xcode highlighted them as warnings? [@jakemarsh](https://twitter.com/jakemarsh) to the rescue! He wrote a [post](https://deallocatedobjects.com/posts/show-todos-and-fixmes-as-warnings-in-xcode-4) on how to have Xcode highlight `TODO`s on a per-project basis.

![](/public/images/todo_highlight.png)

*Note:*

To make the script in the post work for Swift, you’ll want to add a `-or -name "*.swift"` clause in the `find` command.

## VVDocumenter

I was recently introduced to [VVDocumenter](https://github.com/onevcat/VVDocumenter-Xcode) and immediately fell in love. VVDocumenter generates documentation for functions (or any code) by simply typing `///`.

![](http://i.imgur.com/S8zqem9.gif)

## Remap “Show Document Items” Shortcut

![](/public/images/document_items.png)

A wonderful feature of Xcode is the document outline that can be toggled from the toolbar or by pressing `⌃6`. However, the default shortcut is hard to reach, so I usually remap it to ``⌃` ``. This setting can be changed via Preferences > Key Bindings and then searching for“Show Document Items".

![](/public/images/document_items_setting.png)

## Custom Xcode Behavior on Build Success and Failure

Have a project with long compile times? Chances are you context switch while building. I’ve often caught myself getting distracted way after my build finishes. To prevent this, I added a custom Xcode behavior to make a sound when builds finish!

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">Xcode behavior to play a sound when builds finish -&gt; less wasted time when context switching during compilation <a href="https://t.co/9kXWuXjhQZ">pic.twitter.com/9kXWuXjhQZ</a></p>&mdash; jasdev singh (@jasdev) <a href="https://twitter.com/jasdev/status/675345062058921984">December 11, 2015</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Have any Xcode plugins or tweaks you love? [Let me know](https://twitter.com/jasdev)!
