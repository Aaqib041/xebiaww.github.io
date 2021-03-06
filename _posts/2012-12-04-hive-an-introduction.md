---
layout: post
header-img: img/default-blog-pic.jpg
author: Gagan
description: 
post_id: 15379
created: 2012/12/04 14:14:11
created_gmt: 2012/12/04 09:14:11
comment_status: open
---

# Hive an Introduction!

Data is growing like anything and industry specialists think that RDBMS is not going to work for tera/peta bytes of data. Hadoop came as a savior for Big Data related issues. Term Big Data/Hadoop scares people who are well versed in writing sql queries for getting data from database because hadoop jobs are all about Map Reduce and their SQL knowledge will not be going to help in writing Map Reduce.

Hive becomes as a Remedy for this which bridges the gap between SQL and Map Reduce. Hive provides QL(query Language) and convert the query language to Map reduce jobs internally. Hive provides very powerful query language like SQL. People with no knowledge of Map Reduce can also query data from Hadoop. Let's understand Hive from RDMS perspective.  * For Hive Installation you may go through the [Hive Installation Document][1]. I am running all the QLs using Hive CLI. I am assuming that readers of this post are aware of SQL.

** How Hive Manages Data?**

Hive keeps data in any of the Hadoop supported file system like HDFS or local file system. Hive keeps table structure information in meta store which can be any of the JDBC supporting RDMS i.e. Derby, MySql, Postgres etc. By default Hive provides one embedded derby instance which can cater only one user locally.

Hive doesn't validate the table structure while loading the data into table. It only validates the table structure while querying the table, that provides fast load of data which is very much helpful in Big Data scenario.

**let's explore Hive Features.**

  1. Hive supported Data Types Hive supports all Basic data types along with complex data types like Array, Map etc. 
    1. Primitive data types Hive primitive types are almost equivalent to java and names are taken from MySql. TINYINT(1 byte) SMALLINT (2 bytes) INT (4 bytes) BIGINT(8 Bytes) BOOLEAN FLOAT DOUBLE STRING BINARY TIMESTAMP
    2. Complex Data types Hive supports java data types like Array, Map. Struct is also supported which represents a set of named fields. arrays: ARRAY<data_type> maps: MAP <primitive_type, data_type> structs: STRUCT<col_name : data_type [COMMENT col_comment], ...>
  2. Hive Table The table in hive is conceptually analogous to rdbms table. But actually Hive just provides one wrapper to give user feeling that data is stored in table structure. The data actually resides in any Hadoop file system like HDFS or local file system. Hive store table structure and other information in rdbms and call it as metastore. There are two type of tables in Hive. 
    1. Managed Tables By default all the tables created in Hive are Managed Tables. As name suggests Managed tables are tables where data is managed by Hive itself which means once you load data into managed table it move the file to hive warehouse store (one location in file system where hive keeps data), which involves only move operation. When you drop the table it will remove the file from warehouse and delete the metadata information. let's create table RESIDENTS with fields ID, NAME, STATE, CITY. ``` 
 CREATE TABLE RESIDENTS (ID BIGINT, NAME STRING, STATE STRING, CITY STRING); 
 ```

This is sample managed table. We can load data into this table using following statement. ``` 
 LOAD DATA INPATH '/user/gagan/residents.txt' INTO TABLE RESIDENTS; 
 ```

If the file system remains same then this load statement will only result into move the file residents.txt into Hive warehouse.
    2. External Table As name suggests external tables are tables where Hive doesn't manage table create and drop operations ``` 
 CREATE EXTERNAL TABLE RESIDENTS_EXTERNAL (ID BIGINT, NAME STRING, STATE STRING, CITY STRING) LOCATION '/user/gagan/RESIDENTS_EXTERNAL'; LOAD DATA INPATH '/user/gagan/residents.txt' INTO TABLE RESIDENTS_EXTERNAL; 
 ```

Here we explicitly mention that table is external which indicates hive that do not move data to warehouse. Hive event doesn't validate the location while creating the table.
  3. Import Data to Hive table There are couple of ways in which we can import data into Hive. 
    1. Import data from file As described above data can be loaded from into hive using LOAD statement.
    2. Importing Data from another table Like rdbms, we can import data to one hive table by querying other hive tables and the syntax will look like ``` 
 INSERT INTO TABLE RESIDENTS SELECT * FROM RESIDENTS_EXTERNAL; 
 ```

INSERT INTO will append data into RESIDENTS table. INSERT OVERRIDE can be used to override the existing table data. Hive doesn't support INSERT INTO TABLE VALUES clause of RDBMS.
    3. CTAS (CREATE TABLE AS SELECT) Sometimes we need to insert the result of query while creating a new table. This clause help in that scenario. Now the new table will copy the structure from the select statement. CTAS syntax looks like. ``` 
 CREATE TABLE NEW_TABLE AS SELECT ID, NAME FROM RESIDENTS; 
 ```
  4. Querying Hive tables Querying hive tables is very much similar to SQL. Hive supports large numbers of built-in functions like mathematical, date ,string manipulation functions. some examples are as follows. ``` 
 SELECT ID,NAME FROM RESIDENTS; SELECT ID,NAME from RESIDENTS WHERE CITY='DELHI'; SELECT COUNT(*) FROM RESIDENTS WHERE STATE='HARYANA'; SELECT MAX(ID) FROM RESUDENTS; SELECT ID,NAME FROM RESIDENTS WHERE CITY LIKE '%PUR'; 
 ```

Hive provides ORDER BY clause as well. As we know that Hive QL is translated into MR jobs so ORDER BY (runs on global data) make reducers to be 1 which in turn slow down the operation. We can perform sorting using SORT BY if sorting is not required on the entire data set. SORT BY produces sorted output per reducer.
  5. Joins Hive supports only equijoins,outer joins, left semi joins. Join conditions other then equality are very difficult to represent into MR jobs. Hive join syntax is bit different from SQL. let's create two tables to understand joins in hive. ``` 
 CREATE TABLE EMPLOYEES (ID INT, NAME STRING); CREATE TABLE PROJECTS (PROJECT_NAME STRING, EMPLOYEE_ID INT) 
 ``` 
    1. Equi Join Also known as Inner join. let's generate the report for employees who are allocated to some project. ``` 
 SELECT NAME FROM EMPLOYEES JOIN PROJECTS ON (EMPLOYEES.ID = PROJECTS.EMPLOYEE_ID) 
 ```

Here we have only one table name in from clause and other will be in JOIN ON clause.
    2. Outer JOIN LEFT and RIGHT both types of outer joins are supported by Outer Joins. Definition of outer joins remains same as SQL. Below example list all the employees with project information and NULL if no project record is available. ``` 
 SELECT NAME FROM EMPLOYEES LEFT OUTER JOIN PROJECTS ON (EMPLOYEES.ID = PROJECTS.EMPLOYEE_ID) 
 ```
    3. Semi Joins Hive doesn't support IN/EXISTS sub queries. Left Semi Joins are here to help writing these queries. The restrictions of using LEFT SEMI JOIN is that the right-hand-side table should only be referenced in the join condition (ON-clause), but not in WHERE- or SELECT-clauses etc. ``` 
 SELECT * FROM EMPLOYEES WHERE ID IN (SELECT EMPLOYEE_ID FROM PROJECTS); 
 ```

can be written as ``` 
 SELECT EMPLOYESS.* FROM EMPLOYEES LEFT SEMI JOIN PROJECTS ON (EMPLOYEES.ID = PROJECTS.EMPLOYEE_ID) 
 ```
  6. Alter/Drop tables Like SQL, tables can be altered or dropped in Hive. 
    1. Alter Table Alter table helps in renaming the table name or altering the structure of table. ``` 
 ALTER TABLE RESIDENTS RENAME TO INDIA_RESIDENTS; ALTER TABLE INDIA_RESIDENTS ADD COLUMNS (FATHER_NAME STRING, MOTHER_NAME STRING); 
 ```
    2. Drop Table Drop table deletes the data and metadata for a table but only metadata in case of external table. ``` 
 DROP TABLE RESIDENTS; 
 ```
Hive is a good attempt to create SQL like language for querying hadoop data. You can refer to [Hive Documentation][2] for detailed features list and syntaxes.

   [1]: https://cwiki.apache.org/confluence/display/Hive/Home
   [2]: https://cwiki.apache.org/Hive/tutorial.html