---
layout: post
header-img: img/default-blog-pic.jpg
author: vhazratiblog
description: 
post_id: 341
created: 2007/12/09 19:44:49
created_gmt: 2007/12/09 17:44:49
comment_status: open
---

# Static Code Analysis is just tip of the Iceberg!

Most of the times we are content that our code is of the right quality, if somehow, we manage to get the Static Code Analysis (SCA) tools like [Checkstyle][1], [PMD][2] etc. report less number of severe violations. As an example if we see that the class is big in size then we conveniently split it into two or more classes to get rid of the violation. The tool is happy and so are we and most of the times that is the end of the story.

However more frequently than not getting an SCA violation is the start of the story. If you start associating the question “Why' with every SCA violation found then the real reasons start unfolding.

This is similar to the way we resolve impediments on an Scrum project. The impediments rarely represent the isolated incidences of inefficiency. Rather, most of the times they are a part of a larger problem. The way to work out an impediment is fix it so that the team can work effectively and then to look at the root cause which caused the impediment so that the main cause can be fixed. This is called “Bottom-up process re-engineering.”

Similarly the way to work out an SCA violation is to remove it so that the code looks clean and good and then to hunt for the real cause.  I got first hand insight into this when I was doing an audit for a large system which is being used online by millions of people. While doing the analysis my co-auditor suggested that let us look for hot spots which are highlighted by the SCA tools. Then, let us dive into the code at those places to find out bigger issues. At that time I was skeptical if that approach would work however now I am a firm believer of his philosophy.

The SCA tools do a great job of pointing out the hot spots. You can either patch the hot spots to make the SCA tool happy or you could look at the bigger issues so that the root cause of the problem is eliminated.

I would take some examples from my audit to make it more clear. **Let me warn you that these problems look too simple for any deeper analysis but believe me once you do the analysis you do manage to uncover some unexpected thought provoking findings**. And of course these are just a few examples for illustration, you could apply this to ideally every violation that you get.

1.**Size violation:** We found that there were servlets in the application which were more than 3000 lines, having methods more than 800 lines each. A quick fix to this might have been to reduce the method size by splitting the methods and moving some methods out of the class to a helper class so that the class size and method size violations are taken care of.

However, when we looked deeper to answer the question “_Why do the servlets have so much lines and such big methods?_” we realized that all of the business logic was present in the servlets. There was a bigger violation of [Single Responsibility Principle][3] where all the logic was present in the single class. The presentation logic, business logic and data access logic were all clubbed together thus leading to fragile design which is hard to change without breaking. There was no layering and no separation of concerns.

The design of the system was a culprit here and probably none of the developers or the architect on the project felt so.

2.**Methods with large number of parameters:** A quick way to solve this and make the SCA tool happy would be to introduce an object and populate the object with the required parameters.

However, when we took a deep dive we realized that the system did not have any abstraction. There were no domain objects in the system either. When data had to be passed between method calls, all of that would be passed as primitive data, mostly strings. There was no focus on domain driven design. The methods were overloaded with every extra parameter that would be required in a different situation which suggested that the system was open for modification and closed for extension. For any small change a new method had to be written. This violated the [Open Close Principle][4].

Again design was the culprit here.

3.**Large Cyclomatic complexity and NPath:** We were baffled to see CC numbers > 50 for some methods and NPath figures of > 1.7 million.

When we dived deep we found that the methods in question had a lot of nested if..else logic. So far so good, but what is the reason for the nested logic? We found that the method contained all the dispatch logic for the request. If the request has these attributes do this else do that and so on. This also explained why the system had low code coverage by unit tests. A unit test to test 1.7 million possible paths would take weeks if not months to write. Clearly nobody had thought about introducing one of the many readily available MVC frameworks. When we questioned the team on why they did not use a MVC framework we were told that they did not feel the need in the beginning because the system was too small. Once it started growing they never had the time to refactor and introduce a framework.

This could have several causes ranging from lack of communication between the client and the vendor about realistic time lines, refactoring not being a part of the standard development process, lack of interest of the team in project, lack of interest in achieving the right quality and focusing on just getting the work done.

4.**Missing braces and wrong variable naming:** This is probably one of those violations which is immediately reported by a SCA tool and is easy to correct.

However, when we started looking at it deeply we found that certain modules of the application reported huge violations as compared to others. When we compared two modules with highest number of violations we found that both of them were using very different naming strategies. Further investigation revealed that there were no coding standards which were used on the project. Every module and each developer was free to use a nomenclature which went well with his taste. There was no conceptual integrity and no uniformity in which the entire software was built.

Improper software development process along with lack of coordinated communication between the teams were the obvious culprits in this case.

5.**Large number of unused imports, variables and methods:** This is again something which is readily reported and is easy to fix.

However, when we questioned the developers about the reason to have unused code, we were told that they never removed the unused code lest they should need it some day. It would save them time in the future. Obviously nobody was thinking about the time lost when we had to scroll through tons of unused code and probably they were never educated on the principle of [YAGNI][5].

Again improper development process and lack of developer education were the culprits here.

You would concur with me that all these violations were very small and could have easily been fixed without resolving to deeper analysis. However, if you try to dig deeper then most of them would point to bigger issues which need a more focussed approach to fix. **We saw that relatively small problems reported by the SCA tools could be attributed to bigger issues like improper design, sub standard development process, lack of communication and lack of proper education towards effective software development for the development team**.

Hence though doing a root cause analysis for small violations might sound like an overkill but a lot of times the small violation is just the tip of the iceberg!

**Next time when you are resolving the SCA tool reported violation just pause for a moment and ask “_Why do I have this violation in the first place?_”**

   [1]: http://http://checkstyle.sourceforge.net/index.html
   [2]: http://pmd.sourceforge.net/
   [3]: http://www.objectmentor.com/resources/articles/srp.pdf
   [4]: http://www.objectmentor.com/resources/articles/ocp.pdf
   [5]: http://c2.com/xp/YouArentGonnaNeedIt.html