---
layout: post
header-img: img/default-blog-pic.jpg
author: vashishtha
description: 
post_id: 2639
created: 2009/12/03 23:00:39
created_gmt: 2009/12/03 18:00:39
comment_status: open
---

# First Look At Maven 3

After hearing a lot of good things about upcoming Maven 3 but not getting any stable release, I thought I should give it a try with its early snapshot. So I downloaded it from its [svn repo][1]. As I was a bit impatient, without looking at instructions I built it using ANT. Later I realized that I could also do it using existing version of maven. After building it, I thought I should give it a try on my existing project and here are a few observations.

  * Maven 3 has been designed to be backward compatible and it worked nicely for my project with minor hicups.
  * Initially I thought it's my misconception, but guys, here's a good news - it's much faster than Maven 2. 
  * It's more verbose (for better) and provides information (relevant plugin and associated goal) on each step being executed
  * It doesn't seem to like to redundant information anymore. I had duplicate dependency in my pom.xml which used to work in Maven 2. Maven 3 doesn't allow it.
  * While working with it, I simply loved its feature of restartability. In Maven 2, if your test case fails, you have to start all over again. In Maven 3, you can restart from where you left after fixing the test which (I know you already figured out) is a great time saver. Here is an example: ` [ERROR] After correcting the problems, you can resume the build with the command [ERROR] mvn <goals> -rf :elmar-import-incr `

   [1]: https://svn.apache.org/repos/asf/maven/maven-3/trunk