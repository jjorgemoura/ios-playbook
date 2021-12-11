# iOS Playbook

Personal playbook for my iOS work.

## Project setup

### Github Project

I prefer to name my projects (repos) strating with lowercase. It feels better, more balanced.

I like to immediately create, during the creating of the repo, the gitignore and License files.

### Xcode

I like to use a vanilla Xcode. The main/only change I do is use my own color theming.

In order to have consistency with github repo, I also like to create my Xcode projects using a lowercase name. This creates some issues internally, where Xcode immediately create some classes using the project nome, generating some lint errors for those lowercase classes.

### Swiftlint

This is a mandatory step. No questions asked. I tend to enable the max rules as possible. This means all optional rules are added to config file but the ones that I don't want active remain commented.

I have been using swiftlint integrated with Xcode, as a build phase set. However, from now on, will try a different approach. Will run swiftlint only on CI. Not only this will speed up the build (even if only slightly), but also will give more relevance to CI.

### Swiftformat

Idea it to balance Swiftformat with Swiftlint. Swiftlint will focus on linting and Swiftformat will focus more on format. If or when both tools overlap, this will be the dicision factor.

### CI/CD



#### Fastlane

Over the time, I've prepared a complex fastfile. However, it is still a work in progress, mainly the `release` lane.

The fasffile has 4 lanes:

- Run all Tests
- Set next version token
- Run master build
- Do the release

#### Github Actions

This is still Work in Progress. I've worked on 3 main actions but they rely on fastlane. For github actions to run fastlane needs, first, to install it. That takes a significant amount of time. Thus, for now, I have decided to comment the actions.
