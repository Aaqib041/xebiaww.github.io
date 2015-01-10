---
layout: post
header-img: img/default-blog-pic.jpg
author: Gagan
description: 
post_id: 16138
created: 2013/02/27 10:09:12
created_gmt: 2013/02/27 05:09:12
comment_status: open
---

# HBase – A real time access to Big Data

The term Hadoop comes with a feeling of batch processing. Many applications/use cases have demand of real time processing on Big Data. No worries, Apache HBase a versioned, column oriented data store is there to help us in real time processing on Big Data.

Apache HBase is modeled based on Google's paper on [BigTable][1]. HBase is built on Hadoop and HDFS.

HBase provides two basic features. 

  1. It provides us access to Hadoop from database prospective.
  2. It provides us transactional (real time) platform to work on multi cluster environment.
**Is HBase replacement on RDBMS?** In my opinion HBase can be treated as replacement for RDBMS in only case when we have enough data like millions/billions of records. Otherwise RDBMS should be of our choice.

Let's see the basic terminology of HBase 

  1. Table HBase stores data in tables. Table has a row columns structure. There is a concept of Row key (Primary Key in RDBMS) and column values in HBase Table. Any thing which can be converted into byte array can be used as a Row key. Columns are categorized into column families. Column value for each row (a cell in table) is version-ed based on time stamp by default but we can customize this. Let's look at table structure for clear understanding. 

Row Key
Version
ColumnFamily personal
ColumnFamily official

10001
T3
personal:first_name = “firstName”

10001
T2
Personal:last_name = “lastName”

10001
T1
Official:company_name= “Xebia”
In rdbms database we would be storing this record like (10001, firstName, lastName, Xebia). But here we have stored multiple rows for single Row Key. This is the reason that we can add any number of columns to a column family and that is not the case with rdbms table because at runtime we can not add columns to rdbms table. Here we can specify the version in query if we don't want to fetch the latest value for some column. By default latest value is returned by HBase.
  2. Column Family Like columns are fixed in rdbms table means we can not add column on runtime but later we can use DDL Alter table and add columns. In HBase as well we define column families while during schema definition and we can not add column families on runtime. Alter statements are provided by HBase to add column families but on runtime this is not possible. Column names are delimited from column family using colon(:). Tuning and storage specifications are performed at column family level and that will be applied to all the columns present under certain column family.
  3. Regions As soon as data grows beyond the capacity of one node HBase automatically partition the table into regions. Regions are represented with table name, start row (inclusive) and end row (exclusive).
**HBase Architecture**

HBase works on the theme of master and slave like HDFS or Map Reduce. Here master is known as HBase Master and slaves as Region Servers. HBase Master keeps information about all the region servers, it assigns regions to region servers and help in recovery of region server failures.

Region server may have zero or more regions and handle client read/write requests as region is the place where we store the tables.

How HBase stores Data? HBase maintains two internal tables call it as Catalog tables. 

  1. -ROOT- -ROOT- table keeps track of the .META. table. Key : .META. Region key values info : regioninfo (serialized instance of .META.) info : server (server:port of Region Server process holding .META.) info : serverstartcode (start-time of Region Server process holding .META.)
  2. .META. .META. Table keeps track of all the regions in the system. Key : Region key which is comprises of (table name, region start key, region id) values info : regioninfo (serialized instance of region) info : server (server:port of Region Server process holding region) info : serverstartcode (start-time of Region Server process containing region)
**What is Compaction?** When we write some data to regions it first appended to the commit log and the added to the memstore (in memory). When memstore fills and threshold limit crossed then data is flushed to the file system. This process is called Compaction. Compaction serves the physical deletion for the delete requests as well. This memstore helps HBase in providing real time flavor because finally the data is going to store in HDFS only.

Let us try some hands on HBase. 

  1. To install HBase please refer to this [link][2].
  2. Start Habse using ./start-hbase.sh
  3. Start Hbase shell using command “hbase shell”
  4. Let's create one table with name 'my_table' and column family name 'my_cf1'. [code] create 'my_table', 'my_cf1' [/code]

We can list all tables in Hbase using list command.

[code] list hbase(main):013:0> list TABLE my_table 1 row(s) in 0.0090 seconds [/code]

Here Table is the default table of HBase that we discussed above and my_table is the one that we created in this example.
  5. Let's put some data in newly created table. [code] put 'my_table', 'row1', 'my_cf1:column1', 'value11',1 put 'my_table', 'row1', 'my_cf1:column2', 'value12',2 put 'my_table', 'row2', 'my_cf1:column1', 'value21',3 put 'my_table', 'row2', 'my_cf1:column2', 'value22',4 put 'my_table', 'row1', 'my_cf1:column1', 'value13',5 [/code]

Here first parameter is table name, second is row key, third is column of particular column family, fourth is value for that column and finally the time stamp. In case we don't pass the time stamp system will take default values.
  6. We can see the content of table using scan command. [code] scan 'my_table'

Output is:

   [1]: http://research.google.com/archive/bigtable.html (Google Big Table)
   [2]: http://hbase.apache.org/book/quickstart.html (HBase Installation)