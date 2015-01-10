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

<p>While speaking at a conference last month, I was asked to mention some of the automated testing tools available for Mobile Application Automated Testing and to specify about there features and I responded by 'please follow my blog I will write specifically on some of the existing tools and their behaviour'.</p>
<p>Hence the objective of the following post is to respond to the query and also make a list of available tools which allows us to automate the test cases for Mobile Applications. The Post would cover brief introduction to the tools, Explain the architecture of the framework and specify platforms covered by each of them.</p>
<p><strong>Tool 1 - <a href="http://appium.io/">Appium</a>:</strong> Appium is an open source test automation framework for use with native and <a href="https://github.com/appium/appium/wiki/Testing-Hybrid-Apps">hybrid</a> mobile apps. It drives iOS and Android apps using the WebDriver <a href="http://code.google.com/p/selenium/wiki/JsonWireProtocol">JSON wire protocol</a>.</p>
<p><strong>How Appium Works:</strong></p>
<p>Appium drives various native automation frameworks and provides an API based on Selenium's <a href="https://code.google.com/p/selenium/wiki/JsonWireProtocol">WebDriver JSON wire protocol</a>.</p>
<p>Appium drives Apple's UIAutomation library for iOS support, which is based on <a href="http://github.com/penguinho">Dan Cuellar's</a> work on iOS Auto.</p>
<p>Android support uses the UiAutomator framework for newer platforms and <a href="http://github.com/DominikDary/selendroid">Selendroid</a> for older Android platforms.</p>
<p>FirefoxOS support leverages <a href="https://developer.mozilla.org/en-US/docs/Marionette">Marionette</a>, an automation driver that is compatible with WebDriver and is used to automate Gecko-based platforms.</p>
<p>Hence it supports following <strong>Platforms - </strong>iOS, Android and <a href="http://www.mozilla.org/en-US/firefox/os/">FirefoxOS</a>.</p>
<p><strong>Features: </strong>
<ol>
    <li>We don't have to recompile our app or modify it in any way, due to use of standard automation APIs on all platforms.</li>
    <li>We can write tests with our favorite dev tools using any <a href="https://code.google.com/p/selenium/wiki/JsonWireProtocol">WebDriver</a>-compatible language such as Java, <a href="https://github.com/appium/selenium-objective-c">Objective-C</a>, JavaScript with Node.js (in both <a href="https://github.com/admc/wd">callback</a> and <a href="https://github.com/jlipps/yiewd">yield-based</a> flavours), PHP, Python, <a href="https://github.com/appium/ruby_lib">Ruby</a>, C#, Clojure, or Perl with the Selenium WebDriver API and language-specific client libraries.</li>
    <li>We can use any testing framework.</li>
</ol>
Investing in <a href="https://code.google.com/p/selenium/wiki/JsonWireProtocol">WebDriver</a> means we are betting on a single, free and open protocol for testing that has become a de-facto standard. If we use Apple's UIAutomation library without Appium we can only write tests using JavaScript and we can only run tests through the Instruments application. Similarly, with Google's UiAutomator we can only write tests in Java. <strong>Appium opens up the possibility of true cross-platform native mobile automation. Finally!</strong></p>
<p>Refer to the <a href="http://appium.io/getting-started.html#quick-start">link</a> for Quick Start Guide. Appium is being used by the popular test lab Sauce Labs so it looks like it would be more active and get more support down the line. <a href="https://saucelabs.com/mobile" rel="nofollow">https://saucelabs.com/mobile</a></p>
<p><strong>Tool 2 <a href="http://components.xamarin.com/view/calabash/">Calabash</a>: </strong>Calabash is an automated testing technology for Android and iOS native and hybrid applications. This repository contains support for iOS, for Android, see <a href="http://calaba.sh/">Calabash Landing Page</a>. It consists of two libraries <a href="http://github.com/calabash/calabash-android">calabash-android</a> and <a href="http://github.com/calabash/calabash-ios">calabash-ios</a> and are the testing library for Android and iOS.</p>
<p><strong>How Calabash Works:</strong></p>
<p>Calabash-iOS and Calabash-Android are the underlying low-level libraries that empower the <a href="http://cukes.info/">Cucumber</a> tool to run automated functional tests on Android and iOS phones and tablets as well as on simulators. These low-level libraries enable QA, business staff and developers to work at a high level by writing tests in a domain specific language using the terms and concepts of their business domain.<strong></strong></p>
<p>Thes libraries enable test-code to programmatically interact with native and hybrid apps. The interaction consists of a number of end-user actions. Each action can be one of</p>
<dl><dt><strong>Gestures</strong></dt><dd>Touches or gestures (e.g., tap, swipe and rotate).</dd><dt><strong>Assertions</strong></dt><dd>For example: there should be a "Login" button or the web view should contain an "&lt;h1&gt;" element with the text "Hello".</dd><dt><strong>Screenshots</strong></dt><dd>Screendump the current view on the current device model.</dd><dd></dd><dd><a href="https://github.com/calabash/calabash-ios"><strong><em>Calabash iOS</em></strong></a> consists of two parts: a client library written in Ruby, and <code>calabash.framework</code>, a server framework written in Objective-C (a Clojure/JVM version of the client is coming too). To use calabash we make a special test target in XCode that links with <code>calabash.framework</code>. The application is otherwise unchanged. The server framework will start an HTTP server inside your app that listens for requests from the client library.<strong><a style="line-height: 1.5em" href="http://github.com/calabash/calabash-android"><em>Calabash Android</em></a></strong><span style="line-height: 1.5em"> is slightly different even though the idea is much the same. You can read about the architecture of Calabash Android </span><a style="line-height: 1.5em" href="http://blog.lesspainful.com/2012/03/07/Calabash-Android/">here</a><span style="line-height: 1.5em">. The greatest benefit of the different architecture is that you can test your Android app without making any changes to the app.</span><span style="text-decoration: underline"><em>Testing Hybrid Apps</em></span>If you are using Appcelerator, PhoneGap, Sencha Touch or any of the other frameworks for creating <em>hybrid apps</em>. You should really take a look at Calabash. By using the same Cucumber interface as for a native app you can test HTML 5 part of our application. Testing webviews has not been posible with the currently available test frameworks. But with Calabash we can!

Using Calabash you can run your tests on iOS and Android Devices and Simulator.

Calabash is a free open source project, developed and maintained by <a href="http://xamarin.com/">Xamarin</a>.

</dd></dl>

<p><strong>Tool 3 <a href="http://www.testingwithfrank.com/index.html">Frank</a>: </strong>Frank allows us to write structured text test/acceptance tests/requirements (using<a href="http://cukes.info/">Cucumber</a>) and have them execute against your iOS application.</p>
<p>Frank is the library closest to Calabash, and Calabash is highly inspired by it.</p>
<p>First, Frank is licensed using GNU GPLv3 and we wanted to use the less restrictive EPL. Pete actually didn’t want to license Frank as GPL, but since Frank is based on UISpec which is GPL'ed, he had to. We did not want the <a href="http://code.google.com/p/uispec/">UISpec</a> project as a dependency.</p>
<p>Second, is a bit of a pain to set up. It required modification of you app source, inclusion of static resources and source files.</p>
<p>Third, Frank is focused on running in the iOS simulator. There is no fundamental reason that the Frank architecture shouldn’t work on physical devices (indeed Calabash has the same architecture), but many of Frank’s step definitions are simulator only.</p>