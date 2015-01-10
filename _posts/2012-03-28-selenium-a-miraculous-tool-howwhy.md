---
layout: post
header-img: img/default-blog-pic.jpg
author: Khyati
description: 
post_id: 12693
created: 2012/03/28 16:47:53
created_gmt: 2012/03/28 11:47:53
comment_status: open
---

# Selenium-A Miraculous Tool (How,Why?)

** ** In today's fast paced world, the task of manual testing where huge amounts of data is involved looks very cumbersome. Time is like the wind, it lifts the light and leaves the heavy. We have so little time to get all the things done which we want to do in our busy lives. This really comes into picture when it comes to running your own online business or site.

Many, perchance most, software applications today are written as web-based applications to be run in an Internet browser. The efficacy of testing these applications varies broadly amid-st companies and organizations. In an era of highly interactive and susceptible software processes where many organizations are using some form of Agile methodology, test automation is frequently becoming a exigency for software projects. Test automation is oftentimes the answer. Test automation means using a software tool to run repeatable tests against the application to be tested. 

Recently I had started working on this “prodigious”tool called “**Selenium**”in my project([SFDC][1]).

Selenium, a demotic and well established testing framework is a wonderful tool that provides a convenient consolidated  interface that works with a large number of browsers, and allows you to write your tests in well-nigh every language one can imagine .

It was one of the first Open Source projects to fetch browser-based testing to the populace, and because it's written in JavaScript it's possible to quickly add support for new browsers that might be released.

Selenium is written in JavaScript which causes a substantial infirmity.Browsers impose a pretty strict security model on any JavaScript that they execute in order to protect a user from malignant scripts. 

Selenium was originally developed by Jason Huggins, who was later joined by other programmers and testers at ThoughtWorks.  But being a smart guy, he realized there were better uses of his time than manually stepping through the same tests with every change he made. 

**Selenium** is a portable software testing framework for web applications. Selenium provides a record/playback tool for authoring tests without erudition a test scripting language (**Selenium IDE**). It also provides a test domain-specific language (**Selenese**) .It is open-source software, released under the Apache 2.0 license and can be downloaded and used free of cost.

Selenium has multiple software tools. Each has a specific role :

**Selenium IDE**(Integrated Development Environment ) is a prototyping tool for building test scripts.It is easy to use and records user actions.But the drawback of it is, it can only be used as a Firefox plug-in.So to overcome this drawback Selenium1 (or **Selenium RC**)came up with many new features. It explains how to develop an automated test program using the Selenium RC API. Selenium RC components are:

  * The Selenium Server which launches and kills browsers, interprets and runs the Selenese commands passed from the test program, and acts as an _HTTP proxy_, intercepting and verifying _HTTP_ messages passed between the browser and the AUT. 
  * _Client libraries_ which provide the interface between each programming language and the _Selenium RC Server_. 
You may, or may not, need the Selenium Server, depending on how you propound to use Selenium. **Selenium Server ** receives Selenium commands from your test program, interprets them, and reports back to your program the results of running those tests. If you will be strictly using the WebDriver API you do not perforce need the Selenium Server. Selenium-WebDriver makes direct calls to the browser using each browser’s native support for automation. How these direct calls are made, depends on the browser you are using.

**Why migrate to WebDriver?**

WebDriver has Smaller and compact API. WebDriver’s API is more Object Oriented than the original Selenium RC API. This can make it easier to work with. 

  * Better emulation of user interactions. Where-in possible, WebDriver makes use of inherent events in order to interlude with a web page. This more closely mimics the way that the users work with the site and apps. In addition, WebDriver offers the progressive user interactions APIs which allow you to model complicated interactions with the site. 
  * Support by browser vendors. Opera, Mozilla and Google are all active participants in WebDriver’s development, and each have engineers working to improve the framework. 
The first step when starting the migration is to change how you obtain your instance of Selenium. When using Selenium RC, this is done like so: 

> 
>     Selenium selenium = new DefaultSelenium("localhost",4444,"*firefox","[http://www.yoursite.com][2]");
>     selenium.start();

One can also modify and use the Webdriver backed with the Selenium that is using WebDriver throughout and instantiating a Selenium instance on demand ,using this command:

> 
>     **Se****lenium selenium = new WebDriverBackedSelenium(“driver”, “baseUrl”);**
> 
> A typical example using WebDriver in Java looks like this:
> 
> // Create an instance of WebDriver backed by Firefox **WebDriver driver = new FirefoxDriver();**
> 
> // Now go to the Google home page **driver.get("http://www.google.com");**

   [1]: http://www.salesforce.com
   [2]: http://www.yoursite.com/

## Comments

**[Cheshta](#8155 "2012-03-31 08:42:56"):** Commendable effort!

**[Rashmi](#8114 "2012-03-29 12:38:50"):** Its an awesome blog, i really came to know a new things about automation testing selenium tool .This one is really an informative blog for brushing up our skills on selenium testing tool.

**[parina](#8115 "2012-03-29 14:11:50"):** Very Informative :)

**[Vanshaj](#8125 "2012-03-29 23:37:43"):** Appreciated !! :)

