# iOS Playbook

Personal playbook for my iOS work.

## Project setup

### Github Project

Like to name my projects (repos) strating with lowercase. It feels better, more balanced.

I like to create, during the creating of the repo, the gitignore and License files.

### Xcode

I like to use a vanilla Xcode. The main/only change I do is use my own color theming.

In order to have consistency with github repo, I also like to create my Xcode projects using a lowercase name. This creates some issues internally, where Xcode immediately create some classes using the project nome, generating some lint errors for those lowercase classes.

### Version

Probaby the first setup I do on a new Xcode project is set (reset) the version to 0.0.1.

I like to manipulate the version's increments automaticately, using build tools (fastlane). Fastlane relies on an Apple tool called `agvtool`. This usually requires some setup in build settings.

### Swiftlint

This is a mandatory step. No questions asked. I tend to enable the max rules as possible.

### Swiftformat

TBC

### Icon

Another step I usually do as soon as possible is setup the app icon. It is a bit of a brain blockage. Without a icon it feels a project lack some identity or soul.

### Documentation

I consider that having code documentation is a nice feature. This is particular important on libraries, in order to provide help and tips while consuming the api via code completion.

I used to configure my projects with `jazzy`. Jazzy is a really nice tool and create a good documentation. However, the installation of jazzy can be really difficult and trublesome due to it's dependencies (gems). Also, had some issues configuring the yaml file correctly in some ocasions, with some lack of documentation not helping.

More recently I've started using `sdfasdasdasd`

### Fastlane

Over the time, I've prepared a complex fastfile. However, it is still a work in progress, mainly the `release` lane.

The fasffile has 4 lanes:

- Run all Tests
- Set next version token
- Run master build
- Do the release

### Github Actions

This is still Work in Progress. I've worked on 3 main actions but they rely on fastlane. For github actions to run fastlane needs, first, to install it. That takes a significant amount of time. Thus, for now, I have decided to comment the actions.
