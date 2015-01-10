---
layout: post
header-img: img/default-blog-pic.jpg
author: swapnil178
description: 
post_id: 19487
created: 2015/01/02 12:53:30
created_gmt: 2015/01/02 07:53:30
comment_status: open
---

# SQL over HBASE using Apache Phoenix

Hbase is a NoSQL column oriented database inspired from Google Big Table.. Since most of the queries related to analytics are more concentrated towards accessing all the queries in a table simultaneously, Hbase’s column oriented nature proves its worth in such cases.

Hbase uses HDFS (Hadoop File System) to store its data.HDFS use helps Hbase to achieve features like scalability and fault tolerance out of the box.

Hbase provides two features:

  * Accessing the data in HDFS with a table approach.
  * It provides transactiional behaviour to multi cluster setup.

For more information on Hbase, refer to :<http://xebee.xebia.in/index.php/2013/02/27/hbase-a-real-time-access-to-big-data/>

There is long list  NoSql databases in market like Casaandra, MongoDb etc. For accessing the data residing in Cassandra, we have a SQL query like language CQL.The biggest advantage of using SQL is its widespread use since ages.BA and QA are quite use to writing SQL queries.

That is the reason why CQL is quite famous among people who use Cassandra.For Hbase, we have few SQL drivers, which give a SQL type language to access data like - Apache Pheonix. Point to note here is that its just the SQL type language which is common, the way data is accessed in case of an RDBMS and Hbase or Cassandra is completely different.

In this blog we are going to look at the way we can use Apache Phoenix to achieve SQL capabilities over Hbase. 

**What is Apache Phoenix?**

Apache Phoenix provides an SQL driver over Hbase.Its a alternative client API which converts its SQL queries in Hbase calls i.e SQL into get,put and scan constructs of Hbase.

**Advantages of Phoenix-**

  * Give people an API they already know.
  * Reducing the quantity of code.
  * Optimized for fetching data- Provides facilities like- Aggregation,Skip scans etc
  * It can use the existing tools- SQL client/terminal, OLAP engine

**Getting Started with Phoenix:-**

You can download Apache Pheonix from the Apache Phoenix site( <http://phoenix.apache.org/download.html>) .There are many Phoenix versions available.Please read the Phoenix documentation before downloading Phoenix and make sure it is compatible with your currently used Hbase version.

Once the Apache Phoenix is downloaded, unzip the tar file and you have to include the Phoenix jar files to Hbase region servers, so that we can establish a successful communication via Phoenix to Hbase.

Note: If your Hbase is not compatible with Phoenix version, we will get an exception saying Client and Server jars are not compatible.

``` 
$wget http://apache.mirrors.hoobly.com/phoenix/phoenix-3.2.2/bin/phoenix-3.2.2-bin.tar.gz 

$ tar -xvf phoenix-3.2.2-bin.tar.gz 

$ cd phoenix-3.2.2-bin/common 

$ cp phoenix-3.2.2-client-minimal.jar /usr/lib/hbase/lib/ 

$ cp phoenix-core-3.2.2.jar /usr/lib/hbase/lib</span>
 ```

Phoenix comes up with a command line tool: sqlline (written in python).
``` 
 $ cd ~/phoenix/phoenix-core-3.2.2/bin 
 
 $ ./psql.py localhost <sql-script A> <argument for the scrit A> <second-sql script B> <arguments forscript B> 
 ```

For example: 

Script A:

``` 
 CREATE TABLE IF NOT EXISTS EMPLOYEE (NAME VARCHAR NOT NULL,LOCATION VARCHAR NOT NULL,DOB DATE NOT NULL,EMPLOYEEID INTEGER CONSTRAINT PK PRIMARY KEY (EMPLOYEEID)); 
 ``` 

Arguments for A:

``` 
 Abhishek,Delhi,1984-01-01 01:01:01,1 Rakesh,Kolkata,1983-06-25 01:01:01,2 Jemin,Chennai,1990-11-25 01:01:01,3 
 ``` 

Script B:

Running Sample queries:

``` 
SELECT NAME from EMPLOYEE where EMPLOYEEID=’1’
``` 

Now using sqlline we can connect to Hbase:

``` 
 $ ./sqlline.py localhost [cloudera@localhost bin]
 
 $ ./sqlline.py localhost .. Connecting to jdbc:phoenix:localhost Driver: org.apache.phoenix.jdbc.PhoenixDriver (version 3.0) Autocommit status: true Transaction isolation: TRANSACTION_READ_COMMITTED .. Done sqlline version 1.1.2 0: jdbc:phoenix:localhost> select count(*) from EMPLOYEE; +------------+ |  COUNT(1)  | +------------+ | 3          | +------------+ 1 row selected (0.112 seconds) 0: jdbc:phoenix:localhost>
 ``` 

**Running Phoenix on AWS:**

We can run Phoenix on AWS- EMR clusters.

Step 1:Specify the Hadoop Version and choose Hbase in application to be installed.

![1][1]

Step 2: Specify the following in the Bootstrap portion of Cluster configuration.

![2][2]

Once the cluster is running, you can login to the master node using ssh and check your Phoenix configuration.

![3][3]

**Operations not supported in Apache Phoenix:**

  * Relational operators like - UNION,INTERSECT and MINUS is not supported.

  * Transactional support- It does not support and additional transactional capabilities other than what Hbase provides.

We can also integrate Squirrel - SQL plugin with Apache Phoenix as well.

All we need to do is to add appropriate driver on Squirrel client window.Using this we can easily query the Hbase database we use.

**Conclusion:**

Phoenix is actually been used in places where we want to hit the Hbase database using REST calls and the result of these REST calls are actually to be seen on some UI.

Widespread knowledge of SQL increases the usefulness of Phoenix and related tools like CQL (in case of Cassandra) and Phoenix(in case of Hbase)

   [1]: http://xebee.xebia.in/wp-content/uploads/2015/01/1-300x113.png
   [2]: http://xebee.xebia.in/wp-content/uploads/2015/01/2-300x93.png
   [3]: http://xebee.xebia.in/wp-content/uploads/2015/01/3-300x224.png