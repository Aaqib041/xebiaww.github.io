---
layout: post
header-img: img/default-blog-pic.jpg
author: Gagan
description: 
post_id: 15036
created: 2012/10/08 22:15:23
created_gmt: 2012/10/08 17:15:23
comment_status: open
---

# Spring Supports Hadoop Now!

Spring aims to make enterprise aplication development as simple as possible. Spring makes life of a developer very easy by providing JDBC, JMS, Web Services, Security and many more frameworks under one umbrella. Spring reduces the complexity and provide power to developer to do whole lot of things from spring context. Now Spring has extended its scope to add Big Data frameworks like Hadoop, HDFS, PIG, HIVE, No Sql Databases.  Spring announced its first milestone release 1.0.0.M1 for Spring for Apache Hadoop. This Spring initiative reflects the growing importance of Big Data within enterprise applications. Spring has simplified the programming model for complex hadoop jobs. Spring shows the world that Hadoop can now be integrated with Enterprise Applications very easily. Undoubtedly It will help accelerating Hadoop's growth. Spring supports almost all the features of Hadoop and provides very easy and elegant ways to configure Hadoop job. Some basic features supported by Spring are: 

  1. Supports Configuration, creation and execution of MapReduce Job.
  2. HDFS data access support through JVM scripting languages (Groovy, JRuby, Jython, Rhino, etc.)
  3. Support for use with Spring Integration that provides easy access to a wide range of existing systems. Like you can integrate Hadoop Job with Spring MVC.
  4. Powerful Hadoop configuration options and a template mechanism for client connections to Hadoop.
  5. Declarative and programmatic support for Hadoop Tools, including FsShell and DistCp.
Let us create simple and very common Hadoop's WordCount problem with Spring. I am assuming that readers are familiar with existing Hadoop jobs and have already installed Hadoop on their Systems. 
  1. Create Simple maven project. add dependencies for Hadoop and spring in the pom.xml file of your project.
[sourcecode lang="text"] <dependency> <groupId>org.springframework.data</groupId> <artifactId>spring-data-hadoop</artifactId> <version>1.0.0.M2</version> </dependency> <dependency> <groupId>org.apache.hadoop</groupId> <artifactId>hadoop-core</artifactId> <version>1.0.3</version> </dependency> [/sourcecode] Spring Hadoop dependency is not available in maven repository so add below repository link to download this dependency. [sourcecode lang="text"] <repositories> <repository> <id>spring-milestone</id> <name>Spring Maven SNAPSHOT Repository</name> <url>http://repo.springframework.org/milestone</url> </repository> </repositories> [/sourcecode] 
  2. Map and Reduce program will remain same. Following are the Mappers and Reducers for WordCount Problem.
**Map.java** [sourcecode lang="java"] package com.xebia; import java.io.IOException; import org.apache.commons.lang.StringUtils; import org.apache.hadoop.io.IntWritable; import org.apache.hadoop.io.LongWritable; import org.apache.hadoop.io.Text; import org.apache.hadoop.mapreduce.Mapper; public class Map extends Mapper<LongWritable, Text, Text, IntWritable> { private static final IntWritable INITIALCOUNT = new IntWritable(1); private static final Text KEY = new Text(); protected void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException { for (String token : StringUtils.split(value.toString())) { KEY.set(token); context.write(KEY, INITIALCOUNT); } } } [/sourcecode] **Reduce.java** [sourcecode lang="java"] package com.xebia; import java.io.IOException; import org.apache.hadoop.io.IntWritable; import org.apache.hadoop.io.Text; import org.apache.hadoop.mapreduce.Reducer; public class Reduce extends Reducer<Text, IntWritable, Text, IntWritable>{ public void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException { int totalcount = 0; for (IntWritable count : values) { totalcount = totalcount + count.get(); } context.write(key, new IntWritable(totalcount)); } } [/sourcecode] map and reduce functions are very much obvious. Map split the line and set each word as key and "1" as value. Reducer sort the keys and reduce function calculates the sum for each key and prints as reducer output. 
  3. After writing MapReduce the remaining task is to configure Hadoop Job. This is the point where Spring help us in configuring the job. To configure the job we need to write spring context.xml file. [sourcecode lang="xml"] <?xml version="1.0" encoding="UTF-8"?> <beans xmlns="http://www.springframework.org/schema/beans"   
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"   
xmlns:context="http://www.springframework.org/schema/context"   
xmlns:hadoop="http://www.springframework.org/schema/hadoop"   
xmlns:p="http://www.springframework.org/schema/p"   
xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd   
http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd   
http://www.springframework.org/schema/hadoop http://www.springframework.org/schema/hadoop/spring-hadoop.xsd"> <hadoop:configuration> fs.default.name=hdfs://localhost:54310 </hadoop:configuration> <hadoop:job id="wordcount" input-path="/input" output-path="/output"   
mapper="com.xebia.Map" reducer="com.xebia.Reduce"/> </beans> [/sourcecode] 
    1. Line 5 declares Spring for Apache Hadoop namespace prefix. Which is hadoop and will be used in the entire file to refer hadoop specific tags.
    2. In order to use Hadoop we need to create Configuration object which helps in configuring Hadoop.It holds information about the job tracker, the input, output format and the various other parameters of the map reduce job.
    3. The one liner declaration <hadoop:configuration /> defines a Configuration bean (to be precise a factory bean of type ConfigurationFactoryBean) named, by default, hadoopConfiguration. The default name is used, by conventions, by the other elements that require a configuration - this leads to simple and very concise configurations as the main components can automatically wire themselves up without requiring any specific configuration.
    4. Here at line 11 I configured the "fs.default.name" property explicitly because I am using different port for hdfs file system then the default port (9001).
    5. Line 13 defines a Hadoop job with properties like input-path,output-path, mapper and reducer. This is very basic job we can here define properties like combiner, partitioner, input key, input value, output key, output value and many more.
    6. Oops! I haven't defined the input key/value and output key/value in job. Spring job definition is strong enough that it determine these key form the syntax of mapper and reducers. But as mentioned above there is a provision that we can explicitly set these values.