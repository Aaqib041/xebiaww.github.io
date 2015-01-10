---
layout: post
header-img: img/default-blog-pic.jpg
author: rahulagrawal
description: 
post_id: 2734
created: 2009/12/14 12:26:32
created_gmt: 2009/12/14 07:26:32
comment_status: open
---

# Migration Strategies

<p>After we did the migration of a sample application, jmsconsole  to JBoss, I was thinking about the strategy for such migrations. Some of my thoughts on how to attack the migration of applications.</p>
<p><ins></ins>
<h4><ins>Deploy Test and Solve</ins></h4>
<ins></ins></p>
<p>This kind of approach can be used in simple, well coded, or in almost migrated applications (J2EE complaint). It consists of deploying the application, testing it , finding errors, going back to the application, fixing the problems, and deploying the application again. It is can be a good way to start for some one doing the migration for the first time.</p>
<!--more-->

<p><ins></ins>
<h4><ins>Test Driven Approach</ins></h4>
<ins></ins></p>
<p>It is not hard to guess that we need to test the migrated application as quickly as possible to unearth the non obvious issues and to validate the changes done. Hence the order of migrating the modules could be based upon the order of testability. The aim is to migrate the application in incremental steps that can be tested. For example, to start with , the deployment unit , ear/war may just have the migrated module and not the whole application along with a few test classes. As the migration of the various modules progresses, the deployment unit keeps growing.</p>
<p>One order in which we can start the migration could be the following:
<h5>Utility classes:</h5>
Since these classes generally don’t use much of the application server specific code, it should be easy to migrate and test them first.
<h5>Integration points:</h5>
Persistence layer, messaging layer, integration with other products, etc.</p>
<p>The various integration points need to be clearly identified in the assessment phase of the migration. If the existing design is loosely coupled then testing these layers independently is easy. And if we have a spaghetti code then it gives you an opportunity to re factor it, so that the new design can be layered and testable. This can often pave the path for further migrations. For example, if an application uses some proprietary rule engine , then with the above approach one could attempt to think of moving to another rule engine, if the business needs demand so.</p>
<p>To achieve the above , one may need to write few test programs. These test programs would not be bothered about the business logic. They would focus on the basic interactions of the layers with the J2EE container, and the interaction among the layers if that is container dependent.
<h5>Web layer:</h5>
Utility classes are ready, the business layer is ready, the last piece left is the presentation layer.</p>
<p>Conclusion:
The second approach forces us to figure out the container dependencies in a more structured manner. And if the container dependencies are scattered all along the code, then that is exposed early and it helps to prepare yourself for the same.</p>