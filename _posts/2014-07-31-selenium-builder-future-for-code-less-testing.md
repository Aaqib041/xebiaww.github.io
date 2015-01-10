---
layout: post
header-img: img/default-blog-pic.jpg
---

# Selenium Builder – Future for Code-less Testing

# 

Selenium Builder is an extension to Firefox which helps you easily build and export Selenium tests using record and playback capabilities, and then export those scripts to any programming language.

The most important feature to come along in Selenium Builder is built-in support for Selenium 2/Webdriver. You can record, edit, and play back test scripts based on the new Selenium version, which is particularly handy for folks needing help making the switch from Selenium RC to WebDriver. Plus, since Builder uses the same interface for this as it does for Selenium 1, you can use a mixture of both technologies to build your scripts. Selenium Builder can also translate between the two versions with reasonable accuracy.

Additionally, you can export Selenium 2 scripts into a wide variety of languages: Java, Ruby, Python, PHP, Node.js, and C# . Se Builder 2 also defines a clean JSON-based format for storing scripts for later editing. These JSON-based scripts can be played back by a standalone [interpreter](https://github.com/sebuilder/se-builder/downloads), so you don’t have to commit to a particular programming language for playback.

## Record the test

You are now ready to record a test against xebia.in. The test will check that the site does not allow logging in with non-valid credentials.

  * In Firefox: **Tools >Web Developer > Launch Selenium Builder**
![1](/wp-content/uploads/2014/07/11-300x212.png) ******In ****Start recording at****, enter ****http://xebia.in/**

  * Hit the **Selenium 2** button
  * This will open the URL in a browser tab, and that tab will be highlighted, signalling that this is where you are recording the test. The SeleniumBuilder window will now look like this:
![2](/wp-content/uploads/2014/07/2-300x61.png) As you can see, there is already an instruction in the list, which says "get **http://xebia.in/**[".](http://doc.tiki.org/Documentation)

  * Next, you want to carry out the steps in the test
    * Click on link text “ News “ on xebia.in.
    * Click on link text “ Continuous Delivery “ on xebia.in
    * Click on link text “ Advanced Scrum “ on xebia.in
At that point, your SeleniumBuilder window looks something like this: ![3](/wp-content/uploads/2014/07/3-300x114.png) The next step is to add an assertion.  we will add the verification manually... Put the mouse over Step 2 You will see a menu appear on the left. ![4](http://xebee.xebia.in/wp-content/uploads/2014/07/4-300x197.png) Click on **new step below** You will see a new step that says _"clickElement id:"_. This is the default type of step. You will have to change it to be an assertion.

  * Put the mouse over that new step, and click on **edit type**.