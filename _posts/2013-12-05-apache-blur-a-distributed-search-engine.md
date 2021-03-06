---
layout: post
header-img: img/default-blog-pic.jpg
author: Gagan
description: 
post_id: 17719
created: 2013/12/05 10:51:05
created_gmt: 2013/12/05 05:51:05
comment_status: open
---

# Apache Blur: A Distributed Search Engine

[Apache Blur][1] (incubating) is a distributed search engine. Today in Big data age organizations are generating massive amount of structured data but they are struggling to index and search data like traditional RDBMS approach. RDBMS simply cannot scale to this level. Apache Blur is developed to bridge this gap.

Blur is capable of querying massive amount of structured data at incredible speeds. Rather than using the flat, document-like data model used by most search solutions, Blur allows you to build rich data models and search them in a relational manner similar to querying a relational database. Using Blur, you can get precise search results against terabytes or petabytes of data at Google-like speeds.  Blur gives you the ability to correlate structured data in complex and meaningful ways. Here’s an example of the kind of query you could perform with Blur using data from the US Census: show me all of the people in the United States who were born in Alaska between 1940 and 1970 who are now living in Kansas.

When we think about search, only powerful solution that comes to our mind is Lucene and when we think about big data, only thing comes to our mind is Hadoop, so Blur is built on top of Hadoop and Lucene. Blur also leverages open source projects like Thrift, Zookeeper to provide robust and extensible solution.

Already multiple solutions exist in the market that support distributed search like elastic search and solr, but Blur is having solid integration with Hadoop ecosystem and leverages the redundancy and fault tolerance power of HDFS.

**Blur Features**

  * Fast data ingestion
  * Hierarchical data storage
  * Record-level access control
  * Paged results
  * Quick search
  * Boolean search logic
  * Fuzzy searches
  * Wildcard searches
  * Facets
  * Term statistics
  * Term lists
**Blur Data Model** Blur data model is somewhat analogous to RDBMS. Blur stores data in form of rows in tables. Row in blur table must have a unique row id and can contain single or multiple records. Records have a unique record id and a column family. Column family is used to group columns that create a record. Records can contain a name value pairs called columns.

``` 
 rowid:"row-1", records:[ { recordid:"record-1", family:"purchases", columns:[ {name:”item”, value:”item-1”}, {name:”price”, value:”100”} ] }, { recordid:"record-2", family:"purchases", columns:[ {name:”item”, value:”item-2”}, {name:”price”, value:”200”} ] }, { recordid:”record-2”, family:”details”, columns:[ {name:”name”, value:”John”}, {name:”email”, value:”john@a.com”}, {name:”contact”, value:”123456789”},
    
    
      ]
    }
    

] } 
 ```

**Blur Architecture** Blur uses Hadoop's Map Reduce framework for indexing data, and Hadoop's HDFS file system for storing indexes. Thrift is used for all inter-process communications and Zookeeper is used to know the state of the system and to store Meta data. Blur works on the concept of controller and shards. You just need to send your query to controller server then controller server will fire your query to all the relevant shards for that table and returns the consolidated results. Let’s give a closer look to both controller and shard servers. 

    * **Blur Shard Server**
The shard server serves 0 or more shards from all the currently online tables. The calculation of what shards are online in each shard server is done through the state information in Zookeeper. If a shard server goes down, through interaction with Zookeeper the remaining shard servers detect the failure and determine which if any of the missing shards they need to serve from HDFS. 
    * **Blur Controller Server**
The controller server provides a single point of entry (logically) to the cluster for spraying out queries, collecting the responses, and providing a single response.

Both the controller and shard servers expose the same Thrift API which helps to ease debugging. It also allows developers to start a single shard server and interact with it the same way they would with a large cluster. Many controller servers can be (and should be) run for redundancy and to increase search throughput. The controllers act as gateways to all of the data that is being served by the shard servers.

**Getting Started With Blur**

    1. **Installing Blur**
You can download the source of the latest version of Blur form the [website][2]. Blur installation is very simple like other products available in Hadoop ecosystem. For installation and cluster setup refer to this page. (http://incubator.apache.org/blur/docs/0.2.0/cluster-setup.html) 

    1. **Blur Command line interface**
Blur comes with both CLI (command line interface) and java apis. It’s very handy to play with blur using Blur CLI. Run following command to start Blur CLI.

``` 
 $BLUR_HOME/bin/blur shell 
 ```

 

  * **Create a table in blur**
 

``` 
 // t: table name // c: number of shards. // l: location on hdfs. It can be local file system as well. create -t orders -c 1 -l hdfs://localhost:54310/blur/tables/ 
 ```

 

  * **Bulk upload Data to Blur**
 

Blur uses map reduce for bulk upload of data. Single records can be added using mutation with the help of Thrift api that we will discuss in the separate section. Blur comes with example CsvLoader, a map reduce job to load data in blur from a .csv file. This Loader provides a lot of options like specifying table name, structure, file location and many more.

Let’s upload some sample data using CsvLoader to understand further operations.

``` 


row-1,record-1,purchases,item-1,100 row-1,record-2,purchases,item-2,200 row-1,record-3,details,userA,userA@a.com,123456789 row-2,record-1,purchases,item-1,100 row-2,record-2,purchases,item-3,300 row-2,record-3,details,userB,userB@a.com,152346879 row-3,record-1,purchases,item-3,300 row-3,record-2,purchases,item-2,200 row-3,record-3,details,userC,userC@a.com,125317986 
 ```

Above listed data follows the structure that we defined in the Data Model section.

Run following command to execute the CsvLoader Job

``` 
 //t: table name //c: controller server //d: family definition $BLUR_HOME/bin/blur csvloader -t orders -c localhost:40010 \ -d purchases item price -d details name email contact 
 ```

This job will upload data to blur. To know about other features of csvloader refer to documentation [page][3].

 

  * **Query data from Blur**
 

Let’s say we want to query all the users have purchased item “item-1”.

``` 
 // orders is the table name. query orders purchases.item:item-1 
 ```

The output of this command will be all the rows and records of users who have purchased item-1 like in our case row-1 and row-2.

 

  * **Query selective data**
 

The content of output of above query depends on the setting of the Selector objects. By default selector is set to fetch all the content related to resultant rows. Let’s say now we want to select only the user name of the users who purchased “item-1”, then we need to set this definition in selector.

``` 
 // add family “details” and column “name” Selector add details name 
 ```

Now the out of query will return only name column.

 

  * **Mutate Record**
 

   [1]: http://incubator.apache.org/blur/ (Apache Blur)
   [2]: http://incubator.apache.org/blur/docs/0.2.0/getting-started.html#download (Apache Blur)
   [3]: http://incubator.apache.org/blur/docs/0.2.0/using-blur.html#csv-loader (Apache Blur)