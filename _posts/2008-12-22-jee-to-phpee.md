---
layout: post
header-img: img/default-blog-pic.jpg
author: saket_vishal
description: 
post_id: 849
created: 2008/12/22 08:38:50
created_gmt: 2008/12/22 07:38:50
comment_status: open
---

# JEE to PHPEE

<p>PHP is becoming better day by day. As I proceed in my path of catching up with recent developments in PHP, I see more and more of adaption in PHP from JEE(Java Enterprise Edition) world. To begin with, I stumbled upon the unit testing framework <a href="http://www.phpunit.de/">PHPUnit</a>. The amount of familiarity with the J2EE world is simply amazing.
<!--more--></p>
<p>Following table highlights this on various aspects of PHPUnit.
<a href='http://xebee.xebia.in/wp-content/uploads/2008/12/diagram.gif'><img src="http://xebee.xebia.in/wp-content/uploads/2008/12/diagram.gif" title="comparison" class="aligncenter size-medium wp-image-851" /></a></p>
<p>PHPUnit belongs to xUnit family, same as that of the jUnit. PHPUnit not only provides various functions to assert values but also allows many complex operations possible through annotations. PHPUnit provides a lot of extensions, such as for database testing, performance testing. The Database testing extension provided by PHPUnit is very similar to that of what is provided by <a href="http://dbunit.sourceforge.net/">Dbunit</a>. Both follow similar approaches of seeding the database initially, and then after code execution ensuring that expected data is found in the database. If one has some experience with mocking frameworks in Java such as <a href="http://www.easymock.org/">EasyMock</a>, then working with mock testing with PHPUnit would be a cakewalk. The comfort of migration extends to the level of acceptance testing, when one uses <a href="http://seleniumhq.org/">Selenium</a> with PHPUnit.</p>
<p>PHPUnit is one of the many available testing frameworks in PHP world, and testing is one of the many areas where I see commonality between the two worlds(J2EE and PHP). The major reason of the commonality has come because of the inclusion of the best practices in the PHP world and also adapting helpful frameworks from the J2EE world. As I proceed further on my path ahead, I would be sharing my learning with you all.</p>