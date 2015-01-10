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

<p dir="ltr">Hbase is a NoSQL column oriented database inspired from Google Big Table.. Since most of the queries related to analytics are more concentrated towards accessing all the queries in a table simultaneously, Hbase’s column oriented nature proves its worth in such cases.</p>

<p dir="ltr">Hbase uses HDFS (Hadoop File System) to store its data.HDFS use helps Hbase to achieve features like scalability and fault tolerance out of the box.</p>

<p dir="ltr">Hbase provides two features:</p>

<ul>
    <li><span style="line-height: 1.5em">Accessing the data in HDFS with a table approach.</span></li>
    <li><span style="line-height: 1.5em">It provides transactiional behaviour to multi cluster setup.</span></li>
</ul>

<p>For more information on Hbase, refer to :<a href="http://xebee.xebia.in/index.php/2013/02/27/hbase-a-real-time-access-to-big-data/">http://xebee.xebia.in/index.php/2013/02/27/hbase-a-real-time-access-to-big-data/</a>
<p dir="ltr">There is long list  NoSql databases in market like Casaandra, MongoDb etc. For accessing the data residing in Cassandra, we have a SQL query like language CQL.The biggest advantage of using SQL is its widespread use since ages.BA and QA are quite use to writing SQL queries.</p>
<p dir="ltr">That is the reason why CQL is quite famous among people who use Cassandra.For Hbase, we have few SQL drivers, which give a SQL type language to access data like - Apache Pheonix. Point to note here is that its just the SQL type language which is common, the way data is accessed in case of an RDBMS and Hbase or Cassandra is completely different.</p>
In this blog we are going to look at the way we can use Apache Phoenix to achieve SQL capabilities over Hbase.
<p dir="ltr"><strong>What is Apache Phoenix?</strong></p>
<p dir="ltr">Apache Phoenix provides an SQL driver over Hbase.Its a alternative client API which converts its SQL queries in Hbase calls i.e SQL into get,put and scan constructs of Hbase.</p>
<p dir="ltr"><strong>Advantages of Phoenix-</strong></p></p>
<ul>
    <li><span style="line-height: 1.5em">Give people an API they already know.</span></li>
    <li><span style="line-height: 1.5em">Reducing the quantity of code.</span></li>
    <li><span style="line-height: 1.5em">Optimized for fetching data- Provides facilities like- Aggregation,Skip scans etc</span></li>
    <li><span style="line-height: 1.5em">It can use the existing tools- SQL client/terminal, OLAP engine</span></li>
</ul>

<p><strong>Getting Started with Phoenix:-</strong>
<p dir="ltr">You can download Apache Pheonix from the Apache Phoenix site( <a href="http://phoenix.apache.org/download.html">http://phoenix.apache.org/download.html</a>) .There are many Phoenix versions available.Please read the Phoenix documentation before downloading Phoenix and make sure it is compatible with your currently used Hbase version.</p>
<p dir="ltr">Once the Apache Phoenix is downloaded, unzip the tar file and you have to include the Phoenix jar files to Hbase region servers, so that we can establish a successful communication via Phoenix to Hbase.</p>
Note: If your Hbase is not compatible with Phoenix version, we will get an exception saying Client and Server jars are not compatible.</p>
<p>[code language="java"]</p>
<p>$wget http://apache.mirrors.hoobly.com/phoenix/phoenix-3.2.2/bin/phoenix-3.2.2-bin.tar.gz
$ tar -xvf phoenix-3.2.2-bin.tar.gz $ cd phoenix-3.2.2-bin/common
$ cp phoenix-3.2.2-client-minimal.jar /usr/lib/hbase/lib/
$ cp phoenix-core-3.2.2.jar /usr/lib/hbase/lib&lt;/span&gt;[/code]</p>
<p>Phoenix comes up with a command line tool: sqlline (written in python).</p>
<p>[code language="java"]
$ cd ~/phoenix/phoenix-core-3.2.2/bin
$ ./psql.py localhost &lt;sql-script A&gt; &lt;argument for the scrit A&gt; &lt;second-sql script B&gt; &lt;arguments forscript B&gt;
[/code]</p>
<p>For example:
<p dir="ltr">Script A:</p></p>
<p>[code language="java"]
CREATE TABLE IF NOT EXISTS EMPLOYEE (NAME VARCHAR NOT NULL,LOCATION VARCHAR NOT NULL,DOB DATE NOT NULL,EMPLOYEEID INTEGER CONSTRAINT PK PRIMARY KEY (EMPLOYEEID)); [/code]
<p dir="ltr">Arguments for A:</p></p>
<p>[code language="java"]
Abhishek,Delhi,1984-01-01 01:01:01,1
Rakesh,Kolkata,1983-06-25 01:01:01,2
Jemin,Chennai,1990-11-25 01:01:01,3 [/code]
<p dir="ltr">Script B:</p>
<p dir="ltr">Running Sample queries:</p></p>
<p>[code language="java"]
SELECT NAME from EMPLOYEE where EMPLOYEEID=’1’[/code]
<p dir="ltr">Now using sqlline we can connect to Hbase:</p></p>
<p>[code language="java"]
$ ./sqlline.py localhost
[cloudera@localhost bin]$ ./sqlline.py localhost
..
Connecting to jdbc:phoenix:localhost
Driver: org.apache.phoenix.jdbc.PhoenixDriver (version 3.0)
Autocommit status: true
Transaction isolation: TRANSACTION_READ_COMMITTED
..
Done
sqlline version 1.1.2
0: jdbc:phoenix:localhost&gt; select count(*) from EMPLOYEE;
+------------+
|  COUNT(1)  |
+------------+
| 3          |
+------------+
1 row selected (0.112 seconds)
0: jdbc:phoenix:localhost&gt;[/code]
<p dir="ltr"><strong>Running Phoenix on AWS:</strong></p>
<p dir="ltr">We can run Phoenix on AWS- EMR clusters.</p>
<p dir="ltr">Step 1:Specify the Hadoop Version and choose Hbase in application to be installed.</p>
<p dir="ltr"><a href="http://xebee.xebia.in/wp-content/uploads/2015/01/1.png"><img class="alignnone size-medium wp-image-19494" alt="1" src="http://xebee.xebia.in/wp-content/uploads/2015/01/1-300x113.png" width="300" height="113" /></a></p>
<p dir="ltr">Step 2: Specify the following in the Bootstrap portion of Cluster configuration.</p>
<p dir="ltr"><a href="http://xebee.xebia.in/wp-content/uploads/2015/01/2.png"><img class="alignnone size-medium wp-image-19495" alt="2" src="http://xebee.xebia.in/wp-content/uploads/2015/01/2-300x93.png" width="300" height="93" /></a></p>
<p dir="ltr">Once the cluster is running, you can login to the master node using ssh and check your Phoenix configuration.</p>
<p dir="ltr"><a href="http://xebee.xebia.in/wp-content/uploads/2015/01/3.png"><img class="alignnone size-medium wp-image-19496" alt="3" src="http://xebee.xebia.in/wp-content/uploads/2015/01/3-300x224.png" width="300" height="224" /></a></p>
<p dir="ltr"><strong>Operations not supported in Apache Phoenix:</strong></p></p>
<ul>
    <li>
<p dir="ltr">Relational operators like - UNION,INTERSECT and MINUS is not supported.</p>
</li>
    <li>
<p dir="ltr">Transactional support- It does not support and additional transactional capabilities other than what Hbase provides.</p>
</li>
</ul>

<p dir="ltr">We can also integrate Squirrel - SQL pluggin with Apache Phoenix as well.</p>

<p dir="ltr">All we need to do is to add appropriate driver on Squirrel client window.Using this we can easily query the Hbase database we use.</p>

<p dir="ltr"><strong>Conclusion:</strong></p>

<p dir="ltr"><strong></strong>Phoenix is actually been used in places where we want to hit the Hbase database using REST calls and the result of these REST calls are actually to be seen on some UI.</p>

<p dir="ltr">Widespread knowledge of SQL increases the usefulness of Phoenix and related tools like CQL (in case of Cassandra) and Phoenix(in case of Hbase)</p>