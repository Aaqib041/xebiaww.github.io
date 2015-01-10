---
layout: post
header-img: img/default-blog-pic.jpg
author: pranjan
description: 
post_id: 3836
created: 2010/06/09 09:41:53
created_gmt: 2010/06/09 04:41:53
comment_status: open
---

# Architecture Terminologies - An Introduction

<p>At times I was involved in discussions where people of top brass where also present. I was having (and still have) little experience compared to them. I used to hear lots of terms which clearly and smoothly surpassed my head. I was at times amazed by listening about friction, surface area and all in software design talks.</p>
<p>I started preparing for architect certification and came across a book which said that there are real terms like these. Each term define a certain property of software design. These are known as architecture terminologies. I'll explain few of them here.</p>
<p><strong>Abstraction</strong></p>
<p>Abstraction is the process of creating unique entities which can be used in a repetitive way. Generally abstraction is used in discussions where boxes are made with lines connecting them. Each box is a entity which symbolizes something.
<!--more-->
The breakdown of a software application in boxes is done from the point of view of the person who creates the architecture. The abstraction depends on the understanding and thinking of the architect about the software application.</p>
<p><strong>Surface Area</strong></p>
<p>Surface area is the way in which different components interact with each other. More the surface area, more will be the way in which changes in one component will affect the changes in other component.</p>
<p><strong>Brittleness</strong></p>
<p>Brittleness is the impact of change of code in one part of application on other part of the application.</p>
<p><strong>Boundaries</strong></p>
<p>Boundaries are the areas where two different components interact.</p>
<p><strong>Friction</strong></p>
<p>Friction is the amount of interaction which occurs between two components. This determines the effect of one component due to changes in another component.</p>
<p><strong>Capabilities</strong></p>
<p>Capabilities are system properties like scalability, manageability, performance, availability , security etc. The capabilities are described below.</p>
<p><em><strong>Availability</strong></em></p>
<p>This the total amount of time in which the system is available. If the system is available for 24*7 then its called total availability.</p>
<p><em><strong>Reliability</strong></em></p>
<p>This is the measure of the consistency of the application. This is the measure of the ability of the application to perform the required function for the specified amount of time under stated conditions.</p>
<p><em><strong>Manageability</strong></em></p>
<p>Manageability is the set of services which say whether a application can be continuously integrated, and remain correct. It also considers server management.</p>
<p><em><strong>Flexibility</strong></em></p>
<p>The ability to address architectural and hardware configuration changes without a great deal of impact to the underlying system. Flexibility is the ability of the application to cope with changes.</p>
<p><em><strong>Performance</strong></em></p>
<p>The ability to carry out functionality in a time frame that meets specified goals. The response time of the application dictates its performance. The key is to minimize expensive calls. A target response time is set and the application tries to meet that.</p>
<p><em><strong>Capacity</strong></em></p>
<p>Capacity is the measure of the number of executions that a application can handle. This measurement helps in scaling up the application in case of increase in usage.</p>
<p><em><strong>Scalability</strong></em></p>
<p>The ability to support the required availability and performance as transactional load increases. There are two types of scalabilities:
a) Horizontal scalability : It can be increased by increasing the memory and CPU.
b) Vertical scalability : Its increased by increasing the number of servers.</p>
<p><em><strong>Extensibility</strong></em></p>
<p>It is the careful modeling of the system for continued addition of functionality with ease.</p>
<p><strong>Validity</strong></p>
<p>Validity, or testability, is the ability to determine what the expected results should be.</p>
<p><em><strong>Re usability</strong></em></p>
<p>The ability to use a component in more than one context without changing its internals.</p>
<p><em><strong>Security</strong></em></p>
<p>The ability to ensure that information is not accessed and modified unless done so in accordance with the enterprise policy.</p>