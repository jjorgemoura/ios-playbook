# Libraries and Frameworks

After all these years, I keep struggling with the differences (real or just nomenclature) on libraries.

What's the difference between a Library, a Framework, a Module and a package?

What's the difference between Static, Dynamic and Shared?


## The diffent flavours of modularization

A library is a piece of code that doesn't belong to our main code. It's something that is external to our project and that we need, in order to use it, to reference (or linking).

### Module

Is the smallest piece of software. A module is a set of methods or functions ready to be used somewhere else. 

In the iOS world, a module is a "*target*", i.e. something that is declared as a library/framework or an app (or a test target). When we need to use the "import" keyword, that means we are importing a module. Can be the main app/target or a library/framework.

A module can be also used as a *generic* name for a library or a framework.

From Apple's SPM page:

> Swift organizes code into modules. Each module specifies a namespace and enforces access controls on which parts of that code can be used outside of the module.

> A program may have all of its code in a single module, or it may import other modules as dependencies. Aside from the handful of system-provided modules, such as Darwin on macOS or Glibc on Linux, most dependencies require code to be downloaded and built in order to be used.

> When you use a separate module for code that solves a particular problem, that code can be reused in other situations. For example, a module that provides functionality for making network requests can be shared between a photo sharing app and a weather app. Using modules lets you build on top of other developers’ code rather than reimplementing the same functionality yourself.


### Package

Is a collection of modules. A package gathers a number of modules holding in general the same functional purpose. It is easier to include all the related modules at once.

In the iOS world, a package is quite related with [Swift Package Manager](https://swift.org/package-manager/). Similar with modules, it feels a package can also be a "generic" term for a library or a framework.

In [Swift Package Manager](https://swift.org/package-manager/) page, we can read:

>  A package consists of Swift source files and a manifest file. The manifest file, called Package.swift, defines the package’s name and its contents using the PackageDescription module.

> A package has one or more targets. Each target specifies a product and may declare one or more dependencies.

So, yes, the package can hold multiple modules (targets)

### Library

Both frameworks and libraries are code written by someone else that is used to help solve common problems.

There isn’t anything magic about frameworks or libraries. Both libraries and frameworks are reusable code written by someone else. Their purpose is to help us solving common problems in easier ways.

A library is just a collection of related functionality. Nothing more, but also nothing less. The defining characteristic of a library is that you are in control, you call the library.

The process of merging external libraries with app’s source code files is known as linking. 

### Framework

Quite similar with a Library. However, the defining characteristic of a framework is Inversion of Control. The framework calls you, not the other way round. (This is known as the Hollywood Principle: "Don't call us, we'll call you."). The framework is in control. The flow of control and the flow of data is managed by the framework.

Framework is a package that can contain resources such as dynamic libraries, strings, headers, images, storyboards etc. With small changes to its structure, it can even contain other frameworks. Such aggregate is known as umbrella framework.

Frameworks are also bundles ending with .framework extension. They can be accessed by NSBundle / Bundle class from code and, unlike most bundle files, can be browsed in the file system that makes it easier for developers to inspect its contents. Frameworks have versioned bundle format which allows to store multiple copies of code and headers to support older program version

### Library vs Framework

When you use a library, we are in charge of the flow of the application. We are choosing when and where to call the library. When we use a framework, the framework is in charge of the flow. It provides some places for us to plug in our code, but it calls the code we plugged in as needed.

In both cases, there is an application, and this application has holes in it, where code has been left out, and these holes need to be filled in. The difference between a library and a framework is:

- who writes the application,
- what are the holes and
- who fills in the holes.

With a library, you write the application, and you leave out the boring details, which gets filled in by a library.
With a framework, the framework writer writes the application, and leaves out the interesting details, which you fill in.
This can be a little confusing at times, because the framework itself might also contain boring details, that the framework author filled in with libraries, the parts that you write might contain boring details, that you fill in with libraries, and the framework might provide a set of bundled libraries that either work well with the framework or that are often needed in conjunction with the framework. For example, you might use an XML generator library when writing a web application using a web framework, and that XML library might have been provided by the framework or even be an integral part of it.

That doesn't mean, however, that there is no distinction between a library and a framework. The distinction is very clear: Inversion of Control is what it's all about.

So, let's recap:

- library: collection of related functionality
- framework: Inversion of Control

Can I create a Library and have it behave as framework or vice-versa?

Probably we can have a basic framework that could have been just a library. Not the other way around. A Library can only contain code, functions. On the other hand, a framework is basically another program inside the main program. Can have resources. It lives inside the main app.

A Library doesn't have version, bundle ID, etc.

## Static vs Dynamic

Let's start with the fundamental programmatically difference, the Static vs Dynamic.

If the linking is done at compile time, with the external code being included in our build, this means we are statically referencing a library. Otherwise, if we don't have the code in our build, this means the linking is done at runtime, dynamically.

Libraries fall into two categories based on how they are linked to the app:

- Static libraries — .a
- Dynamic libraries — .dylib

### Static Library

A collection or archive of object files. Static linker collects app compiled source code with library code into a single executable file, which loads into memory in its entirety at runtime.

Static libraries are ending with .a suffix and are created with an archiver tool. If it sounds very similar to a ZIP archive, then it’s exactly what it is.

Since static library, basically, is a sequence of statements or instructions in a machine code language (Object files have Mach-O format), this means some limitations to create and distribute them:

- needs to build a library for the same processor architecture as the client code. When working on a library for the iOS application, it will be needed to create a library for iOS Simulator and iOS devices.

- The library can’t include resource files: images, assets, nibs, strings file and other visual data. Usually a potential solution to this problem group all related external resources provided within another independent bundle.

#### Pros and cons

✓ Pros:

- Static libraries are guaranteed to be present in the app and have correct version.
- No need to keep an app up to date with library updates.
- Better performance of library calls.

✕ Cons:

- Inflated app size.
- Launch time degrades because of bloated app executable.
- Must copy whole library even if using single function.

### Dynamic Library

Dynamic libraries, as opposed to the static ones, rather than being copied into single monolithic executable, are loaded into memory when they are actually needed. This could happen either at load time or at runtime.

Dynamic libraries are usually shared between applications, therefore the system needs to store only one copy of the library and let different processes access it. As a result, invoking code and data from dynamic libraries happens slower than from the static ones.

All iOS and macOS system libraries are dynamic. Hence our apps will benefit from the future improvements that Apple makes to standard library frameworks without creating and shipping new builds.

#### Pros and cons

✓ Pros:

- Can benefit from library improvements without app re-compile. Especially useful with system libraries.
- Takes less disk space, since it is shared between applications.
- Faster startup time, as it is loaded on-demand during runtime.
- Loaded by pieces: no need to load whole library if using single function.

✕ Cons:

- Can potentially break the program if anything changes in the library.
- Slower calls to library functions, as it is located outside application executable.

### Frameworks

Basically the same applies to frameworks as is with Libraries. They can be static or dynamic.

### Who decides what

An important question is, how defines if a library is static or dynamic? Is the library itself or is in the consumer, at the moment the library is included in consumer project? 

Eventually we will want a library to be statically included in some projects and dynamically included in others.

It seems the consumer apps have the tools and the power to decide to include a library/framework statically or dynamically.

### Discussion

This is not a simple question. After reading and watching tons of information, it's still unclear what the difference between a library and framework (in particular since SPM hasintroduced (in 2020) the possibility of a library to have resources [1]). 

Note [1]: I wonder if the SPM's resources support is not, behind the scenes, keeping the vanilla library and just automate the generation of a bundle to include all the resources.

It is also important to discuss the settings available in Xcode, in particular the `embed` options and the build settings that says how to include the library.

## My Tips

Againg, there is still multiple unkwonws to me regrading the best pratices or procedures regarding the use of libraries vs frameworks, static vs dynamic binding.

Without a doubt, SMP is a really tool when compared with Cocoapods. Thus, we should prioritize SPM over Cocoapods. Unfortunatelly, not all dependencies are SPM ready.

**Types:**

**Rule #1** - For internal modules, start with a SPM.

**Rule #2** - For external modules, use SPM if available.

**Rule #3** - For external modules, if SPM is not available, consider Carthage to manage the dependency (this requires some manual setup)

**Rule #4** - If, by any change, SPM doesn't fullfil the needs for internal modules, switch for a framework (but manage it locally and manually).

**Golden Rule** - Avoid Cocoapods at all costs.

**Binding** 

**Rule #1** - If the module is fundaitional, use static. It takes more time to load but that time will be recover because that module is needed from step 1.

**Rule #2** - If the module is only use in a specific part of the app that doesn't belong to typically flow, use dynamic. It takes less time to load app. Only when the user will use that resource (if that happens), the system will load the dynamic framework.

## Other stuff

Swift Package Manager only manages open source libraries. In order to be able to manage bynary frameworks, Apple has introduced **XCFramework**. Binary in this context means compiled/closed source code.

A **XCFramework** wrappes an binary module.

**Xcode 11**

- Swift packages -> To distribute libraries as open source code
- XCFrameworks -> To distribute closed source code binary frameworks and libraries

**Xcode 12**

- Add support (bring the advantages) on Swift Packages for the distribution of closed source, with support for binary dependencies.

## Resources

- https://medium.com/@zippicoder/libraries-frameworks-swift-packages-whats-the-difference-764f371444cd

Anurag Ajwani

- https://medium.com/onfido-tech/reusing-code-with-swift-frameworks-cf60f5fa288a
- https://medium.com/onfido-tech/distributing-swift-frameworks-via-cocoapods-152002b41783
- https://medium.com/onfido-tech/distributing-compiled-swift-frameworks-via-cocoapods-8cb67a584d57
- https://medium.com/onfido-tech/reusing-code-and-resources-with-swift-static-libraries-and-resource-bundles-d070e82d3b3d
- https://medium.com/onfido-tech/distributing-compiled-ios-swift-static-libraries-and-swift-static-frameworks-7fecc4f3d182
- https://medium.com/better-programming/distributing-ios-swift-libraries-with-swift-package-manager-3c9630149bb3
- https://medium.com/@anuragajwani/how-to-build-universal-ios-frameworks-74b6b07bf31d
- https://medium.com/@anuragajwani/how-to-build-universal-ios-frameworks-using-xcframeworks-4c2790cfa623
- https://medium.com/@anuragajwani/how-to-distribute-compiled-static-frameworks-via-cocoapods-817a4c57cb10
- https://medium.com/@anuragajwani/how-to-distribute-compiled-universal-ios-xcframeworks-using-swift-package-manager-8eaf8395985f
- https://medium.com/@anuragajwani/how-to-build-universal-ios-static-libraries-using-xcframework-a3f70f998c38
- https://medium.com/@anuragajwani/distributing-universal-ios-frameworks-as-xcframeworks-using-cocoapods-699c70a5c961
- https://medium.com/@anuragajwani/distributing-compiled-universal-ios-static-libraries-as-xcframeworks-via-cocoapods-9d5c0f8b6a21
- https://medium.com/@anuragajwani/modular-ios-guide-60810f5a7f97

Apple

- https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/OverviewOfDynamicLibraries.html

- https://developer.apple.com/library/archive/technotes/tn2435/_index.html#//apple_ref/doc/uid/DTS40017543-CH1-PROJ_CONFIG-APPS_WITH_DEPENDENCIES_BETWEEN_FRAMEWORKS

- https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPFrameworks/Tasks/CreatingFrameworks.html#//apple_ref/doc/uid/20002258-BAJDHDAF

- https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW1

- https://developer.apple.com/documentation/xcode/creating_a_standalone_swift_package_with_xcode

More

- https://pewpewthespells.com/blog/static_and_dynamic_libraries.html

- https://medium.com/ios-expert-series-or-interview-series/ios-app-launch-static-binding-vs-dynamic-binding-linking-vs-embedded-e69ea9f03f72

- https://stackoverflow.com/questions/15331056/library-static-dynamic-or-framework-project-inside-another-project

Others

- https://medium.com/flawless-app-stories/improve-your-ios-teams-productivity-by-building-features-as-frameworks-9d2a64cbcab5
- https://tech.just-eat.com/2019/12/18/modular-ios-architecture-just-eat/
- https://ricardojpsantos.medium.com/building-and-end-to-end-encryption-framework-in-swift-cff7c8909130
- https://stackoverflow.com/questions/4099975/difference-between-a-module-library-and-a-framework
- https://www.freecodecamp.org/news/the-difference-between-a-framework-and-a-library-bd133054023f/
- https://medium.com/ieee-ensias-student-branch/framework-vs-library-vs-package-vs-module-the-debate-e1013a3e114d
- https://medium.com/joshtastic-blog/frameworks-and-libraries-in-swift-2359e4274faa
- https://engineering.zalando.com/posts/2017/02/how-the-zalando-ios-app-abandoned-cocoapods-and-reduced-build-time.html
- https://medium.com/@acecilia/static-vs-dynamic-frameworks-in-swift-an-in-depth-analysis-ff61a77eec65
- https://www.vadimbulavin.com/static-dynamic-frameworks-and-libraries/
- https://stackoverflow.com/questions/27899799/ios-static-vs-dynamic-frameworks-clarifications
