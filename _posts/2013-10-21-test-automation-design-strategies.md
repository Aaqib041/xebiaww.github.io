---
layout: post
header-img: img/default-blog-pic.jpg
author: ngaur
description: 
post_id: 17516
created: 2013/10/21 15:37:07
created_gmt: 2013/10/21 10:37:07
comment_status: open
---

# Test Automation Design Strategies.

Automation has always been an attractive domain as the very fact that the test automation reduces repetitive testing effort and speed up the regression of application functionality. So today every test team want automation and has it.![][1]

However, test automation can go expensive if it is not managed properly and may lead to the situation that can ruin the testing strategies and targets. So here I am going to elaborate this key point.

Usually test automation is managed and used in productive way through well organised framework , typically the strategy that usually followed is called "Script less Automation".Framework design ease the test automation that of course doesn't depends on the capability of testing tool chosen rather it aims to remove the script writing that usually require tool specific skills.

Lets discuss in short the needs of going script less :

a). **Time-to-automate :**

Faster the automation faster the application going Live.But, usually automation require typical scripting and coding skills that is too expensive in terms of time. This time slag can be very dangerous in SDLC.

b). **M****aintenance:**

Maintaining the automation process is the biggest challenge I guess.With the new releases ,team need to maintain the automation process and this becomes the problem when script automation is into the process. As your test suite increases, you will face an excessive maintenance burden - which will lead to loss of more time and money.

c). **Tool specific skills required:**

Test automation require scripting and for that tool specific knowledge is required. So here it require good programmers.

**Automation Design Strategy:**

Basic and widely used strategy for framework development is the combination of Data Driven Design and Keyword driven design typically named as Hybrid. Hybrid design basically aims to use the positive feature from all approches.

Keyword Driven framework requires the development of data storage(like data-tables etc) and keywords for defining the actions, this is entirely independent of the automation tool used to execute or perform keywords on target application and the test script code that "drives" the application-under-test is written accordingly for that particular keyword. Keyword-driven tests look very similar to manual test cases. In a keyword-driven test, the functionality of the application-under-test is documented in a table as well as in step-by-step instructions for each test. In this method, the entire process is data-driven, including functionality.

Data-Driven design reads the inputs and outputs fron the data files, nothing is statically assign in script.So this approach helps automation with multiple sets of data.But it has its limitations that vary according to the requirement of users.

![hybrid2-300x250][2]

Hybrid design basically aims to utilise the positive feature from all approaches. As hybrid support both data-driven and keyword-driven approach, it smoothly handle the cases where it is difficult or you can say not feasible to re-implement a new keyword for that particular scenario.And that too can be changed later on to keyword-driven approach.Hybrid framework mainly constitute of Data-driven engine,application independent functions,Utility libraries. The data store that engine would be using devides into three parts. First a repository map that basically containing the logical names of application objects, then comes the data tables that would be used in both data-driven and keyword driven approach and ofcourse the data tables that contains the flow, these may be managed at different levels according to the requirements.

These design mainly provide a way of going scriptless , through which automation process could be optimized and managed.This one time effort in developing such design could save a lot of time to automate applications, and the tool specific skills that normally require in automation is no more headache and above all cost of maintaining the automation process reduces tremendously.

To  know more about Xebia's offereing on Agile testing, click here ([http://www.xebia.in/application-testing.html][3])

   [1]: http://xebee.xebia.in/wp-includes/js/tinymce/plugins/wordpress/img/trans.gif (More...)
   [2]: http://pinksolutionlayer.org/blog/wp-content/uploads/2012/12/hybrid2-300x250.jpg
   [3]: https://owa.exchange-login.net/owa/redir.aspx?C=D4mKoJyavkKWbnXjvILVWtIFWDsIqNAIOGfuu5Ys-dcF3tyrwXWx1LyZeq4R7qylZzA-minPQVQ.&URL=http%3a%2f%2fwww.xebia.in%2fapplication-testing.html

## Comments

**[Gaurav Bansal](#9452 "2013-10-25 08:59:03"):** Very informative. Data Driven, Keyword Driven and Hybrid Frameworks are very crisply defined. Scriptless automation that leverages hybrid f/w benefits is going to be choice of automation frameworks where we want manual testers to also contribute into adding more and more test cases to automated suites as they have quite good functional expertise on the different features. Moreover I also see this as right platform where even other functional experts like BA, Product Owner or any other important stakeholder can create their own test cases and see running those. Scriptless automation with proper blending with BDD concepts can turn out into a state of the art testing tool itself. Thanks for sharing the excellent info. Keep up the good work!! ~Gaurav

