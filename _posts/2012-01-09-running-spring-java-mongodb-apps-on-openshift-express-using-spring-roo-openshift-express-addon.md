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

Last couple of years Spring Roo has been one of my favorite tool to rapidly build Spring applications for demo, POC, and learning new technologies. With Spring Roo Cloud Foundry integration you can not only build applications rapidly but deploy to Cloud Foundry public cloud with a couple of commands. In case you are not aware of Spring Roo and Cloud Foundry you can refer to my [Spring Roo series on IBM DeveloperWorks][1]. From last 5-6 months I have been following OpenShift platform as a service and I am really in love with OpenShift Express because of its feature set and simplicity. In case you are not aware of OpenShift Express please refer to the [documentation][2]. There are two ways Java applications can be deployed to express --- using ruby [command line tool called RHC][3] and using Eclipse plugin. Personally I like rhc command line more than eclipse plugin. The rhc command line tool is great but you should have ruby runtime installed on your machine. Being Spring Roo afficianado I decided to write Spring Roo OpenShift Express add-on to create, deploy, add cartridges from with Roo shell. This can be thought of as a third way to deploy applications to OpenShift Express. The project is hosted on Google Code at <http://code.google.com/p/spring-roo-openshift-express-addon/>. In this blog I will show you how you can install the add-on, create a template OpenShift Express application, convert that application to a Spring MongoDB application using Spring Roo, and finally deploy application to OpenShift Express. All of this will be done from with in Roo shell.

**Prerequisites**

  1. Sign up at <https://openshift.redhat.com/app/express>
  2. Download Spring Roo 1.2 Release from [Spring Roo website][4].
  3. Git. OpenShift uses git to deploy applications to OpenShift Express cloud.
  4. SSH keys. OpenShift uses ssh keys for authentication. Please refer to [documentation for generating your ssh keys][5].
**Lets Get Started**

  1. After you have installed Spring Roo on your machine fire the roo command to load the roo shell.
  2. Install the add-on by executing the following command. [sourcecode lang="text"] osgi start --url http://spring-roo-openshift-express-addon.googlecode.com/files/org.xebia.roo.addon.openshift-0.1.RELEASE.jar [/sourcecode]

This command will take couple of seconds to execute as it has to download the jar.
  3. Verify that add-on is active by viewing all the osgi process using osgi ps command. You will see output like as shown below that openshift add-on is Active. [sourcecode lang="text"] [ 166] [Active ] [ 1] spring-roo-openshift-express-addon (0.1.0.RELEASE) [/sourcecode]
  4. You can view all the available add-on command by typing help command as shown below. [sourcecode lang="text"] roo> help --command rhc _ rhc app destroy - Destroys the application _ rhc app restart - Restarts the deployed application _ rhc app start - Starts the deployed application _ rhc app status - Gives status of the deployed application _ rhc app stop - stops the deployed application _ rhc cartridge add - Adds a cartridge to the application _ rhc cartridge list - Lists all the avaliable embeddable cartridges _ rhc create jbossas7 app - Creates a new JBoss AS7 OpenShift Express Application _ rhc create jenkins app - Creates a new Jenkins OpenShift Express Application _ rhc deploy - Deploys code to OpenShift Express _ rhc pom - Adds OpenShift Profile to pom.xml _ rhc user info - Displays information about user's OpenShift Express applications

[/sourcecode]

You can also view commands by typing rhc and then pressing tab. As you can see from the commands you can do a lot of stuff with this add-on. You can create the application (both jboss and jenkins), do application management (start, stop, destroy, status), view user information, add cartridges (MySQL, MongoDB, Jenkins client, RockMongo client, PhpMyAdmin etc ) and adding OpenShift profile in pom.xml.
  5. Now that add-on has been successfully installed we can create the application. But before you can create the application you have to make sure that you have created the domain. Currently the add-on does not support creating domain but I will be adding support for creating domain in next release. In case you have not created domain please refer to the [documentation to create one][6]. The domain name should be unique. I have created domain name called xke which stands for Xebia Knowledge Exchange.
  6. You can view the information of the user like all the applications created by user using rhc user info command. This is same as rhc-user-info command.
  7. Lets create a simple application  called bookshop which will be simple CRUD application for creating books.  The application will be using MongoDB as datastore. The Java applications created in Express are deployed on JBoss AS 7 server. To create the application type the command shown below. [sourcecode lang="text"] rhc create jbossas7 app --applicationName bookshop --rhLogin <rhlogin> \--password <please> [/sourcecode]

Please type the email and password with which you registered for express. You will see output like as shown below. This command first create the default application, clone the git repository, and deploy the application on express. You can view the application at

[sourcecode lang="text"] Loaded OpenShift configuration from /home/shekhar/.openshift/express.conf Waiting for OpenShift to propagate DNS Trying to contact https://bookshop-xke.rhcloud.com/ (attempt 1 of 500) Git Clone Command : git clone ssh://b85866270d85472886c7f2611b7774ad@bookshop-xke.rhcloud.com/~/git/bookshop.git/ bookshop Initialized empty Git repository in /home/shekhar/dev/playground/bookshop/.git/ Application Path ./bookshop Cloned the git repository Application created and deployed to https://bookshop-xke.rhcloud.com/ [/sourcecode]
  8. Quit the Roo shell and cd into bookshop directory and you will see that bookshop is a maven based project. Delete these files and commit using git as shown below. [sourcecode lang="text"] git rm -rf src pom.xml git commit -m "deleting default files" [/sourcecode]
  9. Fire the roo command again. Make sure that you are in bookshop directory.
  10. Add the MongoDB cartridge to your application using rhc cartridge add command as shown below. [sourcecode lang="text"] roo> rhc cartridge add --applicationName bookshop --rhLogin <email> \--password <password> \--cartridge MONGODB Loaded OpenShift configuration from /home/shekhar/.openshift/express.conf Embedded cartridge: mongodb-2.0

MongoDB 2.0 database added. Please make note of these credentials:
    
           Root User: admin
    

Root Password: j7eCdNsjANyY Database Name: bookshop Connection URL: mongodb://127.1.13.1:27017/

You can manage your new MongoDB by also embedding rockmongo-1.1 [/sourcecode]
  11. Now that we have added the MongoDB cartridge to our application lets create a simple Spring MongoDB application using Spring Roo. Create the project using Spring Roo project command as shown below. [sourcecode lang="text"]

   [1]: http://www.ibm.com/developerworks/views/opensource/libraryview.jsp?search_by=introducing+spring+roo,
   [2]: https://docs.redhat.com/docs/en-US/OpenShift_Express/1.0/html/User_Guide/index.html
   [3]: https://docs.redhat.com/docs/en-US/OpenShift_Express/1.0/html/User_Guide/sect-User_Guide-Application_Development-Creating_Applications.html#sect-User_Guide-Creating_Applications-Using_the_Command_Line
   [4]: http://s3.amazonaws.com/dist.springframework.org/release/ROO/spring-roo-1.2.0.RELEASE.zip
   [5]: https://docs.redhat.com/docs/en-US/OpenShift_Express/1.0/html/User_Guide/sect-User_Guide-Authenticating_SSH_Keys.html#sect-User_Guide-Authenticating_SSH_Keys-Generating_new_SSH_keys
   [6]: https://docs.redhat.com/docs/en-US/OpenShift_Express/1.0/html/User_Guide/sect-User_Guide-Application_Development-Creating_a_Domain.html

## Comments

**[Jayaram Sankaranarayanan](#7546 "2012-02-13 00:55:00"):** Hi Shekhar, Thanks for the informative article. I attended your presentation in JUDCOn2012 and got charged up and tried out a sample app built using Roo on openshift :) Here are the additional steps I had to do to get my app running. Update applciationcontext-mongo.xml to add the attributes mongo.username, mongo.password and mongo.dbname to the mon:dbFactory bean defintion Access the mogo shell of openshift and create the collections. Thought of letting you know about this. regards, Jayaram.

**[Sunil Prakash Inteti](#7975 "2012-03-20 11:19:33"):** Nice article. A recap of the XKE sesson.

