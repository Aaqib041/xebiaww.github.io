---
layout: post
header-img: img/default-blog-pic.jpg
author: balajidl
description: 
post_id: 429
created: 2008/03/01 15:12:45
created_gmt: 2008/03/01 13:12:45
comment_status: open
---

# Appfuse 2.0 Review

<p><a target="new" href="http://appfuse.org/">AppFuse</a> is an open source project and application that uses open source tools built on the Java platform to help you develop
web applications quickly and efficiently.</p>
<p>In a typical web application, it is common to have a login screen, registration screen, content storage in database, security and most importantly testing.</p>
<p>When building a Java web application, we might start adding one jar file after another to implement a particular user story.<br />
For example, if i want to have ACEGI security for my web app, I might download ACEGI jar, map and configure it in web.xml and security.xml files.</p>
<p>What if, there exists a toolkit that already provides these common features to us. ?
<!--more-->
For instance...
<blockquote> ACEGI security
 Hibernate integration
 AJAX templates
 Switchable CSS framework
 Unit test
 Layout reuse
 PDF support
 Excel support and so on...</blockquote>
Yes!. Appfuse can provide the above listed features and many more as a cool Maven bundle. All the hassels for getting the base setup for your web application has been done already. All it requires is understanding the Appfuse structure and making use of it.</p>
<p>If you are interested, you can also checkout the Appfuse roadmap for various frameworks supports.
<a href="http://static.raibledesigns.com/repository/images/appfuse-history.png" target="new"><img src="http://static.raibledesigns.com/repository/images/appfuse-history.png" alt="Roadmap-previous" /></a>
<a href="http://static.raibledesigns.com/repository/images/appfuse-history.png" target="new">Fullsize image link1</a>
<a href="http://static.raibledesigns.com/repository/images/appfuse-roadmap.png" target="new"><img src="http://static.raibledesigns.com/repository/images/appfuse-roadmap.png" alt="Roadmap-future" /></a>
<a href="http://static.raibledesigns.com/repository/images/appfuse-roadmap.png" target="new">Fullsize image link2</a></p>
<p>One of my last project was based on <a target="new" href="http://appfuse.org/display/APF/Home">Appfuse 2.0</a>. We blindly took the risk of basing our project on Appfuse, and Appfuse never turned us down.</p>
<p>Installing Appfuse is easy for people who have basic knowledge of Maven 2. We took the <b>Spring MVC Basic</b> archetype of Appfuse, as the team already had good expertise on Spring/Hibernate/Junit.</p>
<p>The Spring MVC Basic archetype was made with excellent set of framework and libraries, some of them include Hibernate, JUnit, Spring MVC,DisplayTag, DWR and so on. The good thing is, its also supports most of commonly used databases.</p>
<p>It took very short time to add a new spring-mvc jsp page, a spring controller and the related test cases.
It took hardly no time to add our own language extension for internationalization, because Appfuse comes with ready made templates.</p>
<p>At one point, we wanted to extend the user registration form, spring controller and the database. I was able to do it with the help of documentation provided on the Appfuse website.</p>
<p>It takes a while to understand the whole structure of the project, but I dont feel it wasted my time on learning it's structure.
Heavily based on many external jars files so you never know when they will become obsolete.One can also argue that is not the problem of Appfuse but the individual package. We faced some compatibility issues on using the jars but later we managed to correct them using our pom.xml</p>
<p>I strongly recommend to the spring developers to look in to the Appfuse application before you start developing your own set of common features and requirements for your web application.
This helps you not to re-invent wheel and saves time and effort.</p>
<p>The mailing list/user community is very active in answering the queries. The Appfuse team is also very active in keeping their application at par with new changes in the technology.
Apart from Spring MVC archetype, Appfuse also has its archetype for JSF, Struts and Tapestry. Checkout Appfuse website for more info about online demos, tutorials and release roadmap.</p>
<p><b>Conclusion</b></p>
<p>The learning curve is not that steep, but once you learn it, you will have top-notch technologies binded to your application.
While the Appfuse homepage provides a good documentation and video tutorials, it would be nice to have book written on it.
<a href="http://raibledesigns.com/rd/" target="new">Matt Raible</a>!.. Will you write a book ;)</p>