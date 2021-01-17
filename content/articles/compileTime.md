# Xcode/Swift Compile Time

I'm following [this article](https://useyourloaf.com/blog/slow-swift-compiler-performance/).

One important information is that the building times reported can be longer than the total compilation time. This is due to paralelization of the compilation.

> Yes – many commands, especially compilation, are able to run in parallel with each other, so multicore machines will finish the build much faster than the time it took to run each of the commands. 


## Indentify some culprits

### xcodebuild flags

Run the command

```
xcodebuild -workspace ArticleUIKit.xcworkspace -scheme ArticleUIKit clean build OTHER_SWIFT_FLAGS="-Xfrontend -debug-time-function-bodies" | grep .[0-9]ms | grep -v ^0.[0-9]ms | sort -nr > culprits.txt
```

note: the command is not 100%, several duplicated lines.

### Xcode flags

Flags we can use to identify compilation bottlenecks directly in Xcode. Add the flags in the build settings for a target in the **Swift Compiler - Custom Flags** section using the **Other Swift Flags** setting::

```
    -Xfrontend -warn-long-function-bodies=<limit>
    -Xfrontend -warn-long-expression-type-checking=<limit>
```

### Xcode Build with Timming

Xcode has an option to compile with timming information.

`Product -> Perform Action -> Build with Timming Summary`

Note: The is also a flag for `xcodebuild`: `-buildWithTimingSummary`


## Fixes

Identifiying methods that take more time to compile is a first step. Next, we need to try to improve the compilation time. Not always it's easy or possible to improve those times.

It seems one of the things it takes some significat time to compile is operators overload. Things such as `x + y` takes quite a time because Swift needs to seach for declarations for all the types because the variables `x` and `y` can be `Ints`, `Doubles`, `Strings`, `Arrays`, etc.

Most all the issues are multiplication of Floats/CGFloats.



#### Resources:

https://useyourloaf.com/blog/slow-swift-compiler-performance/

https://www.avanderlee.com/optimization/analysing-build-performance-xcode/

https://irace.me/swift-profiling

https://www.cocoawithlove.com/blog/2016/07/12/type-checker-issues.html