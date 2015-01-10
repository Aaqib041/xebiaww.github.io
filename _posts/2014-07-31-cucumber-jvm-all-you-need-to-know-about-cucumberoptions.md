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

<p>When we talk about Cucumber JVM for Behavior Driven development, we often talk about Feature files, Scenarios, Background and Glue code(step definitions). There is no iota of doubt that you cant implement BDD untill you know the concepts mentioned above but other area that is really important and is very useful in day to day BDD life is @CucumberOptions.</p>
<p>Lets start by understanding the structure of the cucumber project i am going to use in examples.Here is the snapshot for project structure.</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/07/project-setup.png"><img class="alignnone size-medium wp-image-18638" alt="project setup" src="http://xebee.xebia.in/wp-content/uploads/2014/07/project-setup-300x168.png" width="300" height="168" /></a></p>
<p>To simplify the learning process, i have divided the blog in 3 parts:
<ul>
    <li><span style="line-height: 14px">Usage of @CucumberOptions</span></li>
    <li>How to use it ( code part)</li>
    <li>Different options available</li>
</ul>
<em style="line-height: 1.5em"><span style="text-decoration: underline"><strong>Usage</strong></span></em><span style="line-height: 1.5em">: @CucumberOptions annotation provides the same options as the cucumber jvm command line. for example: we can specify the path to feature files, path to step definitions, if we want to run the execution in dry mode or not etc. </span></p>
<p><span style="line-height: 1.5em">           Basically @CucumberOptions enables us to do all the things that we could have done if we have used cucumber command line. This is very helpful and of utmost importance if we are using IDE such eclipse only to execute our project.</span></p>
<p><em><span style="text-decoration: underline"><strong>How to use</strong></span><strong> it:</strong></em> Create one empty class with the @RunWith(Cucumber.class) annotation.  The options to be used are defined with the @CucumberOptions.</p>
<p>Executing this class as any JUnit test class will run all features found on the classpath in the same package as this class.Name of the class could be anything and most common name for this class is RunCukeTest.java</p>
<p><span style="text-decoration: underline">Note: @Cucumber.Options is deprecated from version 1.1.5</span></p>
<p>lets look at the sample code for the RunCukeTest.java structure:</p>
<p><code>
package com.sample;
import org.junit.runner.RunWith;
import cucumber.api.CucumberOptions;
import cucumber.api.junit.Cucumber;
@RunWith(Cucumber.class)
@CucumberOptions(
//your cucumber options goes here
}
)
public class RunCukeTest {
// This class will be empty
}
</code>
<em><span style="text-decoration: underline"><strong>Different options:</strong></span></em><b>
</b>
<table style="width: 596px" border="0" cellspacing="0" cellpadding="0"><col width="95" /> <col width="420" /> <col width="81" />
<tbody>
<tr>
<td width="95" height="20">Element</td>
<td width="420">Purpose</td>
<td width="81">Default</td>
</tr>
<tr>
<td width="95" height="20">dryRun</td>
<td width="420">true (Skip execution of glue code)</td>
<td width="81">FALSE</td>
</tr>
<tr>
<td width="95" height="20">strict</td>
<td width="420">true (will fail execution if there are undefined or pending steps)</td>
<td width="81">FALSE</td>
</tr>
<tr>
<td width="95" height="20">features</td>
<td width="420">the paths to the feature(s)</td>
<td width="81">{}</td>
</tr>
<tr>
<td width="95" height="20">glue</td>
<td width="420">where to look for glue code (stepdefs and hooks)</td>
<td width="81">{}</td>
</tr>
<tr>
<td width="95" height="20">tags</td>
<td width="420">what tags in the features should be executed</td>
<td width="81">{}</td>
</tr>
<tr>
<td width="95" height="20">monochrome</td>
<td width="420">whether or not to use monochrome output</td>
<td width="81">FALSE</td>
</tr>
<tr>
<td width="95" height="20">format</td>
<td width="420">what formatter(s) to use</td>
<td width="81">{}</td>
</tr>
</tbody>
</table>
Here is the code snippet which shows hows to write these options in code format
<code>
package com.sample;
import org.junit.runner.RunWith;
import cucumber.api.CucumberOptions;
import cucumber.api.junit.Cucumber;
@RunWith(Cucumber.class)
@CucumberOptions(
dryRun = false,
strict = true,
features = "src/test/features/com/sample",
glue = "com.sample",
tags = { "~@wip", "@executeThis" },
monochrome = true,
format = {
"pretty",
"html:target/cucumber",
"json:target_json/cucumber.json",
"junit:taget_junit/cucumber.xml"
}
)
public class RunCukeTest {
}
</code>
Although the table and code above summaries things very clearly but still lets look at each option one by one to understand it better.
<ul>
    <li><span style="line-height: 14px"> <em><span style="text-decoration: underline"><strong>dryRun: </strong></span><strong> </strong></em>if dryRun option is set to true then cucumber only checks if all the steps have their corresponding step definitions defined or not. The code mentioned in the Step definitions is not executed. This is used just to validate if we have defined a step definition for each step or not.</span></li>
    <li><em><span style="text-decoration: underline"><strong>Strict: </strong></span><strong> </strong></em>if strict option is set to false then at execution time if cucumber encounters any undefined/pending steps then cucumber does not fail the execution and undefined steps are skipped and BUILD is <strong>SUCCESSFUL</strong>. This is what the Console output looks like:</li>
</ul>
<a style="line-height: 1.5em" href="http://xebee.xebia.in/wp-content/uploads/2014/07/StrictFalse.png"><img class="alignnone size-medium wp-image-18664" alt="StrictFalse" src="http://xebee.xebia.in/wp-content/uploads/2014/07/StrictFalse-300x168.png" width="300" height="168" /></a></p>
<p>and if Strict option is set to true then at execution time if cucumber encounters any undefined/pending steps then cucumber does fails the execution and undefined steps are marked as fail and BUILD is <strong>FAILURE</strong>. This is what the Console output looks like:</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/07/StrictFalse1.png"><img class="alignnone size-medium wp-image-18665" alt="StrictFalse" src="http://xebee.xebia.in/wp-content/uploads/2014/07/StrictFalse1-300x168.png" width="300" height="168" /></a>
<ul>
    <li><span style="line-height: 14px"><em><span style="text-decoration: underline"><strong>Monochrome:</strong></span><strong> </strong></em>if monochrome option is set to False, then the console output is not as readable as it should be. may be the output images will make this more clear.</span></li>
</ul>
Output When monochrome option is set to false is shown in below screenshot.</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/07/monochromeFalse.png"><img class="alignnone size-medium wp-image-18666" alt="monochromeFalse" src="http://xebee.xebia.in/wp-content/uploads/2014/07/monochromeFalse-300x168.png" width="300" height="168" /></a></p>
<p>Output When monochrome option is set to true is shown in below screenshot.</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/07/monochromeFalse1.png"><img class="alignnone size-medium wp-image-18667" alt="monochromeFalse" src="http://xebee.xebia.in/wp-content/uploads/2014/07/monochromeFalse1-300x168.png" width="300" height="168" /></a>
<ul>
    <li><em><span style="text-decoration: underline"><strong><span style="line-height: 14px">Features: </span></strong></span></em><strong><span style="line-height: 14px"> </span></strong><span style="line-height: 14px">features option is to specify the path to feature files. when cucumber starts execution, Cucumber looks for .feature files at the path/folder mentioned in features option. Whichever files are with .feature extension ( if there are no compilation errors) at the path/folder mentioned in features, are executed. below snapshot will make it clear on what path to define keeping in mind the project structure:</span></li>
</ul>
<a href="http://xebee.xebia.in/wp-content/uploads/2014/07/featuresPath.png"><img class="alignnone size-medium wp-image-18670" alt="featuresPath" src="http://xebee.xebia.in/wp-content/uploads/2014/07/featuresPath-300x168.png" width="300" height="168" /></a>
<ul></p>

## Comments

**[sharathkonda](#9526 "2014-11-20 04:44:24"):** Dear Shankar, Now format has been deprecated, use plugin.

