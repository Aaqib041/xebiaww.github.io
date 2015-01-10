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

<p><span style="font-family: Arial, sans-serif"><i>Main purpose of <b>Apache sqoop</b> is to <b>import and export data from RDBMS</b>(for example: MySql,Oracle etc) to Hadoop(HDFS/Hbase etc). </i></span></p>
<p><span style="font-family: Arial, sans-serif">Imagine, if you have a system where you are producing huge volume of data, and you want to extract some part of the data and do some analysis such . You wish to view the results of such analysis quite often. You can't use a Hadoop system to have quick views of your data. So a possible work flow for such a problem can be :-</span></p>
<p><span style="font-family: Arial, sans-serif;line-height: 1.5em">                 capture data into HDFS - &gt; Analyze data with Map Reduce- &gt;HDFS to RDBMS</span></p>
<p><span style="font-family: Arial, sans-serif">The benefit of using this kind of work flow is based on simple fact that Hadoop systems are not for quick reads whereas an RDBMS system can effectively resolve the problem of quick reads. We are using the MapReduce paradigm to turn our raw data to substantial information(may be usually at the end of day using some batch process) and finally pushing it into an RDBMS system for display purpose.</span></p>
<p><span style="font-family: Arial, sans-serif;line-height: 1.5em">Sqoop can play a vital role here, if we are facing a problem similar to above mention work flow.</span></p>
<!--more-->

<p><span style="font-family: Arial, sans-serif"><b>Typical Workflows:-</b></span>
<ol>
    <li><span style="font-family: Arial, sans-serif">RDBMS to HDFS - Analyze data with Map Reduce- HDFS to RDBMS</span></li>
    <li><span style="font-family: Arial, sans-serif">RDBMS to Hive/Habase- Analyze data with HQL-HIve To RDMS</span></li>
    <li><span style="font-family: Arial, sans-serif">NoSql to HDFS/Hive/Hbase- Analyze with HQL/MR - HDFS/Hive/Hbase</span></li>
</ol>
<span style="font-family: Arial, sans-serif"><b>Example Data PipeLine:-</b></span></p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/03/image1.png"><img class="alignnone  wp-image-18187" alt="image1" src="http://xebee.xebia.in/wp-content/uploads/2014/03/image1-300x190.png" width="500" height="300" /></a></p>
<p><span style="font-family: Arial, sans-serif">In the above shown example, we are collecting data from two different resources, i.e Log of Webserver </span><span style="font-family: Arial, sans-serif;line-height: 1.5em">and User Profiles from MySql DB. These two need to to be inserted into a form where our Map Reduce can run(i.e Hive in our case) . So for pushing data from MySql DB to Hive we can use Sqoop.</span></p>
<p><span style="font-family: Arial, sans-serif"><span style="font-size: large">W<b>orking of Sqoop:-</b></span></span></p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/03/image2.png"><img class="alignnone  wp-image-18188" alt="image2" src="http://xebee.xebia.in/wp-content/uploads/2014/03/image2-300x168.png" width="600" height="600" /></a></p>
<p><span style="font-family: Arial, sans-serif;line-height: 1.5em">  1 .When we try to use any sqoop tools.Sqoop will communicate with the database store , it will fetch meta-data information from RDBMS. Sqoop will use this meta data     for generating the java class.</span>
<ol start="2">
    <li><span style="font-family: Arial, sans-serif">Sqoop gets the metadata from DB store.</span></li>
    <li><span style="font-family: Arial, sans-serif">Sqoop will internally create a java class using JDBC API. Sqoop will compile the java class using JDK and create a .class file.A .jar file will be created from . class file.</span></li>
    <li><span style="font-family: Arial, sans-serif">After creating the jar files, sqoop will try to communicate with DB store again, and will try to find out the split column. Based on te split column Sqoop will fetch the data from DB</span></li>
    <li><span style="font-family: Arial, sans-serif">Finallly, Sqoop places the retrieved data into HDFS. </span></li>
</ol>
<span style="font-family: Arial, sans-serif">Once we understand the above internal process of Sqoop, impport and export process is self explanatory:-</span></p>
<p><span style="font-family: Arial, sans-serif"><b>Import Process of SQOOP:-</b></span></p>
<p><span style="font-family: Arial, sans-serif"><span style="color: #000000">This diagram is from the Apache documentation...</span></span></p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/03/image3.png"><img class="alignnone  wp-image-18189" alt="image3" src="http://xebee.xebia.in/wp-content/uploads/2014/03/image3-300x285.png" width="500" height="400" /></a></p>
<p><span style="font-family: Arial, sans-serif"><b>Export process of SQOOP:-</b></span></p>
<p><span style="font-family: Arial, sans-serif"><span style="color: #000000"><span>This diagram is from the Apache documentation...</span></span><b> </b></span></p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/03/image4.png"><img class="alignnone  wp-image-18190" alt="image4" src="http://xebee.xebia.in/wp-content/uploads/2014/03/image4-300x292.png" width="500" height="300" /></a></p>
<p><span style="font-family: Arial, sans-serif"><span><b>Databases supported by Sqoop: -</b></span></span></p>
<p><span style="font-family: Arial, sans-serif"><span style="font-size: small">Sqoop works well with lots of vendors.</span></span>
<table style="width: 100%" border="1" cellspacing="0" cellpadding="4"><col width="64*" /> <col width="64*" /> <col width="64*" /> <col width="64*" />
<tbody>
<tr valign="TOP">
<th width="25%">
<p align="LEFT"><span style="font-family: Arial, sans-serif"><span style="text-decoration: underline">Database</span></span></p>
</th>
<th width="25%">
<p align="LEFT"><span style="font-family: Arial, sans-serif"><span style="text-decoration: underline">version</span></span></p>
</th>
<th width="25%">
<p align="LEFT"><code><span style="color: #111111"><span style="font-family: Arial, sans-serif"><span style="text-decoration: underline">--direct </span></span></span></code><span style="color: #111111"><span style="font-family: Arial, sans-serif"><span style="text-decoration: underline">support?</span></span></span></p>
</th>
<th width="25%">
<p align="LEFT"><span style="color: #111111"><span style="font-family: Arial, sans-serif"><span style="text-decoration: underline">connect string matches</span></span></span></p>
</th>
</tr>
<tr valign="TOP">
<td width="25%">
<p align="LEFT"><span style="font-family: Arial, sans-serif">HSQLDB</span></p>
</td>
<td width="25%">
<p align="LEFT"><span style="font-family: Arial, sans-serif">1.8.0+</span></p>
</td>
<td width="25%">
<p align="LEFT"><span style="color: #111111"><span style="font-family: Arial, sans-serif">No</span></span></p>
</td>
<td width="25%">
<p align="LEFT"><code><span style="color: #111111"><span style="font-family: Arial, sans-serif">jdbc:hsqldb:<em>//</span></span></code></p>
</td>
</tr>
<tr valign="TOP">
<td width="25%">
<p align="LEFT"><span style="font-family: Arial, sans-serif">MySQL</span></p>
</td>
<td width="25%">
<p align="LEFT"><span style="font-family: Arial, sans-serif">5</span></p>
</td>
<td width="25%">
<p align="LEFT"><span style="color: #111111"><span style="font-family: Arial, sans-serif">Yes</span></span></p>
</td>
<td width="25%">
<p align="LEFT"><code><span style="color: #111111"><span style="font-family: Arial, sans-serif">jdbc:mysql://</span></span></code></p>
</td>
</tr>
<tr valign="TOP">
<td width="25%">
<p align="LEFT"><span style="font-family: Arial, sans-serif">Oracle</span></p>
</td>
<td width="25%">
<p align="LEFT"><span style="font-family: Arial, sans-serif">10.2.0+</span></p>
</td>
<td width="25%">
<p align="LEFT"><span style="color: #111111"><span style="font-family: Arial, sans-serif">No</span></span></p>
</td>
<td width="25%">
<p align="LEFT"><code><span style="color: #111111"><span style="font-family: Arial, sans-serif">jdbc:oracle:</em>//</span></span></code></p>
</td>
</tr>
<tr valign="TOP">
<td width="25%">
<p align="LEFT"><span style="font-family: Arial, sans-serif">PostgreSQL</span></p>
</td>
<td width="25%">
<p align="LEFT"><span style="font-family: Arial, sans-serif">8.3</span></p>
</td>
<td width="25%"></p>