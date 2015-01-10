---
layout: post
header-img: img/default-blog-pic.jpg
author: balajidl
description: 
post_id: 431
created: 2008/03/04 20:54:13
created_gmt: 2008/03/04 18:54:13
comment_status: open
---

# Spring with Stripes - A Maven Based Sample Code

<p>The purpose of this post is to give a <strong>Maven based sample code</strong> of <em><strong>Spring with Stripes</strong></em> integration. The source code set is already in Eclipse project format, so you can use eclipse to view files content and structure.
This post will not explain about the techniques of integrating spring with stripes. The spring with stripes integration is very well explained at <a href="http://www.stripesframework.org/display/stripes/Spring+with+Stripes">Stripes framework wiki page - Spring with Stripes</a>. I kindly suggest you to read that documentation first before trying out the sample code given in this post. This post also assume that you are familiar with the basics of Maven, Eclipse, Stripes and Spring.</p>
<p>Few of the other <a href="http://www.stripesframework.org/display/stripes/How+To%27s">"How to"</a> samples (like Ajax addition, addition, echo, stripes layout reuse) that were documented in Stripes framework wiki page were also included in this sample code.
<!--more--></p>
<p><strong>Downloading and running the sample code.</strong>
<ol></p>
<li>Click <a id=p434 href="http://xebee.xebia.in/wp-content/uploads/2008/03/simplewebapp.tar">simplewebapp.tar</a> to download the Spring with Stripes sample code based on Maven</li>

<li>Untar the downloaded tar file to a known location</li>

<li>You will see a folder structure simillar to the image shown below
<img id="image432" alt=stripeseclipse.png src="http://xebee.xebia.in/wp-content/uploads/2008/03/stripeseclipse.png" /> </li>

<li>Go to extracted folder "simplewebapp" and analyse the <strong>pom.xml</strong> file content, especially the stripes and spring dependency nodes<pre lang="xml">
...........pom.xml.....
<dependencies>
 <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>3.8.1</version>
    <scope>test</scope>
 </dependency>
 <dependency>
    <groupId>net.sourceforge.stripes</groupId>
    <artifactId>stripes</artifactId>
    <version>1.4</version>
 </dependency>
 <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring</artifactId>
    <version>2.0.6</version>
 </dependency>
 <dependency>
    <groupId>commons-logging</groupId>
    <artifactId>commons-logging</artifactId>
    <version>1.1</version>
 </dependency>
</dependencies>
..............
</pre></li>

<li>Run the maven commands with goals like install or package to build the application. Example <strong>mvn clean package</strong>
<li>The pom.xml has already plugin reference for Jetty and Tomcat. If you want to quickly build, run and view the sample code, then use the maven goal jetty:run or tomcat:run. Example, executing <strong>mvn clean jetty:run</strong>, will clean, build and deploy the application on an inplace Jetty server.
Hit the browser with url http:/localhost:8080 to view the sample code home or index page. Sample home or index page is shown below
<img alt=Safaru src="http://xebee.xebia.in/wp-content/uploads/2008/03/spring-stripesweb.png" />
</li>
</ol>
Thats it.
If you happen to face some problem on the using the given sample code, then please let me know by adding a comment to this post. I will try to reply for that.