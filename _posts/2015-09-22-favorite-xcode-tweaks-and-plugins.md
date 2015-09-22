---
layout: post
title: A Few of My Favorite Xcode Plugins and Tweaks
permalink: xcode-plugins-and-tweaks
---

Eight months ago, I switched to focusing on iOS and Swift development full-time. I've spent a lot of time tying out different plugins and tweaks to make Xcode feel like home. Below are a handful that I can't live without:

## Polychromatic

[Polychromatic](https://github.com/kolinkrewinkel/Polychromatic), by [@kkrewink](https://twitter.com/kkrewink), takes Xcode coloring to the next logical level. Instead of assigning each token type with a fixed color, it "gives properties, ivars, and local variables each a unique, dynamic color, stripping away emphasis from types which don't need it."

*Before*

![](/public/images/wo_polychromatic.png)

*After*

![](/public/images/w_polychromatic.png)

## eppz! Theme

My favorite theme by far has to be [eppz](https://github.com/eppz/iOS.Library.eppz_xCode). It is easy on the eyes and looks beautiful (used in the '*After*' screenshot above).

## Replacement for the Default Xcode I-Beam Cursor

One of the problems with using darker Xcode themes is that the I-beam cursor is hard to find. [@stumbling_eric](https://twitter.com/stumbling_eric) solved this. He made a [replacement cursor](https://github.com/egold/better-xcode-ibeam-cursor) that is much easier to spot!

*Before*

![](/public/images/dark_ibeam.png)

*After*

![](/public/images/light_ibeam.png)

## Xcode TODO highlighting

In-between commits, I often leave `TODO`s sprinkled in my code. To avoid letting them slip into the repo's history, I prefer to have Xcode make them as noticeable as possible. Wouldn't it be nice if Xcode highlighted them as warnings? [@jakemarsh](https://twitter.com/jakemarsh) to the rescue! He wrote a [post](https://deallocatedobjects.com/posts/show-todos-and-fixmes-as-warnings-in-xcode-4) on how to have Xcode highlight `TODO`s on a per-project basis.

![](/public/images/todo_highlight.png)

*Note:*

To make the script in the post work for Swift, you'll want to add a `-or -name "*.swift"` clause in the `find` command.

Have any Xcode plugins or tweaks you love? [Let me know](https://twitter.com/jasdev)!
