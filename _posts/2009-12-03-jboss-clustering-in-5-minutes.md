---
layout: post
header-img: img/default-blog-pic.jpg
author: pallavi
description: 
post_id: 2574
created: 2009/12/03 13:51:02
created_gmt: 2009/12/03 08:51:02
comment_status: open
---

<!--When one is not Enough. We would like to scale our Middle ware infrastructure to JBoss clustering is simple cost effective solution to achieve scalability, fault tolerance, Fail over & responsiveness. This is setup steps& troubleshooting tips to build a JBoss cluster-->

# JBoss Clustering in 5 minutes

<p><strong>Building a JBoss Appliaction Server  5.1 Cluster</strong></p>
<p>Having spent a couple of months working on a JBoss  App server clustering, I would like to share some thoughts about it.</p>
<p>Clustering of JBoss is  relatively straightforward but there are a few gotchas.
Simply executing “run -c all” on two AS instances on the same network is enough for them to form a cluster. But there is lot more to it than executing a command to make a working cluster.
<!--more-->
This is the most common production scenario. Assume the machines are named "node1" and "node2", while node1 has an IP address of 192.168.0.101 and node2 has an address of 192.168.0.102.</p>
<p><span style="color: #0000ff;">run -c all -g ProductionCluster -u 239.255.100.100  -b 192.168.0.101 -Djboss.messaging.ServerPeerID=1</p>
<p><span style="color: #0000ff;">run -c all -g ProductionCluster -u 239.255.100.100   -b 192.168.0.102 -Djboss.messaging.ServerPeerID=2</span></p>
<p>The -c switch says to use the all config, which includes clustering support.
The -g switch sets the cluster name, this should be same for all server nodes in a cluster.
The -u switch sets the multicast address that will be used for intra-cluster communication. This is generally a IPv4 format, remember this IP should not be in use in your Network.
The -b switch sets the address on which sockets will be bound.
The -D switch sets system property jboss.messaging.ServerPeerID, from which JBoss Messaging gets its unique id.</p>
<p>The clustering features in JBoss Application Server are built on top of lower level libraries that provide much of the core functionality -</p>
<pre><code>&lt;li&gt;&lt;strong&gt;JGroups&lt;/strong&gt; which is a toolkit for reliable point-to-point and point-to-multipoint communication. JGroups is used for all clustering-related communications between nodes in a JBoss Cluster.&lt;/li&gt;


&lt;li&gt;&lt;strong&gt;JBoss Cache&lt;/strong&gt; which is a highly flexible clustered transactional caching library.&lt;/li&gt;
</code></pre>
<p>So after done with your cluster setup you may like to configure your application for clustering.</p>
<pre><code>&lt;li&gt;&lt;strong&gt;Web Application:&lt;/strong&gt; This aspect involves telling JBoss you want clustering behavior for a particular web app, and it couldn't be simpler. Just add an empty distributable element to your application's web.xml file:&lt;/li&gt;
</code></pre>
<pre lang="xml">
<?xml version="1.0"?> 
<web-app>
          ...
    <distributable/>
          ...
</web-app>
      </pre>

<pre><code>&lt;li&gt;&lt;strong&gt;EJBs&lt;/strong&gt; : For EJB2 / EJB3 beans  simply add a clustered element to the bean's section in the JBoss-specific deployment descriptor, jboss.xml:&lt;/li&gt;
</code></pre>
<pre lang="xml">
<?xml version="1.0"?> 
<jboss>    
    <enterprise-beans>      
        <session>        
            ....      
            <clustered>true</clustered>
        </session>
    </enterprise-beans>
</jboss>
           </pre>

<h3><strong>Common Problems & Solution</strong></h3>

<p>If there is firewall is existing in your network  then it will not allow JGroups to communicate</p>
<p>Start a Gossip Server:</p>
<p><span style="color: #000080;">java  -cp  $JBOSS_HOME\server\all\lib\jgroups.jar;$JBOSS_HOME\common\lib\commons-logging.jarorg.jgroups.stack.GossipRouter -port 5555 -bindaddress localhost</span></p>
<p>Then in the JGroups configuration of the file %JBOSS_HOME\server\all\deploy\ cluster\jgroups-channelfactory.sar\META-INF\jgroups-channelfactory-stacks.xml make the following modifications in all nodes of the cluster:
<pre lang="xml">
<UDP ip_mcast="false" mcast_addr="239.255.100.100" mcast_port="45566" ip_ttl="32"
 mcast_send_buf_size="150000" mcast_recv_buf_size="80000"/>
<PING gossip_host="gossip_server_name" gossip_port="5555"   <br />gossip_refresh="15000" timeout="2000" num_initial_members="3"/>
</pre>
Now, instead of nodes multicasting to find other nodes, they will register and get information about the cluster from the gossip server. 
If multicasting is not enabled in the  network, this means that JGroups will not work by default, which in turns means that JBoss Clustering will not work either.  Then enable the TCP protocol for communication by changing the Entity refrence from shared-up to TCP in file %JBOSS_HOME\server\all\deploy\ cluster\jgroups-channelfactory.sar\META-INF\jgroups-channelfactory-stacks.xml.
<pre lang="xml">
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;!DOCTYPE protocol_stacks [
   &lt;!ENTITY tcp '
....</p>
<pre><code>'&gt;
</code></pre>
<p>]&gt;
<protocol_stacks>
    <stack name="udp"
           description="Default: IP multicast based stack, with flow control.">
        <config>
          ...
          &tcp;
</config>
    </stack>
    ...
<stack name="jbm-control"
           description="Stack optimized for the JBoss Messaging Control Channel">
       <config>
            ....
            &tcp;
            ...
        </config>
    </stack>
  </protocol_stacks>
 </pre></p>

## Comments

**[forrest](#9170 "2012-07-17 07:05:18"):** Very useful. Thank you

