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

<p><span style="font-family: 'Times New Roman', serif;font-size: small"><b> </b></span><span style="font-size: small;font-family: 'times new roman', times">
<span style="font-size: medium"> In today's fast paced world, the task of manual testing where huge amounts of data is involved looks very cumbersome. </span><span style="font-size: medium">Time is like the wind, it lifts the light and leaves the heavy. We have so little time to get all the things done which we want to do in our busy lives. This really comes into picture when it comes to running your own online business or site.</span></span></p>
<p><span style="font-size: medium;font-family: 'times new roman', times">Many, perchance most, software applications today are written as web-based applications to be run in an Internet browser. The efficacy of testing these applications varies broadly amid-st companies and organizations. In an era of highly interactive and susceptible software processes where many organizations are using some form of Agile methodology, test automation is frequently becoming a exigency for software projects. Test automation is oftentimes the answer. Test automation means using a software tool to run repeatable tests against the application to be tested. </span></p>
<p><span style="font-size: medium;font-family: 'times new roman', times">Recently I had started working on this “prodigious”tool called “<strong>Selenium</strong>”in my project(<a href="http://www.salesforce.com">SFDC</a>).</span></p>
<p><span style="font-size: medium;font-family: 'times new roman', times"><!--more--></span></p>
<p><span style="font-size: medium;font-family: 'times new roman', times">Selenium, a demotic and well established testing framework is a wonderful tool that provides a convenient consolidated  interface that works with a large number of browsers, and allows you to write your tests in well-nigh every language one can imagine .</span></p>
<p><span style="font-size: medium;font-family: 'times new roman', times">It was one of the first Open Source projects to fetch browser-based testing to the populace, and because it's written in JavaScript it's possible to quickly add support for new browsers that might be released.</span></p>
<p><span style="font-size: medium;font-family: 'times new roman', times">Selenium is written in JavaScript which causes a substantial infirmity.Browsers impose a pretty strict security model on any JavaScript that they execute in order to protect a user from malignant scripts. </span></p>
<p><span style="font-size: medium;font-family: 'times new roman', times">Selenium was originally developed by Jason Huggins, who was later joined by other programmers and testers at ThoughtWorks.  But being a smart guy, he realized there were better uses of his time than manually stepping through the same tests with every change he made. </span></p>
<p><span style="font-size: medium;font-family: 'times new roman', times"><b>Selenium</b> is a portable software testing framework for web applications. Selenium provides a record/playback tool for authoring tests without erudition a test scripting language (<b>Selenium IDE</b>). It also provides a test domain-specific language (<b>Selenese</b>) .It is open-source software, released under the Apache 2.0 license and can be downloaded and used free of cost.</span></p>
<p><span style="font-size: medium;font-family: 'times new roman', times">Selenium has multiple software tools. Each has a specific role :</span></p>
<p><span style="font-size: medium;font-family: 'times new roman', times"><strong>Selenium IDE</strong>(Integrated Development Environment ) is a prototyping tool for building test scripts.It is easy to use and records user actions.But the drawback of it is, it can only be used as a Firefox plug-in.So to overcome this drawback Selenium1 (or <strong>Selenium RC</strong>)came up with many new features. It explains how to develop an automated test program using the Selenium RC API. Selenium RC components are:</span>
<ul>
    <li><span style="font-size: medium;font-family: 'times new roman', times">The   Selenium Server which launches and kills browsers, interprets and   runs the Selenese commands passed from the test program, and acts as    an <em>HTTP     proxy</em>,     intercepting and verifying <em>HTTP</em> messages passed between the browser    and the AUT. </span></li>
    <li><span style="font-size: medium;font-family: 'times new roman', times"><em>Client    libraries</em> which provide the interface between each programming     language and the <em>Selenium RC Server</em>. </span></li>
</ul>
<span style="font-size: medium;font-family: 'times new roman', times">You may, or may not, need the Selenium Server, depending on how you propound to use Selenium. </span><span style="color: #20435c;font-size: medium;font-family: 'times new roman', times"><b>Selenium Server </b></span><span style="font-size: medium;font-family: 'times new roman', times"> receives Selenium commands from your test program, interprets them, and reports back </span><span style="font-size: medium;font-family: 'times new roman', times">to your program the results of running those tests. </span><span style="font-family: 'times new roman', times;font-size: medium">If you will be strictly using the WebDriver API you do not perforce need the Selenium Server. Selenium-WebDriver makes direct calls to the browser using each browser’s native support for automation. How these direct calls are made, depends on the browser you are using.</span></p>
<p><span style="font-size: medium;font-family: 'times new roman', times"><b>Why migrate to WebDriver?</b></span></p>
<p><span style="font-size: medium;font-family: 'times new roman', times">WebDriver has Smaller and compact API. WebDriver’s API is more Object Oriented than the original Selenium RC API. This can make it easier to work with. </span>
<ul>
    <li><span style="font-size: medium;font-family: 'times new roman', times">Better    emulation of user interactions. Where-in possible, WebDriver makes use  of inherent events in order to interlude with a web page. This more     closely mimics the way that the users work with the site and apps.  In addition, WebDriver offers the progressive user interactions APIs    which allow you to model complicated interactions with the site. </span></li>
</ul>
<ul>
    <li><span style="font-size: medium;font-family: 'times new roman', times">Support by    browser vendors. Opera, Mozilla and Google are all active   participants in WebDriver’s development, and each have engineers    working to improve the framework. </span></li>
</ul>
<span style="font-size: medium;font-family: 'times new roman', times">The first step when starting the migration is to change how you obtain your instance of Selenium. When using Selenium RC, this is done like so: </span>
<blockquote>
<pre><span style="font-size: medium;font-family: 'times new roman', times">Selenium selenium = new DefaultSelenium("localhost",4444,"*firefox","<a href="http://www.yoursite.com/">http://www.yoursite.com</a>");
selenium.start();</span></pre>
</blockquote>
<span style="font-size: medium;font-family: 'times new roman', times">One can also modify and use the Webdriver backed with the Selenium that is using WebDriver throughout and instantiating a Selenium instance on demand ,using this command:</span>
<blockquote>
<pre><span style="font-size: medium;font-family: 'times new roman', times"><strong>Se</strong><b>lenium selenium = new WebDriverBackedSelenium(“driver”, “baseUrl”);</b></span></pre>
<span style="font-size: medium;font-family: 'times new roman', times">A typical example using WebDriver in Java looks like this:</span></p>
<p><span style="font-size: medium;font-family: 'times new roman', times">// Create an instance of WebDriver backed by Firefox</span>
<span style="font-size: medium;font-family: 'times new roman', times"> <b>WebDriver driver = new FirefoxDriver();</b></span></p>
<p><span style="font-size: medium;font-family: 'times new roman', times">// Now go to the Google home page</span>
<span style="font-size: medium;font-family: 'times new roman', times"> <b>driver.get("http://www.google.com");</b></span></p>

## Comments

**[Cheshta](#8155 "2012-03-31 08:42:56"):** Commendable effort!

**[Rashmi](#8114 "2012-03-29 12:38:50"):** Its an awesome blog, i really came to know a new things about automation testing selenium tool .This one is really an informative blog for brushing up our skills on selenium testing tool.

**[parina](#8115 "2012-03-29 14:11:50"):** Very Informative :)

**[Vanshaj](#8125 "2012-03-29 23:37:43"):** Appreciated !! :)

