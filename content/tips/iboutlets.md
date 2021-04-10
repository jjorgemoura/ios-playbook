# IBOutlets

What is the correct signature of an IBOutlet?

- weak vs strong?
- optional vs implicitly unwrapped?
- public/internal or private

This is a heated discussion and the “official” documentation from Apple is quite old. Also, from a recent WWDC video [1], the speaker recommended the contrary version of the one from the old/outdated documentation [2][3].

## weak vs strong

It seems we should use `strong`. From Apple [1]:

> And the last option I want to point out is the storage type, which can either be strong or weak. In general you should make your outlet strong, especially if you are connecting an outlet to a subview or to a constraint that's not always going to be retained by the view hierarchy. The only time you really need to make an outlet weak is if you have a custom view that references something back up the view hierarchy and in general that's not recommended.

Using `strong` is the current (confirmed by an engineer from IB team) recommended best practice unless `weak` is specifically needed to avoid a retain cycle.

Let's go with that recommendation.

## optional vs implicitly unwrapped

The decision might depend on what was decided before. If the option was for a `weak` variable, the instance needs to be an optional. 

The safest option is being an `Optional`. Most of the times is safe to use `Implicitly Unwrapped Optional`.

A important case to bare in mind is when we are using storyboards. Often with Storyboards, when we can access the ViewController during `prepareForSegue(_:sender:)`, the IBOutlets will be still nil because the view is not loaded yet. Most of the times we access the ViewController this way in order to perform some setup that set some label texts.

All the other cases, the IBOutlet should have an value. If not, probably is a bug in our code.

## public/internal or private

IBOutlets should be always `private`, unless some weird design is in place. There are some temptation to have them internal in order to test them. Personaly, I don't think we need to access directly to ui elements to check then. Also, a better way to test the UI is via snapshot tests.

## UI programatically

All the previous discussion only applies to when the UI is implemented with Nib files. In case the UI is done programmatically, typically the variables are strong and non-optional.

```swift
private var someLabel = UILabel()
```

Eventually some variables could be optional if the UI requires that.

## Conclusion

From now on, my recommendation/preference is to use:

```swift
@IBOutlet private var someLabel: UILabel?
```

However, I don't think it's wrong to use an `Implicitly Unwrapped Optional` as well:

```swift
@IBOutlet private var someLabel: UILabel!
```

#### Resources:

[1] - https://developer.apple.com/videos/play/wwdc2015/407/

[2] - https://developer.apple.com/library/archive/documentation/General/Conceptual/CocoaEncyclopedia/Outlets/Outlets.html

[3] - https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/LoadingResources/CocoaNibs/CocoaNibs.html

https://nshipster.com/ibaction-iboutlet-iboutletcollection/

https://developer.apple.com/forums/thread/51044

https://developer.apple.com/forums/thread/96763

https://stackoverflow.com/questions/7678469/should-iboutlets-be-strong-or-weak-under-arc/31395938#31395938

https://forums.raywenderlich.com/t/weak-vs-strong-iboutlets/114950/6

http://scottberrevoets.com/2016/03/21/outlets-strong-or-weak/

#### Extra

https://developer.apple.com/videos/play/wwdc2011/323/

https://www.raywenderlich.com/966538-arc-and-memory-management-in-swift
