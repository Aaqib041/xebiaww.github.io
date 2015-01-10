---
layout: post
header-img: img/default-blog-pic.jpg
author: saket_vishal
description: 
post_id: 3752
created: 2010/05/31 17:20:57
created_gmt: 2010/05/31 12:20:57
comment_status: open
---

# Test your Hibernate Search integration end to end

Testing Hibernate Search integration end to end can be a quite complicated task, which can be attributed to its dependencies on both data and indexes. A test framework built by a combination of HsqlDB and DbUnit can provide a solution that can overcome the fore mentioned complexity.

To begin with I assume, you have already integrated Hibernate Search into your code, and have that working. Follow these steps to set up the test framework.

**Integrate HsqlDb**

[HsqlDb][1] is a popular in-memory database. Integrating HsqlDb with Hibernate should not be a problem, as you just need to provide the connection related information in your data-source and hibernate properties. You must have already annotated or configured for hibernate and hibernate search mappings, you can easily create your schema, using hbm2ddl schema export property.

**Integrate DbUnit**

[DbUnit][2] is also a known framework for database unit testing. However, I faced one of the issues while working with HsqlDb. DbUnit ignores boolean column,while making insertion of data into HsqlDb, and this can be a big problem. You can use set a database configuration in the DbUnit connection object to use the HsqlDb datatype factory to overcome this problem.

config.setProperty(DatabaseConfig._PROPERTY_DATATYPE_FACTORY_,**new** HsqldbDataTypeFactory());

Provision of an inital dataset would complete your setup. Now, you have a test framework that has an in-memory database, with respective schema, and has initial data using DbUnit, and this whole data unit can further be used by Hibernate Search to create indexes, and perform searches.

You still need to keep few things in mind, while working with above setup. When DbUnit inserts initial data-set into HsqlDb it won't be indexing the data, so you need to create initial indexes pragmatically. Another point worth mentioning is that the above tests could run slowly, as it needs to perform database insertions, and then create lucene indexes. The speed of the test cases can be improved by selecting small data-sets, and performing indexing only when required, and not for each test case.

   [1]: http://hsqldb.org/
   [2]: http://www.dbunit.org/