---
layout: post
header-img: img/default-blog-pic.jpg
author: sanchit
description: 
post_id: 17085
created: 2013/09/02 15:01:00
created_gmt: 2013/09/02 10:01:00
comment_status: open
---

# Big Data Sessions at IndicThreads'13

The Big Data day at Indic Threads Delhi 2013 started with session by _Gagan Agarwal_ on _"Using Graph Databases for Insights into Connected Data"_ He took us through the basics of Graph databases , how they are fundamentally different from R-DBMS and NoSQL stores (i.e. key/value, column family and document stores) and then show cased some advanced features which can be useful in certain application development scenarios. Being a technology enthusiast Gagan handled queries quite well clearing the doubts in minds of audience on using Graph Databases in most scenarios whereas he highlighted their useful in scenarios that are more like the problems solved at Facebook and Linkedin. A relevant blog post covering a use case on above topic can be looked at <http://xebee.xebia.in/index.php/2013/08/24/neo4j-vs-rdbms-handling-access-control-in-document-management-system/>** **

First session gave a new dimension to the audience who got to know database in a different way that was earlier known to them and also made a good start for the day to continue with next session by _Srihari Srinivasan_ on_ "SQL on Hadoop - Is SQL the next big thing for Hadoop"_. His session took us from the early journey of Hadoop to its the present condition where it is being feared as a competing technology by a lot many and the number of engineers having Map-reduce skills is far less than those who already know SQL is a reason good enough for Hadoop as a technology to provide support for SQL like language which will find it a better position in enterprises and close the gap in technology. Later on Srihari took the audience through following types of query processing methods that Hadoop can support

i) Querying using Hive Query Language

ii) Distributed Querying on Hadoop in which he talked about Bloom filters

iii) Split Query Processing by Hadoop

iv) Cloudera Impala to issue low-latency SQL queries to data stored in HDFS

The talk also started a day long question about the relevance of Big Data / Hadoop hype, is it really useful and why should an organization have Hadoop at first place, to which Srihari told us that it is based on the problem or the requirement that one has which will call for using it or discarding it. Being quite experienced and having a keen sense of business acumen, Srihari also gave a perspective to the audience on how a new technology although seamlessly scalable can also have its own limiting boundaries when it comes to being adopted by businesses and decision makers.

Previous insightful session was followed by much more in to the code session by tech-geek and Apache Crunch committer _Rahul Sharma_ who gave a session on _"Building Hadoop pipelines using Apache Crunch". _The session focused on how Apache Crunch project make life easier for writing map reduce tasks for Hadoop by reducing the amount of boiler plate code that one has to write to run a map-reduce job through and how crunch has provided support for different useful in-built methods for not so trivial and difficult to implement using plain map-reduce tasks like joining & data aggregation which are better to be used with Crunch's pipeline. We then went through some helpful classes and methods present in Crunch. Rahul also mentioned the advantages of this approach which not only helped in faster development but also in cleaner approach as writing Unit Test cases with this approach covers the limitations of other unit testing frameworks like MRUnit. The session was received well by the audience and followed by an un-conference round where all had a chance to talk on a topic of their choice and get the views of audience on it, theme for un-conference session was on emergence of functional languages, application of Web Semantics, storing JSON in Postgres vs NoSQL, and light discussion on capacity planning in a cloud environment.

As was the need of hour after a sumptuous lunch, _Manoj Mohan_ gave a high energy filled session on _"Big Data Search Simplified with Elastic Search"_. He shared his experiences of working on Elastic Search and compared it with other solutions available in the market like Sphinx and how Elastic Search makes use of Solr capabilities and extends them to provide better search in a distributed environment. He also explained the idea of generating indexes and removal of Stop words (I, am, an, the...) from the search queries to make indexing and searching faster. He also covered some details about percolation which can be thought of as an reverse operation of indexing and then searching. Instead of sending docs, indexing them and then running queries, one sends queries , registers them and then send docs to find out which queries match that document. There were a few concerns that audience had left in their minds with regard to the scale to which Manoj had used in his project and raised a few concerns in audience's minds which were well cleared by next speaker _Harpreet Singh_ who had a lot of experience in building scalable applications.

_Harpreet Singh_ gave a very insightful session on _"Building an Enterprise Big Data Platform for 100TB Dataset". _He shared with us case studies on the projects he had worked on. He shared his experience in big data and also told that a lot of big data work is done in the field of image processing and pattern recognition as well and possibly the human genome project will trigger more development in this field as the amount of data generated and to be processed is huge in this field. He mentioned about a Hadoop compatible library for image processing known as HIPI which was used in a project for disease detection using pattern recognition in images from blood samples. The primary function of HIPI is not to do pattern recognition but to break an image of very large size (6GB) into small pieces based on a criteria that user sets like area of pixels , different color type of pixels and others. A very salient point was told by him that most of the work that is done in big data is around the cleaning of data and aggregating it together for the purpose of analysis which is easier to build when the statistical models are already in place. To give a complete architectural view for a Big Data solution, he told that a Big Data solution is a combination of cloud infrastructure and other components. He also shared a case study on text classification which can be used for blogs analysis.

The day concluded with distribution of some goodies from Indic Threads organizers for eco-travelers (ones who used public transport like metro or bus) and some networking among the attendees with light snacks.

The overall experience of attending IndicThreads was quite enriching in terms of the knowledge gained.

To  know more about Xebia's offereing on Big Data, click here : <http://xebia.in/big-data-solutions.html>