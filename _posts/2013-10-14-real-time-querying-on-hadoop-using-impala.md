---
layout: post
header-img: img/default-blog-pic.jpg
author: sanchit
description: 
post_id: 17359
created: 2013/10/14 13:59:25
created_gmt: 2013/10/14 08:59:25
comment_status: open
---

# Real Time Querying on Hadoop using Impala

Bring it on! is the message from Cloudera team with the release of Impala, an open source library for doing speed of thought querying on distributed data stores like HDFS and Hbase. When it had begun with Hadoop Map-Reduce using Java or other languages like python using Hadoop streaming API, the processing speed had already been improved many folds as compared to then existing data warehousing tools. To improve the business usability and adaptability across different functions of an organization , more similar to SQL syntax languages started to emerge like Pig, Hive which had hidden the complexity of writing a map reduce code from the end user and internally process it by converting them into map reduce tasks. Now, Impala is the next link in the chain providing Hive SQL(HQL) Syntax low latency querying on HDFS and Hbase with its advanced query processing engine whose key features, basic functioning and usage shall be the discussed further. There is another similar open source project based on Google Dremel paper, Apache Drill, also available in the market but any comparison with it is out of scope of this discussion.

**Key Business Benefits of using Impala compelling its adoption**

a) Lightning fast speed for getting insights into the data by reducing the bottlenecks of moving data, so it can be used for quick insights into the newly arrived or existing data in the warehouse.

b) Leverages existing BI tools and skill sets (SQL) to interact with data stored in Hadoop to avoid costly data modeling and ETL for analytics as all the data is immediately query-able.

c) Enables more users to effectively interact with data providing a single repository and metadata store from source to analysis.

d) Doesn't put the burden of creating its own set of concerns by providing a unified experience by leveraging the same file and data formats, metadata, security and resource management frameworks that are used for the rest of the Hadoop system.

**Key Technical Features of Impala that makes it stand out**

a) Provides low latency/fast querying on CDH, here latency is measured in terms of submitting a query on Impala Daemon and getting the final result and its best performance is achieved with Cloudera distribution of Hadoop and Hbase.

b) Native Massive Parallel Processing (MPP) engine , this feature is the game changer and major performance enhancer as compared to previous engines used for Pig or Hive. HQL queries are not converted to Map-Reduce internally to run on a JVM, it is directly converted into native C++ code that interacts directly with the underlying distributed file system.

c) Seamless kind of support for varied file formats ranging from unstructured formats like text and structured data in SequenceFiles, Avro, RCFile, LZO and Parquet.

d) In-memory data transfers , this feature is supported with many file formats using compression techniques.

e) Fine grained access using role based authorization with Sentry which provides better control to allow DBAs to define what users and applications can do with data within a server, data classification feature to enable content producers and owners intersperse sensitive and in-sensitive data in the same data set.

f) Leverages already existing metadata , allows access through multiple channels like JDBC, CLI, Hue and a workbench

g) Last but not the least , an open source project to be savored by all and not just the well-to-do ones.

**High Level Architecture of Impala**

[caption id="attachment_17392" align="aligncenter" width="637"]![Impala Architecture][1] **Impala High Level Architecture**[/caption]

**Brief description about different Impala components**

**_Impala Daemon_ \- **This is the core component of Impala which runs on all the data nodes of the cluster and is physically represented by **_impalad_** process. It is responsible to read/write to data files and accepts queries from various types of clients, then parallelizes the queries and distributes work to other nodes in the Impala cluster and finally transmits the query results back to the central coordinator node.

It is worth noting that a query can be submitted on any node in cluster and that node acts as the **_coordinator node_** for that query. Other nodes in the cluster transmit partial query results to the coordinator which constructs the final query result set.

For enhanced performance, a simple load balancing can be done by submitting queries from the client in round-robin fashion.

These daemons are in constant communication with the **_Impala statestore_** to confirm which nodes are healthy and can accept new work.

**_Impala StateStore_ \- **This component acts as a health check feature for Impala Daemons on all the nodes in the cluster and continuously relays its findings to each of the daemons. It is physically represented by a daemon process named **_statestored _** and is run only on single node in the cluster. Don't be confused that it is not a master node as an impression might come due to its single presence in the cluster.

If an Impala node becomes offline due to hardware failure, network error, software issue or any other reason, _statestore_ is responsible to inform all the other nodes so that future queries can avoid making requests to the unreachable node.

Now what if a condition occurs where the _statestore_ goes down? In such cases the other nodes continue running and distributing work among themselves but with a limitation that they might be giving work or waiting for results from to an unreachable node while the _statestore_ is offline making the cluster less robust in nature. When it comes back to life again, it re-establishes communication with other nodes and resumes its monitoring function.

_**Impala Clients**** -**_** **Impala daemons are accessible using various options and so it is capable to work in heterogeneous environment accepting requests from Impala CLI, BI tools using JDBC/ODBC interfaces and also through Hue which is a web based interface.

It listens to several ports for incoming requests. Requests coming from Impala shell and Hue are routed to the Impala daemons through the same port where as Impalad daemons listen on separate ports for JDBC or ODBC requests.

_**MetaStore**** - **_Impala keeps its table definitions in traditional MySQL or PostgreSQL database known as its **_metastore_**, the same database where Hive stores its data. It also tracks other metadata of data files like the physical location of blocks within the HDFS.

For tables with a large volume of data and/or many partitions, retrieving all the metadata for a table can be time-consuming so each Impala node caches all of this metadata to reuse for future

If the table definition or the data in the table is updated, any other Impala daemon in the cluster must retrieve the latest metadata, replacing the obsolete cached metadata, before issuing a query against that table.  If it is known that only specific tables have been changed outside of Impala, refresh is needed only for each affected table to only retrieve the latest metadata for those tables.

**Life-Cycle of Query on Impala**

The following images show life-cycle is in a scenario where data locality isn't a concern and any node that is available can be used to execute the query.

   [1]: http://xebee.xebia.in/wp-content/uploads/2013/10/Impala_Final_Arch.jpg