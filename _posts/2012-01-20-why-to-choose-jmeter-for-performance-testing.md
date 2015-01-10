---
layout: post
header-img: img/default-blog-pic.jpg
author: Priyanka
description: 
post_id: 10906
created: 2012/01/20 22:56:17
created_gmt: 2012/01/20 17:56:17
comment_status: open
---

# Why to choose JMeter for performance testing?

<p>During my last project, I was given a task to choose one of the best tools for performance testing<del cite="mailto:Anubhav%20" datetime="2012-01-05T04:12"> </del> among open source tools available in market.  After evaluation and little research I came up with JMeter. I have thought of writing a small blog for those who are new to performance testing and are confused of which tool to use. It’s pretty common that whenever we start looking out for any tool we keep few things in our mind that it should record and playback, allow easy parametrization, generate detailed reports, should be able to simulate a heavy load on a server, analyze overall performance, etc . JMeter would not disappoint you in that sense because it has more than above mentioned features. There are important features of JMeter which have been listed down in this blog. Let's start from first thing which is its installation. It is very easy to install which requires only Java set up on your machine since it is a Java based tool.</p>
<!--more-->

<p><strong>Installation
</strong></p>
<ol>
<li>Download JMeter  from  link provided below :-</li>
</ol>
<p>http://jmeter.apache.org/download_jmeter.cgi</p>
<ol>
<li>
<p>Unzip it anywhere in your system.</p>
</li>
<li>
<p>Then run jmeter.bat (for Windows) which is kept in its bin folder and start using it.</p>
</li>
</ol>
<p><b>Record and Play<ins cite="mailto:Anubhav%20" datetime="2012-01-05T04:51"> </ins></b></p>
<p>JMeter lets you record your test plan by using its <b>proxy server.</b> Besides recording, you can also exclude non required sampler requests by giving  required regular expressions in its proxy server.</p>
<p><b>Parametrization</b></p>
<p>You can also easily parametrize your test cases by using user-parameter pre-processors or config-elements. <ins cite="mailto:Anubhav%20" datetime="2012-01-05T04:51"></ins></p>
<p><b>Passing data from one Request to other</b></p>
<p>Passing response data from one request to another can be easily done using its powerful post processors like XPath Extractors and Regular Expression<ins cite="mailto:Anubhav%20" datetime="2012-01-05T04:52"> </ins>Extractor.</p>
<p><b>Allow working with multiple types of Requests</b></p>
<p><b> </b></p>
<p>JMeter does not only work with simple HTTP<span style="text-decoration: underline;"> </span>requests but it also works with other requests which includes FTP Request, JDBC Request , LDAP Request , SOAP/XML-RPC Request, etc</p>
<p><b>Allow Customization of test case flow with its Logic Controllers</b></p>
<p>Logic Controllers can be consumed to customize logic to decide when to send requests to cover different scenarios we come across during performance testing. They can modify the requests themselves; cause JMeter to repeat requests, etc. JMeter provides different types of controllers like once-only-controller, interleave controller, etc which have their own functioning and you can decide test plan logic with the help of these controllers. For example, Loop controllers can be used to send request repeatedly many times to put stress on server.</p>
<p><b> </b></p>
<p><b>Remote Testing/Distributed Testing</b></p>
<p><b> </b></p>
<p>It is the most powerful feature of JMeter. JMeter client machine is not able to simulate enough users to stress your server so there is one option exists which is to control multiple remote JMeter engines from a single JMeter GUI client. By running JMeter remotely, you can replicate a test across many low-end computers and thus simulate a larger load on the server. One instance of the JMeter GUI client can control any number of remote JMeter instances, and collect all the data from them. This offers the following features:
<ul>
    <li>Saving      of test samples to the local machine</li>
    <li>Management      of multiple JMeter Engines from a single machine</li>
    <li>No      need to copy the test plan to each server - the client sends it to all the      servers</li>
</ul>
<b>Generate Reports</b></p>
<p>Unless you can show effectively, the tests and results you achieve is of little use to others. Tool should be able to collect information, organize it, and present it in form of reports. JMeter has capability to produce nice reports with help of its listeners is a component that shows the results of the samples.</p>
<p><b> </b></p>
<p><b>Allow addition of  Think Time </b></p>
<p>We know that Think time is a time taken by end user of an application between navigation of application pages/screens. It also includes the time taken by end user to fill in any application page. To make test cases realistic it is good practice to include think time in between transaction steps. We can add think time in test cases with the help of different types of timer options available in JMeter for example:- constant timer, random uniform timer which can be added in test cases as per requirement.</p>
<p><b>Contains important browser features like Cache Manager, etc</b></p>
<p>JMeter is not a browser but it simulates like a browser however it does not perform all the actions supported by browsers but it provides some of important browser features like HTTP cache manager, HTTP cookie manager, HTTP header manager. For example: by adding HTTP cookie manager, it ensures that all HTTP requests will share the same cookies.</p>
<p><b>Allow Assertions to be added in each Request</b></p>
<p>Assertions allow the ability to assert facts about responses received from the server being tested. Using an assertion, you can essentially "test" that your application is returning the results you expect it to. Assertions can be added to any controller in a test plan (simply right-click on the controller and add an assertion). As always, these assertions will be attached to any test samples that "pass through" that controller.</p>
<p><b>Provides access to Functions and Bean- shell scripting </b></p>
<p>JMeter has some useful build in functions<ins cite="mailto:Anubhav%20" datetime="2012-01-05T04:59">,</ins> which can compute new values at run-time based on previous response data and Bean Shell function evaluates the script passed to it, and returns the result.</p>
<p>Components used in JMeter seem like a lot of jargon initially but eventually you will get used to these elements and then testing will be a piece of cake .The image below gives the brief description of components used in JMeter.</p>

## Comments

**[Alon Girmonsky](#7064 "2012-01-21 23:42:12"):** Hi Priyanka. Excellent post! I really enjoyed reading it. It provides a great overal description of JMeter. I'm a great fan of JMeter myself, but I believe that any newcomer after reading your post will opt to use JMeter. I also liked your previous post about regex vs xpath. Last thing, the link you provided to download JMeter provides only old versions of Jmeter. The new version 2.5.1 is superior and provides awesome functionality. Best, Alon http://blazemeter.com

**[ram](#8411 "2012-04-10 18:12:33"):** and one more thing is please suggest me a best site or material to get update on the Jmeter. Thanks, Ram

**[Steve Schols](#7098 "2012-01-24 13:25:28"):** Indeed, as Alon said, you already have newer versions of JMeter. You should always check http://jmeter.apache.org/ to get the newest version and a lot of extra documentation about JMeter. It was a very nice post Priyanka. You gave a very good résumé about it's features and functionality.

**[Priyanka](#7105 "2012-01-24 15:48:59"):** Hi Alon, Thanks for reading my blogs. I have changed Jmeter URL version mentioned in my blog. The new version of Jmeter seems to really amazing . I will try its new version also. Regards, Priyanka

**[Priyanka](#7106 "2012-01-24 15:49:53"):** Thanks Steve :)

**[Viru](#8844 "2012-05-22 12:12:24"):** Hey Priyanka... I am having issue integrating Jmeter & Maven?? Do you have any steps which can help me with it... Thanks,

**[Madhu](#7869 "2012-03-07 11:17:15"):** Hi Priyanka, Can we use JMeter for Report Performance Testing

**[Priyanka](#7941 "2012-03-16 15:42:59"):** Yes Madhu we can use JMeter for repoting. Jmeter has provided different types of listeners for it.

**[Priyanka](#8412 "2012-04-10 18:16:57"):** Hi Ram, you can read one more blog http://xebee.xebia.in/2011/12/06/advantage-of-using-regular-expression-extracter-postprocessor-over-xpath-exptractor-in-jmeter/ which is one example of one of its functionality. I will def. post one more blog with its functionality. Thanks, Priyanka

**[ram](#8410 "2012-04-10 18:10:38"):** Hi Priyanka, Thanks a lot for you infor in the blog . You are saying what are the features available in Jmeter but I would like to know the Jmeter functionality with an example. Thanks In Adavance. Thanks, Ram

**[Rahul Bhalla](#7504 "2012-02-09 11:20:58"):** Thanks for the info Priyanka. This blog is really Helpful and Nice. Regards Rahul Bhalla

**[Priyanka](#7505 "2012-02-09 11:30:40"):** thanks Rahul :)

**[subhavani](#9176 "2012-07-18 13:00:36"):** Nice article. Does JMeter (2.7 or earlier) support recording RPC requests?

