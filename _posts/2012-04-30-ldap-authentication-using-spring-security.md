---
layout: post
header-img: img/default-blog-pic.jpg
author: aparida
description: 
post_id: 13465
created: 2012/04/30 23:04:47
created_gmt: 2012/04/30 18:04:47
comment_status: open
---

# LDAP Authentication using Spring Security

Recently, the project I was working on, required implementation of simple login functionality, where users were supposed to be authenticated against a remote Active Directory via LDAP. It was a maven project based on spring framework so we implemented the application security using the spring security framework.

**_Problem Statement_** The Active Directory had users stored in a tree-structure which looked similar to the below example.

## Comments

**[yoviluz](#9377 "2013-05-08 00:56:03"):** okay sorry to bother again but...Im using tomcat to do this sample and Im not sure if I have to download an ldap server or if I can configure tomcat to do this I tried to download and connect but it gives me errors using a free ldap server any suggustions?

**[Haripriya](#8955 "2012-06-04 23:59:00"):** This is awesome! Made my day! looking for this simple example and found nowhere but here!

**[yoviluz](#9368 "2013-05-03 20:15:03"):** Hi, I like your instructions but Im a little lost here since Im knew to all this Im trying this out at work and Im getting errors do I have to change the url in the <beans:constructor-arg value="[ldap://tst.nl.eur.local:389/][1]" /> example to my localhost? Or can I use the values stated in this example? Thanks for posting this its really informative as well as easy to understand. 

   [1]: 389/

**[yoviluz](#9375 "2013-05-07 21:09:30"):** Thanks I will try that now :)

**[Anita Parida](#9371 "2013-05-06 15:38:25"):** Hi yoviluz 

The string 'tst.nl.eur.local' represents the ldap server name while 389 is the default port. You will have to use the connection string as per the ldap server you are trying to connect to.

