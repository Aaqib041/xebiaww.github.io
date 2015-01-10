---
layout: post
header-img: img/default-blog-pic.jpg
author: pranjan
description: 
post_id: 9518
created: 2011/07/31 19:36:13
created_gmt: 2011/07/31 14:36:13
comment_status: open
---

# Hadoop - What is MapReduce?

<p style="text-align: justify;">In this post I will be explaining the concept of <a href="http://hadoop.apache.org/mapreduce/" target="_blank">MapReduce</a> and how <a href="http://hadoop.apache.org/" target="_blank">Hadoop</a> uses it. I will be talking mostly about what Hadoop is, what can it do, when to use it etc. So, lets try to associate Hadoop with the technical problem it solves.</p>

<p style="text-align: justify;">Suppose the application you are working on has ever growing data. Let it be a search engine collecting data from crawlers. After a certain time, the data will become so huge that the database servers won’t be able to handle it. Then, you will buy more powerful database servers (machines) which can handle such data. But the data will keep growing, then you will again have to buy a more powerful and sophisticated machine and this will go on until more powerful machines are not there. These ultra masculine machines are pretty costly. They have special hardware which is not that common, this accounts for the high price.</p>

<p style="text-align: justify;">Before knowing what Hadoop is, and how it solves this problem, its necessary to know what MapReduce is. So, lets begin.
<!--more-->
MapReduce is way of processing data. Its basic concepts are “mappers” (data transformers) and “reducers” (data aggregators). You can also think of MapReduce as an algorithm. Hadoop is an implimentation of MapReduce algorithm. Hadoop is an open source framework developed by Apache which enables you to easily create an application in a MapReduce model on a distributed system.</p>

<p style="text-align: justify;">What do I mean by distributed system here? Different machines act as different nodes of the cluster. Huge data is distributed on this cluster. The nodes map (transform) this data and then reduce it ( aggregate). Different nodes work on different parts of data, so the comuputation is done in parallel, and then, the aggregation is also done parallely. The only challenge is to write your program in a MapReduce form.</p>

<p style="text-align: justify;">So, you can think of the Hadoop cluster as a group of machines doing mapping and then distributing data again on cluster for reeducing. Somewhat like shown in the picture below:</p>

<p style="text-align: justify;"><a rel="attachment wp-att-9522" href="http://xebee.xebia.in/2011/07/31/hadoop-what-is-mapreduce/hadoop-cluster/"><img class="aligncenter size-medium wp-image-9522" title="Hadoop Cluster" src="http://xebee.xebia.in/wp-content/uploads/2011/07/Hadoop-Cluster-300x248.jpg" alt="" width="300" height="248" /></a></p>

<p style="text-align: justify;">The mapper mostly transform the data on different nodes, then, the output of the mapper is shuffled and partitioned,  between differenent reducer nodes, which the reducer aggregates. Lets try to understand Mappers and Reducers using an example. The most common example of this is : program to count words in a file. This MapReduce program is bundled ( in a jar ) as a example with hadoop installation files . It counts which word appeared how many times in a file.</p>

<p style="text-align: justify;">Suppose the file contains following lines, and different lines will go to different mappers (nodes in the cluster) for processing.</p>

<h4 style="text-align: justify;">“The first line.”</h4>

<p style="text-align: justify;">-------- This line will be processed by the first node (mapper)</p>

<h4 style="text-align: justify;">“The second line. The second line”</h4>

<p style="text-align: justify;">--------- This line will be processed by the second node (mapper)</p>

<p style="text-align: justify;">So, the Mapper can be written in such way, that, each mapper returns a list of words appearing in the line it got for processing and its word count.</p>

<p style="text-align: justify;">First Node’s mapper returns :</p>

<p style="text-align: justify;">Word, Count</p>

<h4 style="text-align: justify;">“The”, 1
“first”, 1
“line”, 1</h4>

<p style="text-align: justify;">Second Node’s mapper returns</p>

<p style="text-align: justify;">Word, Count</p>

<h4 style="text-align: justify;">“The” 2
“second” 2
“line” 2</h4>

<p style="text-align: justify;">Now, this data (output of mapper) will be distributed to the reducers. The reducer will aggregate this data and the end result can be.</p>

<h4 style="text-align: justify;">“The” 3
“line” 3
“first” 1
“second” 2</h4>

<p style="text-align: justify;">What we have seen above is a mapreduce way for writing (designing) programs. The input is “mapped” (transformed) and then “reduced” (aggregated).</p>

<p style="text-align: justify;">So, when the data grows enormously, this data is distributed on different nodes for mapping. As the data increases, you can add new nodes at runtime. This helps to scale out as the data increases. This scale out is different than scale up of the “strong machine” concept. These machines of cluster are normal machines which are pretty cheap and can be easily added. The nodes can be added dynamically to the cluster.  No matter how big the data becomes, the nodes can be added as the data increases.</p>

<p style="text-align: justify;">This is all about the basic concept of Hadoop and MapReduce. Setting up hadoop is pretty easy on linux. It can be done on windows also using cygwin. Even Mac and other OS support it. Personally, I have tried it on windows I felt that it was a pain. Setting it up on linux is almost a piece of cake. Happy Hadooping.</p>

<h4 style="text-align: justify;"><span style="text-decoration: underline;">References :</span></h4>

<p><address style="text-align: justify;">Hadpoop in Action - For Hadoop and MapReduce concepts.
<a href="http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-single-node-cluster/" target="_blank">http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-single-node-cluster/</a> - Excellent tutorial to setup hadoop on linux.</address></p>

## Comments

**[Riccardo Pasquini (@morphy76)](#5809 "2011-08-04 03:15:37"):** Mmmmm i've 2 questions: what about binary content? ...and... it seems to me that reduce phase destroys informations... like... the relation between the words... am i missing something?

**[Paritosh Ranjan](#5811 "2011-08-04 08:51:49"):** I don't have any idea about binary content. Saying that reduce phase destroys information will not be valid, as in this program, the motto was to find the word count, so, the relation between the words hardly mattered. It purely depends on your business scenario and it will be your responsibility to code the business scenario in a mapreduce way. Paritosh

