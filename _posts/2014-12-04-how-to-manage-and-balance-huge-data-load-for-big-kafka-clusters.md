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

<h2 id="Replicationtools-5.AddPartitionTool"><span style="color: #993366">1. Add Partition Tool</span></h2>

<p>Partitions act as unit of parallelism. Messages of a single topic are distributed to multiple partitions that can be stored and served on different servers. Upon creation of a topic, the number of partitions for this topic has to be specified. Later on more partitions may be needed for this topic when the volume of this topic increases. This tool helps to add more partitions for a specific topic and also allow manual replica assignment of the added partitions. You can refer to the previous blog <a href="http://xebee.xebia.in/index.php/2014/12/03/quick-steps-have-a-kafka-cluster-up-running-in-3-minutes/" target="_blank">Quick steps : Have a Kafka Cluster Up &amp; Running in 3 minutes</a> to setup kafka cluster and create topics.</p>
<p>[code]</p>
<p>bin/kafka-add-partitions.sh</p>
<p>Option                                                    Description
------                                                       -----------
--partition &lt;Integer: # of partitions&gt;      REQUIRED: Number of partitions to add
to the topic</p>
<p>--replica-assignment-list                       For manually assigning replicas to
&lt;broker_id_for_part1_replica1 :            brokers for the new partitions
broker_id_for_part1_replica2,              (default: )
broker_id_for_part2_replica1 :
broker_id_for_part2_replica2, ...&gt;</p>
<p>--topic &lt;topic&gt;                                      REQUIRED: The topic for which
partitions need to be added.</p>
<p>--zookeeper &lt;urls&gt;                               REQUIRED: The connection string for
the zookeeper connection in the form
host:port. Multiple URLS can be
given to allow fail-over.</p>
<p>[/code]
<h2></h2>
<h2><span style="color: #993366">2. Reassign Partitions Tool</span></h2>
<h3 id="Replicationtools-Whatdoesthetooldo?.4"><span style="color: #993366">What does the tool do?</span></h3>
The goal of this tool is similar to the Referred Replica Leader Election Tool as to achieve load balance across brokers. But instead of only electing a new leader from the assigned replicas of a partition, this tool allows to change the assigned replicas of partitions – remember that followers also need to fetch from leaders in order to keep in sync, hence sometime only balance the leadership load is not enough.</p>
<p>A summary of the steps that the tool does is shown below -</p>
<ol>
<li>The tool updates the zookeeper path "/admin/reassign_partitions" with the list of topic partitions and (if specified in the Json file) the list of their new assigned replicas.</li>
<li>The controller listens to the path above. When a data change update is triggered, the controller reads the list of topic partitions and their assigned replicas from zookeeper.</li>
<li>For each topic partition, the controller does the following:
3.1. Start new replicas in RAR - AR (RAR = Reassigned Replicas, AR = original list of Assigned Replicas)
3.2. Wait until new replicas are in sync with the leader
3.3. If the leader is not in RAR, elect a new leader from RAR
3.4 4. Stop old replicas AR - RAR
3.5. Write new AR
3.6. Remove partition from the /admin/reassign_partitions path
<h3 id="Replicationtools-Howtousethetool?.5"><span style="color: #993366">How to use the tool?</span></h3>
[code]</li>
</ol>
<p>bin/kafka-reassign-partitions.sh</p>
<p>bin/kafka-reassign-partitions.sh</p>
<p>Option                                                        Description
------                                                           -----------
--broker-list &lt;brokerlist&gt;                            The list of brokers to which the
partitions need to be reassigned in
the form &quot;0,1,2&quot;. This is required
for automatic topic reassignment.
--execute [execute]                                   This option does the actual
reassignment. By default, the tool
does a dry run
--manual-assignment-json-file &lt;manual                 The JSON file with the list of manual
assignment json file path&gt;                          reassignmentsThis option or topics-
to-move-json-file needs to be
specified. The format to use is -
{&quot;partitions&quot;:
[{&quot;topic&quot;: &quot;foo&quot;,
&quot;partition&quot;: 1,
&quot;replicas&quot;: [1,2,3] }],
&quot;version&quot;:1
}
--topics-to-move-json-file &lt;topics to                The JSON file with the list of topics
reassign json file path&gt;                           to reassign.This option or manual-
assignment-json-file needs to be
specified. The format to use is -
{&quot;topics&quot;:
[{&quot;topic&quot;: &quot;foo&quot;},{&quot;topic&quot;: &quot;foo1&quot;}],
&quot;version&quot;:1
}
--zookeeper &lt;urls&gt;                                   REQUIRED: The connection string for
the zookeeper connection in the form
host:port. Multiple URLS can be
given to allow fail-over.
[/code]
<h2><span style="color: #993366">3.  Add Brokers(Cluster Expansion)</span></h2>
Cluster expansion involves including brokers with new broker ids in a Kafka 08 cluster. Typically, when you add new brokers to a cluster, they will not receive any data from existing topics until this tool is run to assign existing topics/partitions to the new brokers. The tool allows 2 options to make it easier to move some topics in bulk to the new brokers. These 2 options are a) topics to move b) list of newly added brokers. Using these 2 options, the tool automatically figures out the placements of partitions for the topics on the new brokers.</p>
<p>The following example moves 2 topics (foo1, foo2) to newly added brokers in a cluster (5,6,7).</p>
<p>[code]</p>
<p>&gt; ./bin/kafka-reassign-partitions.sh --topics-to-move-json-file topics-to-move.json --broker-list &quot;5,6,7&quot; --execute</p>
<p>&gt;  cat topics-to-move.json
{&quot;topics&quot;:
[{&quot;topic&quot;: &quot;foo1&quot;},{&quot;topic&quot;: &quot;foo2&quot;}],
&quot;version&quot;:1
}</p>
<p>[/code]
<h3><span style="color: #993366"><strong>Selectively moving some partitions to a broker</strong></span></h3>
The partition movement tool can also be moved to selectively move some replicas for certain partitions over to a particular broker. Typically, if you end up with an unbalanced cluster, you can use the tool in this mode to selectively move partitions around. In this mode, the tool takes a single file which has a list of partitions to move and the replicas that each of those partitions should be assigned to.</p>
<p>The following example moves 1 partition (foo-1) from replicas 1,2,3 to 1,2,4</p>
<p>[code]</p>
<p>&gt; ./bin/kafka-reassign-partitions.sh --manual-assignment-json-file partitions-to-move.json --execute</p>
<p>&gt; cat partitions-to-move.json
{&quot;partitions&quot;:
[{&quot;topic&quot;: &quot;foo&quot;,
&quot;partition&quot;: 1,
&quot;replicas&quot;: [1,2,4] }],
}],
&quot;version&quot;:1
}</p>
<p>[/code]</p>
<p><strong>Note</strong><strong> : </strong>These tools are available in version 0.8 , not prior versions.</p>