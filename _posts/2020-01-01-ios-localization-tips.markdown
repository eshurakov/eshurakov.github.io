---
layout: post
title: "Localization tips for iOS"
date: 2020-01-01 12:00
comments: true
published: true
categories: 
---

**Start from day one**

Think about localization from day one, even if the app supports only one language. It doesn't take much time but makes your life easier in the future.

**Use identifiers as keys**

* Use identifiers that reflect the function of the element as well as its location in the app. 
* Use the same identifier prefix for keys that belong to the same functional unit/page.

**Write good comments**

Give the key a comment, saying where it is located on the screen, what kind of element it is, and what is the function. Describe all parameters that are used in this string. Use business terms, that is used in the app and not developers slang, cause most likely it won't be localized by developers :-)

**Always use numbered format specifiers**

When using format specifiers, use numbered ones so that translators can shuffle them around.

**Example**

```
/* Action button to go to the next screen after user picks a date / Select countdown date screen / Create new event flow */
"create_event_select_countdown_date_action_button" = "Go";

/* Interval description / Select countdown date screen / Create new event flow. Parameters: %1$@ - formatted number of days (1 day, 5 days) */
"create_event_select_countdown_date_interval_description" = "Start %1$@ countdown";

/* Screen title / Select countdown date screen / Create new event flow */
"create_event_select_countdown_date_title" = "Select countdown date";
```

**Skip interface builder**

I find it very hard to localize interface builder files and storyboards.
I tried two options:

* one interface file per language for the same screen is **a lot** of work to maintain and support
* using Xcode string export/import results in keys that don't make sense and it's not possible to add a comment

My solution is to create UI elements in code or connect them from the interface file to the code and do all localization from the code.

**Provide screenshots**

Screenshots are a great way to understand the context in which strings are used. Understanding context is important to get quality localization.
Many of localization services (OneSky, Lokalise) provide a library that can take a screenshot and mark all keys it sees on it.

**Audit language files**

When making a release build, add a step to check that everything is localized. This can be done by going through language files and comparing key and its value. If they are the same, it means the localization is missing.
