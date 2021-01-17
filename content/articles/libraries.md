# Libraries

After all these years, I keep struggling with the differences (real or just nomenclature) on libraries.

What's the difference between a Library, a Framework, a Module and a package?

What's the difference between Static, Dynamic and Shared

Let's start with the fundamental programmatically difference, the Static vs Dynamic.

## Static, Shared or Dynamic

### Static

Static Library was the only option until iOS8. Static Library is a unit of code linked at compile time, which does not change. Static Library is generally used for simplifying the build system, where each major module is put into its own static library. This helps avoiding having to much coupling. Then all the static libraries are linked together to make the final executable program. Libraries cannot contain images/assets.

So, it is a set amount of code that is added into the build of main project. It will be part of the compiled code and will be bundled together.

### Shared

It seems it is the terms for libraries shipped within the OS.

### Dynamic

Similar with Static libraries but the code will not be included in the bundled. It will, however, available at runtime to be consumed by many consumers. This is an ideal solution for when we have multiple consumers of a Library in the same system. Avoids duplicated code. However, the runtime execution will be slower because the compiled code is not optimized.


### Who decides what

An important question is, how defines if a library is static or dynamic? Is the library itself or is in the consumer, at the momement the library is included in consumer project. Eventually we will want a library to be statically included in some projects and dynamically included in others. 

### Discussion

It is also important to discuss the settings available in Xcode, in particular the `embed` options and the build settings that says how to include the library.

If a library only has one consumer, for example, it is a library used by only one consumer app and that will never change, it feels there is not advantage in having a library to be included dynamically. It will not benefit anything in terms of size (it needs to exists at least one instance/copy of the library) neither will have a better performance.



## Resources
