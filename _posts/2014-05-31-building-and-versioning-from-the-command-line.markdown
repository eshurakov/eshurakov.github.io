---
layout: post
title: "Building and versioning from the command line"
date: 2014-05-31 23:21
comments: true
published: false
categories: 
---
Whenever I had to submit the app to the app store or sent a new beta to developers I used to do it by hand. I thought that even if I do that not that often, I will benefit from an automated or even semi-automated solution.  

My build procedure usually contains of the following steps:

* Update internal build version number and inject hash of the current commit. This step is pretty important as you want to be able to easily identify installed app and get the source code for the version.  
* Build the app, run unit tests and integration tests.  
* Package and sign the app.    
* Upload dSYM file to the crash reporting tool and the app to the distribution service.  

Which translates to
```
make test-release
make build-release
make testflight
```

<!-- More -->

#### Versioning

There are two kind of version strings that are used in Xcode projects: `CFBundleShortVersionString` and `CFBundleVersion`.

<blockquote>
    <p>
        CFBundleShortVersionString specifies the release version number of the bundle, which identifies a released iteration of the application.
    </p>
    <p>
        CFBundleVersion specifies the build version number of the bundle, which identifies an iteration (released or unreleased) of the bundle. This is a monotonically increased string, comprised of one or more period-separated integers.
    </p>
    <footer>
        <cite>
            <a href="https://developer.apple.com/library/mac/#documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html">Apple Documentation</a>
        </cite>
    </footer>
</blockquote>

`CFBundleShortVersionString` is a marketing version shown on the App Store (ex.: 1.0, 1.5.1, 2.3). It is something you update by hand, bumping the number whenever it is necessary.

On the other hand, `CFBundleVersion` can be updated automatically during the build process. We all use version control systems and the best thing is that we can just reuse the version number as it is increasing monotonously, which is exactly what we need.

There are several ways to inject a build version number to the app and the most common one is to set it to the info.plist file using `PlistBuddy` tool. Instead of doing it this way I prefer to provide a file with a build number to the build system and let it set it for me. It is possible to do if you allow to preprocess info.plist file and add a dependent target, which generates a header file with a couple of variables.

#### Building and testing
I tried to use [xcodebuild][] first, but then found a nice wrapper around it, called [xctool][].

`xctool` is developed by Facebook and has a bunch of nice additions to xcodebuild tool, such as structured reporters (junit, json).

I find it very handy to store provisioning profiles along with a source code. As a part of the build script required provisioning profile is copied to the users `Library` folder.

#### Packaging
Packaging is pretty straightforward, the only thing is that you have to have a private key in your keychain and the keychain must be unlocked.

#### Distribution
This totally depends on your needs. I use [TestFlight][] and [Crashlytics][]. 

#### A Bit of Code
I ended up having three scripts:

* `Version.sh` calculates build version number 
* `Build.sh` builds, tests and packages
* `Testflight.sh` uploads signed ipa file to testflight

On top of that I created a very simple `Makefile` with all the variables.
``` bash
BUILD_SCRIPT = "Scripts/Build.sh"

WORKSPACE = "XcodeBuildScripts.xcworkspace"
IOS_SDK = "iphoneos7.1"
IOS_SIMULATOR_SDK = "iphonesimulator7.1"

ARTIFACTS_DIR = "artifacts"

build-release:
    /bin/bash ${BUILD_SCRIPT} \
        -workspace ${WORKSPACE} \
        -scheme "XcodeBuildScripts" \
        -configuration "Release" \
        -sdk ${IOS_SDK} \
        -artifacts ${ARTIFACTS_DIR} \
        -identity "iPhone Distribution: That Guy" \
        -profile "Provisioning/TestApp-Release.mobileprovision"
```

After you run `make build-release`, signed ipa file will be placed inside the `artifacts` folder.

#### Continuous Integration
All tasks from the scripts can be done using continuous integration software, but not all projects require having a separate build machine, though it's a good practice to have one. 

For my personal projects I don't use continuous integration server for a very simple reason, I don't have a spare machine to run it on.

#### Source
Source code, usage instructions and example project:  
[https://github.com/eshurakov/xcode-build-scripts](https://github.com/eshurakov/xcode-build-scripts)

[xcodebuild]: https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/xcodebuild.1.html
[xctool]: https://github.com/facebook/xctool
[TestFlight]: https://www.testflightapp.com
[Crashlytics]: https://www.crashlytics.com