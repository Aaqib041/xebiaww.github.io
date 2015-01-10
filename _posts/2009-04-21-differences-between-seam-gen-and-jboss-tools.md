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

In this article we will basically try to understand the basic difference between different ways in which we can do Seam application development. So before we get into details, let have a brief introduction about Seam

**So what is Seam??**

Seam is a powerful open source development platform for building rich Internet applications in Java. Seam integrates technologies such as Asynchronous JavaScript and XML (AJAX), JavaServer Faces (JSF), Java Persistence (JPA), Enterprise Java Beans (EJB 3.0) and Business Process Management (BPM) into a unified full-stack solution, complete with sophisticated tooling(1). The simple chart of architectural design will show you about this:

![design-2][1]

Diagram 1.1 Architecute of Seam

So Seam basically does the job of plumbing these disconnected frameworks. We had all the good things in J2EE world, but making them work together was a pain. But, now we do have Seam to do all the dirty work for us.

How to get started with Seam application development?? Following could be the possible ways in which we could get started with a simple seam project structure.

  * Seam- gen
  * Jboss Tools
  * Maven

It was really fun to work on Seam, as it greatly reduced the initial project setup time which we normally have with J2EE project. Seam is basically a tool which give you more Return on Investment (ROI) for the same number of lines you write in any other Java framework like spring or struts.

So, let’s get started with the main aim of this article to have feature differentitation between seam-gen and Jboss Tools .

First of all I would write something about both the Tools and after that will try to get into more details about each.

### Seam-gen

We will start with using seam-gen to generate a project structure, as seam-gen provides out-of –box scripts to generate Seam Action, Entity and Conversations

**So what is Seam-gen??**

Seam-gen is a rapid application generator shipped with the Seam distribution. With a few command line commands, seam-gen generates an array of artifacts for Seam projects to help you get started. In particular, seam-gen is useful for the following.

  * Automatically generate an empty Seam project with common configuration files, a build script, and directories for Java code and JSF view pages.
  * Automatically generate complete Eclipse and NetBeans project files for the Seam project.
  * Reverse-engineer entity bean objects from relational database tables.
  * Generate template files for common Seam components.

### Jboss tools

JBoss Tools is a collection of Eclipse plugins. JBoss Tools a project creation wizard for Seam, Content Assist for the Unified Expression Language (EL) in both facelets and Java code, a graphical editor for jPDL, a graphical editor for Seam configuration files, support for running Seam integration tests from within Eclipse, and much more. In short, if you are an Eclipse user, then you'll want to use JBoss Tools!

Now, we will mainly look at different aspects in which seam-gen and Jboss Tools are different and what could be the best option for building Seam application from scratch

Aspects in which they are different: 

  1. Project Structure
  2. Build process
  3. Deployment strategy
  4. Profiles
  5. Testing approach

### Difference in Project Structure:

**Seam-gen source directories**

The source directories created by seam-gen are each dedicated to holding certain types of source files. This enables seam-gen to determine what tests need to be executed as well as selectively hot-deploy your Seam components.(Diagram 1.2)

/src/main 
Classes to include in the main classpath 

/src/hot 
Classes to include in the hot deployable classpath when running in development mode. 

/src/test 
These classes will not be deployed but will be executed as part of the test target.

So in this project structure, we only have one source folder which contains both, the EJB entities and the War content. This is one thing, which I don’t like about seam-gen. They could have provided a better project structure like the one we have with Jboss tools.

**Jboss tools source directories**

Jboss tools source directory is well arranged compared to the seam-gen project directory, as it follow the standard J2EE application packaging.In this struture we have a war project, ejb project and finally both of them bundled into a ear distribution. In addition to that they have separated out a test project, which is good for doing testing.This kind of structure also helps to manage large projects, where we have teams working on different module of the project.

HelloSeam 
Contains the deployment descriptor, the Web component files, and related resources. 

HelloSeam-ear 
The final ear distribution 

HelloSeam-ejb/ejbModule 
Contains ejb deployment descriptor, the enterprise bean files, and related files. 

HelloSeam -test/test-src 
The test classes. 

![seam-gen-project-structure-22][2]

Diagram 1.2 Project struture of Seam-gen and JBoss tools

### Differences in Build process:

First let start with the build process seam-gen follows. The seam-gen project is built using the ant tool and they provide an extensive target in the build.xml. So most of what you want to do in different phases of application development, you can do that using the build.xml provided.

Some sample targets which might be of interest to everyone 

explode , reexplode 
These are for doing the seam application deployment as a exploded directory. 

deploy , reploy 
these are for deploying the seam application as a packaged archive 

groovy.compile 
If you have some groovy code, so this would just compile them into java classes 

restart, restart-deployed, restart-exploded 
for doing the restart , depending on the kind of deployment 

others like jar, war, javadoc 
Utility targets for generating different things. 

   [1]: http://xebee.xebia.in/wp-content/uploads/2009/04/design-2.png (design-2)
   [2]: http://xebee.xebia.in/wp-content/uploads/2009/04/seam-gen-project-structure-22.png (seam-gen-project-structure-22)