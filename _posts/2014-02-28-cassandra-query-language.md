---
layout: post
header-img: img/default-blog-pic.jpg
author: manisha
description: 
post_id: 18079
created: 2014/02/28 20:05:54
created_gmt: 2014/02/28 15:05:54
comment_status: open
---

# CQL : Cassandra Query Language

# CQL : Cassandra Query Language

Many of you know must be heard of or worked on Cassandra - The Columnar NoSQL Database. Most of us gets a feel from the term NoSQL as it is not much like RDBMS because of ease of use in terms of the Query Language. But with Cassandra 2.0 , It is providing a better Query Language Support using CQL3.

Here in CQL 3.0, it has borrowed many of features from SQL like Table creation, OrderBy clause and many more.

The main difference is that Cassandra does not support joins or subqueries, except for batch analysis through Hive. Instead, Cassandra emphasizes denormalization through CQL features like collections and clustering specified at the schema level.

CQL is the recommended way to interact with Cassandra. The simplicity of reading and using CQL is an advantage over older Cassandra APIs.

The main goal of this approach is to give the end user a familiar feel of working with SQL. Additionally CQL is useful in knowing the information related to cluster, Keyspaces structure. And it also provides commands to read the TTL(Time To Live) for data and many other much useful information 

## Launching the CQL Shell

If one has Cassandra installed and running, Open a terminal, you need to go to the your CASSANDRA_HOME directory.

Just e.g. If you have Cassandra setup in** /home/user/cassandra/**, then

[code lang="java"] cd /home/user/cassandra [/code]

And type Command :

[code lang="java"] $ bin/cqlsh [/code]

This will open up the CQL Shell like:

![Screenshot from 2014-02-27 15:43:22][1]

## Creating and updating a keyspace

Creating a keyspace is the CQL counterpart to creating an SQL database, but a little different. The Cassandra keyspace is a namespace that defines how data is replicated on nodes. Typically, a cluster has one keyspace per application. Replication is controlled on a per-keyspace basis, so data that has different replication requirements typically resides in different keyspaces.

1.**Create a keyspace.**

[code lang="SQL"] cqlsh> CREATE KEYSPACE "sampledb" WITH REPLICATION = { 'class' : 'SimpleStrategy' , 'replication_factor' :3 }; [/code]

**2.  Use the KeySpace.**

[code lang="SQL"] cqlsh>Use sampledb; [/code]

**3\. Update the replication factor**

[code lang="SQL"] cqlsh> ALTER KEYSPACE "sampledb" WITH REPLICATION = { 'class' : 'SimpleStrategy' , 'replication_factor' :3};[/code] 

## Create the Column Family

**Creating the table employee with Composite Primary Key.**

Note --> Use a compound primary key when you want to create columns that you can query to return sorted results.

[code lang="SQL"] CREATE TABLE emp ( empID int, deptID int, first_name varchar, last_name varchar, PRIMARY KEY (empID, deptID)); [/code] 

## Inserting Data into Column Family

Similar to SQL Insert Statement.

[code lang="SQL"] INSERT INTO emp (empID, deptID, first_name, last_name) VALUES (104, 15, 'jane', 'smith');[/code] 

## Retrieving and sorting results

To retrieve results, use the SELECT command.

[code lang="SQL"] SELECT * FROM users WHERE first_name = 'jane' and last_name='smith'; [/code]

Similar to a SQL query, use the WHERE clause and then the ORDER BY clause to retrieve and sort results. Procedure

**1\. Retrieve and sort results in descending order.**

[code lang="SQL"] cqlsh:demodb> SELECT * FROM emp WHERE empID IN (130,104) ORDER BY deptID DESC; empid | deptid | first_name | last_name \-------+--------+------------+----------- 104 |     15 |       jane |     smith 130 |      5 |     sughit |     singh (2 rows) [/code]

**2\. Retrieve and sort results in ascending order.**

[code lang="SQL"] cqlsh:demodb> SELECT * FROM emp where empID IN (130,104) ORDER BY deptID ASC; empid | deptid | first_name | last_name <p>\-------+--------+------------+----------- 130 |      5 |     sughit |     singh 104 |     15 |       jane |     smith [/code] 

## Using a Counter

A counter is a special kind of column used to store a number that incrementally counts the occurrences of a particular event or process. For example, you might use a counter column to count the number of times a page is viewed.

Counter column tables must use Counter data type. Counters may only be stored in dedicated tables. You cannot index a counter column.

You load data into a counter column using the UPDATE command instead of INSERT. To increase or decrease the value of the counter, you also use UPDATE.

**Steps:**

**1\. Create keySpace and table for counter column.**

[code lang="SQL"] CREATE KEYSPACE counterks WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 3 }; CREATE TABLE counterks.page_view_counts (counter_value counter, url_name varchar, page_name varchar, PRIMARY KEY (url_name, page_name) ); [/code]

**2.  Load data into the counter column.**

[code lang="SQL"] UPDATE counterks.page_view_counts SET counter_value = counter_value + 1 WHERE url_name='www.xebee.xebia.in' AND page_name='home'; [/code]

**3.  Take a look at the counter value.**

[code lang="SQL"] SELECT * FROM counterks.page_view_counts; [/code]

**Output is:**

url_name         | page_name | counter_value \------------------+-----------+--------------- www.datastax.com |      home |             1

(1 rows)

**4.  Increase the value of the counter.**

[code lang="SQL"] UPDATE counterks.page_view_counts SET counter_value = counter_value + 2 WHERE url_name='www.datastax.com' AND page_name='home'; [/code]

**5\. Take a look at the counter value.**

[code lang="SQL"] SELECT * FROM counterks.page_view_counts; [/code]

**Output :** url_name         | page_name | counter_value \------------------+-----------+--------------- www.datastax.com |      home |             3 

## Collection Columns

CQL introduces these collection types: 

  * set
  * list
  * map
In a relational database, to allow users to have multiple email addresses, you create an email_addresses table having a many-to-one (joined) relationship to a users table. CQL handles the classic multiple email addresses use case, and other use cases, by defining columns as collections. Using the set collection type to solve the multiple email addresses problem is convenient and intuitive.

**1\. Adding a collection to a table**

Here we are altering an existing Table Named songs to add a collection to store tags for each Song.

[code lang="SQL"] ALTER TABLE songs ADD tags set<text>; [/code]

**2\. Updating a collection**

[code lang="SQL"] UPDATE songs SET tags = tags + {'1973'} WHERE id = a3e64f8f-bd44-4f28-b8d9-6938726e34d4; UPDATE songs SET tags = tags + {'blues'} WHERE id = a3e64f8f-bd44-4f28-b8d9-6938726e34d4; UPDATE songs SET tags = tags + {'rock'} WHERE id = 7db1a490-5878-11e2-bcfd-0800200c9a66; [/code]

**3\. Querying a Collection** To query a collection, include the name of the collection column in the select expression.

**Example:**

[code lang="SQL"] SELECT id, tags FROM songs; [/code] 

## Prepared statements

   [1]: http://xebee.xebia.in/wp-content/uploads/2014/02/Screenshot-from-2014-02-27-154322-300x39.png