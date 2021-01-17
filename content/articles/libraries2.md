# iOS
==================================

## Static Libraries vs Dynamic Frameworks

### Library:

- Static -> Packaged into our code
- Shared -> Just a reference to the library that must reside on OS

Static Library was the only option until iOS8.

Static Library is a unit of code linked at compile time, which does not change.

Static Library is generally used for simplifying the build system, where each major module is put into its own static library. This helps to avoid to much coupling. Then all the static libraries are linked together to make the final executable program. Libraries cannot contain images/assets.

With Library we need to ship the binary (.a) and headers separately.

### Frameworks

Libraries only has executable code. A framework is something more complete and powerfull. A framework is a bundle that contains shared libraries as well as sub directories of headers and other resources.

Frameworks are a unit of code and/or assets linked at runtime that may change.

### Differences

- Inversion of Control is a key part which makes a framework different from a library. When we call a method from a library we are in control, but with the framework the control is inverted, the framework calls our code. (E.g a GUI framework calls our code through the event handlers).

- A library is essentially a set of functions (well defined operations) that we can call. Each fucntion does some work and then returns the control to the client.

- A framework embodies some abstract design with more behavior built in. In order to use it, we need to insert our behavior into various places in the framework either by subclassing or by plugging in our code. The framework code then calls our code at these points.

- A framework can also be considered as a skeleton where the application defines the meat of the operation by filling out the skeleton. The skeleton still has code to link up the parts.

### Libraries or Frameworks?

Nowadays the common option is to use Frameworks. They can be a bit over engineering solution for some situations but overall, it's the best solution.

### Tutorials and samples

[Building Modern Frameworks (WWDC14)](https://developer.apple.com/videos/play/wwdc2014/416/)

[How to Create a Framework for iOS](https://www.raywenderlich.com/65964/create-a-framework-for-ios)

[Creating and Distributing iOS Frameworks](https://www.raywenderlich.com/126365/ios-frameworks-tutorial)

[Creating your first iOS Framework](https://robots.thoughtbot.com/creating-your-first-ios-framework)

[CocoaPods 0.36 - Framework and Swift Support](https://blog.cocoapods.org/CocoaPods-0.36/)

[CocoaPods and Frameworks](https://artsy.github.io/blog/2015/01/04/cocoapods-and-frameworks/)

**Resources:**

[http://www.knowstack.com/framework-vs-library-cocoa-ios/](http://www.knowstack.com/framework-vs-library-cocoa-ios/)

## XPTO

