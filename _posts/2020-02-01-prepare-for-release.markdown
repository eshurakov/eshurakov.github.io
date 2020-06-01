---
layout: post
title: "Prepare for release"
date: 2020-02-01 12:00
comments: true
published: true
categories: 
---

There is more to the process of releasing an app than archiving a project using release build configuration and uploading to the store. As the app grows you want to be sure about the quality.

In the mobile development world delivering the app to the customer takes time. Quite some time :-) Some years ago it was multiple days or even weeks of waiting time, now it's a bit better and usually takes one or two days (thanks Apple!). Still, if you have a critical bug in the app that you want to fix, one or two days is a lot of time.


I collected several ways to help to ensure the quality of the app.

<!-- More -->

**Code freeze**
After code freeze and before release, modifications to the code are discouraged and normally only bugfixes would be applied. Unfinished features normally not included and stay either in their branches or hidden from the user with feature toggles.

**Internal build (Private Beta)**
Private Beta is a version of the app for internal testing. If the app uses a backend, it usually connects to the beta version of the backend.
To distinguish between different versions of the app, it is a good idea to use a different bundle identifier, app name, and icon.

**Unit, UI and Integration Tests**
Tests are nice to have. They can help to save time by automating common usage scenarios or finding regression of older bugs.

**Bug bash**
During bug bash, a group of people tests features and bugfixes that will be released. Participants don't need to be technical, but they do need a good knowledge of the app. It's good to test on different phone models and OS versions.

**Check localization**
Localization can be checked during bug bash or as a separate event.
Automate what can be automated. For example, check that all strings are translated while making an internal build.

**External build (Public Beta)**
Public Beta is a version of the app that connects to live BE and is as close to the live app as possible. Depending on the length of the release cycle, the app can stay in public beta for multiple weeks.

**Collecting feedback, checking crash reporting tool**
Feedback from users is a good way to check if things are working as expected.
Keep an eye on incoming crash reports. Is there something suspicious, are there new crashes? Depending on the severity you can decide to fix it now and send a new beta build or fix it in the next version.

**Incentives to use a Public Beta**
Using Public Beta app is beneficial to you, but why would users want to be a guinea pig?
* early access to new features. This one is easy, no additional work is required :-)
* priority support. All questions and messages from beta users can be prioritized and answered first 
* benefits, that only exist in Public Beta version. For example, if the app works via subscription, users could get some small discount as long as they're using the beta app and keep it up to date.

**Staged rollout**
Unfortunately, the staged rollout for iOS devices is very primitive in functionality, but still may be useful to limit the number of users updating to the latest version at the same time.