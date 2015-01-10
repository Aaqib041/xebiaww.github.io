---
layout: post
header-img: img/default-blog-pic.jpg
author: rjaiswal
description: 
post_id: 4183
created: 2010/07/21 11:18:56
created_gmt: 2010/07/21 06:18:56
comment_status: open
---

# Google App Engine + Maven + JSF2

<p>I have tried to put all the keywords in the title of this blog, hoping that it will turn some heads. Google App Engine (GAE) provides a great hosting platform for open source apps written in Java/Python. So if you have no qualms about hosting you Java applications on a public server then you have no better choice than GAE.</p>
<p>The only catch however is, that GAE does not support certain APIs (see the white-list here - <a href="http://code.google.com/intl/de/appengine/docs/java/jrewhitelist.html" target="_blank">http://code.google.com/intl/de/appengine/docs/java/jrewhitelist.html</a>). This restriction makes it difficult to host certain apps / frameworks.</p>
<p>GAE provides the infrastructure, a modified Jetty server included in the GAE SDK (<a href="http://code.google.com/appengine/downloads.html" target="_blank">http://code.google.com/appengine/downloads.html</a>) though which you can test your app. If it runs on the local GAE Jetty server then it will also run on GAE itself. The project structure that Google asks you to create for uploading an app is also quite typical (unless you use Ant) which brings me to my next point.</p>
<!--more-->

<p>Maven is my favorite build tool, it would really take something for me to move on to Ant now. So I tried to create my application using Maven and then test it locally on GAE's Jetty and finally upload it to GAE. After a little search I found the Maven GAE plugin (<a href="http://code.google.com/p/maven-gae-plugin/">http://code.google.com/p/maven-gae-plugin/</a>).</p>
<p>Below is my pom.xml to make this plugin work - </p>
<p>[code language="xml"]
&lt;project xmlns=&quot;http://maven.apache.org/POM/4.0.0&quot; xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot;
         xsi:schemaLocation=&quot;http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd&quot;&gt;</p>
<pre><code>&amp;lt;modelVersion&amp;gt;4.0.0&amp;lt;/modelVersion&amp;gt;
&amp;lt;groupId&amp;gt;net.rocky&amp;lt;/groupId&amp;gt;
&amp;lt;artifactId&amp;gt;rocky-gae-app&amp;lt;/artifactId&amp;gt;
&amp;lt;packaging&amp;gt;war&amp;lt;/packaging&amp;gt;
&amp;lt;version&amp;gt;1&amp;lt;/version&amp;gt;
&amp;lt;name&amp;gt;rocky-gae-app&amp;lt;/name&amp;gt;
&amp;lt;url&amp;gt;http://maven.apache.org&amp;lt;/url&amp;gt;

&amp;lt;repositories&amp;gt;
    &amp;lt;repository&amp;gt;
        &amp;lt;id&amp;gt;java.net&amp;lt;/id&amp;gt;
        &amp;lt;url&amp;gt;http://download.java.net/maven/2&amp;lt;/url&amp;gt;
    &amp;lt;/repository&amp;gt;
&amp;lt;/repositories&amp;gt;

&amp;lt;pluginRepositories&amp;gt;
    &amp;lt;pluginRepository&amp;gt;
        &amp;lt;id&amp;gt;maven-gae-plugin-repo&amp;lt;/id&amp;gt;
        &amp;lt;name&amp;gt;maven-gae-plugin repository&amp;lt;/name&amp;gt;
        &amp;lt;url&amp;gt;http://maven-gae-plugin.googlecode.com/svn/repository&amp;lt;/url&amp;gt;
    &amp;lt;/pluginRepository&amp;gt;
&amp;lt;/pluginRepositories&amp;gt;

&amp;lt;dependencies&amp;gt;
    &amp;lt;dependency&amp;gt;
        &amp;lt;groupId&amp;gt;javax.servlet&amp;lt;/groupId&amp;gt;
        &amp;lt;artifactId&amp;gt;servlet-api&amp;lt;/artifactId&amp;gt;
        &amp;lt;version&amp;gt;2.5&amp;lt;/version&amp;gt;
        &amp;lt;scope&amp;gt;provided&amp;lt;/scope&amp;gt;
    &amp;lt;/dependency&amp;gt;
    &amp;lt;dependency&amp;gt;
        &amp;lt;groupId&amp;gt;javax.servlet.jsp&amp;lt;/groupId&amp;gt;
        &amp;lt;artifactId&amp;gt;jsp-api&amp;lt;/artifactId&amp;gt;
        &amp;lt;version&amp;gt;2.1&amp;lt;/version&amp;gt;
        &amp;lt;scope&amp;gt;provided&amp;lt;/scope&amp;gt;
    &amp;lt;/dependency&amp;gt;
    &amp;lt;dependency&amp;gt;
        &amp;lt;groupId&amp;gt;com.sun.faces&amp;lt;/groupId&amp;gt;
        &amp;lt;artifactId&amp;gt;jsf-api&amp;lt;/artifactId&amp;gt;
        &amp;lt;version&amp;gt;2.0.2&amp;lt;/version&amp;gt;
    &amp;lt;/dependency&amp;gt;
    &amp;lt;dependency&amp;gt;
        &amp;lt;groupId&amp;gt;com.sun.faces&amp;lt;/groupId&amp;gt;
        &amp;lt;artifactId&amp;gt;jsf-impl&amp;lt;/artifactId&amp;gt;
        &amp;lt;version&amp;gt;2.0.2&amp;lt;/version&amp;gt;
        &amp;lt;classifier&amp;gt;gae&amp;lt;/classifier&amp;gt;
    &amp;lt;/dependency&amp;gt;
    &amp;lt;dependency&amp;gt;
        &amp;lt;groupId&amp;gt;org.glassfish.web&amp;lt;/groupId&amp;gt;
        &amp;lt;artifactId&amp;gt;el-impl&amp;lt;/artifactId&amp;gt;
        &amp;lt;version&amp;gt;2.2&amp;lt;/version&amp;gt;
    &amp;lt;/dependency&amp;gt;
    &amp;lt;dependency&amp;gt;
        &amp;lt;groupId&amp;gt;junit&amp;lt;/groupId&amp;gt;
        &amp;lt;artifactId&amp;gt;junit&amp;lt;/artifactId&amp;gt;
        &amp;lt;version&amp;gt;4.8.1&amp;lt;/version&amp;gt;
        &amp;lt;scope&amp;gt;test&amp;lt;/scope&amp;gt;
    &amp;lt;/dependency&amp;gt;
&amp;lt;/dependencies&amp;gt;

&amp;lt;build&amp;gt;
    &amp;lt;plugins&amp;gt;
        &amp;lt;plugin&amp;gt;
            &amp;lt;groupId&amp;gt;org.apache.maven.plugins&amp;lt;/groupId&amp;gt;
            &amp;lt;artifactId&amp;gt;maven-compiler-plugin&amp;lt;/artifactId&amp;gt;
            &amp;lt;version&amp;gt;2.0.2&amp;lt;/version&amp;gt;
            &amp;lt;configuration&amp;gt;
                &amp;lt;source&amp;gt;1.6&amp;lt;/source&amp;gt;
                &amp;lt;target&amp;gt;1.6&amp;lt;/target&amp;gt;
            &amp;lt;/configuration&amp;gt;
        &amp;lt;/plugin&amp;gt;
        &amp;lt;plugin&amp;gt;
            &amp;lt;groupId&amp;gt;net.kindleit&amp;lt;/groupId&amp;gt;
            &amp;lt;artifactId&amp;gt;maven-gae-plugin&amp;lt;/artifactId&amp;gt;
            &amp;lt;version&amp;gt;0.6.0&amp;lt;/version&amp;gt;
            &amp;lt;configuration&amp;gt;
                &amp;lt;sdkDir&amp;gt;/home/rockyj/01rocky/02apps/appengine-java-sdk-1.3.5&amp;lt;/sdkDir&amp;gt;
            &amp;lt;/configuration&amp;gt;
        &amp;lt;/plugin&amp;gt;
    &amp;lt;/plugins&amp;gt;
    &amp;lt;finalName&amp;gt;rocky-gae-app&amp;lt;/finalName&amp;gt;
&amp;lt;/build&amp;gt;
</code></pre>
<p>&lt;/project&gt;
[/code]</p>
<p>I have included the whole pom as there is hardly anything I can miss here. The GAE plugin is not in maven central so we need to include the plugin repository, the dependencies are pretty standard except one (which I will explain in a moment) and the plugin itself needs the configuration to know where you have the GAE SDK.</p>
<p>As I mentioned earlier, some classes are blacklisted on the GAE so your default jsf-impl.jar (Mojarra) wil not work on GAE. You will need a modified jar which you can download from here (<a href="http://code.google.com/p/joshjcarrier/source/browse/trunk/Sun%20JSF%20GAE/jsf-impl-gae.jar">http://code.google.com/p/joshjcarrier/source/browse/trunk/Sun%20JSF%20GAE/jsf-impl-gae.jar</a>), if you have a Nexus repo you can install the modified jar there or install the jar in you local repository manually. Also as seen in the pom, for resolving EL (Expression Language) you need el-impl.jar, GAE does not support EL unlike Tomcat. </p>
<p>To make JSF work you need the following entries in your web.xml -</p>
<p>[code language="xml"]
    &lt;!-- Seems like GAE 1.2.6 cannot handle server side session management. At least for JSF 2.0.1 --&gt; 
    &lt;context-param&gt; 
      &lt;param-name&gt;javax.faces.STATE_SAVING_METHOD&lt;/param-name&gt; 
      &lt;param-value&gt;client&lt;/param-value&gt; 
    &lt;/context-param&gt; 
    &lt;!-- Recommendation from GAE pages  --&gt; 
    &lt;context-param&gt; 
      &lt;param-name&gt;javax.faces.PROJECT_STAGE&lt;/param-name&gt; 
      &lt;param-value&gt;Production&lt;/param-value&gt; 
    &lt;/context-param&gt;
    &lt;!-- Accommodate Single-Threaded Requirement of Google AppEngine  --&gt;
    &lt;context-param&gt;
      &lt;param-name&gt;com.sun.faces.enableThreading&lt;/param-name&gt;
      &lt;param-value&gt;false&lt;/param-value&gt;
    &lt;/context-param&gt;
[/code]</p>
<p>Finally, in your appengine-web.xml (the GAE configuration file), you need the following entry - </p>
<p>[code language="xml"]
&lt;sessions-enabled&gt;true&lt;/sessions-enabled&gt;
[/code]</p>
<p>That's it. Now you can work on your JSF2 application. To test, all you need to do is : <strong>mvn clean gae:run</strong></p>
<p>For uploading the application, I would still recommend using the SDK provided script (otherwise you need a whole lot of other changes). What I do is - </p>
<p>$HOME/01rocky/02apps/appengine-java-sdk-1.3.5/bin/appcfg.sh update /home/rockyj/01rocky/03workspace/scbcd/rocky-gae-app/target/rocky-gae-app/</p>
<p>This means run the GAE script with the param <strong>"update"</strong> and the path to your maven created target folder. If you have signed up for GAE, it will ask your username and password and your app will be uploaded to GAE.</p>

## Comments

**[Uri](#68 "2010-08-12 03:00:19"):** Great write up. Thank you for putting this online. I didn't have suc a great experience with the maven age plugin which required some tweaking, especially with class paths when imported to eclipse. Q: can you comment on jsf/gae performance, cold start time etc. In comparison with other java frameworks?

**[Rocky Jaiswal](#88 "2010-08-16 08:26:06"):** Hi Uri, Thanks for your comments. From the little experience I have, I don't think that performance is that great for JSF apps on GAE. First, JSF itself is slow as compared to other F/Ws, then GAE also imposes some restrictions on threads (which can't be good for performance). App start time is good, no major problems there. But I would still recommend to look at other frameworks for GAE. JSF for production app on GAE can'e be good, its ok for prototypes etc.

**[notcourage](#3461 "2010-12-11 05:23:17"):** Couldn't get myfaces/appengine to work reliably until I disabled view saving encryption: org.apache.myfaces.USE_ENCRYPTION false javax.faces.STATE_SAVING_METHOD client

**[Daniel](#3068 "2010-10-27 15:11:08"):** There is away to use not modified implementation of mojarra... Works fine for me... Here is how to set it up. I wrote it on my page.. http://www.neverslair-blog.net/daniels-tips-and-tutorials/

**[Ara Minosian](#1581 "2010-09-14 04:45:02"):** Dear Rocky Thank you for great job. Do you now a way to use not modified implementation of mojarra? What do you think about: https://sites.google.com/a/wildstartech.com/adventures-in-java/Java-Platform-Enterprise-Edition/JavaServer-Faces/sun-javaserver-faces-reference-implementation/configuring-jsf-20-to-run-on-the-google-appengine Unfortunately this for Eclipse only. May be you can make new brilliant post.

**[Rocky Jaiswal](#1614 "2010-09-14 12:10:24"):** Hi Ara, Thanks for your feedback. Unfortunately due to Google App Engine restrictions the default Mojarra implementation cannot be used. In the article you have mentioned, which provides a nice intro to JSF2, there is also a link - "JavaServer Faces 2.0 and Google App Engine Compatibility Issues" at the bottom which explains this in detail. Finally, another advantage of using Maven is that the project can work seamlessly between Eclipse / Netbeans/ IntelliJ, as you are not relying on IDE but a standard Java tool (although for Eclipse you need the M2Eclipse plugin), so the project pom above should work on any IDE.

