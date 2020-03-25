---
layout: post
title: A tour of iOS interactive notifications
permalink: ios-interactive-notifications
type: interlude
---

Starting in iOS 8, Apple opened up APIs to allow actions to be embedded in notifications. Moreover, in the upcoming release of iOS 9, these actions can now accept text input as opposed to simply being buttons (this had previously been restricted to system apps, like Messages). In a recent project of mine at [Recurse Center](https://www.recurse.com), I got the chance to use these notification actions and wanted to create a post on how to incorporate them into your existing apps!

![](/public/images/reply.png)
![](/public/images/textinput.png)

*Note: The example below uses local notifications, but there are analogs for their remote counterparts.*

## Request Permissions

These actions must be registered upfront alongside the normal notifications permissions. For the sake of this example, I’m going to place this logic in my `AppDelegate`. However, in a larger project, I’d recommend factoring this out into a Notification helper class.

Let’s start by building two instances of `UIMutableUserNotificationAction` and attaching them to a `UIMutableUserNotificationCategory`. To allow text input on the `replyAction`, note that the `behavior` property is set to `.TextInput`.

<script src="https://gist.github.com/Jasdev/8eae09fb1efcb79019b7.js?file=AppDelegate.swift"></script>

That’s all you need for permissions! Let’s schedule a sample notification to see the results and implement the appropriate `UIApplicationDelegate` methods to handle them. I make use of the handy Pod, [Timepiece](https://github.com/naoty/Timepiece), which adds syntactic sugar to `NSDate`.

<script src="https://gist.github.com/Jasdev/8eae09fb1efcb79019b7.js?file=NotificationScheduler.swift"></script>

Swiping down on the notification when the app is backgrounded.

![](/public/images/actions.png)

To handle notifications when the app is foregrounded, implement the [`application(_:didReceiveLocalNotification:)`](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intfm/UIApplicationDelegate/application:didReceiveLocalNotification:) method in the `UIApplicationDelegate` protocol).

UI for text input reply flow
![](/public/images/replyaction.png)

## Handling Notification Actions

Next, we’ll implement the `UIApplicationDelegate` protocol method for handling notification actions. To access the user-inputted text, we key into `responseInfo` with the `UIUserNotificationActionResponseTypedTextKey` constant. Also, the `Notifications.Actions` enum we defined earlier combined with a guard at the top of the function makes the subsequent `switch` very clean!

<script src="https://gist.github.com/Jasdev/8eae09fb1efcb79019b7.js?file=AppDelegateContinued.swift"></script>

That’s it! As an additional note, if you happened to set your `UIMutableUserNotificationAction`’s `activationMode` to `.Foreground`, the way to handle this flow on application launch is as follows:

<script src="https://gist.github.com/Jasdev/8eae09fb1efcb79019b7.js?file=AppDelegateAside.swift"></script>

You can reference the full source of this post [here](https://gist.github.com/Jasdev/8eae09fb1efcb79019b7) and for more info, check out the [“What’s New in Notifications” WWDC 2015](https://developer.apple.com/videos/wwdc/2015/?id=720) session.

Enjoyed this post? [Let me know](https://twitter.com/intent/tweet?text=A%20Tour%20of%20iOS%20Interactive%20Notifications%20by%20%40jasdev%20-%20http%3A%2F%2Fjasdev.me%2Fios-interactive-notifications%2F)!
