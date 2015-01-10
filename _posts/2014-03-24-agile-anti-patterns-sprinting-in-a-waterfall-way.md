---
layout: post
header-img: img/default-blog-pic.jpg
author: Savita Pahuja
description: 
post_id: 18180
created: 2014/03/24 20:15:25
created_gmt: 2014/03/24 15:15:25
comment_status: open
---

# Agile anti-patterns – Sprinting in a waterfall way

Practicing Scrum doesn’t include only implementation of scrum ceremonies. Sprinting is not the only way to practice scrum. In my coaching experience, I have seen many teams using sprints but in a waterfall way. Why I am saying here in a waterfall way because of following reasons:

1)   Most of the user stories in the sprint backlog get closed at the end of the sprint because of which QAs feel stress at the end of the sprint.

2)   Writing Unit Test Cases after development.

3)   Less automation of test cases.

**1) User stories completion at the end of the sprint:**

As per one the Agile Principles, teams should work in a constant pace:

**_“Agile processes promote sustainable development. The sponsors, developers, and users should be able to maintain a constant pace indefinitely.”_**

If we look into burndown charts following scenario found very often.

![Untitled][1]

 

This happens when team members don’t work together for one user story. If one user story is being completed by at least 2-3 team members by dividing and distributing tasks, those can be completed constantly.

Ideal burn down chart should looks like this:

![Untitled1][2]

 

If we work together to limit work in progress then we can smoothly deliver user stories in a constant pace hence constant work for QAs.

**2) Writing Unit Test Cases after development.**

Writing unit test cases after the development doesn’t help much. Test-driven development offers more than just simple validation of correctness, but can also drive the design of a program.] By focusing on the test cases first, one must imagine how the functionality is used by clients (in the first case, the test cases). So, the programmer is concerned with the interface before the implementation.

This way of programming generates less number of defects and increase collaboration between developer and programmer in getting to know about scenarios and developing according to them.

**3) Less automation of test cases**

Without automation, one can’t achieve frequent delivery of user stories. Sprint after sprint you have to add new features and ensure that whatever you built previously continues to work. Having an automated testing framework, which takes care of both system and integration tests, adds a lot of firepower to a team. It also frees up a lot of developer and tester time.

There are many more anti-agile patterns, which I would like to describe in further blog posts.

   [1]: http://xebee.xebia.in/wp-content/uploads/2014/03/Untitled.jpg
   [2]: http://xebee.xebia.in/wp-content/uploads/2014/03/Untitled1.jpg