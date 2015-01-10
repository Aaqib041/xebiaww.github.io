---
layout: post
header-img: img/default-blog-pic.jpg
---

# Spring with Stripes - A Maven Based Sample Code

The purpose of this post is to give a **Maven based sample code** of _**Spring with Stripes**_ integration. The source code set is already in Eclipse project format, so you can use eclipse to view files content and structure. This post will not explain about the techniques of integrating spring with stripes. The spring with stripes integration is very well explained at [Stripes framework wiki page - Spring with Stripes](http://www.stripesframework.org/display/stripes/Spring+with+Stripes). I kindly suggest you to read that documentation first before trying out the sample code given in this post. This post also assume that you are familiar with the basics of Maven, Eclipse, Stripes and Spring. Few of the other ["How to"](http://www.stripesframework.org/display/stripes/How+To%27s) samples (like Ajax addition, addition, echo, stripes layout reuse) that were documented in Stripes framework wiki page were also included in this sample code.  **Downloading and running the sample code.**

  1. Click [simplewebapp.tar](/wp-content/uploads/2008/03/simplewebapp.tar) to download the Spring with Stripes sample code based on Maven
  2. Untar the downloaded tar file to a known location
  3. You will see a folder structure simillar to the image shown below ![stripeseclipse.png](/wp-content/uploads/2008/03/stripeseclipse.png)
  4. Go to extracted folder "simplewebapp" and analyse the **pom.xml** file content, especially the stripes and spring dependency nodes
    
        ...........pom.xml.....
    
     
    	junit
    	junit
    	3.8.1
    	test
     
     
    	net.sourceforge.stripes
    	stripes
    	1.4
     
     
    	org.springframework
    	spring
    	2.0.6
     
     
    	commons-logging
    	commons-logging
    	1.1
     
    
    ..............
    

  5. Run the maven commands with goals like install or package to build the application. Example **mvn clean package**
  6. The pom.xml has already plugin reference for Jetty and Tomcat. If you want to quickly build, run and view the sample code, then use the maven goal jetty:run or tomcat:run. Example, executing **mvn clean jetty:run**, will clean, build and deploy the application on an inplace Jetty server. Hit the browser with url http:/localhost:8080 to view the sample code home or index page. Sample home or index page is shown below ![Safaru](/wp-content/uploads/2008/03/spring-stripesweb.png)
Thats it. If you happen to face some problem on the using the given sample code, then please let me know by adding a comment to this post. I will try to reply for that.