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

<p>Alfresco server configuration checked for version 3.4, 4.0 and 5.0 community.</p>
<p><span><span><span style="color: #000000;">Alfresco is the DMS product and in all these type of system we don't want to loose our document either by hardware or software failure for that very purpose we need to have some solid product with replication feature inbuilt that will take care for same.</span></span></span></p>
<p><span style="color: #000000;">Alfresco content management system come with the same idea to serve you with better replication mechanism with very little configuration at user or administrator end.</span></p>
<p><span style="color: #000000;"><strong>First of all we have to know what is Replication and what is it original meaning?</strong></span></p>
<p><span style="color: #000000;">Original meaning of Replication is <strong>R</strong><strong>eproducibility</strong> in which we are creating duplicate of either any object, data etc to be used for the same purpose as original one.</span></p>
<p><strong><span style="color: #003300;">There are basically two type of replication of data within the content store.</span></strong></p>
<ol>
<li>
<p><strong>Hot :</strong> In which there is no need to restart server for replicating data.</p>
</li>
<li>
<p><strong>Cold :</strong> In which server needed to be restarted for replication job to be done.</p>
</li>
</ol>
<p><span style="color: #003300;"><strong><span style="font-family: Arial, Helvetica, sans-serif;">There are many practical cases where we need the replication of data among the various server</span></strong></span></p>
<p><span style="color: #000000;"><span style="font-family: Arial, Helvetica, sans-serif;"><span>1. In the Load balancing environment we have more than one live node so there  we need to keep data in shrink with two or more nodes so that if one node is down other should be there to serve the request.</span></span></span></p>
<p><span><span><span style="color: #000000;">2. Recovery of the lost data from the live instance.</span></span></span></p>
<p>There are some more cases where we need replication....</p>
<p><span style="color: #000000;"><span style="font-family: Arial, Helvetica, sans-serif;"><span>For setup the load balance environment we have many blogs that serve you with better dish but our main concern is not have a load balanced environment here.</span></span></span></p>
<p><span style="color: #000000;"><span style="font-family: Arial, Helvetica, sans-serif;"><span>Here we give you an idea how we can replicate data between two or more live nodes at run time and how you will configure live replication between two or more Alfresco server.</span></span></span></p>
<p><strong><span style="color: #003300;">There are many ways how we can setup replication between two or more nodes using Alfresco server two out of them are.</span></strong></p>
<p>1. Replication using alfresco live configuration.</p>
<ol>
<li>Replication using database and repository replication.</li>
</ol>
<p>Here in this blog we share configuration how to setup live replication between two alfresco instances working on same or different server.</p>
<p><span style="color: #003300;"><strong><span style="font-size: 18px;">Replication using alfresco live configuration.</span></strong></span></p>
<ol>
<li>First of all we have to make replication active for both of the Alfresco server using alfresco-global.properties file located in {alfresco}/tomcat/shared/classes/ folder by adding following line at the end of the file.
<p style="padding-left: 30px;"><span style="color: #008000;">replication.enabled=true</span></p></li>
<li>
<p>Restart your server. Now replication is active on each of the Alfresco server installation.</p>
</li>
<li>
<p>Login as admin user on source alfresco server.</p>
</li>
<li>
<p>Now we have to create destination target on source alfresco server for that we have to create new folder in Repository&gt; Data Dictionary&gt; Transfers&gt; Transfer Target Groups&gt; Default Group folder like.</p>
</li>
</ol>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/09/Insert_image_1.png"><img class="size-medium wp-image-19002 aligncenter" alt="Insert_image_1" src="http://xebee.xebia.in/wp-content/uploads/2014/09/Insert_image_1-300x168.png" width="300" height="168" /></a></p>
<p>all the folder that are created in Default Group is of type trx:transferTarget because rule is already set on Default Group to transform all the folder created init into trx:transferTarget type.</p>
<ol>
<li>Now we have to add some properties like Host-name, Port, User-name and Password of remote Alfresco server etc. Just Go  to edit properties of new target folder created and add Target Host-name, port, Username, Password, Enable or not just like shown in image.</li>
</ol>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/09/image_2.png"><img class="size-medium wp-image-18994 aligncenter" alt="image_2" src="http://xebee.xebia.in/wp-content/uploads/2014/09/image_2-300x168.png" width="300" height="168" /></a></p>
<ol>
<li>If all is done. Now create new transformation job using Admin Tools in Alfresco 5.0 and Tools option in Alfresco 4.0. Then go to Replication Jobs Option and click on Create Job.</li>
</ol>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/09/image_3.png"><img class="size-medium wp-image-18995 aligncenter" alt="image_3" src="http://xebee.xebia.in/wp-content/uploads/2014/09/image_3-300x168.png" width="300" height="168" /></a></p>
<ol>
<li>Add details required like Job Name, Description, Source Item to be replicated, Locate Transfer target we just created in above step, Schedule Job with start date, time and repeat interval, Enabled or not just like image attached.</li>
</ol>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/09/image_4.png"><img class="size-medium wp-image-18996 aligncenter" alt="image_4" src="http://xebee.xebia.in/wp-content/uploads/2014/09/image_4-300x168.png" width="300" height="168" /></a></p>
<ol>
<li>Now click on the Run Job to start replication job.</li>
</ol>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/09/image_5.png"><img class="size-medium wp-image-18997 aligncenter" alt="image_5" src="http://xebee.xebia.in/wp-content/uploads/2014/09/image_5-300x168.png" width="300" height="168" /></a></p>
<ol>
<li>Check whether replication job working or not if there is error then please rectify error if not then go to the Target Alfresco server login as Admin then check whether same folder set in the replication job is created on Target server or not.
<p style="text-align: center;"><a href="http://xebee.xebia.in/wp-content/uploads/2014/09/image_6.png"><img class="alignnone size-medium wp-image-18998" alt="image_6" src="http://xebee.xebia.in/wp-content/uploads/2014/09/image_6-300x168.png" width="300" height="168" /></a></p></li>
<li>Now upload some content in same folder set for replication in source folder and check if its replicated on target server.
<p style="text-align: center;"><a href="http://xebee.xebia.in/wp-content/uploads/2014/09/image_7.png"><img class="alignnone size-medium wp-image-18999" alt="image_7" src="http://xebee.xebia.in/wp-content/uploads/2014/09/image_7-300x168.png" width="300" height="168" /></a></p>
<p style="text-align: center;"><a href="http://xebee.xebia.in/wp-content/uploads/2014/09/image_8.png"><img class="alignnone size-medium wp-image-19000" alt="image_8" src="http://xebee.xebia.in/wp-content/uploads/2014/09/image_8-300x168.png" width="300" height="168" /></a></p></li>
<li>
<p>This is how live replication works in Alfreso in my case i have two alfresco installation on same server we can have different installation on different server in same case also that do work in same manner.</p>
</li>
<li>
<p>Document that is replicated on remote server is view only we can't edit document or folder replicated for that to work we have to change configuration in share-config-custom.xml file located at {alfresco}/tomcat/shared/classes/alfresco/web-extension/ folder at target server end.
<p style="padding-left: 30px;"><span style="color: #008000;">&lt;config evaluator="string-compare" condition="Replication"&gt;</span>
<span style="color: #008000;"> &lt;share-urls&gt;</span>
<span style="color: #008000;"> &lt;!--</span>
<span style="color: #008000;"> To discover a Repository Id, browse to the remote server's CMIS landing page at:</span>
<span style="color: #008000;"> http://{server}:{port}/alfresco/service/cmis/index.html</span></p>
</li>
</ol>