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

PHP is becoming better day by day. As I proceed in my path of catching up with recent developments in PHP, I see more and more of adaption in PHP from JEE(Java Enterprise Edition) world. To begin with, I stumbled upon the unit testing framework [PHPUnit][1]. The amount of familiarity with the J2EE world is simply amazing. 

Following table highlights this on various aspects of PHPUnit. ![][2]

PHPUnit belongs to xUnit family, same as that of the jUnit. PHPUnit not only provides various functions to assert values but also allows many complex operations possible through annotations. PHPUnit provides a lot of extensions, such as for database testing, performance testing. The Database testing extension provided by PHPUnit is very similar to that of what is provided by [Dbunit][3]. Both follow similar approaches of seeding the database initially, and then after code execution ensuring that expected data is found in the database. If one has some experience with mocking frameworks in Java such as [EasyMock][4], then working with mock testing with PHPUnit would be a cakewalk. The comfort of migration extends to the level of acceptance testing, when one uses [Selenium][5] with PHPUnit.

PHPUnit is one of the many available testing frameworks in PHP world, and testing is one of the many areas where I see commonality between the two worlds(J2EE and PHP). The major reason of the commonality has come because of the inclusion of the best practices in the PHP world and also adapting helpful frameworks from the J2EE world. As I proceed further on my path ahead, I would be sharing my learning with you all.

   [1]: http://www.phpunit.de/
   [2]: http://xebee.xebia.in/wp-content/uploads/2008/12/diagram.gif (comparison)
   [3]: http://dbunit.sourceforge.net/
   [4]: http://www.easymock.org/
   [5]: http://seleniumhq.org/