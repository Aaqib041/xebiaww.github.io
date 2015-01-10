---
layout: post
header-img: img/default-blog-pic.jpg
author: sprasadjha
description: 
post_id: 9915
created: 2011/11/01 06:28:10
created_gmt: 2011/11/01 01:28:10
comment_status: open
---

# All about Managed Domain - JBoss AS7

In a series of blogs I will deal with different aspects/features of JBoss AS7. This is first blog of the series. In this blog I will discuss a new feature in AS-7 called Managed Domain. After reading this blog readers should have good understanding of the managed domain, its use and advantages.

Before I start the discussion on Managed Domain, it is important to have a good understanding of the directory structure of AS7 server. To read about directory structure click [here][1].

**What is a domain**

As developers all of us know that a single instance of JBoss server can be started by starting the JBoss installation in standalone mode. This is true for AS7 also. But the real world UAT or Production setup almost always has a cluster of JBoss application servers running from JBoss installations in different virtual/physical machines. The collection of such JBoss AS servers is called a domain. 

![][2]

**What is a managed domain**

If all the servers of a domain have to be managed independently then tasks like app deployment, configuration fine tuning, starting/stopping the server will have to be done for each servers individually. Suppose the servers in the domain are divided into two groups, and say group-1 should have common apps deployed and a common start-up configuration then it becomes the responsibility of the administrator to ensure the details.

Managed Domain is one of the primary new features of AS7 which provides a single point of control to all application servers in the domain. Before we see how Managed Domain simplifies the task of managing the servers in a domain let us first understand how Managed Domain works on JBoss AS7.

**What is Domain Mode**

If there is a need for a mutliple JBoss installation - multi server setup and want them to be Domain Managed then the JBoss installation should be started in Domain mode. To do that go to <JBoss Home>/bin folder and run domain.bat or domain.sh. Definitely some configuration information will have to be fed into some configuration file before you run the domain.sh. I will discuss the configuration details in my next blog.

When the JBoss instance is started in Domain mode then the processes HostController and the DomainController come into existence. HostController and Domain Controller are the basic processes which manage the server instances and help create a managed domain.

Summarily it can be said that HostControllers are responsible for managing all the server instances created from a single JBoss installation. Whereas the DomainController manages the complete domain that  includes all the servers in the domain by talking to all the HostControllers (one per JBoss installation) . Which means all the HostControllers act as slaves to the DomainController.

**HostController**

When you run the domain.sh/bat file the second line of info log will be

12:08:25,543 INFO [org.jboss.as.process.Host Controller.status] (main) **Starting process 'Host Controller'**.

Which means that one of the first few process which start as part of domain mode start-up is the HostController.

**What is a HostController**

HostController is the process which is responsible for 

  1. Starting up the servers it is supposed to start.
  2. Provide the start up options and configuration information to each server it has started.
  3. Expose the management and public interface to the servers started by it.
  4. Administer the servers under its purview through the management interfaces.
  5. Start up as Domain controller or register with the DomainController of the domain, as slave (Please wait till we discuss the DomainController).
Starting up the servers it is supposed to start????? What does that mean? To understand this we will have to navigate to <JBoss Home>/domain/configuration folder. There you will find the file named host.xml. When the HostController starts up it refers to this file to take instructions/configuration information which you as administrator have provided. So let us take a brief look at what is host.xml all about:

**Host.xml contains**

  * **Listing of servers** \- which the HostController should start. In the server element the name attribute uniquely identifies the server. 'group' attribute groups the servers of a domain into different groups. Just keep this in mind. We will discuss the grouping in detail when we discuss the DomainController. The auto-start attribute tells the HostController to auto-start the server on start-up or to suspend starting till it is explicitly instructed to start the server through the management interface. And other start up parameters can be passed to each server element which will be passed on by HostController to servers while starting them up.
_Lsiting 1-1_

[sourcecode type="xml"] <servers> <server name="server-one" group="main-server-group" auto-start="true"> <jvm name="default"/> </server> <server name="server-two" group="main-server-group" auto-start="true"> <jvm name="default"> <heap size="64m" max-size="256m"/> </jvm> <socket-binding-group ref="standard-sockets" port-offset="150"/> </server> <servers> [/sourcecode] 

  * **Management interface ** \- Host controller also exposes the management interfaces. Management interface can be native or HTTP. Exposing the native interface is mandatory, because the DomainController uses the native interface to interact with the HostController and pass on the admin instructions to HostController. More on this when we discuss the DomainController.
  * **Master/Slave configuration ** \- The host controller can be configured to act as the DomainController or the Master HostController of the Domain. To do that just add a <local/> element to the _domain-controller _declaration. Look at _Listing 1-2_. To make all other HostController in the domain slave HostControllers just add a remote element in the _domain-controller_ declaration. The remote element provides the inet-addr of the DomainController and the port at which it is available. Please look at_ Listing 1-3_. HostController uses these details to register itself with the domain controller at start up.
_Listing 1-2_

[sourcecode type="xml"] <domain-controller> <local/> </domain-controller> [/sourcecode]

_Listing 1-3_

[sourcecode type="xml"] <domain-controller> <remote host="192.168.1.143" port="9999"/> </domain-controller> [/sourcecode]

**Conclusions**

  1. Every installation of JBoss servers when started in domain mode will have one HostController per host machine.
  2. HostController can start zero, one or many servers, based on the server declarations in host.xml.
  3. One HostController in a domain acts as DomainController(Master).
  4. All other HostControllers act as slaves to the DomainController.
  5. The HostControllers expose a native management interface so that the HostControllers can communicate with the DomainController and vice-versa.
**DomainController**

   [1]: https://docs.jboss.org/author/display/AS7/Getting+Started+Guide#GettingStartedGuide-AS7DirectoryStructure
   [2]: http://xebee.xebia.in/wp-content/uploads/2011/10/ManagedDomain1-300x182.png (ManagedDomain)

## Comments

**[Shankar Jha](#6429 "2011-12-22 09:02:19"):** Thanks Bernd for your inputs. Its good to get the feedback and understand that the words are not communicating the exact meaning to the readers. I have changed the sentence. Can you please see if the changed sentence conveys the right meaning. Regarding ProcessController, I believe that is implicit and not JBoss specific.

**[Martin](#6431 "2011-12-22 13:49:50"):** Good to see someone expanding on the available documentatie. thanks for that. Got a few questions: 1.Could you send me examples of a working domain, with multiple servers. I would like to know how the host.xml and domain.xml is configured. I am not sure what and what not to alter in those files. 2\. getting the error Failed to start server (server-two): java.util.NoSuchElementException: No child 'main-server-group' exists What does this mean?

**[bateka](#6162 "2011-11-09 20:48:37"):** Excellent article explaining clearly what domain controller/hostcontroler is. Thank you a lot

**[Enthusiastic](#6222 "2011-11-18 13:49:07"):** Can domain be configured in such a way that at the time of peak load a new node is added and hence correspondingly removed based on load shedding? The reason why I am asking this question is that this configuration if not provided by the domain then it would be very difficult to manage by scripts. As a result I would have no choice than to choose cloud envt rather than AS7 managed domain.

**[Bernd](#6205 "2011-11-15 22:30:11"):** You should mention the Process-Controller here, which is the JVM spawning all other VMs on a Host (including the host-controller). And also fixe the line "Every installation of JBoss when started in domain mode will have one HostController." to "Every installation of JBoss when started in domain mode will have one HostController per host." (since "installation" in some cases refers to a whole system and sometimes one host)

**[sanjay](#7104 "2012-01-24 15:42:05"):** Hi, I am starting jboss 7.1 in domain mode using domain.sh in linux environment. but it is not started and error shows as 16:06:47,862 INFO [org.jboss.modules] (main) JBoss Modules version 1.1.0.CR6 16:06:48,049 INFO [org.jboss.as.process.Host Controller.status] (main) JBAS012017: Starting process 'Host Controller' 16:06:48,049 ERROR [org.jboss.as.process.Host Controller.status] (main) JBAS012008: Failed to start process 'Host Controller' Can you please give me suggestion on this issue? its urgent. you can mail me your suggestion to sanjay.6655@gmail.com Cheers

**[Yougal](#9292 "2012-09-28 09:54:35"):** Fantastic blog. Nicely explained.

**[bob@akuacom.com](#9307 "2012-12-18 05:37:27"):** Not sure if you have done the other referenced blogs mentioned within this blog or if I am just not seeming them. Any chance you are going to post the other articles?

