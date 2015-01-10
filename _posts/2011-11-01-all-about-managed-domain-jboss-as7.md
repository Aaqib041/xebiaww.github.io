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

<p>In a series of blogs I will deal with different aspects/features of  JBoss AS7. This is first blog of the series. In this blog I will discuss a new feature in AS-7 called Managed Domain. After reading this blog readers should have good understanding of the managed domain, its use and advantages.</p>
<!--more-->

<p>Before I start the discussion on Managed Domain, it is  important to have a good understanding of the directory structure of  AS7 server. To read about directory structure click <a href="https://docs.jboss.org/author/display/AS7/Getting+Started+Guide#GettingStartedGuide-AS7DirectoryStructure">here</a>.</p>
<p><strong>What is a domain</strong></p>
<p>As developers all of us know that a single instance of JBoss server can be started by starting the JBoss installation in standalone mode. This is true for AS7 also. But the real world UAT or Production setup almost always has a cluster of JBoss application servers running from JBoss installations in different virtual/physical machines. The collection of such JBoss AS servers is called a domain.
<div><a href="http://xebee.xebia.in/wp-content/uploads/2011/10/ManagedDomain1.png" target="_blank"><img class="aligncenter size-medium wp-image-9975" title="ManagedDomain" src="http://xebee.xebia.in/wp-content/uploads/2011/10/ManagedDomain1-300x182.png" alt="" width="300" height="182" /></a></div>
<strong>What is a managed domain</strong></p>
<p>If all the servers of a domain have to be managed independently then tasks like app deployment, configuration fine tuning, starting/stopping the server will have to be done for each servers individually. Suppose the servers in the  domain are divided into two groups, and say group-1 should have common apps deployed and a common start-up configuration then it becomes the responsibility of the administrator to ensure the details.</p>
<p>Managed Domain is one of the primary new features of AS7 which provides a single point of control to all application servers in the domain. Before we see how Managed Domain simplifies the task of managing the servers in a domain let us first understand how Managed Domain works on JBoss AS7.</p>
<p><strong>What is Domain Mode</strong></p>
<p>If  there is a need for a mutliple JBoss installation - multi server setup and want them to be Domain Managed then the JBoss installation should be started in Domain mode. To do that go to &lt;JBoss Home&gt;/bin folder and run domain.bat or domain.sh. Definitely some configuration information will have to be fed into some configuration file before you run the domain.sh. I will discuss the configuration details in my next blog.</p>
<p>When the JBoss instance is started in Domain mode then the processes HostController and the DomainController come into existence. HostController and Domain Controller are the basic processes which manage the server instances and help create a managed domain.</p>
<p>Summarily it can be said that HostControllers are responsible for managing all the server instances created from a single JBoss installation. Whereas the DomainController manages the complete domain that  includes all the servers in the domain by talking to all the HostControllers (one per JBoss installation) . Which means all the HostControllers act as slaves to the DomainController.</p>
<p><strong>HostController</strong></p>
<p>When you run the domain.sh/bat file the second line of info log will be</p>
<p>12:08:25,543 INFO  [org.jboss.as.process.Host Controller.status] (main) <strong>Starting process 'Host Controller'</strong>.</p>
<p>Which means that one of the first few process which start as part of domain mode start-up is the HostController.</p>
<p><strong>What is a HostController</strong></p>
<p>HostController is the process which is responsible for
<ol>
    <li>Starting up the servers it is supposed to start.</li>
    <li>Provide the start up options and configuration information to each server it has started.</li>
    <li>Expose the management and public interface to the servers started by it.</li>
    <li>Administer the servers under its purview through the management interfaces.</li>
    <li>Start up as Domain controller or register with the DomainController of the domain, as slave (Please wait till we discuss the DomainController).</li>
</ol>
Starting up the servers it is supposed to start????? What does that mean? To understand this we will have to navigate to &lt;JBoss Home&gt;/domain/configuration folder. There you will find the file named host.xml. When the HostController starts up it refers to this file to take instructions/configuration information which you as administrator have provided. So let us take a brief look at what is host.xml all about:</p>
<p><strong>Host.xml contains</strong>
<ul>
    <li><strong>Listing of servers</strong> - which the HostController should start. In the server element the name attribute uniquely identifies the server. 'group' attribute groups the servers of a domain into different groups. Just keep this in mind. We will discuss the grouping in detail when we discuss the DomainController. The auto-start attribute tells the HostController to auto-start the server on start-up or to suspend starting till it is explicitly instructed to start the server through the management interface. And other start up parameters can be passed to each server element which will be passed on by HostController to servers while starting them up.</li>
</ul>
<em>Lsiting 1-1</em></p>
<p>[sourcecode type="xml"]
&lt;servers&gt;
 &lt;server name=&quot;server-one&quot; group=&quot;main-server-group&quot; auto-start=&quot;true&quot;&gt;
 &lt;jvm name=&quot;default&quot;/&gt;
 &lt;/server&gt;
 &lt;server name=&quot;server-two&quot; group=&quot;main-server-group&quot; auto-start=&quot;true&quot;&gt;
 &lt;jvm name=&quot;default&quot;&gt;
 &lt;heap size=&quot;64m&quot; max-size=&quot;256m&quot;/&gt;
 &lt;/jvm&gt;
 &lt;socket-binding-group ref=&quot;standard-sockets&quot; port-offset=&quot;150&quot;/&gt;
 &lt;/server&gt;
 &lt;servers&gt;
[/sourcecode]
<ul>
    <li><strong>Management interface </strong> - Host controller also exposes the management interfaces. Management interface can be native or HTTP. Exposing the native interface is mandatory, because the DomainController uses the native interface to interact with the HostController and pass on the admin instructions to HostController. More on this when we discuss the DomainController.</li>
</ul>
<ul>
    <li><strong>Master/Slave configuration </strong> - The host controller can be configured to act as the DomainController or the Master HostController of the Domain. To do that just add a &lt;local/&gt; element to the <em>domain-controller </em>declaration. Look at <em>Listing 1-2</em>. To make all other HostController in the domain slave HostControllers just add a remote element in the <em>domain-controller</em> declaration. The remote element provides the inet-addr of the DomainController and the port at which it is available. Please look at<em> Listing 1-3</em>. HostController uses these details to register itself with the domain controller at start up.</li>
</ul>
<em>Listing 1-2</em></p>
<p>[sourcecode type="xml"]
&lt;domain-controller&gt;
 &lt;local/&gt;
&lt;/domain-controller&gt;
[/sourcecode]</p>
<p><em>Listing 1-3</em></p>
<p>[sourcecode type="xml"]
&lt;domain-controller&gt;
 &lt;remote host=&quot;192.168.1.143&quot; port=&quot;9999&quot;/&gt;
&lt;/domain-controller&gt;
[/sourcecode]</p>
<p><strong>Conclusions</strong>
<ol>
    <li>Every installation of JBoss servers when started in domain mode will have one HostController per host machine.</li>
    <li>HostController can start zero, one or many servers, based on the server declarations in host.xml.</li>
    <li>One HostController in a domain acts as DomainController(Master).</li>
    <li>All other HostControllers act as slaves to the DomainController.</li>
    <li>The HostControllers expose a native management interface so that the HostControllers can communicate with the DomainController and vice-versa.</li>
</ol>
<strong>DomainController</strong></p>

## Comments

**[Shankar Jha](#6429 "2011-12-22 09:02:19"):** Thanks Bernd for your inputs. Its good to get the feedback and understand that the words are not communicating the exact meaning to the readers. I have changed the sentence. Can you please see if the changed sentence conveys the right meaning. Regarding ProcessController, I believe that is implicit and not JBoss specific.

**[Martin](#6431 "2011-12-22 13:49:50"):** Good to see someone expanding on the available documentatie. thanks for that. Got a few questions: 1.Could you send me examples of a working domain, with multiple servers. I would like to know how the host.xml and domain.xml is configured. I am not sure what and what not to alter in those files. 2\. getting the error Failed to start server (server-two): java.util.NoSuchElementException: No child 'main-server-group' exists What does this mean?

**[bateka](#6162 "2011-11-09 20:48:37"):** Excellent article explaining clearly what domain controller/hostcontroler is. Thank you a lot

**[Enthusiastic](#6222 "2011-11-18 13:49:07"):** Can domain be configured in such a way that at the time of peak load a new node is added and hence correspondingly removed based on load shedding? The reason why I am asking this question is that this configuration if not provided by the domain then it would be very difficult to manage by scripts. As a result I would have no choice than to choose cloud envt rather than AS7 managed domain.

**[Bernd](#6205 "2011-11-15 22:30:11"):** You should mention the Process-Controller here, which is the JVM spawning all other VMs on a Host (including the host-controller). And also fixe the line "Every installation of JBoss when started in domain mode will have one HostController." to "Every installation of JBoss when started in domain mode will have one HostController per host." (since "installation" in some cases refers to a whole system and sometimes one host)

**[sanjay](#7104 "2012-01-24 15:42:05"):** Hi, I am starting jboss 7.1 in domain mode using domain.sh in linux environment. but it is not started and error shows as 16:06:47,862 INFO [org.jboss.modules] (main) JBoss Modules version 1.1.0.CR6 16:06:48,049 INFO [org.jboss.as.process.Host Controller.status] (main) JBAS012017: Starting process 'Host Controller' 16:06:48,049 ERROR [org.jboss.as.process.Host Controller.status] (main) JBAS012008: Failed to start process 'Host Controller' Can you please give me suggestion on this issue? its urgent. you can mail me your suggestion to sanjay.6655@gmail.com Cheers

**[Yougal](#9292 "2012-09-28 09:54:35"):** Fantastic blog. Nicely explained.

**[bob@akuacom.com](#9307 "2012-12-18 05:37:27"):** Not sure if you have done the other referenced blogs mentioned within this blog or if I am just not seeming them. Any chance you are going to post the other articles?

