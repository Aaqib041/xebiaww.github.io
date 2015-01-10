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

<p>Spring aims to make enterprise aplication development as simple as possible. Spring makes life of a developer very easy by providing JDBC, JMS, Web Services, Security and many more frameworks under one umbrella. Spring reduces the complexity and provide power to developer to do whole lot of things from spring context. Now Spring has extended its scope to add Big Data frameworks like Hadoop, HDFS, PIG, HIVE, No Sql Databases.
<!--more-->
Spring announced its first milestone release 1.0.0.M1 for Spring for Apache Hadoop. This Spring initiative reflects the growing importance of Big Data within enterprise applications. Spring has simplified the programming model for complex hadoop jobs. Spring shows the world that Hadoop can now be integrated with Enterprise Applications very easily. Undoubtedly It will help accelerating Hadoop's growth. Spring supports almost all the features of Hadoop and provides very easy and elegant ways to configure Hadoop job. Some basic features supported by Spring are:
<ol></ol>
<ol>
<li>Supports Configuration, creation and execution of MapReduce Job.</li>
<li>HDFS data access support through JVM scripting languages (Groovy, JRuby, Jython, Rhino, etc.)</li>
<li>Support for use with Spring Integration that provides easy access to a wide range of existing systems. Like you can integrate Hadoop Job with Spring MVC.</li>
<li>Powerful Hadoop configuration options and a template mechanism for client connections to Hadoop.</li>
<li>Declarative and programmatic support for Hadoop Tools, including FsShell and DistCp.</li>
</ol>
Let us create simple and very common Hadoop's WordCount problem with Spring. I am assuming that readers are familiar with existing Hadoop jobs and have already installed Hadoop on their Systems.
<ol>
<li>Create Simple maven project. add dependencies for Hadoop and spring in the pom.xml file of your project.</li>
[sourcecode lang="text"]
      &lt;dependency&gt;
        &lt;groupId&gt;org.springframework.data&lt;/groupId&gt;
        &lt;artifactId&gt;spring-data-hadoop&lt;/artifactId&gt;
        &lt;version&gt;1.0.0.M2&lt;/version&gt;
      &lt;/dependency&gt;
      &lt;dependency&gt;
        &lt;groupId&gt;org.apache.hadoop&lt;/groupId&gt;
        &lt;artifactId&gt;hadoop-core&lt;/artifactId&gt;
        &lt;version&gt;1.0.3&lt;/version&gt;
      &lt;/dependency&gt;
[/sourcecode]
Spring Hadoop dependency is not available in maven repository so add below repository link to download this dependency.
[sourcecode lang="text"]
&lt;repositories&gt;
 &lt;repository&gt;
 &lt;id&gt;spring-milestone&lt;/id&gt;
 &lt;name&gt;Spring Maven SNAPSHOT Repository&lt;/name&gt;
 &lt;url&gt;http://repo.springframework.org/milestone&lt;/url&gt;
 &lt;/repository&gt;
 &lt;/repositories&gt;
[/sourcecode]
<li>Map and Reduce program will remain same. Following are the Mappers and Reducers for WordCount Problem.</li>
<strong>Map.java</strong>
[sourcecode lang="java"]
      package com.xebia;
      import java.io.IOException;
      import org.apache.commons.lang.StringUtils;
      import org.apache.hadoop.io.IntWritable;
      import org.apache.hadoop.io.LongWritable;
      import org.apache.hadoop.io.Text;
      import org.apache.hadoop.mapreduce.Mapper;
        public class Map extends Mapper&lt;LongWritable, Text, Text, IntWritable&gt; {
        private static final IntWritable INITIALCOUNT = new IntWritable(1);
        private static final Text KEY = new Text();
        protected void map(LongWritable key, Text value, Context context)
                                          throws IOException, InterruptedException {
          for (String token : StringUtils.split(value.toString())) {
            KEY.set(token);
            context.write(KEY, INITIALCOUNT);
          }
        }
      }
     [/sourcecode]
<strong>Reduce.java</strong>
[sourcecode lang="java"]
       package com.xebia;
       import java.io.IOException;
       import org.apache.hadoop.io.IntWritable;
       import org.apache.hadoop.io.Text;
       import org.apache.hadoop.mapreduce.Reducer;
          public class Reduce extends Reducer&lt;Text, IntWritable, Text, IntWritable&gt;{
          public void reduce(Text key, Iterable&lt;IntWritable&gt; values, Context context)
                                             throws IOException, InterruptedException {
            int totalcount = 0;
            for (IntWritable count : values) {
               totalcount = totalcount + count.get();
            }
            context.write(key, new IntWritable(totalcount));
          }
       }
[/sourcecode]
    map and reduce functions are very much obvious. Map split the line and set each word as key and "1" as value. Reducer sort the keys and reduce function calculates the sum for each key and prints as reducer output.
<li>After writing MapReduce the remaining task is to configure Hadoop Job. This is the point where Spring help us in configuring the job. To configure the job we need to write spring context.xml file.
[sourcecode lang="xml"]
      &lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
        &lt;beans xmlns=&quot;http://www.springframework.org/schema/beans&quot;                   <br />
                    xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot;                 <br />
                    xmlns:context=&quot;http://www.springframework.org/schema/context&quot;                    <br />
                    xmlns:hadoop=&quot;http://www.springframework.org/schema/hadoop&quot;                    <br />
                    xmlns:p=&quot;http://www.springframework.org/schema/p&quot;                    <br />
                   xsi:schemaLocation=&quot;http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd                                 <br />
                                 http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd                    <br />
                                 http://www.springframework.org/schema/hadoop http://www.springframework.org/schema/hadoop/spring-hadoop.xsd&quot;&gt;
             &lt;hadoop:configuration&gt; 
                 fs.default.name=hdfs://localhost:54310 
             &lt;/hadoop:configuration&gt; 
             &lt;hadoop:job id=&quot;wordcount&quot; input-path=&quot;/input&quot; output-path=&quot;/output&quot;                                <br />
                  mapper=&quot;com.xebia.Map&quot; reducer=&quot;com.xebia.Reduce&quot;/&gt;
         &lt;/beans&gt; 
      [/sourcecode] 
<ol>
<li> Line 5 declares Spring for Apache Hadoop namespace prefix. Which is hadoop and will be used in the entire file to refer hadoop specific tags.</li>
<li>In order to use Hadoop we need to create Configuration object which helps in configuring Hadoop.It holds information about the job tracker, the input, output format and the various other parameters of the map reduce job.</li>
<li>The one liner declaration &lt;hadoop:configuration /&gt; defines a Configuration bean (to be precise a factory bean of type ConfigurationFactoryBean) named, by default, hadoopConfiguration. The default name is used, by conventions, by the other elements that require a configuration - this leads to simple and very concise configurations as the main components can automatically wire themselves up without requiring any specific configuration.</li>
<li>Here at line 11 I configured the "fs.default.name" property explicitly because I am using different port for hdfs file system then the default port (9001).</li>
<li>Line 13 defines a Hadoop job with properties like input-path,output-path, mapper and reducer. This is very basic job we can here define properties like combiner, partitioner, input key, input value, output key, output value and many more.</li>
<li>Oops! I haven't defined the input key/value and output key/value in job. Spring job definition is strong enough that it determine these key form the syntax of mapper and reducers. But as mentioned above there is a provision that we can explicitly set these values.</li>
</ol></p>