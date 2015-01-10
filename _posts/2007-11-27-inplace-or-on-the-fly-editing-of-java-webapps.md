---
layout: post
header-img: img/default-blog-pic.jpg
author: balajidl
description: 
post_id: 331
created: 2007/11/27 13:38:15
created_gmt: 2007/11/27 11:38:15
comment_status: open
---

# Inplace or On-the-fly editing of Java webapp's

While developing and testing the Java EE web applications, people often needs a way to immediately web-view the changes that they have made in their JSP or HTML or Javascript.   
For people who are using Eclipse or intelliJ as their IDE might also like to see their changes in serverside Java files applied over the deployed webapp's immediately.   
Lets see how this can be done using **Maven** In general, to view the changes immediately, people will edit those files on the exploded directory under webapp. Later, they have to manually copy the changes to their original workspace. This is a pain for projects which has version control enabled.   
  
_Maven comes for resque_:)  
  
Some of the common goals used in Maven based build process includes **test, clean, package, install**. Apart from this maven provides excellent plugin capability. One of such plugin allows you to directly deploy your webapp on your java application server like Tomcat/Jetty/Websphere etc.,  
Lets see how we can make use of that plugin for inplace or on-the-fly editing and still be on hold with version control system. 

### With Tomcat

The [Tomcat Maven plugin][1] provides many goals to play around with webapp's.  
For example, running **mvn tomcat:run** from the root of the maven based webapp project will deploy the webapp exploded war inside the embedded tomcat. The deployed webapp can be viewed by browsing http://localhost:8080/   
  
If you make any changes to the source folder jsp's, html or javascript, the changes can be immediately viewed by refreshing the browser.   
  
Now their might also be a requirement where you want to make changes on server side Java file and see them being applied immediately on the deployed war file.   
Note that, for this to work, you must do editing from Eclipse or IntelliJ kind of IDE's.  
  
The **war:inplace** goal from maven helps us to do this easily. Running **mvn war:inplace** will create the exploded webapp inside src/main/webapp.   
Note that this goal does not have any prerequisites, so whenever you use it, you must also do a compile or test preceded if you want to.   
Example:** mvn clean compile test war:inplace**.  
![war in place][2] Note that the war:inplace goal copies the dependent/external jars into WEB-INF/lib and the compiled classes into WEB-INF/classes. Do configure your version control system (example subversion) to ignore these files. You can also use **clean** goal of maven to additionally delete those directories.

  
  
If you want to add maven plugin to your project, simply add the below lines to your pom.xml. For more info on this, please check the [Tomcat Maven plugin][1] site. [xml] org.codehaus.mojo tomcat-maven-plugin [/xml]

### With Jetty

The [Maven2 Jetty plugin][3] provides goals for running a webapplication directly from Maven.   
Example: **mvn jetty:run** will create the exploded war and make it available ruuning on the root context for Jetty server.   
  
mvn war:inplace jetty:run** do the simillar job as mentioned in tomcat part above.   
  
To add Jetty plugin, add the following code to your pom.xml [xml] org.mortbay.jetty maven-jetty-plugin 6.1.5 / 3 src/main/webapp/WEB-INF **/*.jsp **/_.properties __/_.xml [/xml] 

### Sample application

I have also included a maven based simple web application project (zip file size 6kb).   
If you wanna try this stuff mentioned above, please [download the application here][4]. This requires you having Maven2 installed as well.

   [1]: http://mojo.codehaus.org/tomcat-maven-plugin/introduction.html
   [2]: http://xebee.xebia.in/wp-content/uploads/2007/11/warinplace.png
   [3]: http://mojo.codehaus.org/jetty-maven-plugin/usage.html
   [4]: http://xebee.xebia.in/wp-content/uploads/2007/11/simplewebapp.zip