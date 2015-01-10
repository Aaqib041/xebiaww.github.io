---
layout: post
header-img: img/default-blog-pic.jpg
---

# Article Series: "Automated code reviews with Checkstyle" on JavaWorld

Today JavaWorld published my article series "Automated code reviews with Checkstyle" in 2 parts. Part 1: [Automated code reviews with Checkstyle, Part 1](http://www.javaworld.com/javaworld/jw-11-2008/jw-11-checkstyle1.html ) Part 2: [Automated code reviews with Checkstyle, Part 2](http://www.javaworld.com/javaworld/jw-11-2008/jw-11-checkstyle2.html ) This article series attempts to bridge the gap of code review with applying automated Checkstyle checks in a complete and proactive way. First goal is to make the task of custom Checkstyle rules creation so simple so that any enterprise IT team could create new custom rules suiting to their project (IT standards) needs. Second goal is to apply these rules in PROCTIVE fashion. Instead of waiting the build to fail or waiting for rule violation reports and working on them in a reactive way, the idea is to apply these checks proactively with Checkstyle Eclipse plugin or applying them at SVN level itself. Irrespective of which IDE you are using, if your code contains some of the high severity violations, you will not be able to commit the code in SVN. You will see the same kind errors and location on SVN console as you see with Eclipse plugin. This is achieved using SVN pre-commit hooks.