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

As I am preparing for my JUDCon session on OpenShift one topic that I will be covering is setting up MongoDB Replica Set on OpenShift Flex. There are various reasons to setup MongoDB replication. The primary reason for replication is to ensure data survive single or multiple machine failures. The more replicas you have, the more likely is your data to survive one or more hardware crashes. With three replicas, you can afford to loose two nodes and still serve the data.Replication also helps to scale reads as you can distribute your read load accross multiple machines. There are two ways to support replication in MongoDB -- Master Slave and Replica Set. Replica Set is the recommended way to do replication and OpenShift only supports replica set.  OpenShift Flex MongoDB replica set is yet not polished so you might face issues in setting up. This blog will show steps to configure a working MongoDB replica set on OpenShift Flex. If you are not aware of MongoDB Replica Set you should first refer to the [MongoDB documentation][1]. Also you can refer to my blog on[ how to use MongoDB replica set with Spring][2].

  1. Login to [OpenShift Flex console][3] and create a cluster. as shown below in a screenshot. As you can see below I have defined  cluster named replica with 1server and used EC2 medium instance.![][4]
  2. After cluster has been setup you will see one server running in server tab.  Go to the "Applications" tab and press Add Application. Please select a server cluster , give application name and application version and then press submit button. My settings are shown in screenshot below.![][5][ ][6]
  3. Next you will see a application named bookshop (or the name you gave to application) under the Applications tab. Name of the application is a hyperlink and on clicking that you will be able to configure the application. So click on the application name and you will see five sub-tabs -- Overview, Components,Files, Configure, and Deploy Changes.
  4. Click on the Components tab to select the components required for running the application. The required components are Tomcat, Java6 and MongoDB 2. ![][7][ ][8]
  5. Next go to the Configure tab to configure the components selected. This view gives us the power to configure the components according to our needs. We will only configure MongoDB components as we need to set up replica set. So click MongoDB-2.0.1 and enable replica sets. Right now you can only can have replica set of only 3 machines. But I think in future you will have the choice to select a replica set from 3-7 machines.![][9][ ][10]
  6. Then go to Deploy Changes tab and deploy the changes and start the application.
  7. After you have started the application you will notice that two more instances are added to the cluster. Please keep patience it takes some minutes (2-5 minutes). After that you will have 3 instance cluster with MongoDB setup.![][11][ ][12]
  8. Please note until this point replica set is not set up and if you ssh into any of the machines you will notice that normal mongod instances are running. To enable replica set you need to deploy a application. I am deploying a simple Spring MongoDB application. You can download the application from my [google code project][13].
  9. Go to the File tab and upload the bookshop.war and deploy the changes. This will restart the application.
  10. In the Overview tab you will find the url of the application. Append bookshop to the url as bookshop is the context path. The application url will be like <http://replica107550541-1150999735.us-east-1.elb.amazonaws.com/bookshop/>. You will be able to do CRUD operations on Book.
  11. There is a bug in the flex code that sometimes it is not able to add a particular node to the replica set. To check that all of the three nodes are added to replica set lets ssh to first instance in our cluster. To do it type ssh admin@107.21.186.11. Replace ip address with your instance ip address.
  12. Next connect to mongo on machine by typing command shown below. [sourcecode lang="text"] /opt/vostok/cartridges/mongodb-2.0.1/bundle/bin/mongo [/sourcecode]
  13. If you see the output that you are logged into PRIMARY >. It means replica set is configured. [sourcecode lang="text"] [admin@ip-10-194-195-117 ~]$ /opt/vostok/cartridges/mongodb-2.0.1/bundle/bin/mongo MongoDB shell version: 2.0.1 connecting to: test PRIMARY> [/sourcecode]
  14. Next we should check that all the three instances are added to replica set. To check do execute rs.status() command. The command and output are shown below. [sourcecode lang="text"] PRIMARY> rs.status() { "set" : "SET1", "date" : ISODate("2012-01-22T18:46:02Z"), "myState" : 1, "members" : [ { "_id" : 0, "name" : "10.194.195.117:27017", "health" : 1, "state" : 1, "stateStr" : "PRIMARY", "optime" : { "t" : 1327256835000, "i" : 1 }, "optimeDate" : ISODate("2012-01-22T18:27:15Z"), "self" : true }, { "_id" : 1, "name" : "10.212.138.54:27017", "health" : 1, "state" : 2, "stateStr" : "SECONDARY", "uptime" : 1121, "optime" : { "t" : 1327256835000, "i" : 1 }, "optimeDate" : ISODate("2012-01-22T18:27:15Z"), "lastHeartbeat" : ISODate("2012-01-22T18:46:02Z"), "pingMs" : 0 } ], "ok" : 1 }

[/sourcecode]

   [1]: http://www.mongodb.org/display/DOCS/Replica+Set+Tutorial
   [2]: http://whyjava.wordpress.com/2011/12/12/using-mongodb-replica-set-with-spring-mongodb-1-0-0-rc1/
   [3]: https://openshift.redhat.com/flex/flex/index.html
   [4]: http://xebee.xebia.in/wp-content/uploads/2012/01/Define-Server-Cluster-292x300.png (Define-Server-Cluster)
   [5]: http://xebee.xebia.in/wp-content/uploads/2012/01/Create-Application-222x300.png (Create-Application)
   [6]: http://whyjava.files.wordpress.com/2012/01/create-application.png
   [7]: http://xebee.xebia.in/wp-content/uploads/2012/01/Application-Components-300x154.png (Application-Components)
   [8]: http://whyjava.files.wordpress.com/2012/01/application-components.png
   [9]: http://xebee.xebia.in/wp-content/uploads/2012/01/enable-mongodb-replicaset-300x245.png (enable-mongodb-replicaset)
   [10]: http://whyjava.files.wordpress.com/2012/01/enable-mongodb-replicaset.png
   [11]: http://xebee.xebia.in/wp-content/uploads/2012/01/Server-Cluster-300x128.png (Server-Cluster)
   [12]: http://whyjava.files.wordpress.com/2012/01/server-cluster.png
   [13]: http://code.google.com/p/shekhar-playground/downloads/list