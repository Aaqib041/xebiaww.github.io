---
layout: post
header-img: img/default-blog-pic.jpg
author: swapnil178
description: 
post_id: 18186
created: 2014/03/25 17:22:54
created_gmt: 2014/03/25 12:22:54
comment_status: open
---

# Apache Sqoop: Parallel Import/Export from RDBMS to Hadoop

_Main purpose of **Apache sqoop** is to **import and export data from RDBMS**(for example: MySql,Oracle etc) to Hadoop(HDFS/Hbase etc). _

Imagine, if you have a system where you are producing huge volume of data, and you want to extract some part of the data and do some analysis such . You wish to view the results of such analysis quite often. You can't use a Hadoop system to have quick views of your data. So a possible work flow for such a problem can be :-

                 capture data into HDFS - > Analyze data with Map Reduce- >HDFS to RDBMS

The benefit of using this kind of work flow is based on simple fact that Hadoop systems are not for quick reads whereas an RDBMS system can effectively resolve the problem of quick reads. We are using the MapReduce paradigm to turn our raw data to substantial information(may be usually at the end of day using some batch process) and finally pushing it into an RDBMS system for display purpose.

Sqoop can play a vital role here, if we are facing a problem similar to above mention work flow.

**Typical Workflows:-**

  1. RDBMS to HDFS - Analyze data with Map Reduce- HDFS to RDBMS
  2. RDBMS to Hive/Habase- Analyze data with HQL-HIve To RDMS
  3. NoSql to HDFS/Hive/Hbase- Analyze with HQL/MR - HDFS/Hive/Hbase
**Example Data PipeLine:-**

![image1][1]

In the above shown example, we are collecting data from two different resources, i.e Log of Webserver and User Profiles from MySql DB. These two need to to be inserted into a form where our Map Reduce can run(i.e Hive in our case) . So for pushing data from MySql DB to Hive we can use Sqoop.

W**orking of Sqoop:-**

![image2][2]

  1 .When we try to use any sqoop tools.Sqoop will communicate with the database store , it will fetch meta-data information from RDBMS. Sqoop will use this meta data     for generating the java class.

  2. Sqoop gets the metadata from DB store.
  3. Sqoop will internally create a java class using JDBC API. Sqoop will compile the java class using JDK and create a .class file.A .jar file will be created from . class file.
  4. After creating the jar files, sqoop will try to communicate with DB store again, and will try to find out the split column. Based on te split column Sqoop will fetch the data from DB
  5. Finallly, Sqoop places the retrieved data into HDFS. 
Once we understand the above internal process of Sqoop, impport and export process is self explanatory:-

**Import Process of SQOOP:-**

This diagram is from the Apache documentation...

![image3][3]

**Export process of SQOOP:-**

This diagram is from the Apache documentation...** **

![image4][4]

**Databases supported by Sqoop: -**

Sqoop works well with lots of vendors.

Database

version

`\--direct `support?

connect string matches

HSQLDB

1.8.0+

No

`jdbc:hsqldb:_//`

MySQL

5

Yes

`jdbc:mysql://`

Oracle

10.2.0+

No

`jdbc:oracle:_//`

PostgreSQL

8.3

   [1]: http://xebee.xebia.in/wp-content/uploads/2014/03/image1-300x190.png
   [2]: http://xebee.xebia.in/wp-content/uploads/2014/03/image2-300x168.png
   [3]: http://xebee.xebia.in/wp-content/uploads/2014/03/image3-300x285.png
   [4]: http://xebee.xebia.in/wp-content/uploads/2014/03/image4-300x292.png