---
layout: post
header-img: img/default-blog-pic.jpg
author: pranjan
description: 
post_id: 3393
created: 2010/04/30 09:10:16
created_gmt: 2010/04/30 04:10:16
comment_status: open
---

# Junit tests and Abstraction go hand in hand

<p>I have been writing JUnit tests for a while. Sometimes I feel proud of my JUnit tests as they really look meaningful. However, sometimes the JUnit tests look so ugly and pointless. I relate this difference to the Abstraction of the code.</p>
<p>Lets talk about abstraction first. Abstraction is generally governed by “separation of concerns”. Each unit of code should have a separate responsibility. These components are later integrated to perform certain functionality. So, if these separate components are tested separately, then its tested that each component is working perfectly.</p>
<p>Don't confuse abstract with abstract keyword in java.</p>
<p>If a class is truly Abstract, then it handles only one responsibility. The sole purpose of the class is to perform a function which can not be broken down further into more non-trivial functions. In a JUnit test class for a truly abstract component (class), the test cases will be dedicated to test a fine grained module. So, it will be easier to concentrate on the functionality and more test scenarios cases can be thought upon.</p>
<!--more-->

<p>This in turn increases the meaningfulness of the test code. The test case doesn’t need to bother about behavior of the dependencies. This will automatically decrease the time taken to write JUnit test cases.</p>
<p>The number of test cases per test class will also decrease a lot as the JUnit test class will be dedicated to a single reason only. This will make the JUnit test classes lot more easy to manage.</p>
<p>Adding new functionality always results in test failures and its always a pain to fix lots of test cases. Abstraction will reduce the number of failed test cases for a change in code. It will also be easier to fix the failing test cases if the test cases are doing only one thing.</p>
<p>In a class doing multiple things at a time ( a non abstract class), the number of test cases will be lot more than actually needed. Most of the test cases will test more than a single condition. The number of test cases will shoot up due to this, and the meaningfulness of the test cases will be lost. So, you end up writing duplicate test cases to handle scenarios influenced by dependencies.</p>
<p>This works best in Agile projects where functionality is added iteration wise and code changes rapidly.</p>
<p>So, we can say that, abstraction in class brings up abstraction in JUnit test code.</p>