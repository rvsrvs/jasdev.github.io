---
layout: post
title: Case Closed&#58; A Dive into Open Enumerations
permalink: open-enums
---

Time for a pop quiz. Given that [`NSFetchedResultsChangeType`](https://developer.apple.com/reference/coredata/nsfetchedresultschangetype) is defined as follows:

```objc
typedef NS_ENUM(NSUInteger, NSFetchedResultsChangeType) {
	NSFetchedResultsChangeInsert = 1,
	NSFetchedResultsChangeDelete = 2,
	NSFetchedResultsChangeMove = 3,
	NSFetchedResultsChangeUpdate = 4
} NS_ENUM_AVAILABLE(NA, 3_0);
```

What is the type of `changeType` in the following expression (assuming Swift 3.x)?

```swift
let changeType = NSFetchedResultsChangeType(rawValue: 5)
```

_Hint: Enumerations with raw values in Swift implicitly conform to [`RawRepresentable`](https://developer.apple.com/reference/swift/rawrepresentable) (⇒ the presence of `init?(rawValue:)`)._

__(Corgi for padding between question and answer)__

<blockquote class="instagram-media" data-instgrm-version="7" style=" background:#FFF; border:0; border-radius:3px; box-shadow:0 0 1px 0 rgba(0,0,0,0.5),0 1px 10px 0 rgba(0,0,0,0.15); margin: 1px; max-width:658px; padding:0; width:99.375%; width:-webkit-calc(100% - 2px); width:calc(100% - 2px);"><div style="padding:8px;"> <div style=" background:#F8F8F8; line-height:0; margin-top:40px; padding:36.1111111111% 0; text-align:center; width:100%;"> <div style=" background:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACwAAAAsCAMAAAApWqozAAAABGdBTUEAALGPC/xhBQAAAAFzUkdCAK7OHOkAAAAMUExURczMzPf399fX1+bm5mzY9AMAAADiSURBVDjLvZXbEsMgCES5/P8/t9FuRVCRmU73JWlzosgSIIZURCjo/ad+EQJJB4Hv8BFt+IDpQoCx1wjOSBFhh2XssxEIYn3ulI/6MNReE07UIWJEv8UEOWDS88LY97kqyTliJKKtuYBbruAyVh5wOHiXmpi5we58Ek028czwyuQdLKPG1Bkb4NnM+VeAnfHqn1k4+GPT6uGQcvu2h2OVuIf/gWUFyy8OWEpdyZSa3aVCqpVoVvzZZ2VTnn2wU8qzVjDDetO90GSy9mVLqtgYSy231MxrY6I2gGqjrTY0L8fxCxfCBbhWrsYYAAAAAElFTkSuQmCC); display:block; height:44px; margin:0 auto -44px; position:relative; top:-22px; width:44px;"></div></div><p style=" color:#c9c8cd; font-family:Arial,sans-serif; font-size:14px; line-height:17px; margin-bottom:0; margin-top:8px; overflow:hidden; padding:8px 0 7px; text-align:center; text-overflow:ellipsis; white-space:nowrap;"><a href="https://www.instagram.com/p/BIJhXdhghyO/" style=" color:#c9c8cd; font-family:Arial,sans-serif; font-size:14px; font-style:normal; font-weight:normal; line-height:17px; text-decoration:none;" target="_blank">A photo posted by Zoey Corgella the corgi (@zoeydacorgi)</a> on <time style=" font-family:Arial,sans-serif; font-size:14px; line-height:17px;" datetime="2016-07-22T03:32:07+00:00">Jul 21, 2016 at 8:32pm PDT</time></p></div></blockquote> <script async defer src="//platform.instagram.com/en_US/embeds.js"></script>

Turns out `changeType` is non-`nil`, [works in a `switch` statement](https://twitter.com/jckarter/status/720413224110129152), and has [undefined behavior](https://twitter.com/jckarter/status/720413571734118400)!

To understand why this is the case<sup>1</sup>, we have to take a step back and define two terms: open and closed enumerations. `NSFetchedResultsChangeType` is an `NS_ENUM` and when bridged to Swift, it's [represented as an "open" enumeration](https://twitter.com/jckarter/status/720413830329663488). The distinction between open and closed enumerations can be found in [Jordan Rose](https://twitter.com/UINT_MIN)'s manifesto on Swift [Library Evolution](https://github.com/apple/swift/blob/26fcd8c1e2a716b1b695de39e9be470c2a1814ba/docs/LibraryEvolution.rst#enums). In summary, open enumerations will be the [_default once ABI resilience ships_](https://twitter.com/jckarter/status/720413830329663488) and __allow__ the following changes between library versions __without breaking binary compatibility__:


> - Adding a new case<sup>2</sup>
> - Reordering existing cases is a binary-compatible source-breaking change. In particular, if an enum is `RawRepresentable`, changing the raw representations of cases may break existing clients who use them for serialization
> - Adding a raw type to an enum that does not have one
> - Removing a non-public, non-versioned case<sup>3</sup>
> - Adding any other members
> - Removing any non-public, non-versioned members
> - Adding a new protocol conformance (with proper availability annotations)
> - Removing conformances to non-public protocols

On the flip side, [closed enumarations](https://github.com/apple/swift/blob/26fcd8c1e2a716b1b695de39e9be470c2a1814ba/docs/LibraryEvolution.rst#closed-enums) must be explicitly marked with the `@closed` attribute. This attribute __prevents__ the enumeration from "having any cases with less access than the enum itself<sup>3</sup> and adding new cases in the future." In addition to __guaranteeing case exhaustion for clients__, the attribute implies:

> - Adding new cases is not permitted
> - Reordering existing cases is not permitted.
> - Adding a raw type to an enum that does not have one is still permitted.
> - Removing a non-public case is not applicable.
> - Adding any other members is still permitted.
> - Removing any non-public, non-versioned members is still permitted.
> - Adding a new protocol conformance is still permitted.
> - Removing conformances to non-public protocols is still permitted.

Despite "open" being the default for enumerations moving forward, adding new cases (and consequences of this) can lead to gnarly bugs like the [one](https://forums.developer.apple.com/thread/11662) my teammate, [Paul](http://twitter.com/paulrehkugler), and I found earlier this week.

## Invalid Changes Sent to `NSFetchedResultsControllerDelegate`

[`NSFetchedResultsControllerDelegate`](https://developer.apple.com/reference/coredata/nsfetchedresultscontrollerdelegate) provides an [optional function](https://developer.apple.com/reference/coredata/nsfetchedresultscontrollerdelegate/1622296-controller) to notify concrete implementations that a fetched object has been changed due to an add, remove, move, or update. During testing, we noticed "extra" calls<sup>4</sup> to this function, which caused crashes via to subsequent invalid table view updates. To set some context, our concrete implementation had the following structure:

```swift
public func controller(_ controller: NSFetchedResultsController<NSFetchRequestResult>, 
               didChange anObject: AnyObject, 
                      at indexPath: IndexPath?, 
                     for type: NSFetchedResultsChangeType, 
            newIndexPath: IndexPath?) {

			/* Precondition checks */

			// ...

			switch type {
			case .insert:
			/* Insertion logic */
			case .delete:
			/* Deletion logic */
			case .move:
			/* Move logic */
			case .update:
			/* Update logic */
			}
		}
```

Breakpointing on calls to this function, we noticed something odd in the debug menu:

![](https://i.imgur.com/XZbBx1a.png)

Looks like Core Data is making calls to this delegate function with invalid `NSFetchedResultsChangeType`s! However, due to the nature of open enumerations, we have no way of disambiguating this from a scenario where a new case is added (i.e. `NSFetchedResultsChangeType(rawValue: x) != nil, ∀x ∈ {UInt(0), UInt(1), UInt(2), ... , UInt.max}`, due to the lack of guaranteed case exhaustion). To make this even trickier to find, the example case seemed to match against the first case, `.insert`, (as [Caleb](https://twitter.com/calebd) noted [in his testing](https://twitter.com/calebd/status/720413415894683649) as well). But, this [behavior is technically undefined](https://twitter.com/jckarter/status/720413571734118400).

## Getting Around This

To safeguard against this bug [until a `default` case is required for open enumerations](https://twitter.com/jckarter/status/723339235126849537), we came up with an __imperfect__ workaround. In the delegate function body, we added a `guard` to ensure the presence of a valid raw value on the `type` parameter:

```swift
guard [
		NSFetchedResultsChangeType.update.rawValue,
		NSFetchedResultsChangeType.delete.rawValue,
		NSFetchedResultsChangeType.insert.rawValue,
		NSFetchedResultsChangeType.move.rawValue
	].contains(type.rawValue) else {

	/* Note about the open enumeration issue we've described and logging to track this in production */
	return
}
```

This keeps our code crash-free until ABI resilience alleviates the need for the `guard`. Of course, this doesn't handle the scenario in which Apple adds a new case, but that's why we perform logging in the `else` clause to keep an eye on any API updates. You can watch [this issue](https://bugs.swift.org/browse/SR-1258) on Swift's JIRA.

Hope this helps make open and closed enumerations more clear! Huge shoutout to [Joe Groff](http://twitter.com/jckarter), [Caleb Davenport](https://twitter.com/calebd), and [Jordan Rose](https://twitter.com/UINT_MIN) for discussing this topic in the _open_<sup>1</sup> on Twitter and in the [Swift repository docs](https://github.com/apple/swift/tree/26fcd8c1e2a716b1b695de39e9be470c2a1814ba/docs).

---

<sup>1</sup>: Pun intended 😁

<sup>2</sup>: We'll soon see why this feature causes `changeType` in the quiz to be non-`nil`.

<sup>3</sup>: [Non-`public` cases don't exist at the moment](https://github.com/apple/swift/blame/26fcd8c1e2a716b1b695de39e9be470c2a1814ba/docs/LibraryEvolution.rst#L828).

<sup>4</sup>: Masked as insertions due to this [undefined behavior](https://twitter.com/calebd/status/720413415894683649).