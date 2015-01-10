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

# Introduction

For beginners to understand, kafka is nothing but a messaging system which is distributed and partitioned across brokers, having its data replicated, so as to provide the scalability and fault tolerance. For advanced configuration and tools to manage data load , please read [How to manage and balance “Huge Data Load” for Big Kafka Clusters][1]. 

## Terminology

  1. **Topic**: Category associated with each message, known as "Topic" 
  2. **Broker**: Cluster has servers, known as "Brokers". 
  3. **Producer**: Processes which publish message to brokers. 
  4. **Consumer**: Processes which are subscribed to topics in the cluster and process the published messages.

# Steps 

##### 1.  Download the kafka and untar it. (Latest stable release is 0.8.1.1 and 0.8.2 is in beta)

Click to [Download][2] from mirror sites.

``` 
$ tar -xzf kafka_2.9.2-0.8.1.1.tgz

$ cd kafka_2.9.2-0.8.1.1
 ``` 

##### 2.  Start zookeeper server.

Here we are starting the zookeeper which comes bundled with kafka. you may also use a  standalone zookeeper cluster with odd number of nodes and then point the kafka brokers to this zookeeper cluster. For production environment , do not use the bundled zookeeper.

``` 
$ bin/zookeeper-server-start.sh config/zookeeper.properties
 ``` 

##### 3.  Start the kafka servers(brokers).

Here we are setting up 3 broker cluster. so we will be having 3 different server.properties files corresponding to each broker.

you can copy the server.properties file and change the broker ids, log directory and port in the copied files. These 3 configuration properties must be unique to all the brokers which are on same machine. When you are using multiple machines and having one broker on one machine. you need to have only "broker.id" as unique. Ports and log directory need not be unique then. On same machine, if these parameters are same, overriding will happen. 

######       A. Copy the server.properties files

``` 
$ cp config/server.properties config/server-1.properties cp config/server.properties config/server-2.properties
 ```

For server.properties, we already have :

``` 
$ broker.id=0 port=9092 log.dir=/tmp/kafka-logs
``` 

######       B. Edit the two new files :

config/server-1.properties:

``` 
$ broker.id=1 port=9093 log.dir=/tmp/kafka-logs-1
 ```

config/server-2.properties:

``` 
$ broker.id=2 port=9094 log.dir=/tmp/kafka-logs-2
``` 

######         C. Start all 3 brokers.

``` 
$ bin/kafka-server-start.sh config/server.properties & bin/kafka-server-start.sh config/server-1.properties & bin/kafka-server-start.sh config/server-2.properties & 
``` 

##### 4\. Create a topic

You may specify replication factor and partitions per topic, else default values from properties file will be used. Topic name must be there in creation command. Once topic is created as per requirements of partitioning and replication , cluster is ready to receive and send the messages from producers and consumers.

``` 
$ bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 3 --partitions 9 --topic test
```

Here we have specified 9 partitions , it will create 9 directories 0-8 numbered on each broker , as replication factor is 3. 

##### 5\. Produce sample messages from command Line

######         A. Run Producer

``` 
$ bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
```

Here we have specified the one broker , which will be contacted first and further zookeeper will co-ordinate to find out on which broker to put the message , as it has complete list of brokers registered to it. 

######      B. Send Some messages

This is message 1 on test topic.

This is message 2 on test topic.

We have sent the messages to the broker, which will be stored in partitions for the test topic. 

##### 6\. Consume the sample messages.

######       A. Run the Consumer.

**Note** : Use different terminal for producer and consumer so as to get better understanding.

``` 
$ bin/kafka-console-consumer.sh --zookeeper localhost:2181 --from-beginning --topic test
```

As and when command executes, it consumes messages from test topic , from beginning . If we do not specify "--from-beginning" parameter, consumer will consume only the messages produced after the consumer is running.

Here Response is the list of messages on the broker. 

######       B. Response

This is message 1 on test topic.

This is message 2 on test topic

**Note** : You have to kill the consumer(^C) yourself , else it will keep running and waiting for the messages to be consumed. You can verify it by sending new messages if you have producer open in other terminal.

   [1]: http://xebee.xebia.in/index.php/2014/12/04/how-to-manage-and-balance-huge-data-load-for-big-kafka-clusters/
   [2]: https://www.apache.org/dyn/closer.cgi?path=/kafka/0.8.1.1/kafka_2.9.2-0.8.1.1.tgz (Download)