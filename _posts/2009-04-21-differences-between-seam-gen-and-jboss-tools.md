---
layout: post
header-img: img/default-blog-pic.jpg
author: gkohli
description: 
post_id: 1114
created: 2009/04/21 13:18:25
created_gmt: 2009/04/21 12:18:25
comment_status: open
---

# Differences between Seam-gen and Jboss Tools

<p>In this article we will basically try to understand the basic difference between different ways in which we can do Seam application development. So before we get into details, let have a brief introduction about Seam</p>

<!--more-->

<p><strong>So what is Seam??</strong></p>
<p>Seam is a powerful open source development platform for building rich Internet applications in Java. Seam integrates technologies such as Asynchronous JavaScript and XML (AJAX), JavaServer Faces (JSF), Java Persistence (JPA), Enterprise Java Beans (EJB 3.0) and Business Process Management (BPM) into a unified full-stack solution, complete with sophisticated tooling(<a href="#build">1</a>). The simple chart of architectural design will show you about this:</p>
<p style="text-align: center;"><img class="aligncenter size-full wp-image-1118" title="design-2" src="http://xebee.xebia.in/wp-content/uploads/2009/04/design-2.png" alt="design-2" width="254" height="332" /></p>

<p style="text-align: center;">Diagram 1.1 Architecute of Seam</p>

<p>So Seam basically does the job of plumbing these disconnected frameworks. We had all the good things in J2EE world, but making them work together was a pain. But, now we do have Seam to do all the dirty work for us.</p>
<p>How to get started with Seam application development??
Following could be the possible ways in which we could get started with a simple seam project structure.</p>
<ul>
<li>Seam- gen</li>
<li>Jboss Tools</li>
<li>Maven</li>
</ul>

<p>It was really fun to work on Seam, as it greatly reduced the initial project setup time which we normally have with J2EE project. Seam is basically a tool which give you more Return on Investment (ROI) for the same number of lines you write in any other Java framework like spring or struts.</p>
<p>So, let’s get started with the main aim of this article to have feature differentitation between seam-gen and Jboss Tools .</p>
<p>First of all I would write something about both the Tools and after that will try to get into more details about each.</p>
<h3 style="margin-left:6px">Seam-gen</h3>

<p>We will start with using seam-gen to generate a project structure, as seam-gen provides out-of –box scripts to generate Seam Action, Entity and Conversations</p>
<p><strong>So what is Seam-gen??</strong></p>
<p>Seam-gen is a rapid application generator shipped with the Seam distribution. With a few command line commands, seam-gen generates an array of artifacts for Seam projects to help you get started. In particular, seam-gen is useful for the following.</p>
<ul style="margin-top: 0cm;" type="disc">
    <li>Automatically      generate an empty Seam project with common configuration files, a build      script, and directories for Java code and JSF view pages.</li>
    <li>Automatically      generate complete Eclipse and NetBeans project files for the Seam project.</li>
    <li>Reverse-engineer      entity bean objects from relational database tables.</li>
    <li>Generate      template files for common Seam components.</li>
</ul>

<h3 style="margin-left:6px">Jboss tools</h3>

<p>JBoss Tools is a collection of Eclipse plugins. JBoss Tools a project creation wizard for Seam, Content Assist for the Unified Expression Language (EL) in both facelets and Java code, a graphical editor for jPDL, a graphical editor for Seam configuration files, support for running Seam integration tests from within Eclipse, and much more.
In short, if you are an Eclipse user, then you'll want to use JBoss Tools!</p>
<p>Now, we will mainly look at different aspects in which seam-gen and Jboss Tools are different and what could be the best option for building Seam application from scratch</p>
<p>Aspects in which they are different:
<ol>
    <li>Project Structure</li>
    <li>Build process</li>
    <li>Deployment strategy</li>
    <li>Profiles</li>
    <li>Testing approach</li>
</ol></p>
<h3 style="margin-left:6px">Difference in Project Structure:</h3>

<p><strong>Seam-gen source directories</strong>
<p style="text-align: justify;">The source directories created by seam-gen are each dedicated to holding certain types of source files. This enables seam-gen to determine what tests need to be executed as well as selectively hot-deploy your Seam components.(<a href="#d12">Diagram 1.2</a>)</p></p>
<table border="1" cellspacing="0" cellpadding="0" width="628">
<tbody>
<tr>
<td  width="104" valign="top">
/src/main
</td>
<td width="498" valign="top">
Classes to include in the main classpath
</td>
</tr>
<tr >
<td width="104" valign="top">
/src/hot
</td>
<td width="498" valign="top">
Classes to include in the hot deployable   classpath when running in development mode.
</td>
</tr>
<tr >
<td width="104" valign="top">
/src/test
</td>
<td  width="498" valign="top">These classes will not be deployed but will be executed as   part of the test target.</td>
</tr>
</tbody></table>

<p style="text-align: justify;">So in this project structure, we only have one source folder which contains both, the EJB entities and the War content. This is one thing, which I don’t like about seam-gen. They could have provided a better project structure like the one we have with Jboss tools.</p>

<p><strong>Jboss tools source directories</strong>
<p style="text-align: left;">Jboss tools source directory is well arranged compared to the seam-gen project directory, as it follow the standard J2EE application packaging.In this struture we have a war project, ejb project and finally both of them bundled into a ear distribution. In addition to that they have separated out a test project, which is good for doing testing.This kind of structure also helps to manage large projects, where we have  teams working on different module of the project.</p></p>
<table  border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="132" valign="top">
HelloSeam
</td>
<td  width="477" valign="top">
Contains the deployment   descriptor, the Web component files, and related resources.
</td>
</tr>
<tr>
<td  width="132" valign="top">
HelloSeam-ear
</td>
<td  width="477" valign="top">
The final ear distribution
</td>
</tr>
<tr>
<td  width="132" valign="top">
HelloSeam-ejb/ejbModule
</td>
<td  width="477" valign="top">
Contains ejb deployment descriptor, the   enterprise bean files, and related files.
</td>
</tr>
<tr>
<td width="132" valign="top">
HelloSeam -test/test-src
</td>
<td  width="477" valign="top">
The test classes.
</td>
</tr>
</tbody></table>

<p><a name="d12"></a>
<p style="margin-left: 36pt; text-align: center;"><img class="aligncenter size-full wp-image-1307" title="seam-gen-project-structure-22" src="http://xebee.xebia.in/wp-content/uploads/2009/04/seam-gen-project-structure-22.png" alt="seam-gen-project-structure-22" /></p></p>
<p style="margin-bottom: 0.0001pt; line-height: normal; text-align: center;">Diagram 1.2 Project struture of Seam-gen and JBoss tools</p>

<h3  style="margin-left:6px">Differences in Build process:</h3>

<p style="text-align: justify;">First let start with the build process seam-gen follows. The seam-gen project is built using the ant tool and they provide an extensive target in the build.xml. So most of what you want to do in different phases of application development, you can do that using the build.xml provided.</p>

<p>Some sample targets which might be of interest to everyone
<table  border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="180" valign="top">
explode , reexplode
</td>
<td width="429" valign="top">
These are for doing the seam   application deployment as a exploded directory.
</td>
</tr>
<tr>
<td width="180" valign="top">
deploy , reploy
</td>
<td width="429" valign="top">
these are for deploying the   seam application as a packaged archive
</td>
</tr>
<tr>
<td width="180" valign="top">
groovy.compile
</td>
<td width="429" valign="top">
If you have some groovy code,   so this would just compile them into java classes
</td>
</tr>
<tr>
<td width="180" valign="top">
restart, restart-deployed,   restart-exploded
</td>
<td width="429" valign="top">
for doing the restart ,   depending on the kind of deployment
</td>
</tr>
<tr>
<td width="180" valign="top">
others like jar, war, javadoc
</td>
<td width="429" valign="top">
Utility targets for generating   different things.
</td>
</tr>
</tbody></table></p>