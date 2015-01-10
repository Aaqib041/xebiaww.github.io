---
layout: post
header-img: img/default-blog-pic.jpg
---

<!--When one is not Enough. We would like to scale our Middle ware infrastructure to JBoss clustering is simple cost effective solution to achieve scalability, fault tolerance, Fail over & responsiveness. This is setup steps& troubleshooting tips to build a JBoss cluster-->

# JBoss Clustering in 5 minutes

**Building a JBoss Appliaction Server 5.1 Cluster** Having spent a couple of months working on a JBoss App server clustering, I would like to share some thoughts about it. Clustering of JBoss is relatively straightforward but there are a few gotchas. Simply executing “run -c all” on two AS instances on the same network is enough for them to form a cluster. But there is lot more to it than executing a command to make a working cluster.  This is the most common production scenario. Assume the machines are named "node1" and "node2", while node1 has an IP address of 192.168.0.101 and node2 has an address of 192.168.0.102. run -c all -g ProductionCluster -u 239.255.100.100 -b 192.168.0.101 -Djboss.messaging.ServerPeerID=1 run -c all -g ProductionCluster -u 239.255.100.100 -b 192.168.0.102 -Djboss.messaging.ServerPeerID=2 The -c switch says to use the all config, which includes clustering support. The -g switch sets the cluster name, this should be same for all server nodes in a cluster. The -u switch sets the multicast address that will be used for intra-cluster communication. This is generally a IPv4 format, remember this IP should not be in use in your Network. The -b switch sets the address on which sockets will be bound. The -D switch sets system property jboss.messaging.ServerPeerID, from which JBoss Messaging gets its unique id. The clustering features in JBoss Application Server are built on top of lower level libraries that provide much of the core functionality - 
* **JGroups** which is a toolkit for reliable point-to-point and point-to-multipoint communication. JGroups is used for all clustering-related communications between nodes in a JBoss Cluster.
* **JBoss Cache** which is a highly flexible clustered transactional caching library.
So after done with your cluster setup you may like to configure your application for clustering. 
* **Web Application:** This aspect involves telling JBoss you want clustering behavior for a particular web app, and it couldn't be simpler. Just add an empty distributable element to your application's web.xml file:
    
    
     
    
              ...
        
              ...
    
          

* **EJBs** : For EJB2 / EJB3 beans simply add a clustered element to the bean's section in the JBoss-specific deployment descriptor, jboss.xml:
    
    
     
        
              
                    
                ....      
                true
            
        
    
               

### **Common Problems & Solution**

If there is firewall is existing in your network then it will not allow JGroups to communicate Start a Gossip Server: java -cp $JBOSS_HOME\server\all\lib\jgroups.jar;$JBOSS_HOME\common\lib\commons-logging.jarorg.jgroups.stack.GossipRouter -port 5555 -bindaddress localhost Then in the JGroups configuration of the file %JBOSS_HOME\server\all\deploy\ cluster\jgroups-channelfactory.sar\META-INF\jgroups-channelfactory-stacks.xml make the following modifications in all nodes of the cluster: 
    
    
    
    
    

Now, instead of nodes multicasting to find other nodes, they will register and get information about the cluster from the gossip server. If multicasting is not enabled in the network, this means that JGroups will not work by default, which in turns means that JBoss Clustering will not work either. Then enable the TCP protocol for communication by changing the Entity refrence from shared-up to TCP in file %JBOSS_HOME\server\all\deploy\ cluster\jgroups-channelfactory.sar\META-INF\jgroups-channelfactory-stacks.xml. 
    
    
    
    
    ]>
    
        
            
              ...
              &tcp;
    
        
        ...
    
           
                ....
                &tcp;
                ...

## Comments

**[forrest](#9170 "2012-07-17 07:05:18"):** Very useful. Thank you

