---
layout: post
header-img: img/default-blog-pic.jpg
author: ngaur
description: 
post_id: 12758
created: 2012/03/22 22:23:44
created_gmt: 2012/03/22 17:23:44
comment_status: open
---

# Few words on automation framework design strategies.

<p lang="en" class="alignleft"><span style="font-family: 'andale mono', times; font-size: medium;">Automation has always been an attractive domain as the very fact that the test automation reduces repetetive testing effort and speed up the regression of application functionality. So today every test team want automation and has it.<!--more--></span></p>

<p lang="en" class="alignleft"><span style="font-family: 'andale mono', times; font-size: medium;"> </span></p>

<p lang="en"><span style="font-family: 'andale mono', times; font-size: medium;">However, test automation can go expensive if it is not managed properly and may lead to the situation that can ruin the testing strategies and targets. So here I am going to elaborate this key point.</span></p>

<p lang="en"><span style="font-family: 'andale mono', times; font-size: medium;">Usually test automation is managed and used in productive way through well organised framework , typically the strategy that usually followed is called "Scriptless Automation". </span><span style="font-family: 'andale mono', times; font-size: medium;">Framework design ease the test automation that of course doesn't depends on the capability of testing tool choosen rather it aims to remove the script writing  that usually require tool specific skills.</span></p>

<p lang="en"><span style="font-family: 'andale mono', times; font-size: medium;">Lets discuss in short the needs of going scriptless :</span></p>

<p><span style="font-family: 'andale mono', times; font-size: medium;"><b>a). Time-to-automate </b></span><span style="font-size: x-small;"> </span>
<p lang="en"><span style="font-family: 'andale mono', times; font-size: medium;">Faster the automation faster the application going Live.But, usually automation require typical scripting and coding skills that is too expensive in terms of time. This time slag can be very dangerous in SDLC.</span></p>
<p lang="en"><span style="font-family: 'andale mono', times; font-size: medium;"><b>b). Maintenance</b></span></p>
<p lang="en"><span style="font-family: 'andale mono', times; font-size: medium;">Maintaining the automation process is the biggest challange i guess.With the new releases ,team need to maintain the automation process and this becomes the problem when script automation is into the process. As your test suite increases, you will face an excessive maintenance burden - which will lead to loss of more time and money.</span></p>
<p lang="en"><span style="font-family: 'andale mono', times; font-size: medium;"><b>c). Tool specific skills required</b>: </span></p>
<p lang="en"><span style="font-family: 'andale mono', times; font-size: medium;">Test automation require scripting and for that tool specific knowledge is required. So here it require good programmers.</span></p>
<p align="JUSTIFY" lang="en"><span style="font-family: 'andale mono', times; font-size: medium;"><span><b>Automation Design Strategy:</b></span></span></p>
<p align="JUSTIFY" lang="en"><span style="font-family: 'andale mono', times; font-size: medium;"><span>Basic and widely used strategy for framework development is the combination of Data Driven Design and Keyword driven design typically named as Hybrid. Hybrid design basically aims to use the positive feature from all approches. </span></span></p>
<p lang="en"><span style="font-family: 'andale mono', times; font-size: medium;"><span>Keyword Driven framework requires the development of data storage(like data-tables etc) and keywords for defining the actions, this is entirely independent of the  automation tool used to execute or perform keywords on target application and the test script code that "drives" the application-under-test is written accordingly for that particular keyword. Keyword-driven tests look very similar to manual test cases. In a keyword-driven test, the functionality of the application-under-test is documented in a table as well as in step-by-step instructions for each test. In this method, the entire process is data-driven, including functionality.</span></span><span style="font-family: 'andale mono', times; font-size: medium;">﻿</span></p>
<p lang="en"><span style="font-family: 'andale mono', times; font-size: medium;"><span>Data-Driven design reads the inputs and outputs fron the data files, nothing is statically assign in script.So this approach helps automation with multiple sets of data.But it has its limitations that vary according to the requirement of users.</span></span></p>
<p lang="en"><span style="font-family: 'andale mono', times; font-size: medium;"><a href="http://xebee.xebia.in/2012/03/22/few-words-on-automation-framework-design-strategies/hybrid2/" rel="attachment wp-att-12759"><img src="http://xebee.xebia.in/wp-content/uploads/2012/03/hybrid2-300x250.jpg" width="300" height="250" class="aligncenter size-medium wp-image-12759" /></a>
</span></p>
<p align="JUSTIFY"><span style="font-family: 'andale mono', times; font-size: medium;"><span>Hybrid design basically aims to utilise the positive feature from all approches. As hybrid support both data-driven and keyword-driven approach, it smoothly handle the cases where it is difficult or you can say not feasible to re-implement a new keyword for that particular scenario.And that too can be changed later on to keyword-driven approach.Hybrid framework mainly constitute of Data-driven engine,application independent functions,Utility libraries. The data store that engine would be using devides into three parts. First a repository map that basically containing the logical names of application objects, then comes the data tables that would be used in both data-driven and keyword driven approach and ofcourse the data tables that contains the flow, these may be managed at different levels according to the requirements.</span></span></p>
<p lang="en"><span style="font-family: 'andale mono', times; font-size: medium;"><span>These design mainly provide a way of going scriptless , through which automation process could be optimized and managed.This one time effort in developing such design could save a lot of time to automate applications, and the tool specific skills that normally require in automation is no more headache and above all cost of maintaining the automation process reduces tremendously.</span></span></p>
<p lang="en"><span style="font-family: 'andale mono', times; font-size: medium;">
</span></p>
<p lang="en"><span style="font-family: 'andale mono', times; font-size: medium;">
</span></p></p>

## Comments

**[Neeraj Pandey](#8084 "2012-03-28 14:47:23"):** Thanks for providing such a useful information

