---
layout: post
header-img: img/default-blog-pic.jpg
author: shekhargulati
description: 
post_id: 10923
created: 2012/01/18 22:22:35
created_gmt: 2012/01/18 17:22:35
comment_status: open
---

# Using Hibernate SafeHtml Validator In Application Running On OpenShift Express

I use Spring Roo to create sample applications for me. Spring Roo uses JSR 303 (Bean Validation) to validate the entities for invalid values. Today I created a very simple application -- a simple PasteBin clone where you can anonymously create post or share text snippets. I wanted to validate that field post should not contain any Unsafe HTML content in it. So I decided to use Hibernate SafeHtml validator to validate the input field. This post will first show you how to use @SafeHtml validator in an Spring application and then we will deploy the application to OpenShift Express.

**Create JBoss AS 7 OpenShift Express**

Create a jbossas-7.0 OpenShift Express application using rhc-create-app command as shown below.

[sourcecode lang="text"] rhc-create-app -l <rhlogin email> -a pastebin -t jbossas-7.0 -d [/sourcecode]

In case you are not aware of OpenShift Express please refer to the documentation at <https://docs.redhat.com/docs/en-US/OpenShift_Express/1.0/html/User_Guide/index.html>

**Generate PasteBin clone source using Spring Roo**

I am using Spring Roo to create a pastebin clone application and will be using MongoDB as the datastore. In case you are not aware of Spring Roo you can refer to my [IBM DeveloperWorks series on Spring Ro][1]o. But before we create application remove the generated src and pom.xml files using commands shown below.

[sourcecode lang="text"] git rm -rf pom.xml src git commit -m "removed default files" [/sourcecode]

Now fire the roo shell and execute the following commands to create the application.

[sourcecode lang="text"] project --topLevelPackage com.shekhar.pastebin --projectName pastebin mongo setup entity mongo --class ~.domain.Post field string --fieldName body --notNull --sizeMax 4000 repository mongo --interface ~.repository.PostRepository service --interface ~.service.PostService web mvc setup web mvc all --package ~.web [/sourcecode]

**Embed MongoDB Cartridge **

After the application has generated add MongoDB cartridge to the application using rhc-ctl-app command as shown below.

[sourcecode lang="text"] rhc-ctl-app -l <rhlogin email> -a pastebin -e add-mongodb-2.0 [/sourcecode] 

**Do Mongo Setup Again**

Now that we have embed MongoDB cartridge we should run the mongo setup command again to correctly configure the MongoDB. Fire the roo shell and execute the following line.

[sourcecode lang="text"] mongo setup --databaseName pastebin --username admin --password _cvF8XgIxIzv --host 127.1.27.1 --port 27017 [/sourcecode]

Make sure your database.properties file has all the correct value.

**Adding HTMLSafe Validator**

It is very simple to add HTML validator just add SafeHtml annotation as shown below.

[sourcecode lang="java"] package com.shekhar.pastebin.domain;

import javax.validation.constraints.NotNull; import javax.validation.constraints.Size; import org.springframework.roo.addon.javabean.RooJavaBean; import org.springframework.roo.addon.layers.repository.mongo.RooMongoEntity; import org.springframework.roo.addon.tostring.RooToString; import org.hibernate.validator.constraints.SafeHtml;

@RooJavaBean @RooToString @RooMongoEntity public class Post {
    
    
    @NotNull
    @Size(max = 4000)
    @SafeHtml
    private String body;
    

} [/sourcecode]

Once you have added this annotation if you try and run this on tomcat using mvn tomcat:run you will face **java.lang.ClassNotFoundException : org.jsoup.safety.Whit****elist **while creating a post.

To fix this add jsoup dependency to pom.xml

[sourcecode lang="xml"] <dependency> <groupId>org.jsoup</groupId> <artifactId>jsoup</artifactId> <version>1.6.1</version> </dependency> [/sourcecode]

Now if you try and run again on tomcat you will be able to successfully create a post.

**Running on OpenShift Express**

Before you commit the code and push to express you need to first define a maven  profile called openshift. OpenShift uses this profile when it run the build in cloud.

[sourcecode lang="xml"] <profile> <!-- When built in OpenShift the 'openshift' profile will be used when invoking mvn. --> <!-- Use this profile for any OpenShift specific customization your app will need. --> <!-- By default that is to put the resulting archive into the 'deployments' folder. --> <!-- http://maven.apache.org/guides/mini/guide-building-for-different-environments.html --> <id>openshift</id> <build> <finalName>demo1</finalName> <plugins> <plugin> <artifactId>maven-war-plugin</artifactId> <version>2.1.1</version> <configuration> <outputDirectory>deployments</outputDirectory> <warName>ROOT</warName> </configuration> </plugin> </plugins> </build> </profile> [/sourcecode]

To push the code on OpenShift Express cloud add the files and commit them as shown below.

[sourcecode lang="text"] git add src pom.xml git commit -m "adding pastebin application" [/sourcecode]

finally push the code using git push command.

After build has run fine and code deployed to express. Hit the url and try creating a post and you will get "Internal Error". And if you tail the logs using rhc-tail-files command and you will see exception as shown below

[sourcecode lang="text"] org.jsoup.safety.Whitelist from [Module "org.hibernate.validator:main" from local module loader @16aa37a6 (roots: /var/lib/libra/521ca7af66d64093ae386ed99a49a790/linkbin/jbossas-7.0/standalone/configuration/modules,/var/lib/libra/521ca7af66d64093ae386ed99a49a790/linkbin/jbossas-7.0/modules)] at org.jboss.modules.ModuleClassLoader.findClass(ModuleClassLoader.java:191) at org.jboss.modules.ConcurrentClassLoader.performLoadClassChecked(ConcurrentClassLoader.java:361) at org.jboss.modules.ConcurrentClassLoader.performLoadClassChecked(ConcurrentClassLoader.java:333) at org.jboss.modules.ConcurrentClassLoader.performLoadClass(ConcurrentClassLoader.java:310) at org.jboss.modules.ConcurrentClassLoader.loadClass(ConcurrentClassLoader.java:103) at java.lang.Class.getDeclaredFields0(Native Method) [:1.6.0_22] at java.lang.Class.privateGetDeclaredFields(Class.java:2308) [:1.6.0_22] at java.lang.Class.getDeclaredFields(Class.java:1760) [:1.6.0_22] [/sourcecode]

You might thought why this error is coming as you have already added dependency in pom.xml. The reason is that this validation is carried by Hibernate Validator in JBoss AS7 and you need to add JSoup module.

**Adding JSoup and Hibernate ****Validator Module **

To add module go to the directory where your application exists. There will be folder called .openshift inside which there will be folder called config and under which there will be folder called modules. Create the folder structure as shown below![][2]

We are adding hibernate validator module also because we need to specify the dependency of jsoup in its module.xml.

The module.xml of hibernate validator is shown below

[sourcecode lang="xml"] <?xml version="1.0" encoding="UTF-8"?>

<module xmlns="urn:jboss:module:1.0" name="org.hibernate.validator"> <resources> <resource-root path="hibernate-validator-4.2.0.Final.jar"/> <!-- Insert resources here --> </resources>

<dependencies>

   [1]: http://www.ibm.com/developerworks/views/opensource/libraryview.jsp?search_by=introducing+spring+roo,
   [2]: http://whyjava.files.wordpress.com/2012/01/modules.png (Modules)