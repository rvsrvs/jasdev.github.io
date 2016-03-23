---
layout: post
title: Hiding Selector Methods
permalink: private-selector-methods
---

On the [Tumblr](https://www.tumblr.com) iOS team, we have a strict rule that everything with an `internal` or `public` visibility must be documented<sup>1</sup>. This gets a bit awkward when using the [Target-Action](https://developer.apple.com/library/ios/documentation/General/Conceptual/Devpedia-CocoaApp/TargetAction.html) pattern that `UIKit` often forces you into. When dealing with multiple action selectors, it makes sense to factor out the target into a separate object. However, we'll often just need a one-off action selector to handle an event from a `UIControl` subclass. Previously, this would lead to the following scenario and awkward comment:

<script src="https://gist.github.com/Jasdev/3338cda9d6c799323abe.js"></script>

I was curious if we could make `SampleViewController.buttonTapped(_:)` `private`, thus avoiding the need specify that the method _shouldn't_ be called externally, in its documentation. My teammate [Paul](https://twitter.com/paulrehkugler) pointed out that this is actually possible! To do this, we simply have to expose the method to the Objective-C runtime and give it a `private` access modifier:

<script src="https://gist.github.com/Jasdev/ea842f5a4527dae5d9e3.js"></script>

This allows our action to be invoked from a selector, but prevents other files from accessing the method directly 🎉 Hope this helps better encapsulate your types!

---

<sup>1</sup>: To help with this, we use [VVDocumenter](https://github.com/onevcat/VVDocumenter-Xcode).
