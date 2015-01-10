---
layout: post
header-img: img/default-blog-pic.jpg
author: Gagan
description: 
post_id: 17191
created: 2013/09/27 09:19:48
created_gmt: 2013/09/27 04:19:48
comment_status: open
---

# Let's understand Hadoop!

Hadoop came in market as savior for various problems in processing Huge Data (tera/peta bytes). Hadoop is growing on very fast pace as a solution for many Big Data related problems. Today a lot more tools are available in market to make development of Hadoop jobs very easy, but basic knowledge of How Hadoop works is very important in order to select tools and decide whether Hadoop solves your problem or not.

Many resources are available on internet on this topic but the information available, is very much scattered. I made a very easy to understand comic style presentation keeping in mind that “action speaks more than words”.  Hadoop stores the peta/tera bytes of data over cluster of commodity machines and run the same job in parallel over all the machines in order to achieve following advantages. 

  1. Complete big jobs in considerably less time. 
  2. To avoid OutOfMemoryError for huge data. 
  3. Cluster will be created with simple/cheap commodity hardware. No expensive servers are required. 

Basically Hadoop job is divided in two important tasks. 

  1. Storing data with HDFS. 
  2. Create/Run Map Reduce job. 

**Storing data with HDFS**

HDFS is Hadoop Distributes File System. Hadoop comes with HDFS and we need to store data on HDFS if we want to run job with Hadoop on clustered environment. HDFS is file system coded in java over OS file system. HDFS is created to have single file system over thousands on data nodes in cluster so that entire data can be accessed/controlled from one single point of contact, in Hadoop terminology we call it as Name Node.

We can simply store the data to HDFS using its client. Hadoop provides shell based CLI(command line interface) and java library for interaction with HDFS.

I have created the following presentation to showcase how a user can store data with HDFS.

** [Hdfs][1] ** from **[gaganjuneja][2]**

**MapReduce**

Hadoop works on the principle that we should run the same job in parallel on thousands of machines to solve problems related to big data in lesser time. This job is called as Map Reduce job. Map Reduce concept can be applicable only where we can divide our data in independent chunks, we call them as split in Hadoop terminology. Split size plays here very important rule in optimizing the MapReduce job. If the split size is small then we could end up with more MR jobs and if Split size is large then It will take more time to complete one job.

  1. Map

Map task iterates over the split data (may be line by line in case of file) and creates key value pairs as defined in the Map job and store these key value pairs some where in the local file system.

  1. Reduce

Reduce phase collect the output of Map phase, sort them based on the key, merge all the values of same key and provide the key and iterator of values to reduce task which is specified y the user.

I have created the following presentation to showcase how a Map Reduce job works. I am not covering here that how to write MR jobs.

** [Map reduce][3] ** from **[gaganjuneja][2]**

See Xebia's offering in Big Data [here][4].

   [1]: https://www.slideshare.net/gaganjuneja/hdfs-15568286 (Hdfs)
   [2]: http://www.slideshare.net/gaganjuneja
   [3]: https://www.slideshare.net/gaganjuneja/map-reduce-15569921 (Map reduce)
   [4]: http://www.xebia.in/big-data-solutions.html