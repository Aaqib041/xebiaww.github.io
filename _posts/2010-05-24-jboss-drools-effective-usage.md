---
layout: post
header-img: img/default-blog-pic.jpg
author: pranjan
description: 
post_id: 3587
created: 2010/05/24 10:33:43
created_gmt: 2010/05/24 05:33:43
comment_status: open
---

# JBoss Drools - Effective Usage

I have been using JBoss Drools in my project for a while. In the beginning we were not confident about how it will fit in the project. There were also doubts about the performance and maintainability of Drools Stuff.

So, to start with, we decided to implemented a small functionality using Drools. A big list of objects was passed to drools and some functionality was implemented with Drools. This resulted in setting up the infrastructure for Drools framework along with measuring Drools performance with a big set of data.

The best thing noticed was the readability of the logic. Had the same code been written in java, no matter how readable it would have been , the java code could never surpass the crispness and simplicity of the Drools code.

Reason being, Drools has certain features which saves some amount of java code to be written . Suppose you use 5 such features and then guess the amount of java code you just saved. This in turn increased the readability of the code.

The readability of the Drools code helped in transforming the business logic whenever needed. This tempted to add more business logic in Drools.

Finally we got two layers of of Drools where we insert a large amount of objects in the Drools session, and perform a set of business rules there. The order of business rules can be controlled in Drools which give a solid control over rules. There had been times where we have just reversed the rule order. Doing this in java would not had been so easy. In Drools it was just changing some numbers.

And I must confess that I am really impressed with the efficiency with which Drools uses memory. I think Drools don't replicate objects, rather it just creates handles to objects in java heap which results if efficient memory usage.

The performance was further improved by using some clever business logic. The rules which filtered out useful object were executed first which removed the useless objects after filtering. This significantly reduces the number of objects which results in lessening the number of executions of the rules to be fired, thus improving the efficiency.

Try to use the no loop functionality of drools, if the business logic allows that, this prevents in re executing same rule on same objects. A rule without no loop can easily cause Out Of Memory or can slow down system performance easily in some conditions.

Another performance improvement can be done by using the insert functionality of Drools which inserts new Objects in Drools session from rules. So, if few objects can be combined into one then insert the new object and retract (remove) the other objects from Drools session.

To increase the readability of the Drools code, try to combine 4-5 lines of Drools code in a single method in java and call that method from Drools rule. Drools gives the functionality to call java methods from Drools rules which helps a lot.

At the end of it, we have lots of business logic segregated in two layers which are changed very frequently, and every time we change it , we thank Drools. This has resulted in increasing the productivity of the team as well as decreasing the pain which would have been caused by altering lots of java code frequently to suit to business logic.