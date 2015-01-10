---
layout: post
header-img: img/default-blog-pic.jpg
author: vashishtha
description: 
post_id: 821
created: 2008/11/26 01:23:44
created_gmt: 2008/11/25 23:23:44
comment_status: open
---

# Article Series: "Automated code reviews with Checkstyle" on JavaWorld

<p>Today JavaWorld published my article series "Automated code reviews with Checkstyle" in 2 parts.</p>
<p>Part 1:
<a href="http://www.javaworld.com/javaworld/jw-11-2008/jw-11-checkstyle1.html ">Automated code reviews with Checkstyle, Part 1</a></p>
<p>Part 2:
<a href="http://www.javaworld.com/javaworld/jw-11-2008/jw-11-checkstyle2.html ">Automated code reviews with Checkstyle, Part 2</a></p>
<p>This article series attempts to bridge the gap of code review with applying automated Checkstyle checks in a complete and proactive way. First goal is to make the task of custom Checkstyle rules creation so simple so that any enterprise IT team could create new custom rules suiting to their project (IT standards) needs.</p>
<p>Second goal is to apply these rules in PROCTIVE fashion. Instead of waiting the build to fail or waiting for rule violation reports and working on them in a reactive way, the idea is to apply these checks proactively with Checkstyle Eclipse plugin or applying them at SVN level itself. Irrespective of which IDE you are using, if your code contains some of the high severity violations, you will not be able to commit the code in SVN. You will see the same kind errors and location on SVN console as you see with Eclipse plugin. This is achieved using SVN pre-commit hooks.</p>