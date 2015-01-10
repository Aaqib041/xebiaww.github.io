---
layout: post
header-img: img/default-blog-pic.jpg
author: Shankar Garg
description: 
post_id: 18637
created: 2014/07/31 14:28:03
created_gmt: 2014/07/31 09:28:03
comment_status: open
---

# Cucumber-JVM: All you need to know about @CucumberOptions

When we talk about Cucumber JVM for Behavior Driven development, we often talk about Feature files, Scenarios, Background and Glue code(step definitions). There is no iota of doubt that you cant implement BDD untill you know the concepts mentioned above but other area that is really important and is very useful in day to day BDD life is @CucumberOptions.

Lets start by understanding the structure of the cucumber project i am going to use in examples.Here is the snapshot for project structure.

![project setup][1]

To simplify the learning process, i have divided the blog in 3 parts: 

  * Usage of @CucumberOptions
  * How to use it ( code part)
  * Different options available
_**Usage**_: @CucumberOptions annotation provides the same options as the cucumber jvm command line. for example: we can specify the path to feature files, path to step definitions, if we want to run the execution in dry mode or not etc. 

           Basically @CucumberOptions enables us to do all the things that we could have done if we have used cucumber command line. This is very helpful and of utmost importance if we are using IDE such eclipse only to execute our project.

_**How to use**** it:**_ Create one empty class with the @RunWith(Cucumber.class) annotation.  The options to be used are defined with the @CucumberOptions.

Executing this class as any JUnit test class will run all features found on the classpath in the same package as this class.Name of the class could be anything and most common name for this class is RunCukeTest.java

Note: @Cucumber.Options is deprecated from version 1.1.5

lets look at the sample code for the RunCukeTest.java structure:

` package com.sample; import org.junit.runner.RunWith; import cucumber.api.CucumberOptions; import cucumber.api.junit.Cucumber; @RunWith(Cucumber.class) @CucumberOptions( //your cucumber options goes here } ) public class RunCukeTest { // This class will be empty } ` _**Different options:**_** **

Element
Purpose
Default

dryRun
true (Skip execution of glue code)
FALSE

strict
true (will fail execution if there are undefined or pending steps)
FALSE

features
the paths to the feature(s)
{}

glue
where to look for glue code (stepdefs and hooks)
{}

tags
what tags in the features should be executed
{}

monochrome
whether or not to use monochrome output
FALSE

format
what formatter(s) to use
{}
Here is the code snippet which shows hows to write these options in code format ` package com.sample; import org.junit.runner.RunWith; import cucumber.api.CucumberOptions; import cucumber.api.junit.Cucumber; @RunWith(Cucumber.class) @CucumberOptions( dryRun = false, strict = true, features = "src/test/features/com/sample", glue = "com.sample", tags = { "~@wip", "@executeThis" }, monochrome = true, format = { "pretty", "html:target/cucumber", "json:target_json/cucumber.json", "junit:taget_junit/cucumber.xml" } ) public class RunCukeTest { } ` Although the table and code above summaries things very clearly but still lets look at each option one by one to understand it better. 

  *  _**dryRun: **** **_if dryRun option is set to true then cucumber only checks if all the steps have their corresponding step definitions defined or not. The code mentioned in the Step definitions is not executed. This is used just to validate if we have defined a step definition for each step or not.
  * _**Strict: **** **_if strict option is set to false then at execution time if cucumber encounters any undefined/pending steps then cucumber does not fail the execution and undefined steps are skipped and BUILD is **SUCCESSFUL**. This is what the Console output looks like:
![StrictFalse][2]

and if Strict option is set to true then at execution time if cucumber encounters any undefined/pending steps then cucumber does fails the execution and undefined steps are marked as fail and BUILD is **FAILURE**. This is what the Console output looks like:

![StrictFalse][3]

  * _**Monochrome:**** **_if monochrome option is set to False, then the console output is not as readable as it should be. may be the output images will make this more clear.
Output When monochrome option is set to false is shown in below screenshot.

![monochromeFalse][4]

Output When monochrome option is set to true is shown in below screenshot.

![monochromeFalse][5]

  * _**Features: **_** **features option is to specify the path to feature files. when cucumber starts execution, Cucumber looks for .feature files at the path/folder mentioned in features option. Whichever files are with .feature extension ( if there are no compilation errors) at the path/folder mentioned in features, are executed. below snapshot will make it clear on what path to define keeping in mind the project structure:
![featuresPath][6]

   [1]: http://xebee.xebia.in/wp-content/uploads/2014/07/project-setup-300x168.png
   [2]: http://xebee.xebia.in/wp-content/uploads/2014/07/StrictFalse-300x168.png
   [3]: http://xebee.xebia.in/wp-content/uploads/2014/07/StrictFalse1-300x168.png
   [4]: http://xebee.xebia.in/wp-content/uploads/2014/07/monochromeFalse-300x168.png
   [5]: http://xebee.xebia.in/wp-content/uploads/2014/07/monochromeFalse1-300x168.png
   [6]: http://xebee.xebia.in/wp-content/uploads/2014/07/featuresPath-300x168.png

## Comments

**[sharathkonda](#9526 "2014-11-20 04:44:24"):** Dear Shankar, Now format has been deprecated, use plugin.

