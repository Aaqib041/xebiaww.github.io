---
layout: post
header-img: img/default-blog-pic.jpg
author: Khyati
description: 
post_id: 18340
created: 2014/05/31 19:54:49
created_gmt: 2014/05/31 14:54:49
comment_status: open
---

# How Page Object can help in reducing complexities of test cases.

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva"><span style="line-height: 1.5em">By Design patterns what I mean is a formal technique which one can follow so as to achieve optimization. And if applied to any project can results in more optimal outputs.</span>So here I am going to talk about a best practice which we can apply while designing testing framework. There are multiple design techniques available in market but the selection of one depends on many factors such as:</span></p>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva"> 1. Project requirements</span>
<span style="font-family: verdana, geneva"> 2. Time management</span>
<span style="font-family: verdana, geneva"> 3. Skill set</span>
<span style="font-family: verdana, geneva"> 4. Members involved.</span></p>

<p style="text-align: left"><span style="font-family: verdana, geneva"><!--more--></span></p>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva">Some people think design pattern is waste of time , but it is always recommended to apply some design techniques to project. In this blog , I will run through my experience on Page Object Model. This model believes in space of each objects on web and each web page , in simple technical words there are separate methods and classes for each web action and classes respectively.</span></p>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva"> <strong>Why name 'Page Object' ?</strong></span>
<span style="font-family: verdana, geneva"> This design pattern treats each and every element of page as object. And all the functionality of the web application represents a predictable connection between all the web pages.</span></p>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva"><strong>Why this is required ?</strong></span></p>

<ul style="text-align: left">
    <li><span style="font-family: verdana, geneva;line-height: 1.5em">It gives guarantee to user that method written once, need not to be written any where else in the whole project. And web page defined once does not require any more implementation focus.</span></li>
    <li><span style="font-family: verdana, geneva">Also the tests written once will drive the page object so we will never end up in any surplus code implementation.</span></li>
    <li><span style="font-family: verdana, geneva;line-height: 1.5em">This approach separates the complexity of the features from the 'Test scripts' and give testers a space to apply all test strategies of web application in tests.</span></li>
</ul>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva">Lets have a look how we can apply Page Object Pattern to our framework.</span></p>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva">For this firstly you need to have an IDE like eclipse where-in you can write your code. You need to have knowledge of a programming language like Java. And an automation tool like Selenium. I have created three test-project so that we can compare how page objects benefits us.</span></p>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva">1.<span style="text-decoration: underline"><strong> Simple example of Selenium where all the scripts are written in simple selenium.</strong></span></span></p>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva"><a href="http://xebee.xebia.in/wp-content/uploads/2014/05/AddNewBlog.png"><img class="aligncenter size-medium wp-image-18387" alt="AddNewBlog" src="http://xebee.xebia.in/wp-content/uploads/2014/05/AddNewBlog-300x134.png" width="300" height="134" /></a></span></p>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva">In this code , I have made a class which has all the steps which we need to do so as to add a new post to a wordpress account .So if we analyse, there are some flaws to this code snippet , I have highlighted flaws in above picture by red and green boxes. The problem is we are<em> interacting with the driver multiple times</em> within the test script which looks <strong>cluttered</strong>, Other is we are <em>interacting with the HTML code</em> in the test script only that also makes test class very <strong>dirty</strong>. And the major problem is I need to write the below <em>code of log in</em> , in all the test scripts before I want to perform any operation in the test , for which the code is:</span></p>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva"><a href="http://xebee.xebia.in/wp-content/uploads/2014/05/LogIN.png"><img class="aligncenter size-medium wp-image-18388" alt="LogIN" src="http://xebee.xebia.in/wp-content/uploads/2014/05/LogIN-300x86.png" width="300" height="86" /></a></span></p>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva">So for example , if I have a test suite in which there are 1000 test cases and during production '<strong>id</strong>' which I am using in my test case for adding user name (<strong>user_login</strong>) gets changed to <strong>log_in</strong> then what I need to do is I need to change this id manually in 1000 test cases as every test case will stop working as log in is required to perform any operation in any test class.</span></p>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva">Now, we will see how page object helps us in reducing such complexities. I have written same class to add new blog post after applying page object model. Lets see how the code will change. So here what we need to do is, we have to make two packages in our testing framework ,if you are using simple java project :</span></p>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva">1. <em><strong>pages package</strong></em> : where you will be adding all page classes.</span></p>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva">2. <em><strong>test-script package</strong></em> : where in you will add test classes corresponding to each test case you have in your mind.</span></p>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva"><a href="http://xebee.xebia.in/wp-content/uploads/2014/05/WordpressWithPageObject.png"><img class="aligncenter size-medium wp-image-18389" alt="WordpressWithPageObject" src="http://xebee.xebia.in/wp-content/uploads/2014/05/WordpressWithPageObject-238x300.png" width="238" height="300" /></a></span></p>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva">Firstly, we need to create a page class which will have all the functionality and services of <em><strong>'Add new post page</strong></em>' , like this</span></p>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva"><a href="http://xebee.xebia.in/wp-content/uploads/2014/05/AddNewPageClass.png"><img class="aligncenter size-medium wp-image-18390" alt="AddNewPageClass" src="http://xebee.xebia.in/wp-content/uploads/2014/05/AddNewPageClass-300x137.png" width="300" height="137" /></a></span></p>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva">So here I have added all the complex code which will be doing interaction with selenium , HTML , browser , etc. And this is how your test class will look like:</span></p>

<p class="clean" style="text-align: left"><span style="font-family: verdana, geneva"><a href="http://xebee.xebia.in/wp-content/uploads/2014/05/TestAddNewPageClassUsingPageObjectPattern.png"><img class="aligncenter size-medium wp-image-18391" alt="TestAddNewPageClassUsingPageObjectPattern" src="http://xebee.xebia.in/wp-content/uploads/2014/05/TestAddNewPageClassUsingPageObjectPattern-300x59.png" width="300" height="59" /></a></span></p>