---
layout: post
header-img: img/default-blog-pic.jpg
author: SOURABH AGGARWAL
description: 
post_id: 18961
created: 2014/09/13 04:04:21
created_gmt: 2014/09/12 23:04:21
comment_status: open
---

# Alfresco live replication between two or more live nodes

Alfresco server configuration checked for version 3.4, 4.0 and 5.0 community.

Alfresco is the DMS product and in all these type of system we don't want to loose our document either by hardware or software failure for that very purpose we need to have some solid product with replication feature inbuilt that will take care for same.

Alfresco content management system come with the same idea to serve you with better replication mechanism with very little configuration at user or administrator end.

**First of all we have to know what is Replication and what is it original meaning?**

Original meaning of Replication is **R****eproducibility** in which we are creating duplicate of either any object, data etc to be used for the same purpose as original one.

**There are basically two type of replication of data within the content store.**

  1. **Hot :** In which there is no need to restart server for replicating data.

  2. **Cold :** In which server needed to be restarted for replication job to be done.

**There are many practical cases where we need the replication of data among the various server**

1. In the Load balancing environment we have more than one live node so there  we need to keep data in shrink with two or more nodes so that if one node is down other should be there to serve the request.

2\. Recovery of the lost data from the live instance.

There are some more cases where we need replication....

For setup the load balance environment we have many blogs that serve you with better dish but our main concern is not have a load balanced environment here.

Here we give you an idea how we can replicate data between two or more live nodes at run time and how you will configure live replication between two or more Alfresco server.

**There are many ways how we can setup replication between two or more nodes using Alfresco server two out of them are.**

1. Replication using alfresco live configuration.

  1. Replication using database and repository replication.

Here in this blog we share configuration how to setup live replication between two alfresco instances working on same or different server.

**Replication using alfresco live configuration.**

  1. First of all we have to make replication active for both of the Alfresco server using alfresco-global.properties file located in {alfresco}/tomcat/shared/classes/ folder by adding following line at the end of the file. 

replication.enabled=true

  2. Restart your server. Now replication is active on each of the Alfresco server installation.

  3. Login as admin user on source alfresco server.

  4. Now we have to create destination target on source alfresco server for that we have to create new folder in Repository> Data Dictionary> Transfers> Transfer Target Groups> Default Group folder like.

![Insert_image_1][1]

all the folder that are created in Default Group is of type trx:transferTarget because rule is already set on Default Group to transform all the folder created init into trx:transferTarget type.

  1. Now we have to add some properties like Host-name, Port, User-name and Password of remote Alfresco server etc. Just Go  to edit properties of new target folder created and add Target Host-name, port, Username, Password, Enable or not just like shown in image.

![image_2][2]

  1. If all is done. Now create new transformation job using Admin Tools in Alfresco 5.0 and Tools option in Alfresco 4.0. Then go to Replication Jobs Option and click on Create Job.

![image_3][3]

  1. Add details required like Job Name, Description, Source Item to be replicated, Locate Transfer target we just created in above step, Schedule Job with start date, time and repeat interval, Enabled or not just like image attached.

![image_4][4]

  1. Now click on the Run Job to start replication job.

![image_5][5]

  1. Check whether replication job working or not if there is error then please rectify error if not then go to the Target Alfresco server login as Admin then check whether same folder set in the replication job is created on Target server or not. 

![image_6][6]

  2. Now upload some content in same folder set for replication in source folder and check if its replicated on target server. 

![image_7][7]

![image_8][8]

  3. This is how live replication works in Alfreso in my case i have two alfresco installation on same server we can have different installation on different server in same case also that do work in same manner.

  4. Document that is replicated on remote server is view only we can't edit document or folder replicated for that to work we have to change configuration in share-config-custom.xml file located at {alfresco}/tomcat/shared/classes/alfresco/web-extension/ folder at target server end. 

<config evaluator="string-compare" condition="Replication"> <share-urls> <!-- To discover a Repository Id, browse to the remote server's CMIS landing page at: http://{server}:{port}/alfresco/service/cmis/index.html

   [1]: http://xebee.xebia.in/wp-content/uploads/2014/09/Insert_image_1-300x168.png
   [2]: http://xebee.xebia.in/wp-content/uploads/2014/09/image_2-300x168.png
   [3]: http://xebee.xebia.in/wp-content/uploads/2014/09/image_3-300x168.png
   [4]: http://xebee.xebia.in/wp-content/uploads/2014/09/image_4-300x168.png
   [5]: http://xebee.xebia.in/wp-content/uploads/2014/09/image_5-300x168.png
   [6]: http://xebee.xebia.in/wp-content/uploads/2014/09/image_6-300x168.png
   [7]: http://xebee.xebia.in/wp-content/uploads/2014/09/image_7-300x168.png
   [8]: http://xebee.xebia.in/wp-content/uploads/2014/09/image_8-300x168.png