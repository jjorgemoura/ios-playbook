# App Launch time

I'm following [this article](https://medium.com/globant/ios-app-launch-time-analysis-and-optimization-a219ee81447c).


## Pre-Main

### Frameworks

In order to improve the startup time, the time before `main` is called, we need to:

- Do less
	- Embed fewer dylibs
	- Declare fewer classes/methods
	- Use fewer initializers (not sure what this means, it is not class/struct's `init`s)
- Use more Swift (because):
	- has "no initializers"
	- size improvements 

#### Initializers

It seems initializers is a Objective-C feature where a Library/Module may define some code to be executed when the Library is setup up (linked).

Based on [this](https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/DynamicLibraryDesignGuidelines.html) document, initialisers are defined as:

```c
/* Files: File1.c, File2.c */
#include <stdio.h>
__attribute__((constructor))
static void initializer1() {
    printf("[%s] [%s]\n", __FILE__, __FUNCTION__);
}
```

It seems Swift doesn't have this functionality.

Note: On WWDC 2017 it was launched **dyld 3**.

In **dyld 3**, some features are:

- Move complex operations out of process
	- Most of dyld is now a regular daemon
- Make the rest of dyld as small as possible
	- Reduces attack surface
	- Speeds up launch
		- The fastest code is code you never write
		- Followed closely by code you almost never execute

Setup frameworks as static instead of dynamic.

Note: If the framework has Objective-C, there is an extra flag that needs to be set.

Note: From those first two WWDC talks, where the main focus is "pre-main" optimisation, there is a lot of focus on **dyld**. This is super low level, super difficult and specialised. Also, it applies mainly to Objective-C or C/C++ code. Good to know but probably it doesn't apply most of the "recent" times. Probably it applies more to libraries that are low level, things that talk with hardware, system libraries, etc.

Note: Don't remember the source (probably is the WWDC 16 video) but it seems the "before main" time should be, ideally, less than 400ms. [UPDATE] This is actually wrong. The correct is we need to "render first frame" in under 400ms.

## Post-Main

Based on this [WWWD 19](https://developer.apple.com/videos/play/wwdc2019/423/) video

**What is launch**

3 types of launch:

- Cold
	- After reboot
	- Apps is not in memory
	- No process exists

- Warm
	- Recently terminated
	- App is partially in memory
	- No process exists

- Resume
	- App is suspended
	- App is fully in memory
	- Process exists

### Launch Process

When user tap in the app icon and launch the app:

- (pre-main) iOS needs to do the necessary system-wide work. Let's think in around 100ms

- (post-main) app's setup. The remaining 300ms tops. The app should be interactive and responsive at this point, even if with some placeholders.

### Phases

- System Interface
- Runtime Init
- UIKit Init
- Application Init
- Initial Frame Render
- Extended

#### System Interface

**DYLD3** Dynamic Linker loads shared libraries and frameworks. It's now (iOS13) also available for everyone, not only system frameworks. This, basically, means the system is caching app's dependencies to warm launches. Even after an iOS update, the dependencies are processed in the background and cached. Not the dependencies themselves but the resolution of all dependencies (and their versions).

**Recommendations:**

- Avoid linking unused frameworks
- Avoid dynamic library loading during launch (such as DLOpen or NSBundleLoad)
- Hard link all your dependencies

The second part is **libSystem Init**. Initialises the interface with low level system components. This should be faced as a fixed cost. There is nothing we can do.

#### Runtime Init

Static initializer. Here is where/when the system initialises the language (Objective-C or Swift) runtime. Also, it invokes all **class static load methods**.

Usually our app should not do any work here unless there is static initializers. The recommendations are to not have any initialisation here. From a previous WWDC video, it seems this only applies to Objective-C code. Basically, avoid using `+ [Class load];`. If necessary, use `+ [Class initialize];` because it's lazy.

#### UIKit Init

Here is where the System instantiate `UIApplication` and `UIApplicationDelegate`. Begins event processing and integration with the system.

**Recommendations:**

- Minimise work in `UIApplication` subclass
- Minimise work in `UIApplicationDelegate` initialisation

#### Application Init

Here is where the developer has the main impact.

Invokes `UIApplicationDelegate` app lifecycle callbacks:

- application:willFinishLaunchingWithOptions:
- application:didFinishLaunchingWithOptions:

- applicationDidBecomeActive:

Note: Using UISceneDelegate changes the way the app is init. If using Scenes, the ViewControllers should be init in `scene:willConnectToSession:options:` and not in `application:didFinishLaunchingWithOptions:`. 

Recommendation:

Defer any work that is not necessary for first frame. Lot it later or in the background.

#### Initial Frame Render

**First Frame Render** -> Creates, performs layout for and draws views:

```
loadView
viewDidLoad
layoutSubviews
```

**Recommendations:**

- Flatten view hierarchies and lazily load views.
- Optimise Auto-layout

#### Extended

App-specific period after first frame. Not all the apps have this phase. Basically this is the lazy or asynchronous work triggered in previous phase.

This phase displays asynchronously loaded data. The app should be kept interactive and responsive.

**Note:** Leverage `os_signpost` to measure work. More on [WWDC 18](https://developer.apple.com/videos/play/wwdc2018/405/)

### How to properly measure your launch

The difficulty here is making sure we are comparing the same thing.

Remove variables such as `Networking` and `Background processes`. Might be less representative but give us consistent results to evaluate progress. 

**Test in a Clean ans Consistent Environment**

- Reboot the device. Remove any unnecessary state. After every reboot, wait 2-3 minutes to system settle down.
- Enable Airplaine mode or mock the network.
- Use unchanging or no iCloud Account (in the device?!). Note: Probably if use Airplane mode this might not be a problem.
- Use release build of the app
- Measure warm launches. More consistent.

Test with different data-sets to simulate different users.

Check this [WWDC 19](https://developer.apple.com/videos/play/wwdc2019/417/) video for measurements

### Use Instruments to profile your launch

**Tips and Tricks**

- Minimize
	- Defer work unrelated to first frame
	- Move blocking work off main thread
	- Reduce memory usage
- Prioritize
	- Identify the right QoS for the task
	- Utilize scheduler optimizations for app launch
	- Preserve the priority with the right primitives
- Optimize
	- Simplify and limit existing work
	- Optimize algorithms and data structures
	- Cache resources and computations

### Track your launch over time







#### Resources:

https://developer.apple.com/videos/play/wwdc2016/406/

https://developer.apple.com/videos/play/wwdc2017/413/

https://developer.apple.com/videos/play/wwdc2019/423/

https://developer.apple.com/videos/play/wwdc2018/405/

https://developer.apple.com/videos/play/wwdc2019/417/

https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/DynamicLibraryDesignGuidelines.html

https://medium.com/globant/ios-app-launch-time-analysis-and-optimization-a219ee81447c