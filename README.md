# iOS Development Guide

This development guide outlines the standards and the best practices that are adopted by the iOS team at 2359 Media.

This is the first draft of the guide. We are eager for the feedback from our developers. Please feel free to create [issues](https://github.com/2359media/ios-dev-guide/issues) for any suggestions.

## Table of Contents

* [Why This Is Required](why-this-is-required)
* [Objective-C Coding Style](#objective-c-coding-style)
* [Project Technical Conventions](project-technical-conventions)
    * [Xcode](#xcode)
	* [CocoaPods](#cocoapods)
* [Libraries](#libraries)
    * [Networking](#networking)
    * [JSON Parsing](#json-parsing)
    * [Urban Airship (Device Registration)](#urban-airship-device-registration)
    * [HUD](#hud)
    * [Reachability](#reachability)
    * [Pull-to-Refresh](#pull-to-refresh)
    * [Social Network](#social-network)
    * [UICollectionView on iOS 5.x](#uicollectionview-on-ios-5x)
    * [Attributed Label](#attributed-label)
    * [Sidebar Menu (Hamburger UI)](#sidebar-menu-hamburger-ui)
    * [UIView (Nib Loading and Geometry Shortcuts)](#uiview-nib-loading-and-geometry-shortcuts)
    * [Page Control](#page-control)
    * [UIAlertView and UIActionSheet (Block-based wrappers)](#uialertview-and-uiactionsheet-block-based-wrappers)

## Why This Is Required

This guide should be considered as rules for first timers, but guidelines for experienced developers.

## Objective-C Coding Conventions

Please refer to [2359media/objective-c-conventions](https://github.com/2359media/objective-c-conventions)

## Project Technical Conventions

### Xcode

Always use the most recent release version of Xcode. At the time of writing, the lastest version is Xcode 5. You can download it from [Mac App Store](https://itunes.apple.com/en/app/xcode/id497799835?mt=12), or at [Apple developer center](https://developer.apple.com/downloads). 

However, if a project still requires a base SDK of an old version of iOS, it'd handy to keep the lastest version of Xcode that supports the SDK. For example, during the transition from iOS 6 to iOS 7, some legacy projects might not be ready to upgrade to iOS 7 SDK, because it requires a lot of effort to do so. In that case, we still need to keep Xcode 4.6.3 (and iOS 6 SDK) for these projects.

* Add old SDKs in new Xcode

	Each version of Xcode comes with only one SDK. For example, Xcode 5 comes with iOS 7.0 SDK while Xcode 4.6 comes with iOS 6.1 SDK. However, it's possible to add iOS 6.1 SDK from Xcode 4.6 to Xcode 5. To do so, you simply copy the iOS 6.1 SDK folder inside Xcode 4.6 app bundle to the same position in Xcode 5 app bundle. Assuming you rename Xcode 4.6 app bundle to Xcode4.app, this is the command to do that:
		
		cp /Applications/Xcode4.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS6.1.sdk /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs
	
	If you prefer symbolic link instead of copying the entire folder, use this command:
	
	 	ln -s /Applications/Xcode4.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS6.1.sdk /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs

### CocoaPods

Use [CocoaPods](http://cocoapods.org) to manage libraries. We don't recommend to copy library source files into the project. There is a repository of CocoaPods specifications (`podspec`): [CocoaPods/Specs](https://github.com/CocoaPods/Specs), which contains most of the common libraries. To add a library dependency, simply specify the name and the version.

* Library with `podspec` in [CocoaPods/Specs](https://github.com/CocoaPods/Specs)

		pod 'AFNetworking', '2.0.0-RC3'

You can also specify a libaray depedency that is outside of CocosPods/Sepcs.

* Library with `podspec` in the root of the repository. Either local repositories or remote repositories.

		pod 'AFNetworking', :path => '~/Documents/AFNetworking'
		pod 'AFNetworking', :git => 'https://github.com/AFNetworking/AFNetworking.git'

If a library doesn't have a `podspec`, you can create a `podspec` on your own and host it in somewhere like [gist.github.com](), or even in a local folder.

* Library without `podspec`

		pod 'JSONKit', :podspec => 'https://raw.github.com/gist/1346394/1d26570f68ca27377a27430c65841a0880395d72/JSONKit.podspec'
		pod 'JSONKit', :path => '~/Documents/JSONKit.podspec'

### Graphic Assets

* Must include both @1x and @2x sizes.
* Use JPEG format for large images, eg. background images.

## Git

We use Git as our source code management (SCM) system. We host
repositories in GitHub. Here are the best practices of using
Git and GitHub.

### Commit Messages

Please write good commit messages. Here's a model Git commit message:

    Capitalized, short (50 chars or less) summary

    More detailed explanatory text, if necessary.  Wrap it to about 72
    characters or so.  In some contexts, the first line is treated as the
    subject of an email and the rest of the text as the body.  The blank
    line separating the summary from the body is critical (unless you omit
    the body entirely); tools like rebase can get confused if you run the
    two together.

    Write your commit message in the imperative: "Fix bug" and not "Fixed bug"
    or "Fixes bug."  This convention matches up with commit messages generated
    by commands like git merge and git revert.

    Further paragraphs come after blank lines.

    - Bullet points are okay, too

    - Typically a hyphen or asterisk is used for the bullet, preceded by a
      single space, with blank lines in between, but conventions vary here

    - Use a hanging indent

Read [Tim Pope](http://tpo.pe/)'s post [*A Note About Git Commit
Messages*](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)
for more discussions.

### Pivotal Tracker Post-Commit Hooks

If commits are related to some Pivotal Tracker stories, use Pivotal
Tracker post-commit hooks to link the commits to the particular stories.
To do that, firstly, the GitHub repository of the project must enable Pivotal
Tracker Service Hook in the settings. Secondly, add the story ID in the
commit message using the format: '[#12345678]'. 

To automatically finish a story by using a commit message, include
"fixed", "completed", or "finished" in the square brackets in addition
to the story ID. You may use different cases or forms of these verbs,
such as "Fix" or "FIXES", and they may appear before or after the story
ID. Note: For features, use of one of these keywords will put the story
in the finished state. For chores, it will put the story in the accepted
state.

Here're some typical commit messages that include Pivotal Tracker
post-commit hooks:

* [#53928321] Create editorial page
* [Fix #55789490] "$" sign appears in a wrong position
* [Finish #53870315] Update icons for review buttons

## Recommended Third-party Libraries

Here are a list of libraries that are widely used in our projects.
Developers are recommended to choose the listed library for that
particular feature. If, however, developers want to use a different
library or experiment a new library. Please [create a new
issue](https://github.com/2359media/ios-dev-guide/issues/new) to argue
that why an alternate library is better. 

### Networking

[AFNetworking](https://github.com/AFNetworking/AFNetworking) by Mattt Thompson

> A delightful iOS and OS X networking framework.
> http://afnetworking.com

AFNetworking 2.0 officially supports iOS 6+, Mac OS X 10.8+, and Xcode 5. If you'd like to use AFNetworking in a project targeting a base SDK of iOS 5, or Mac OS X 10.7, use the latest tagged 1.x release. For iOS 4.3 and Mac OS X 10.6 support, use the latest tagged 0.10.x release.

### JSON Parsing

1. [NSJSONSerialization](https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSJSONSerialization_Class/Reference/Reference.html) (iOS 5+)

2. [JSONKit](https://github.com/johnezang/JSONKit) by John Engelhart

> A Very High Performance Objective-C JSON Library

### Urban Airship (Device Registration)

[AFUrbanAirshipClient](http://github.com/AFNetworking/AFUrbanAirshipClient) by Mattt Thompson

> An API Client for Registering and Unregistering Devices with Urban Airship.

### HUD

[SVProgressHUD](https://github.com/samvermette/SVProgressHUD) by Sam Vermette

> A clean and lightweight progress HUD for your iOS app.  
> <http://samvermette.com/199>

### Reachability

[Reachability](https://github.com/tonymillion/Reachability) by Tony Million

> ARC and GCD Compatible Reachability Class for iOS and MacOS. Drop in replacement for Apple Reachability.

### Pull-to-Refresh

1. [UIRefreshControl](https://developer.apple.com/library/ios/documentation/uikit/reference/UIRefreshControl_class/Reference/Reference.html) (iOS 6+)

2. [SVPullToRefresh](https://github.com/samvermette/SVPullToRefresh) by Sam Vermette

> Give pull-to-refresh & infinite scrolling to any UIScrollView with 1 line of code.  
> <http://samvermette.com/314>

### Social Network

1. [UIActivityViewController](https://developer.apple.com/library/ios/documentation/uikit/reference/UIActivityViewController_Class/Reference/Reference.html)

	Use system sharing sheet, so no custom UI is allowed. Use it whenever possible.

2. [Social.framework](https://developer.apple.com/library/ios/documentation/Social/Reference/Social_Framework/_index.html) (iOS 6+)

3. [Twitter.framework](https://developer.apple.com/library/ios/documentation/Twitter/Reference/TwitterFrameworkReference/_index.html) (Twitter only and iOS 5 only, deprecated in iOS 6)

4. [Facebook SDK for iOS](https://developers.facebook.com/docs/ios/) by Facebook

	Use it for everything related to Facebook, including Facebook login, Facebook sharing, user profile, friend list, etc.

### UICollectionView on iOS 5.x

[PSTCollectionView](https://github.com/steipete/PSTCollectionView) by Peter Steinberger

> Open Source, 100% API compatible replacement of UICollectionView for iOS4.3+

### Attributed Label

[TTTAttributedLabel](https://github.com/mattt/TTTAttributedLabel) by Mattt Thompson

> A drop-in replacement for UILabel that supports attributes, data detectors, links, and more

### Sidebar Menu (Hamburger UI)

[MMDrawerController](https://github.com/mutualmobile/MMDrawerController) by Mutual Mobile

> A lightweight, easy to use, Side Drawer Navigation Controller

### UIView (Nib loading and Geometry shortcuts)

[ViewUtils](https://github.com/nicklockwood/ViewUtils) by Nick Lockwood

> ViewUtils is a collection of category methods designed that extend UIView with all the handy little properties and functionality that you always wished were built-in to begin with.

### Page Control

[DDPageControl](https://github.com/ddeville/DDPageControl) by Damien DeVille

> An easily customizable alternative to UIKit's UIPageControl

### UIAlertView and UIActionSheet (Block-based wrappers)

[PSAlertView](https://github.com/steipete/PSAlertView) by Peter Steinberger

> Modern block-based wrappers for UIAlertView and UIActionSheet.


