---
layout: post
header-img: img/default-blog-pic.jpg
---

# Web GUI Automation: Tools, Strategies & Patterns

What are the key features an automation framework should have for being successful at an implementation and execution level? Should that be robust, easy-to-use, business-driven, tester friendly? What are the conceptual guidelines that we should follow while designing automation framework? These are the questions that we understand when we have created/seen/used an automation framework that does not have any/most of above-mentioned features. Also some of the root-level requirements of an automation framework that we hear from our clients are: 

  * It should be easy to update test scripts when there is any UI change on web pages.
  * It should be easy to read and understand test script code. Even a quick glance on snippet of code should tell about web page being referred and its underlying functionality.
  * Test script execution should be fast.
  * Failed test methods should enter enough information into logs so that it becomes easy and quick to pin-point the issue and report to different stakeholders.
  * It should have ability to re-run failed test methods.
  * It should be able to pick test data from external data sources (txt, xls, csv, sql, xml etc)
So in this post I am going to talk about tools that we should select while targeting Web UI Automation and then how should we leverage benefits of well known patterns to make maintainable,  extensible and usable automation framework . _** **_ _**Selection of Tools**_ Following are the details of recommended tools for Web UI Automation: **Selenium WebDriver** [ Selenium](http://seleniumhq.org/) is an open source testing framework that allows you to emulate the actions of a user when navigating and interacting with a web site through standard browsers (such as IE, Firefox or Chrome). When your scripts are running your chosen browser pops up and can be watched as your steps are replayed back at you. Selenium 2.0 has many new exciting features and improvements over Selenium 1. The primary new feature is the integration of the [WebDriver](http://code.google.com/p/selenium/) API. This addresses a number of limitations along with providing an alternative, and simpler, programming interface. WebDriver supports not only real browsers (IE, Chrome & Firefox) but also headless ones (using HTMLUnit). **Java** Selenium WebDriver’s [Java binding](http://code.google.com/p/selenium/downloads/list) is the most preferred platform where one gets access to vast array of open source libraries that exist for Java and moreover Java code is maintainable because it has relatively simple syntax and encourages a fairly standardized way of doing things. **TestNG** [ TestNG](http://testng.org/doc/index.html) is a testing framework inspired from JUnit and NUnit but it has some new functionalities that make it more powerful and easier to use like: 

  * Using TestNG's dependsOnMethods attribute, one can easily specify dependent methods. So in case we want the test method 'b' should only execute when test method 'a' has passed, we will be able to do this using dependsOnMethods attribute.
  * Ability to rerun failed tests is especially handy in large test suites, and it's a feature you'll only find in TestNG. Anytime there is a failure in TestNG, it creates an XML configuration file that delineates the failed tests.
  * One can group our test cases under different test groups like sanity, regression, critical-path or module-wise like payment, third-party-integration, user-management, admin-console etc. and can execute specific test group later on.
**Maven** The easiest way to set up a Selenium WebDriver Java project is to use Maven. Maven will download the java bindings (the Selenium 2.0 java client library, TestNG and other required jars) and all its dependencies, and will create the project for us, using a maven pom.xml (project configuration) file. Once we are done with this, we can import the maven project into our preferred IDE, IntelliJ IDEA or Eclipse. **CI Integration** Selenium WebDriver project is a great fit for the continuous integration tools like Jenkins, Hudson, CruiseControl as it can be installed on server testing box and drive/execute test cases remotely on different machines to target browser compatibility with different browsers on different platforms. _** **_ _**Strategies & Patterns**_ There are lot of issues that people face while doing Web Automation with respect to their maintainability, scalability and difficulty in debugging the failed scenarios. However these are generic issues with every automated testing tool and most likely due to improper design of testing framework. I am listing down few strategies and patterns that can help us in building a good framework: **GUI Repository & Page Factory Pattern** A UI map is a mechanism that stores Identifiers/locators for an application’s UI elements. The test script then uses the UI Map for locating the elements to be tested. Basically, a UI map is a repository of test script objects that correspond to UI elements of the application being tested. UI map has two significant advantages: 

  * Using a centralized location for UI objects instead of having them scattered throughout the script. This makes script maintenance more efficient.
  * Cryptic HTML Identifiers and names can be given more human-readable names improving the readability of test scripts.
In order to support the PageObject pattern, WebDriver's support library contains a factory class (org.openqa.selenium.support.[PageFactory](http://code.google.com/p/selenium/wiki/PageFactory)) where we use the annotation @FindBy for making the WebElement object to know to which element it belongs to on web page. By using this pattern we need not to write the code for finding each and every Web Element. **Page-Object Pattern** [ Page Object Pattern](http://code.google.com/p/selenium/wiki/PageObjects) is a pattern that displays web page's UI as a class. In addition to UI, functionality of the page is also described in this class. This provides a bridge between page and test. The idea is to create a level of abstraction to separate the tests from the test subjects, and to provide a simple interface to the elements on the page. Here are the main advantages of Page Object Pattern using: 

  * Simple and clear tests.
  * Good support of tests, because everything is stored in one place.
  * Easy creation of new tests. In fact, tests can be created by a person not knowing the features of automation tools.
PageObjects are used as an abstraction to a concrete web page and may be constructed using a _PageFactory_ . **Utilities** There should be a utility package that will have classes providing common functionalities that are needed by scripts. For example method to read configuration/properties file, method to send test automation results via email, method to take screenshot on every failure etc. **Error Handling Module** This module should be designed in such a way so that it provides exact information about state of app when there is an assertion failure. The objective is to minimize the debug time by making available all the resources/info we need so that we can be sure about the bug, steps to reproduce the bug etc. This can be leveraged using: 

  * Ability to take screen shot on every assertion failure.
  * Ability to take screen shot on every exception and log stack trace for easy debugging.

## Comments

**[Jasmeet](#8158 "2012-03-31 10:26:08"):** Nice write up. Covering almost all of the essentials needed for WebDriver based automation.

**[Nishtha](#8159 "2012-03-31 10:34:06"):** Gaurav, could you please share something more about how should we choose best locator?

**[Priyanka](#8161 "2012-03-31 12:15:53"):** Very nice blog..I got to learn about page object and page factory pattern through your blog.

**[Bệnh trĩ](#8849 "2012-05-23 17:22:27"):** I have been browsing online greater than 3 hours as of late, but I never discovered any interesting article like yours. It is pretty price enough for me. Personally, if all website owners and bloggers made excellent content material as you probably did, the net will probably be much more useful than ever before.

**[Jaya](#8860 "2012-05-25 10:40:51"):** Thanks for the post. I have been using selenium and was confused about the right test automation strategies. This post was incredibly helpful.

**[Jeevan](#8985 "2012-06-08 15:21:54"):** Nice post. tnx for providing info about things to consider while automating using selenium.

**[Sam](#8274 "2012-04-03 16:59:49"):** I prefer to identify the elements with "visible text". I find that changes less frequently. It is part of your public API to your users in documentation etc... XPath etc are surely part of the implementation?

**[Tanu](#8233 "2012-04-02 16:50:35"):** Good one, very well written, it provides information from starting till end, covers almost all required points.

