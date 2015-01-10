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

<p>Hadoop came in market as savior for various problems in processing Huge Data (tera/peta bytes). Hadoop is growing on very fast pace as a solution for many Big Data related problems. Today a lot more tools are available in market to make development of Hadoop jobs very easy, but basic knowledge of How Hadoop works is very important in order to select tools and decide whether Hadoop solves your problem or not.</p>
<p>Many resources are available on internet on this topic but the information available, is very much scattered. I made a very easy to understand comic style presentation keeping in mind that “action speaks more than words”.
<!--more-->
Hadoop stores the peta/tera bytes of data over cluster of commodity machines and run the same job in parallel over all the machines in order to achieve following advantages.
<ol>
<li>
Complete big jobs in considerably less time.
</li>
<li>
To avoid OutOfMemoryError for huge data.
</li>
<li>
Cluster will be created with simple/cheap commodity hardware. No expensive servers are required.
</li>
</ol></p>
<p>Basically Hadoop job is divided in two important tasks.
<ol>
<li>
Storing data with HDFS.
</li>
<li>
Create/Run Map Reduce job.
</li>
</ol></p>
<p><b>Storing data with HDFS</b></p>
<p>HDFS is Hadoop Distributes File System. Hadoop comes with HDFS and we need to store data on HDFS if we want to run job with Hadoop on clustered environment. HDFS is file system coded in java over OS file system. HDFS is created to have single file system over thousands on data nodes in cluster so that entire data can be accessed/controlled from one single point of contact, in Hadoop terminology we call it as Name Node.</p>
<p>We can simply store the data to HDFS using its client. Hadoop provides shell based CLI(command line interface) and java library for interaction with HDFS.</p>
<p>I have created the following presentation to showcase how a user can store data with HDFS.</p>
<div style="margin-bottom: 5px"><strong> <a title="Hdfs" href="https://www.slideshare.net/gaganjuneja/hdfs-15568286" target="_blank">Hdfs</a> </strong> from <strong><a href="http://www.slideshare.net/gaganjuneja" target="_blank">gaganjuneja</a></strong></div>

<p><b>MapReduce</b></p>
<p>Hadoop works on the principle that we should run the same job in parallel on thousands of machines to solve problems related to big data in lesser time. This job is called as Map Reduce job. Map Reduce concept can be applicable only where we can divide our data in independent chunks, we call them as split in Hadoop terminology. Split size plays here very important rule in optimizing the MapReduce job. If the split size is small then we could end up with more MR jobs and if Split size is large then It will take more time to complete one job.</p>
<ol>
<li>Map</li>
</ol>
<p>Map task iterates over the split data (may be line by line in case of file) and creates key value pairs as defined in the Map job and store these key value pairs some where in the local file system.</p>
<ol>
<li>Reduce</li>
</ol>
<p>Reduce phase collect the output of Map phase, sort them based on the key, merge all the values of same key and provide the key and iterator of values to reduce task which is specified y the user.</p>
<p>I have created the following presentation to showcase how a Map Reduce job works. I am not covering here that how to write MR jobs.</p>
<div style="margin-bottom: 5px"><strong> <a title="Map reduce" href="https://www.slideshare.net/gaganjuneja/map-reduce-15569921" target="_blank">Map reduce</a> </strong> from <strong><a href="http://www.slideshare.net/gaganjuneja" target="_blank">gaganjuneja</a></strong></div>

<p>See Xebia's offering in Big Data <a href="http://www.xebia.in/big-data-solutions.html" target="_blank">here</a>.</p>