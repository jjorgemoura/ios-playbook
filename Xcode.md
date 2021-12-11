# Xcode









### Xcode Project Structure

Having a well organized project structure helps understanding the project and navigating through all the files and sections of the codebase. It is, usually, a signal of the health state of the codebase.

There is no magic solution here. There are multiple options. More important than choose the best option (or discuss what is "the best" option) is, instead, choose one and stick with it, in a consistent way. The key here is consistency.

I like to organize in this way:

- application
  - applications` wide entities, such as Typography, Theming, Navigation (router), Model, Application-wide Actions and State and Environment

- environemnt
  - entities and services that are not application specific. Can also contain extensions, etc

- features
  - feature's main entities, such as Views, ViewModels, Actions, State, Environment, reducer

- resources
  - project's resources, such as `xcassets`, `storyboards`, etc.


### Version

Probaby the first setup I do on a new Xcode project is set (reset) the version to 0.0.1.

I like to manipulate the version's increments automaticately, using build tools (fastlane). Fastlane relies on an Apple tool called `agvtool`. This usually requires some setup in build settings.

By default the project is not in a good state to use the `agvtool`.

Setup needed:

- Enable agvtool
  - Set `Current Project Version` to a value of your choosing

    This is the build number. Current Project Version must be an integer or a floating point number such as 34.6. You should set this setting to 1 if you are working on a new project. This should be set att project level.

  - Set `Versioning System` to `Apple Generic`

- Set up your version and build numbers (on target's plist).
  - Set `Bundle versions string, short` to app version, format x.y.z
  - Set/Confirm `Bundle version` value, it should be the same as set before on `Current Project Version`

I'm not sure if fastlane uses `agvtool` correctly. This because looking at Apple's documentation, the `Bundle version` should also be the app's version (x.y.z), not the build number.

More info: https://developer.apple.com/library/content/qa/qa1827/_index.html

### Icon

Another step I usually do as soon as possible is setup the app icon. It is a bit of a brain blockage. Without a icon it feels a project lack some identity or soul.

### Documentation

I consider that having code documentation is a nice feature. This is particular important on libraries, in order to provide help and tips while consuming the api via code completion.

I used to configure my projects with `jazzy`. Jazzy is a really nice tool and create a good documentation. However, the installation of jazzy can be really difficult and trublesome due to it's dependencies (gems). Also, had some issues configuring the yaml file correctly in some ocasions, with some lack of documentation not helping.

More recently I've started using `sdfasdasdasd`