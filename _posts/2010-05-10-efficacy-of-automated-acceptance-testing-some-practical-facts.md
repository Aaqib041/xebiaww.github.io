---
layout: post
header-img: img/default-blog-pic.jpg
author: vashishtha
description: 
post_id: 3509
created: 2010/05/10 11:51:16
created_gmt: 2010/05/10 06:51:16
comment_status: open
---

# Efficacy of Automated Acceptance Testing: Some Practical Facts

These days there is good amount of debate on the efficacy of the automated acceptance tests. Many have started believing that current way of acceptance testing is a waste as it doesn't provide you the relevant ROI as you'd expect from it. People have provided some [alternatives][1] also, but to me they look very subjective where you'll always prompt with - "it depends". In this post, I am not going to talk about yet another alternative of automated acceptance testing, but rather will put some practical facts based on our project experience that worked for us. 

First to answer whether you need automated acceptance/integration tests, I would say you definitely need them. In many of our Sprint retrospectives it came out time and again that because of some commits, happy flow of the application also stopped working and testers resented in wasting a lot of time on these kind of issues. You'd ask why we have't adopted any automated way of acceptance testing. We did but it seemed a lot of waste of time in running acceptance test suite for a small commit for instance. Depending on the complexity of the project, automated test suite may take upto 20 min or more and running it for every commit is not a good investment of time. So the problem is not running automated suits but the time it takes using current testing frameworks (Selenium and FitNesse with Maven2). 

In a discussion, I figured out that it's possible to run your [test suite in multiple threads][2] and also now it's possible to run parallel builds with multiple threads using [new Maven 3 feature][3]. I tried it and it turned out that these features make build a lot faster but still my acceptance test suite execution time was not in expected time range. This option would have worked wonders for the applications with relatively low complexity which was the basis of my experimentation (able to run 150 Selenium tests in 3-4 min). However it may not work that well in applications with relatively complex business flow. In this case your one individual test case with complex permutation combinations can be culprit. So running acceptance test suite in parallel environment on my local machine still didn't work. Also in retrospectives repeatedly it turned out that it's definitely not a good idea to show door to acceptance tests altogether as manual regression is even more expensive. So what to do?

We decided on a middle strategy. At the end of any commit, with all necessary unit and integration tests, happy flow of web flow should not break. People should not get stuck just because after your commit, their flow stopped working altogether. It means a limited set of acceptance tests (called smoke tests) which make sure that application in general doesn't break. Identifying these tests in a Maven profile and running them with every commit doesn't take that much time compared to running entire suite. In these tests, you may not be necessarily validating the data in the screen but the focus is more on validating flow and making sure a normal flow works. It's not ideal but it works. Also remember, we are not saying bye-bye to other remaining tests in acceptance test suite. They still execute but less frequently, for instance twice a day on the build server so that after some cumulative commits if something breaks, development team could fix them.

   [1]: http://jamesshore.com/Blog/Alternatives-to-Acceptance-Testing.html
   [2]: http://incodewetrustinc.blogspot.com/2010/01/run-your-junit-tests-concurrently-with.html
   [3]: https://cwiki.apache.org/confluence/display/MAVEN/Parallel+builds+in+Maven+3