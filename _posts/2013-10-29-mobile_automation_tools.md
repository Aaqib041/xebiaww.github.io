---
layout: post
header-img: img/default-blog-pic.jpg
author: usinha
description: 
post_id: 17539
created: 2013/10/29 16:09:59
created_gmt: 2013/10/29 11:09:59
comment_status: open
---

# Mobile Application Automated Testing - Tools

While speaking at a conference last month, I was asked to mention some of the automated testing tools available for Mobile Application Automated Testing and to specify about there features and I responded by 'please follow my blog I will write specifically on some of the existing tools and their behaviour'.

Hence the objective of the following post is to respond to the query and also make a list of available tools which allows us to automate the test cases for Mobile Applications. The Post would cover brief introduction to the tools, Explain the architecture of the framework and specify platforms covered by each of them.

**Tool 1 - [Appium][1]:** Appium is an open source test automation framework for use with native and [hybrid][2] mobile apps. It drives iOS and Android apps using the WebDriver [JSON wire protocol][3].

**How Appium Works:**

Appium drives various native automation frameworks and provides an API based on Selenium's [WebDriver JSON wire protocol][4].

Appium drives Apple's UIAutomation library for iOS support, which is based on [Dan Cuellar's][5] work on iOS Auto.

Android support uses the UiAutomator framework for newer platforms and [Selendroid][6] for older Android platforms.

FirefoxOS support leverages [Marionette][7], an automation driver that is compatible with WebDriver and is used to automate Gecko-based platforms.

Hence it supports following **Platforms - **iOS, Android and [FirefoxOS][8].

**Features: **

  1. We don't have to recompile our app or modify it in any way, due to use of standard automation APIs on all platforms.
  2. We can write tests with our favorite dev tools using any [WebDriver][4]-compatible language such as Java, [Objective-C][9], JavaScript with Node.js (in both [callback][10] and [yield-based][11] flavours), PHP, Python, [Ruby][12], C#, Clojure, or Perl with the Selenium WebDriver API and language-specific client libraries.
  3. We can use any testing framework.
Investing in [WebDriver][4] means we are betting on a single, free and open protocol for testing that has become a de-facto standard. If we use Apple's UIAutomation library without Appium we can only write tests using JavaScript and we can only run tests through the Instruments application. Similarly, with Google's UiAutomator we can only write tests in Java. **Appium opens up the possibility of true cross-platform native mobile automation. Finally!**

Refer to the [link][13] for Quick Start Guide. Appium is being used by the popular test lab Sauce Labs so it looks like it would be more active and get more support down the line. <https://saucelabs.com/mobile>

**Tool 2 [Calabash][14]: **Calabash is an automated testing technology for Android and iOS native and hybrid applications. This repository contains support for iOS, for Android, see [Calabash Landing Page][15]. It consists of two libraries [calabash-android][16] and [calabash-ios][17] and are the testing library for Android and iOS.

**How Calabash Works:**

Calabash-iOS and Calabash-Android are the underlying low-level libraries that empower the [Cucumber][18] tool to run automated functional tests on Android and iOS phones and tablets as well as on simulators. These low-level libraries enable QA, business staff and developers to work at a high level by writing tests in a domain specific language using the terms and concepts of their business domain.****

Thes libraries enable test-code to programmatically interact with native and hybrid apps. The interaction consists of a number of end-user actions. Each action can be one of

**Gestures**
    Touches or gestures (e.g., tap, swipe and rotate).
**Assertions**
    For example: there should be a "Login" button or the web view should contain an "<h1>" element with the text "Hello".
**Screenshots**
    Screendump the current view on the current device model.
    
    **_[Calabash iOS_**][19] consists of two parts: a client library written in Ruby, and `calabash.framework`, a server framework written in Objective-C (a Clojure/JVM version of the client is coming too). To use calabash we make a special test target in XCode that links with `calabash.framework`. The application is otherwise unchanged. The server framework will start an HTTP server inside your app that listens for requests from the client library.**_[Calabash Android_][16]** is slightly different even though the idea is much the same. You can read about the architecture of Calabash Android [here][20]. The greatest benefit of the different architecture is that you can test your Android app without making any changes to the app._Testing Hybrid Apps_If you are using Appcelerator, PhoneGap, Sencha Touch or any of the other frameworks for creating _hybrid apps_. You should really take a look at Calabash. By using the same Cucumber interface as for a native app you can test HTML 5 part of our application. Testing webviews has not been posible with the currently available test frameworks. But with Calabash we can! Using Calabash you can run your tests on iOS and Android Devices and Simulator. Calabash is a free open source project, developed and maintained by [Xamarin][21]. 

**Tool 3 [Frank][22]: **Frank allows us to write structured text test/acceptance tests/requirements (using[Cucumber][18]) and have them execute against your iOS application.

Frank is the library closest to Calabash, and Calabash is highly inspired by it.

First, Frank is licensed using GNU GPLv3 and we wanted to use the less restrictive EPL. Pete actually didn’t want to license Frank as GPL, but since Frank is based on UISpec which is GPL'ed, he had to. We did not want the [UISpec][23] project as a dependency.

Second, is a bit of a pain to set up. It required modification of you app source, inclusion of static resources and source files.

Third, Frank is focused on running in the iOS simulator. There is no fundamental reason that the Frank architecture shouldn’t work on physical devices (indeed Calabash has the same architecture), but many of Frank’s step definitions are simulator only.

   [1]: http://appium.io/
   [2]: https://github.com/appium/appium/wiki/Testing-Hybrid-Apps
   [3]: http://code.google.com/p/selenium/wiki/JsonWireProtocol
   [4]: https://code.google.com/p/selenium/wiki/JsonWireProtocol
   [5]: http://github.com/penguinho
   [6]: http://github.com/DominikDary/selendroid
   [7]: https://developer.mozilla.org/en-US/docs/Marionette
   [8]: http://www.mozilla.org/en-US/firefox/os/
   [9]: https://github.com/appium/selenium-objective-c
   [10]: https://github.com/admc/wd
   [11]: https://github.com/jlipps/yiewd
   [12]: https://github.com/appium/ruby_lib
   [13]: http://appium.io/getting-started.html#quick-start
   [14]: http://components.xamarin.com/view/calabash/
   [15]: http://calaba.sh/
   [16]: http://github.com/calabash/calabash-android
   [17]: http://github.com/calabash/calabash-ios
   [18]: http://cukes.info/
   [19]: https://github.com/calabash/calabash-ios
   [20]: http://blog.lesspainful.com/2012/03/07/Calabash-Android/
   [21]: http://xamarin.com/
   [22]: http://www.testingwithfrank.com/index.html
   [23]: http://code.google.com/p/uispec/