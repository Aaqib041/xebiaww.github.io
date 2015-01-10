---
layout: post
header-img: img/default-blog-pic.jpg
---

# How Page Object can help in reducing complexities of test cases.

By Design patterns what I mean is a formal technique which one can follow so as to achieve optimization. And if applied to any project can results in more optimal outputs.So here I am going to talk about a best practice which we can apply while designing testing framework. There are multiple design techniques available in market but the selection of one depends on many factors such as:

1\. Project requirements 2\. Time management 3\. Skill set 4\. Members involved.

Some people think design pattern is waste of time , but it is always recommended to apply some design techniques to project. In this blog , I will run through my experience on Page Object Model. This model believes in space of each objects on web and each web page , in simple technical words there are separate methods and classes for each web action and classes respectively.

**Why name 'Page Object' ?** This design pattern treats each and every element of page as object. And all the functionality of the web application represents a predictable connection between all the web pages.

**Why this is required ?**

  * It gives guarantee to user that method written once, need not to be written any where else in the whole project. And web page defined once does not require any more implementation focus.
  * Also the tests written once will drive the page object so we will never end up in any surplus code implementation.
  * This approach separates the complexity of the features from the 'Test scripts' and give testers a space to apply all test strategies of web application in tests.

Lets have a look how we can apply Page Object Pattern to our framework.

For this firstly you need to have an IDE like eclipse where-in you can write your code. You need to have knowledge of a programming language like Java. And an automation tool like Selenium. I have created three test-project so that we can compare how page objects benefits us.

1.** Simple example of Selenium where all the scripts are written in simple selenium.**

![AddNewBlog](/wp-content/uploads/2014/05/AddNewBlog-300x134.png)

In this code , I have made a class which has all the steps which we need to do so as to add a new post to a wordpress account .So if we analyse, there are some flaws to this code snippet , I have highlighted flaws in above picture by red and green boxes. The problem is we are_ interacting with the driver multiple times_ within the test script which looks **cluttered**, Other is we are _interacting with the HTML code_ in the test script only that also makes test class very **dirty**. And the major problem is I need to write the below _code of log in_ , in all the test scripts before I want to perform any operation in the test , for which the code is:

![LogIN](/wp-content/uploads/2014/05/LogIN-300x86.png)

So for example , if I have a test suite in which there are 1000 test cases and during production '**id**' which I am using in my test case for adding user name (**user_login**) gets changed to **log_in** then what I need to do is I need to change this id manually in 1000 test cases as every test case will stop working as log in is required to perform any operation in any test class.

Now, we will see how page object helps us in reducing such complexities. I have written same class to add new blog post after applying page object model. Lets see how the code will change. So here what we need to do is, we have to make two packages in our testing framework ,if you are using simple java project :

1\. _**pages package**_ : where you will be adding all page classes.

2\. _**test-script package**_ : where in you will add test classes corresponding to each test case you have in your mind.

![WordpressWithPageObject](/wp-content/uploads/2014/05/WordpressWithPageObject-238x300.png)

Firstly, we need to create a page class which will have all the functionality and services of _**'Add new post page**_' , like this

![AddNewPageClass](/wp-content/uploads/2014/05/AddNewPageClass-300x137.png)

So here I have added all the complex code which will be doing interaction with selenium , HTML , browser , etc. And this is how your test class will look like:

![TestAddNewPageClassUsingPageObjectPattern](/wp-content/uploads/2014/05/TestAddNewPageClassUsingPageObjectPattern-300x59.png)