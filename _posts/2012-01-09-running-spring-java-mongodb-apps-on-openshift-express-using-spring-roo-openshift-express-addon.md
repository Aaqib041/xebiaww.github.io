---
layout: post
header-img: img/default-blog-pic.jpg
author: shekhargulati
description: 
post_id: 10890
created: 2012/01/09 01:54:08
created_gmt: 2012/01/08 20:54:08
comment_status: open
---

# Running Spring Java MongoDB Apps on OpenShift Express Using Spring Roo OpenShift Express Addon

<p>Last couple of years Spring Roo has been one of my favorite tool to rapidly build Spring applications for demo, POC, and learning new technologies. With Spring Roo Cloud Foundry integration you can not only build applications rapidly but deploy to Cloud Foundry public cloud with a couple of commands. In case you are not aware of Spring Roo and Cloud Foundry you can refer to my <a href="http://www.ibm.com/developerworks/views/opensource/libraryview.jsp?search_by=introducing+spring+roo,">Spring Roo series on IBM DeveloperWorks</a>. From last 5-6 months I have been following OpenShift platform as a service and I am really in love with OpenShift Express because of its feature set and simplicity. In case you are not aware of OpenShift Express please refer to the <a href="https://docs.redhat.com/docs/en-US/OpenShift_Express/1.0/html/User_Guide/index.html">documentation</a>. There are two ways Java applications can be deployed to express --- using ruby <a href="https://docs.redhat.com/docs/en-US/OpenShift_Express/1.0/html/User_Guide/sect-User_Guide-Application_Development-Creating_Applications.html#sect-User_Guide-Creating_Applications-Using_the_Command_Line">command line tool called RHC</a> and using Eclipse plugin. Personally I like rhc command line more than eclipse plugin. The rhc command line tool is great but you should have ruby runtime installed on your machine. Being Spring Roo afficianado I decided to write Spring Roo OpenShift Express add-on to create, deploy, add cartridges from with Roo shell. This can be thought of as a third way to deploy applications to OpenShift Express. The project is hosted on Google Code at <a href="http://code.google.com/p/spring-roo-openshift-express-addon/">http://code.google.com/p/spring-roo-openshift-express-addon/</a>. In this blog I will show you how you can install the add-on, create a template OpenShift Express application, convert that application to a Spring MongoDB application using Spring Roo, and finally deploy application to OpenShift Express. All of this will be done from with in Roo shell.<!--more--></p>
<p><strong>Prerequisites</strong>
<ol>
    <li>Sign up at <a href="https://openshift.redhat.com/app/express">https://openshift.redhat.com/app/express</a></li>
    <li>Download Spring Roo 1.2 Release from <a href="http://s3.amazonaws.com/dist.springframework.org/release/ROO/spring-roo-1.2.0.RELEASE.zip">Spring Roo website</a>.</li>
    <li>Git. OpenShift uses git to deploy applications to OpenShift Express cloud.</li>
    <li>SSH keys. OpenShift uses ssh keys for authentication. Please refer to <a href="https://docs.redhat.com/docs/en-US/OpenShift_Express/1.0/html/User_Guide/sect-User_Guide-Authenticating_SSH_Keys.html#sect-User_Guide-Authenticating_SSH_Keys-Generating_new_SSH_keys">documentation for generating your ssh keys</a>.</li>
</ol>
<strong>Lets Get Started</strong>
<ol>
    <li>After you have installed Spring Roo on your machine fire the roo command to load the roo shell.</li>
    <li>Install the add-on by executing the following command.
[sourcecode lang="text"]
osgi start --url http://spring-roo-openshift-express-addon.googlecode.com/files/org.xebia.roo.addon.openshift-0.1.RELEASE.jar
[/sourcecode]</p>
<p>This command will take couple of seconds to execute as it has to download the jar.</li>
    <li>Verify that add-on is active by viewing all the osgi process using osgi ps command. You will see output like as shown below that openshift add-on is Active.
[sourcecode lang="text"]
[ 166] [Active     ] [    1] spring-roo-openshift-express-addon (0.1.0.RELEASE)
[/sourcecode]</li>
    <li>You can view all the available add-on command by typing help command as shown below.
[sourcecode lang="text"]
roo&gt; help --command rhc
<em> rhc app destroy - Destroys the application
</em> rhc app restart - Restarts the deployed application
<em> rhc app start - Starts the deployed application
</em> rhc app status - Gives status of the deployed application
<em> rhc app stop - stops the deployed application
</em> rhc cartridge add - Adds a cartridge to the application
<em> rhc cartridge list - Lists all the avaliable embeddable cartridges
</em> rhc create jbossas7 app - Creates a new JBoss AS7 OpenShift Express Application
<em> rhc create jenkins app - Creates a new Jenkins OpenShift Express Application
</em> rhc deploy - Deploys code to OpenShift Express
<em> rhc pom - Adds OpenShift Profile to pom.xml
</em> rhc user info - Displays information about user's OpenShift Express applications</p>
<p>[/sourcecode]</p>
<p>You can also view commands by typing rhc and then pressing tab. As you can see from the commands you can do a lot of stuff with this add-on. You can create the application (both jboss and jenkins), do application management (start, stop, destroy, status), view user information, add cartridges (MySQL, MongoDB, Jenkins client, RockMongo client, PhpMyAdmin etc ) and adding OpenShift profile in pom.xml.</li>
    <li>Now that add-on has been successfully installed we can create the application. But before you can create the application you have to make sure that you have created the domain. Currently the add-on does not support creating domain but I will be adding support for creating domain in next release. In case you have not created domain please refer to the <a href="https://docs.redhat.com/docs/en-US/OpenShift_Express/1.0/html/User_Guide/sect-User_Guide-Application_Development-Creating_a_Domain.html">documentation to create one</a>. The domain name should be unique. I have created domain name called xke which stands for Xebia Knowledge Exchange.</li>
    <li>You can view the information of the user like all the applications created by user using rhc user info command. This is same as rhc-user-info command.</li>
    <li>Lets create a simple application  called bookshop which will be simple CRUD application for creating books.  The application will be using MongoDB as datastore. The Java applications created in Express are deployed on JBoss AS 7 server. To create the application type the command shown below.
[sourcecode lang="text"]
rhc create jbossas7 app --applicationName bookshop --rhLogin &lt;rhlogin&gt; --password &lt;please&gt;
[/sourcecode]</p>
<p>Please type the email and password with which you registered for express. You will see output like as shown below. This command first create the default application, clone the git repository, and deploy the application on express. You can view the application at</p>
<p>[sourcecode lang="text"]
Loaded OpenShift configuration from /home/shekhar/.openshift/express.conf
Waiting for OpenShift to propagate DNS
Trying to contact https://bookshop-xke.rhcloud.com/ (attempt 1 of 500)
Git Clone Command : git clone ssh://b85866270d85472886c7f2611b7774ad@bookshop-xke.rhcloud.com/~/git/bookshop.git/ bookshop
Initialized empty Git repository in /home/shekhar/dev/playground/bookshop/.git/
Application Path ./bookshop
Cloned the git repository
Application created and deployed to https://bookshop-xke.rhcloud.com/
[/sourcecode]</li>
    <li>Quit the Roo shell and cd into bookshop directory and you will see that
bookshop is a maven based project. Delete these files and commit using git as shown below.
[sourcecode lang="text"]
git rm -rf src pom.xml
git commit -m &quot;deleting default files&quot;
[/sourcecode]</li>
    <li>Fire the roo command again. Make sure that you are in bookshop directory.</li>
    <li>Add the MongoDB cartridge to your application using rhc cartridge add command as shown below.
[sourcecode lang="text"]
roo&gt; rhc cartridge add --applicationName bookshop --rhLogin &lt;email&gt; --password &lt;password&gt; --cartridge MONGODB
Loaded OpenShift configuration from /home/shekhar/.openshift/express.conf
Embedded cartridge: mongodb-2.0</p>
<p>MongoDB 2.0 database added.  Please make note of these credentials:</p>
<pre><code>   Root User: admin
</code></pre>
<p>Root Password: j7eCdNsjANyY
   Database Name: bookshop
  Connection URL: mongodb://127.1.13.1:27017/</p>
<p>You can manage your new MongoDB by also embedding rockmongo-1.1
[/sourcecode]</li>
    <li>Now that we have added the MongoDB cartridge to our application lets create a simple Spring MongoDB application using Spring Roo. Create the project using Spring Roo project command as shown below.
[sourcecode lang="text"]</p>

## Comments

**[Jayaram Sankaranarayanan](#7546 "2012-02-13 00:55:00"):** Hi Shekhar, Thanks for the informative article. I attended your presentation in JUDCOn2012 and got charged up and tried out a sample app built using Roo on openshift :) Here are the additional steps I had to do to get my app running. Update applciationcontext-mongo.xml to add the attributes mongo.username, mongo.password and mongo.dbname to the mon:dbFactory bean defintion Access the mogo shell of openshift and create the collections. Thought of letting you know about this. regards, Jayaram.

**[Sunil Prakash Inteti](#7975 "2012-03-20 11:19:33"):** Nice article. A recap of the XKE sesson.

