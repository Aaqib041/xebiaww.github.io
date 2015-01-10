---
layout: post
header-img: img/default-blog-pic.jpg
author: manisha
description: 
post_id: 19352
created: 2014/12/04 11:15:13
created_gmt: 2014/12/04 06:15:13
comment_status: open
---

# How to manage and balance "Huge Data Load" for Big Kafka Clusters

## 1\. Add Partition Tool

Partitions act as unit of parallelism. Messages of a single topic are distributed to multiple partitions that can be stored and served on different servers. Upon creation of a topic, the number of partitions for this topic has to be specified. Later on more partitions may be needed for this topic when the volume of this topic increases. This tool helps to add more partitions for a specific topic and also allow manual replica assignment of the added partitions. You can refer to the previous blog [Quick steps : Have a Kafka Cluster Up & Running in 3 minutes][1] to setup kafka cluster and create topics.

[code]

bin/kafka-add-partitions.sh

Option                                                    Description \------                                                       ----------- \--partition <Integer: # of partitions>      REQUIRED: Number of partitions to add to the topic

\--replica-assignment-list                       For manually assigning replicas to <broker_id_for_part1_replica1 :            brokers for the new partitions broker_id_for_part1_replica2,              (default: ) broker_id_for_part2_replica1 : broker_id_for_part2_replica2, ...>

\--topic <topic>                                      REQUIRED: The topic for which partitions need to be added.

\--zookeeper <urls>                               REQUIRED: The connection string for the zookeeper connection in the form host:port. Multiple URLS can be given to allow fail-over.

[/code] 

## 

## 2\. Reassign Partitions Tool

### What does the tool do?

The goal of this tool is similar to the Referred Replica Leader Election Tool as to achieve load balance across brokers. But instead of only electing a new leader from the assigned replicas of a partition, this tool allows to change the assigned replicas of partitions – remember that followers also need to fetch from leaders in order to keep in sync, hence sometime only balance the leadership load is not enough.

A summary of the steps that the tool does is shown below -

  1. The tool updates the zookeeper path "/admin/reassign_partitions" with the list of topic partitions and (if specified in the Json file) the list of their new assigned replicas.
  2. The controller listens to the path above. When a data change update is triggered, the controller reads the list of topic partitions and their assigned replicas from zookeeper.
  3. For each topic partition, the controller does the following: 3.1. Start new replicas in RAR - AR (RAR = Reassigned Replicas, AR = original list of Assigned Replicas) 3.2. Wait until new replicas are in sync with the leader 3.3. If the leader is not in RAR, elect a new leader from RAR 3.4 4. Stop old replicas AR - RAR 3.5. Write new AR 3.6. Remove partition from the /admin/reassign_partitions path 

### How to use the tool?

[code]

bin/kafka-reassign-partitions.sh

bin/kafka-reassign-partitions.sh

Option                                                        Description \------                                                           ----------- \--broker-list <brokerlist>                            The list of brokers to which the partitions need to be reassigned in the form "0,1,2". This is required for automatic topic reassignment. \--execute [execute]                                   This option does the actual reassignment. By default, the tool does a dry run \--manual-assignment-json-file <manual    The JSON file with the list of manual assignment json file path>                       reassignmentsThis option or topics- to-move-json-file needs to be specified. The format to use is - {"partitions": [{"topic": "foo", "partition": 1, "replicas": [1,2,3] }], "version":1 } \--topics-to-move-json-file <topics to        The JSON file with the list of topics reassign json file path>                         to reassign.This option or manual- assignment-json-file needs to be specified. The format to use is - {"topics": [{"topic": "foo"},{"topic": "foo1"}], "version":1 } \--zookeeper <urls>                                REQUIRED: The connection string for the zookeeper connection in the form host:port. Multiple URLS can be given to allow fail-over. [/code] 

## 3.  Add Brokers(Cluster Expansion)

Cluster expansion involves including brokers with new broker ids in a Kafka 08 cluster. Typically, when you add new brokers to a cluster, they will not receive any data from existing topics until this tool is run to assign existing topics/partitions to the new brokers. The tool allows 2 options to make it easier to move some topics in bulk to the new brokers. These 2 options are a) topics to move b) list of newly added brokers. Using these 2 options, the tool automatically figures out the placements of partitions for the topics on the new brokers.

The following example moves 2 topics (foo1, foo2) to newly added brokers in a cluster (5,6,7).

[code]

> ./bin/kafka-reassign-partitions.sh --topics-to-move-json-file topics-to-move.json --broker-list "5,6,7" \--execute

>  cat topics-to-move.json {"topics": [{"topic": "foo1"},{"topic": "foo2"}], "version":1 }

[/code] 

### **Selectively moving some partitions to a broker**

The partition movement tool can also be moved to selectively move some replicas for certain partitions over to a particular broker. Typically, if you end up with an unbalanced cluster, you can use the tool in this mode to selectively move partitions around. In this mode, the tool takes a single file which has a list of partitions to move and the replicas that each of those partitions should be assigned to.

The following example moves 1 partition (foo-1) from replicas 1,2,3 to 1,2,4

[code]

> ./bin/kafka-reassign-partitions.sh --manual-assignment-json-file partitions-to-move.json --execute

> cat partitions-to-move.json {"partitions": [{"topic": "foo", "partition": 1, "replicas": [1,2,4] }], }], "version":1 }

[/code]

**Note**** : **These tools are available in version 0.8 , not prior versions.

   [1]: http://xebee.xebia.in/index.php/2014/12/03/quick-steps-have-a-kafka-cluster-up-running-in-3-minutes/