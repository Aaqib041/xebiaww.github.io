---
layout: post
header-img: img/default-blog-pic.jpg
---

# Mobile Test Automation Framework using Cucumber, Appium and Page Objects 

For teams developing & maintaining mobile apps for both Android & iOS platforms, functional testing is a huge challenge. The current process of updating apps over the air is very easy, raising users’ expectations of new features delivered sooner. So what this means for development teams is, they have to deliver many releases frequently and in turn what this means for Test teams is, they are continuously tasked with testing entire applications faster and more often. Solution for such problems is Test Automation. [Appium ](http://appium.io/)is a mobile Test Automation tool that has made life a lot easier for testers because of its capabilities and powers. Appium has made it easier for teams to test their apps for multiple platforms(one test case can test an app on iOs and android both). But as mobile application matures and the automation test cases grow in number and project enters into a state where Regression test cases are changed frequently, that is when Test Teams starts to feel the heat about maintainability of test automation project and simultaneously adding new test cases. And with the advent of Agile practices, Behavior Driven Development/Testing has also gained more popularity. No tool alone can solve the problem mentioned above.  Appium alone cannot solve the challenges around implementing behavior driven development & frequent UI/functionality updates. What is needed is, the combination of tools which can  address these problems when combined together. So on this thought process,i created a Test Framework combining Appium, Cucumber-JVM & Page Objects. Before we move ahead, Lets try list down the features/qualities that we expect from a good Test Automation Framework for mobile: 

  1. Even Non Technical people like PO and BA's' can help in Test Automation.
  2. Requirements are directly converted into Test cases ( to eliminate difference in understanding of Dev and QA)
  3. Automation tool supports multiple platforms i.e iOS, Android etc. ( Same tool can be used to write test case for iOS and Android)
  4. Inter portability of same test cases on Multiple platform ( Same test case can be run for iOS and Android).
  5. Maintainability ( How easy it is to change/update a test case)
  6. Re-usability ( How easy it is to use existing code to write new test cases)
To solve these problems or to achieve these features we integrated Cucumber-JVM and Appium. Lets see what are the advantages of Cucumber and Appium: _**Cucumber**: ****_is a key tool in implementing BDD. Some of the advantages of Cucumber are: 

  * Cucumber Enables Non Technical team members to help in Test Automation (by writing feature files).
  * Cucumber can be implemented in multiple languages. i.e. Java, Ruby and JavaScript. Since i am more comfortable with Java that's why i used java implementation of cucumber known as cucumber-JVM. But using others implementation should also be more or less same.
  * Cucumber can be used to implement projects in wide business areas like enterprise web, Mobile, Web Services etc.
  * Cucumber is very easy to install, implement and learn.
_**Appium:**** **_is very good for Mobile Automation and Some of the advantages of Appium are:** **

  * Support for multiple platforms iOS, Android and Firefox OS.
  * Support for Native, Web and Hybrid Applications.
  * Can be implemented in multiple languages Java, PHP, Python, Ruby, C# etc.
  * No need to import any library in the application code.
  * Can be implemented with any Testing Framework i.e Junit, TestNG, Cucumber etc.
**Note**: Please click on the images to enlarge them and understand the content. Lets first understand the Application under Test. First Screen is the Application Landing Page, Which has four buttons: Agenda, Speakers, My schedule and Location. ![Application Home](/wp-content/uploads/2014/08/Application-Home-230x300.png) Second screen is displayed when user clicks on Agenda in Home Screen. ![Agenda Screen](http://xebee.xebia.in/wp-content/uploads/2014/08/Agenda-Screen-230x300.png) Third screen is displayed when user clicks on Speakers on Home Screen. ![Speakers Screen](http://xebee.xebia.in/wp-content/uploads/2014/08/Speakers-Screen-230x300.png) Now lets see what Test Cases we are going to write: 

  1. Test Case 1 : From Home Screen, user goes to agenda screen and comes back to home screen
  2. Test Case 2: From Home Screen, user  goes to Speakers screen and comes back to home screen
Now Lets understand the Maven Project that we created for this framework. Step 1: Since its a maven project, we need to add dependencies in the pom.xml for Cucumber and Appium. Below is the snapshot of the pom.xml: ![pom.xml](/wp-content/uploads/2014/08/pom-300x209.png) Step 2: Lets understand the project structure by next snapshot: ![Project Structure](http://xebee.xebia.in/wp-content/uploads/2014/08/Project-Structure-195x300.png) Step 3: Since its a cucumber project, next step is to write a feature file. I have created the feature file as per the application (screen shot and test cases attached above). Below is the snapshot of the feature file: ![feature file](http://xebee.xebia.in/wp-content/uploads/2014/08/feature-file-300x172.png) Step 4: Cucumber needs Glue code for implementation. So next logical step would be to right step definitions. Here is the snapshot for step definitions: ![StepDefinitionNormal](http://xebee.xebia.in/wp-content/uploads/2014/08/StepDefinitionNormal-300x237.png) Step 5: Next step is to right some hooks code,which will invoke Appium Instance and close the instance after test case is finished.Here is the sanpshot for the same: ![Hooks](http://xebee.xebia.in/wp-content/uploads/2014/08/Hooks-300x213.png) Step 6: Here is the code for invoking and closing Appium Instance. you can keep this code in any class and then use that class in hooks class accordingly. I have created a Java Class AppiumTest.java and declared this code there. here is the code snapshot for the same:

## Comments

**[chetandewangan](#9518 "2014-09-23 05:06:50"):** Hi Shankar, Thanks for very descriptive tutorial. My code without page object ass working fine, but when I tried page object by myself and also implemented using your way of DriverFactory, I am getting: Feature: my first app test: Bad Access: 'org.openqa.selenium.WebDriver' is not instantiable Also I tried adding @Before annotation, but it says something like: Tests in error: initializationError(cucumber.test.RunTest): You're not allowed to extend class es that define Step Definitions or hooks. class cucumber.test.pages.LoginPage ex tends class cucumber.test.pages.DriverFactory Do you have any idea whats going wrong? Thanks for help,

**[Shankar Garg](#9519 "2014-09-23 11:23:59"):** Since you have not pasted code for your classes, so i am just assuming the file structure and the code. 1\. First of all why Webdriver? i think you should get AppiumDriver error if at all. Right now what i can think of is, its something to do with 'static' of driver variable. so please check static with driver variable initialisation and see if that resolves the issues. 2\. You can not use @Before in Driver Factory, for that we have a different file called hooks.java. @Before can not be used in any class which is being extended or being used in test case flow. @Before should be used in hooks.java file only. If you could paste the code snippet i will be in a better position to reply.

**[chetandewangan](#9521 "2014-09-25 04:11:37"):** You are right Shankar, It is working now. Thanks very much.

