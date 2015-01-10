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

<p>While developing and testing the Java EE web applications, people often needs a way to immediately web-view the changes that they have made in their JSP or HTML or Javascript.
 <br /> For people who are using Eclipse or intelliJ as their IDE might also like to see their changes in serverside Java files applied over the deployed webapp's immediately.
 <br />Lets see how this can be done using <b>Maven</b><!--more-->
In general, to view the changes immediately, people will edit those files on the exploded directory under webapp. Later, they have to manually copy the changes
to their original workspace. This is a pain for projects which has version control enabled.
<br /><br /><i>Maven comes for resque</i>:)<br /><br />
Some of the common goals used in Maven based build process includes <b>test, clean, package, install</b>. Apart from this maven provides excellent plugin capability.
One of such plugin allows you to directly deploy your webapp on your java application server like Tomcat/Jetty/Websphere etc.,<br />
Lets see how we can make use of that plugin for inplace or on-the-fly editing and still be on hold with version control system.
<h3>With Tomcat</h3>
The <a href="http://mojo.codehaus.org/tomcat-maven-plugin/introduction.html">Tomcat Maven plugin</a> provides many goals to play around with webapp's.<br />
For example, running <b>mvn tomcat:run</b> from the root of the maven based webapp project will deploy the webapp exploded war inside the embedded tomcat.
The deployed webapp can be viewed by browsing http://localhost:8080/<webapp name> <br /><br />
If you make any changes to the source folder jsp's, html or javascript, the changes can be immediately viewed by refreshing the browser. <br /><br />
Now their might also be a requirement where you want to make changes on server side Java file and see them being applied immediately on the deployed war file.
<br />Note that, for this to work, you must do editing from Eclipse or IntelliJ kind of IDE's.<br />
<br />The <b>war:inplace</b> goal from maven helps us to do this easily. Running <b>mvn war:inplace</b> will create the exploded webapp inside src/main/webapp.
<br /> Note that this goal does not have any prerequisites, so whenever you use it, you must also do a compile or test preceded if you want to.
<br /> Example:<b> mvn clean compile test war:inplace</b>.<br />
<img src="http://xebee.xebia.in/wp-content/uploads/2007/11/warinplace.png" alt="war in place" />
Note that the war:inplace  goal copies the dependent/external jars into WEB-INF/lib and the compiled classes into
WEB-INF/classes. Do configure your version control system (example subversion) to ignore these files. You can also use <b>clean</b> goal of maven to additionally delete those directories.</p>
<p><br /><br />
If you want to add maven plugin to your project, simply add the below lines to your pom.xml.
For more info on this, please check the <a href="http://mojo.codehaus.org/tomcat-maven-plugin/introduction.html">Tomcat Maven plugin</a> site.
[xml]<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>tomcat-maven-plugin</artifactId>
</plugin>[/xml]<h3>With Jetty</h3>
The <a href="http://mojo.codehaus.org/jetty-maven-plugin/usage.html">Maven2 Jetty plugin</a> provides goals for running a webapplication directly from Maven.
<br />
Example: <b>mvn jetty:run</b> will create the exploded war and make it available ruuning on the root context for Jetty server.
<br />
<br />mvn war:inplace jetty:run</b> do the simillar job as mentioned in tomcat part above.
<br /> <br />
To add Jetty plugin, add the following code to your pom.xml
[xml]<plugin>
    <groupId>org.mortbay.jetty</groupId>
    <artifactId>maven-jetty-plugin</artifactId>
    <version>6.1.5</version>
    <!-- some of the configurations below can also be ignored -->
    <configuration>
        <contextPath>/</contextPath>
        <scanIntervalSeconds>3</scanIntervalSeconds>
        <scanTargetPatterns>
            <scanTargetPattern>
                <directory>src/main/webapp/WEB-INF</directory>
                <excludes>
                    <exclude><strong>/*.jsp</exclude>
                </excludes>
                <includes>
                    <include></strong>/<em>.properties</include>
                    <include></em><em>/</em>.xml</include>
                </includes>
            </scanTargetPattern>
        </scanTargetPatterns>
    </configuration>
</plugin>[/xml]
<h3>Sample application</h3>
I have also included a maven based simple web application project (zip file size 6kb).
<br /> If you wanna try this stuff mentioned above, please <a href="http://xebee.xebia.in/wp-content/uploads/2007/11/simplewebapp.zip">download the application here</a>. This requires you having Maven2 installed as well.</p>