---
layout: post
header-img: img/default-blog-pic.jpg
author: shekhargulati
description: 
post_id: 11020
created: 2012/01/23 00:25:32
created_gmt: 2012/01/22 19:25:32
comment_status: open
---

# MongoDB Replica Set on OpenShift Flex

<p>As I am preparing for my JUDCon session on OpenShift one topic that I will be covering is setting up MongoDB Replica Set on OpenShift Flex. There are various reasons to setup MongoDB replication. The primary reason for replication is to ensure data survive single or multiple machine failures. The more replicas you have, the more likely is your data to survive one or more hardware crashes. With three replicas, you can afford to loose two nodes and still serve the data.Replication also helps to scale reads as you can distribute your read load accross multiple machines. There are two ways to support replication in MongoDB -- Master Slave and Replica Set. Replica Set is the recommended way to do replication and OpenShift only supports replica set.  OpenShift Flex MongoDB replica set is yet not polished so you might face issues in setting up. This blog will show steps to configure a working MongoDB replica set on OpenShift Flex. If you are not aware of MongoDB Replica Set you should first refer to the <a href="http://www.mongodb.org/display/DOCS/Replica+Set+Tutorial">MongoDB documentation</a>. Also you can refer to my blog on<a href="http://whyjava.wordpress.com/2011/12/12/using-mongodb-replica-set-with-spring-mongodb-1-0-0-rc1/"> how to use MongoDB replica set with Spring</a>.<!--more-->
<ol>
    <li>Login to <a href="https://openshift.redhat.com/flex/flex/index.html">OpenShift Flex console</a> and create a cluster. as shown below in a screenshot. As you can see below I have defined  cluster named replica with 1server and used EC2 medium instance.<a rel="attachment wp-att-11027" href="http://xebee.xebia.in/2012/01/23/mongodb-replica-set-on-openshift-flex/define-server-cluster/"><img class="alignnone size-medium wp-image-11027" height="300" width="292" title="Define-Server-Cluster" src="http://xebee.xebia.in/wp-content/uploads/2012/01/Define-Server-Cluster-292x300.png" /></a></li>
    <li>After cluster has been setup you will see one server running in server tab.  Go to the "Applications" tab and press Add Application. Please select a server cluster , give application name and application version and then press submit button. My settings are shown in screenshot below.<a href="http://xebee.xebia.in/2012/01/23/mongodb-replica-set-on-openshift-flex/create-application/" rel="attachment wp-att-11030"><img src="http://xebee.xebia.in/wp-content/uploads/2012/01/Create-Application-222x300.png" title="Create-Application" width="222" height="300" class="alignnone size-medium wp-image-11030" /></a><a href="http://whyjava.files.wordpress.com/2012/01/create-application.png">
</a></li>
    <li>Next you will see a application named bookshop (or the name you gave to application) under the Applications tab. Name of the application is a hyperlink and on clicking that you will be able to configure the application. So click on the application name and you will see five sub-tabs -- Overview, Components,Files, Configure, and Deploy Changes.</li>
    <li>Click on the Components tab to select the components required for running the application. The required components are Tomcat, Java6 and MongoDB 2. <a href="http://xebee.xebia.in/2012/01/23/mongodb-replica-set-on-openshift-flex/application-components/" rel="attachment wp-att-11031"><img src="http://xebee.xebia.in/wp-content/uploads/2012/01/Application-Components-300x154.png" title="Application-Components" width="300" height="154" class="alignnone size-medium wp-image-11031" /></a><a href="http://whyjava.files.wordpress.com/2012/01/application-components.png">
</a></li>
    <li>Next go to the Configure tab to configure the components selected. This view gives us the power to configure the components according to our needs. We will only configure MongoDB components as we need to set up replica set. So click MongoDB-2.0.1 and enable replica sets. Right now you can only can have replica set of only 3 machines. But I think in future you will have the choice to select a replica set from 3-7 machines.<a href="http://xebee.xebia.in/2012/01/23/mongodb-replica-set-on-openshift-flex/enable-mongodb-replicaset/" rel="attachment wp-att-11032"><img src="http://xebee.xebia.in/wp-content/uploads/2012/01/enable-mongodb-replicaset-300x245.png" title="enable-mongodb-replicaset" width="300" height="245" class="alignnone size-medium wp-image-11032" /></a><a href="http://whyjava.files.wordpress.com/2012/01/enable-mongodb-replicaset.png">
</a></li>
    <li>Then go to Deploy Changes tab and deploy the changes and start the application.</li>
    <li>After you have started the application you will notice that two more instances are added to the cluster. Please keep patience it takes some minutes (2-5 minutes). After that you will have 3 instance cluster with MongoDB setup.<a href="http://xebee.xebia.in/2012/01/23/mongodb-replica-set-on-openshift-flex/server-cluster/" rel="attachment wp-att-11033"><img src="http://xebee.xebia.in/wp-content/uploads/2012/01/Server-Cluster-300x128.png" title="Server-Cluster" width="300" height="128" class="alignnone size-medium wp-image-11033" /></a><a href="http://whyjava.files.wordpress.com/2012/01/server-cluster.png">
</a></li>
    <li>Please note until this point replica set is not set up and if you ssh into any of the machines you will notice that normal mongod instances are running. To enable replica set you need to deploy a application. I am deploying a simple Spring MongoDB application. You can download the application from my <a href="http://code.google.com/p/shekhar-playground/downloads/list">google code project</a>.</li>
    <li>Go to the File tab and upload the bookshop.war and deploy the changes. This will restart the application.</li>
    <li>In the Overview tab you will find the url of the application. Append bookshop to the url as bookshop is the context path. The application url will be like <a href="http://replica107550541-1150999735.us-east-1.elb.amazonaws.com/bookshop/">http://replica107550541-1150999735.us-east-1.elb.amazonaws.com/bookshop/</a>. You will be able to do CRUD operations on Book.</li>
    <li>There is a bug in the flex code that sometimes it is not able to add a particular node to the replica set. To check that all of the three nodes are added to replica set lets ssh to first instance in our cluster. To do it type ssh admin@107.21.186.11. Replace ip address with your instance ip address.</li>
    <li>Next connect to mongo on machine by typing command shown below.
[sourcecode lang="text"]
/opt/vostok/cartridges/mongodb-2.0.1/bundle/bin/mongo
[/sourcecode]</li>
    <li>If you see the output that you are logged into PRIMARY &gt;. It means replica set is configured.
[sourcecode lang="text"]
[admin@ip-10-194-195-117 ~]$ /opt/vostok/cartridges/mongodb-2.0.1/bundle/bin/mongo
MongoDB shell version: 2.0.1
connecting to: test
PRIMARY&gt;
[/sourcecode]</li>
    <li>Next we should check that all the three instances are added to replica set. To check do execute rs.status() command. The command and output are shown below.
[sourcecode lang="text"]
PRIMARY&gt; rs.status()
{
    &quot;set&quot; : &quot;SET1&quot;,
    &quot;date&quot; : ISODate(&quot;2012-01-22T18:46:02Z&quot;),
    &quot;myState&quot; : 1,
    &quot;members&quot; : [
        {
            &quot;_id&quot; : 0,
            &quot;name&quot; : &quot;10.194.195.117:27017&quot;,
            &quot;health&quot; : 1,
            &quot;state&quot; : 1,
            &quot;stateStr&quot; : &quot;PRIMARY&quot;,
            &quot;optime&quot; : {
                &quot;t&quot; : 1327256835000,
                &quot;i&quot; : 1
            },
            &quot;optimeDate&quot; : ISODate(&quot;2012-01-22T18:27:15Z&quot;),
            &quot;self&quot; : true
        },
        {
            &quot;_id&quot; : 1,
            &quot;name&quot; : &quot;10.212.138.54:27017&quot;,
            &quot;health&quot; : 1,
            &quot;state&quot; : 2,
            &quot;stateStr&quot; : &quot;SECONDARY&quot;,
            &quot;uptime&quot; : 1121,
            &quot;optime&quot; : {
                &quot;t&quot; : 1327256835000,
                &quot;i&quot; : 1
            },
            &quot;optimeDate&quot; : ISODate(&quot;2012-01-22T18:27:15Z&quot;),
            &quot;lastHeartbeat&quot; : ISODate(&quot;2012-01-22T18:46:02Z&quot;),
            &quot;pingMs&quot; : 0
        }
    ],
    &quot;ok&quot; : 1
}</p>
<p>[/sourcecode]</li></p>