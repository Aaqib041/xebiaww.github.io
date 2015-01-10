---
layout: post
header-img: img/default-blog-pic.jpg
author: manisha
description: 
post_id: 19321
created: 2014/12/03 15:09:42
created_gmt: 2014/12/03 10:09:42
comment_status: open
---

# Quick steps : Have a Kafka Cluster Up & Running in 3 minutes

<h1><span style="text-decoration: underline"><span style="color: #993366;text-decoration: underline">Introduction</span></span></h1>

<p>For beginners to understand, kafka is nothing but a messaging system which is distributed and partitioned across brokers, having its data replicated, so as to provide the scalability and fault tolerance. For advanced configuration and tools to manage data load , please read <a href="http://xebee.xebia.in/index.php/2014/12/04/how-to-manage-and-balance-huge-data-load-for-big-kafka-clusters/" target="_blank">How to manage and balance “Huge Data Load” for Big Kafka Clusters</a>.
<h2><span style="color: #993366">Terminology</span></h2>
<ol>
    <li><span style="color: #000000"><strong>Topic</strong>: Category associated with each message, known as "Topic"
</span></li>
    <li><span style="color: #000000"><strong>Broker</strong>: Cluster has servers, known as "Brokers".
</span></li>
    <li><span style="color: #000000"><strong>Producer</strong>: Processes which publish message to brokers.
</span></li>
    <li><span style="color: #000000"><strong>Consumer</strong>: Processes which are subscribed to topics in the cluster and process the published messages.</span></li>
</ol>
<h1><span style="color: #993366">Steps </span></h1>
<h5><span style="color: #993366">1.  Download the kafka and untar it. (Latest stable release is 0.8.1.1 and 0.8.2 is in beta)</span></h5>
Click to <a title="Download" href="https://www.apache.org/dyn/closer.cgi?path=/kafka/0.8.1.1/kafka_2.9.2-0.8.1.1.tgz" target="_blank">Download</a> from mirror sites.</p>
<p>[code]</p>
<p>tar -xzf kafka_2.9.2-0.8.1.1.tgz</p>
<p>cd kafka_2.9.2-0.8.1.1</p>
<p>[/code]
<h5><span style="color: #993366">2.  Start zookeeper server.</span></h5>
Here we are starting the zookeeper which comes bundled with kafka. you may also use a  standalone zookeeper cluster with odd number of nodes and then point the kafka brokers to this zookeeper cluster. For production environment , do not use the bundled zookeeper.</p>
<p>[code]</p>
<p>bin/zookeeper-server-start.sh config/zookeeper.properties</p>
<p>[/code]
<h5><span style="color: #993366">3.  Start the kafka servers(brokers).</span></h5>
Here we are setting up 3 broker cluster. so we will be having 3 different server.properties files corresponding to each broker.</p>
<p>you can copy the server.properties file and change the broker ids, log directory and port in the copied files. These 3 configuration properties must be unique to all the brokers which are on same machine. When you are using multiple machines and having one broker on one machine. you need to have only "broker.id" as unique. Ports and log directory need not be unique then. On same machine, if these parameters are same, overriding will happen.
<h6>      <span style="color: #993366">A. Copy the server.properties files</span></h6>
[code]</p>
<p>cp config/server.properties config/server-1.properties
cp config/server.properties config/server-2.properties</p>
<p>[/code]</p>
<p>For server.properties, we already have :</p>
<p>[code]
broker.id=0
port=9092
log.dir=/tmp/kafka-logs[/code]
<h6>      <span style="color: #993366">B. Edit the two new files :</span></h6>
config/server-1.properties:</p>
<p>[code]
 broker.id=1
 port=9093
 log.dir=/tmp/kafka-logs-1</p>
<p>[/code]</p>
<p>config/server-2.properties:</p>
<p>[code]
broker.id=2
port=9094
log.dir=/tmp/kafka-logs-2</p>
<p>[/code]
<h6>        <span style="color: #993366">C. Start all 3 brokers.</span></h6>
[code]
bin/kafka-server-start.sh config/server.properties &amp;
bin/kafka-server-start.sh config/server-1.properties &amp;
bin/kafka-server-start.sh config/server-2.properties &amp;
[/code]
<h5><span style="color: #993366">4. Create a topic</span></h5>
You may specify replication factor and partitions per topic, else default values from properties file will be used. Topic name must be there in creation command. Once topic is created as per requirements of partitioning and replication , cluster is ready to receive and send the messages from producers and consumers.</p>
<p>[code]
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 3 --partitions 9 --topic test</p>
<p>[/code]</p>
<p>Here we have specified 9 partitions , it will create 9 directories 0-8 numbered on each broker , as replication factor is 3.
<h5><span style="color: #993366">5. Produce sample messages from command Line</span></h5>
<h6>      <span style="color: #993366">  A. Run Producer</span></h6>
[code]</p>
<p>bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test</p>
<p>[/code]</p>
<p>Here we have specified the one broker , which will be contacted first and further zookeeper will co-ordinate to find out on which broker to put the message , as it has complete list of brokers registered to it.
<h6><span style="color: #993366">     B. Send Some messages</span></h6>
This is message 1 on test topic.</p>
<p>This is message 2 on test topic.</p>
<p>We have sent the messages to the broker, which will be stored in partitions for the test topic.
<h5><span style="color: #993366">6. Consume the sample messages.</span></h5>
<h6><span style="color: #993366">      A. Run the Consumer.</span></h6>
<strong>Note</strong> : Use different terminal for producer and consumer so as to get better understanding.</p>
<p>[code]</p>
<p>bin/kafka-console-consumer.sh --zookeeper localhost:2181 --from-beginning --topic test</p>
<p>[/code]</p>
<p>As and when command executes, it consumes messages from test topic , from beginning . If we do not specify "--from-beginning" parameter, consumer will consume only the messages produced after the consumer is running.</p>
<p>Here Response is the list of messages on the broker.
<h6><span style="color: #993366">      B. Response</span></h6>
This is message 1 on test topic.</p>
<p>This is message 2 on test topic</p>
<p><strong>Note</strong> : You have to kill the consumer(^C) yourself , else it will keep running and waiting for the messages to be consumed. You can verify it by sending new messages if you have producer open in other terminal.</p>