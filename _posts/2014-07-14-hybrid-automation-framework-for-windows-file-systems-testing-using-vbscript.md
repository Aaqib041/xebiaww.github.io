---
layout: post
header-img: img/default-blog-pic.jpg
author: Msingh
description: 
post_id: 18601
created: 2014/07/14 14:12:35
created_gmt: 2014/07/14 09:12:35
comment_status: open
---

# Hybrid Automation Framework for Windows File Systems Testing using Vbscript

**Overview:-**

==========

The need for better quality means more pressure on QA (Quality Assurance People) to take care of the quality of the product. When testing process become large and complex specially case of long term project where more regression testing is required due to lot of changes in the product then automation testing comes in picture that help manual testing people so automation can perform their task in fastly & accurate manner. To implement automation In a project require a automation framework means Test Automation Framework, Automation Framework  is nothing its sets of concepts, tools, assumptions and lot of reusable code that provided supports for automated software testing. Mainly main advantage of an automation framework is to reduce maintenance cost & also to reuse the generic code between various modules and scripts.

Hybrid Automation Framework is a concept that mainly consists Keyword & Data Driven Framework inside it and makes automation process is more easy and maintainable. Hybrid Automation Framework uses Data Driven framework concept means we can use various data files for Test Data (Input & Output) purpose and with use of Keyword driven testing we can easily handles the automation of newly added test cases without any change to the Main or Driver Script that actually initiate the automation. We can easily integrate Hybrid Automation Framework with various Functional Testing Tools like HP Quick Test Professional (QTP), IBM Rational Functional Tester (RFT), Open source tools like Selenium etc. Hybrid Automation Framework can easily be developed for Web Based, Windows Based, and Database Based, and Standalone application.

**Problem Scope:-**

===============

Here we are going to discuss the automation approach for windows file system testing means if we are making any changes in Windows File system and trying to test various file system operation on windows operating system and want to test it quickly to make sure that none of the file operations like create,edit,delete etc has been changed then this framework gives the more flexibility and power to the automation testing.

**Requirements for Test Automation Frameworks:-**

Automatic test execution is a high level functional requirement for test automation frameworks. Ease of use and maintainability are non-functional in their nature and in general achieving them is harder than simply implementing the functional requirements.

The Hybrid [Test Automation Framework][1] is what most frameworks evolve into overtime and multiple projects. The most successful automation frameworks generally accommodate both [Keyword-driven testing][2] as well as [Data-driven testing][3]. This allows data driven scripts to take advantage of the powerful libraries and utilities that usually accompany a keyword driven architecture. The framework utilities can make the data driven scripts more compact and less prone to failure than they otherwise would have been. The utilities can also facilitate the gradual and manageable conversion of existing scripts to keyword driven equivalents when and where that appears desirable. On the other hand, the framework can use scripts to perform some tasks that might be too difficult to re-implement in a pure keyword driven approach, or where the keyword driven capabilities are not yet in place.

**Framework Capabilities:-**

This section explains what capabilities a test automation framework must have.Discussed features can be seen as functional requirements for automation frameworks.

Automatic Test Execution

Ease of Use

Maintainability

Executing Tests Unattended

Starting and Stopping Test Execution

Handling Errors

Verifying Test Results

Assigning Test Status

Handling Expected Failures

**Detailed Logging**

Test automation framework ought to give enough information to test engineers and developers so that they can investigate failed test cases. Giving only test statuses and short status messages is not enough. Instead more detailed test logs about the

**Fail**:-Used to log the status of a failed test case.

**Pass: -** Used to log the status of a passed test case.

**Error**:-Used when an unexpected error preventing current test’s execution occurs. For example the button to push in a test does not exist.

**Failure: - **Used when the test outcome does not match the expected outcome.

**Warning:-**Used when a recoverable error not affecting test execution occurs. For example deleting some temporary file fails Info Used to log normal test execution like starting test and passed verifications.

**Framework Structure:-**

The requirement for the framework to be either data-driven, keyword-driven, or both means that some kind of system where test data can be designed and created is needed. Multiple requirements for monitoring test execution suggests that another system is needed for monitoring purposes and yet another system is, of course, needed for actually executing test cases. Below are the major parts of the Hybrid Automation Framework.

![Framework Strcuture][4]

 

**Test Design System:-**

Test design system is used for creating new test cases and editing existing ones. It Is test engineers’ and domain experts’ playground and must be easy to use with Minimal training and without programming skills. the created test data can be stored into flat files or databases. A simple solution is using an existing tool like spreadsheet program like Microsoft Excel. A special test design tool made just for this purpose would of course have its benefits but building something like that is out of the reach for most automation projects.

![Architecture][5]

 

**Test Execution System:-**

Test execution system is the core of the framework. Its main components are driver Scripts, the test library, the test data parser and other utilities like logger and report generator.

**Driver Script:-**

Test execution is controlled by driver scripts which are started using the test monitoring system or either by the Test Management tool or from an external test utility... Driver scripts are pretty short and simple because they mainly use services provided by Functions libraries and other reusable modules.

![Driver Script][6]

 

**Function Libraries:-**

Function libraries contain all those reusable functions and modules which are used in testing or in activities supporting it. Testing is mainly interacting with the tested system and checking that it behaves correctly. Supporting activities are often related to setting up the test environment and include tasks like copying files, initializing databases and installing software. Functions in the Function library can do their tasks independently but they can also use other functions or even external tools.

   [1]: http://en.wikipedia.org/wiki/Test_Automation_Framework (Test Automation Framework)
   [2]: http://en.wikipedia.org/wiki/Keyword-driven_testing (Keyword-driven testing)
   [3]: http://en.wikipedia.org/wiki/Data-driven_testing (Data-driven testing)
   [4]: http://xebee.xebia.in/wp-content/uploads/2014/07/Framework-Strcuture-300x179.png
   [5]: http://xebee.xebia.in/wp-content/uploads/2014/07/Architecture-300x257.png
   [6]: http://xebee.xebia.in/wp-content/uploads/2014/07/Driver-Script-300x179.png