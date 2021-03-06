---
layout: post
header-img: img/default-blog-pic.jpg
author: Msingh
description: 
post_id: 18354
created: 2014/06/04 15:15:30
created_gmt: 2014/06/04 10:15:30
comment_status: open
---

# Behavior driven development using Cucumber-JVM:- a Java Approach

**Behavior driven development using Cucumber-JVM:- a Java Approach**

Recently I started working with BDD using Cucumber-JVM in one of the project. Behavior driven development (BDD) is based on test driven development (TDD) and it inherits the benefits and practices from TDD but it one step ahead from TDD as it combines the ideas from domain and object oriented design. Main motive to use BDD in software development to make it easy to understand by everyone. BDD provides the mechanism to write specially user acceptance test in easy to understand language so it can be easily written and maintain in order to write code for user acceptance testing.

There are so many tools available in the market that provides supports to the BDD development like Cucumber,Jbehave,Concordion,Twist,easyB,Fitnesse etc. here I’m going to describe the Cucumber and why I have chosen this tool because it has been designed specifically to ensure the acceptance tests can easily be read and written by anyone on the team. The easy readability of Cucumber tests draws business stakeholders into the process, helping you really explore and understand their requirements.

Cucumber is a testing framework that help to reduce the gap between different stakeholders specially software developers and business analyst. When we are using Cucumber then test always get written in plain language based on behavior like style of Given,When, Then and these terms can be easily understand by any layperson even that layperson doesn't understand the tool. Test steps then need to write in feature files that covers one or multiple test scenarios. Here we are using Cucumber-JVM and will be using Java as programming language and internally will be using Selenium Web driver jar to perform different operations on browsers.

Below is the details how we need to setup the Cucumber-JVM environment so we can start writing Cucumber-JVM tests.

**To Set-up the environment for Cucumber we require below products to be install in the machine:-**

  1. **Java/JDK/JRE**
  2. **IDE like Eclipse**
  3. **Build Automation Tool like Maven**
  4. **Cucumber Plug-in for Eclipse**
**     5 . Selenium Web Driver API as external jar files.**

**Java/JDK/JRE**

We need to install java on machine and we can download the JDK from this location**:**<http://www.oracle.com/technetwork/java/javasebusiness/downloads/java-archive-downloads-javase6-419409.html>**. **Note that JDK version comes bundled with Java Run time Environment (i.e. JRE) so we don't need to install the JRE separately. In order to work with Java we need to set some environment variables values and these are Path and Java_Home variable.

How to Set Path Variable:-

a).Right click the Computer (My Computer) icon.

b).Choose Properties from the context menu.

c).Click the Advanced system settings link.

d).Click Environment Variables. In the section System Variables, find the PATH environment variable

and select it. Click Edit. If the PATH environment variable does not exist, click New.

e).In the Edit System Variable (or New System Variable) window, specify the value of

the PATH environment variable. Click OK. Close all remaining windows by clicking OK.

PATH [C:\Program Files (x86)\Java\jre7\bin;%JAVA_HOME%\bin;]-- This can be path of Java_home

variable or directly you can set jdk path in this variable value.

JAVA_HOME[C:\Program Files\Java\jdk1.8.0_05] ---This should be the path where your jdk install in

your machine. same steps(a to e) need to be repeat for JAVA_HOME as well.

**Eclipse**

Then we need to download “Eclipse IDE for Java Developers”. You can download eclipses from this

location: <http://www.eclipse.org/downloads/>.Make sure to choose correct link for downloading eclipses which corresponds to your OS i.e. for Windows 32 Bit and 64 Bit versions. Once you download the specific zip file for eclipse and then you need to just extract the same in any of the drive.

**Maven**

Now we need to install maven and main objective to use maven is that, it makes build process easy and providing a uniform build system with quality project information. You can install Maven plugin for Eclipse via update site, simply copy the update site link address and paste it into Eclipse’s “Update” or “Install New Software” manager . In order to work with Maven we need to set below environment variables:-

Set Maven environment variables:-

a).Right click the Computer (My Computer) icon.

b). Choose Properties from the context menu.