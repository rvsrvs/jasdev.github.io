---
layout: post
title: Tips for Pair Programming
permalink: tips-for-pair-programming
---

This past Spring, I transitioned to the iOS team at [Imgur](http://imgur.com), after having worked on their API for the previous year. The motivation for this transition was my attendance of [CodePath](https://codepath.com)’s Winter cohort. CodePath is an eight week program that teaches experienced web engineers either iOS or Android[^1]. After completing the course, I was eager to bridge our [existing Objective-C app](https://itunes.apple.com/us/app/imgur/id639881495?mt=8) to [Swift](https://developer.apple.com/swift/)[^2] and help in any capacity that I could.

To ramp up on the codebase, the mobile team uses pair programming extensively. This worked out really well because I would transfer knowledge about the intricacies of Swift to the more senior engineers, while they would teach me the best practices of Cocoa Touch! We’ve learned a lot about what makes pairing work more effectively and even got the opportunity to try remote pairing. Most recently, we used pair programming (almost exclusively) in adding upload functionality to the app. This helped catch a ton of bugs! Below are some tips we highly recommend for pairing:

- Switching drivers is a lot easier when using two keyboards hooked up to the same computer.
- For remote pairing, we’ve been using [Screenhero](https://screenhero.com)[^3].
  - For Xcode pairing specifically, [CoPilot](http://feinstruktur.com/copilot/) looks promising. However, we haven’t tried it yet; so, we can’t vouch for how well it works.
- Switch drivers after completing major logical chunks. This allows the driver to get into the code without having to be interrupted by hard time limits.
  - Both partners should also agree on a cadence for switching before they start pairing.
- It’s sometimes helpful to have a second computer nearby to reference docs, as needed.
- If you’re not currently driving and are having trouble following, speak up. You’ll often catch bugs by having the driver explain their thought process aloud (similar to the benefit of [Rubber Duck Debugging](http://en.wikipedia.org/wiki/Rubber_duck_debugging)).
- Get snacks and drinks before you start pairing to minimize distractions once you’ve gotten in the zone.
- [TDD](https://en.wikipedia.org/wiki/Test-driven_development) and pairing go really well together. One person writes the tests and the other writes the corresponding implementation.
- Shut down anything that can generate notifications. OS X has a “Do Not Disturb” mode available in the Notification Center.
- Make sure to take breathers and have fun!

Hope these help make your pairing sessions better! Have any other tips that I forgot to mention? Let me know on [Twitter](https://twitter.com/jasdev)!

---

## Footnotes:

[^1]: I was lucky enough to be in one of the first few batches that used Swift.

[^2]: Post about the lessons we learned coming soon!

[^3]: They have closed new registrations, post-Slack acquisition. So, you’ll need an existing user to [invite you](http://blog.screenhero.com/post/110852538851/already-a-screenhero-user-heres-how-to-invite)!
