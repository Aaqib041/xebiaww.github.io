---
layout: post
header-img: img/default-blog-pic.jpg
author: hsaini
description: 
post_id: 4365
created: 2010/08/17 15:35:13
created_gmt: 2010/08/17 10:35:13
comment_status: open
---

# Performace Testing made easy using Badboy and JMeter

<p>When it comes to performance testing and especially using open source tools such as <a href="http://jakarta.apache.org/jmeter/" target="_blank">JMeter</a>, it is often observed that testers(especially from black box background) have a hard time. For simple scenarios such as analyzing performance of different pages in a website wherein the pages can be directly accessed through URL directly, writing JMeter scripts is fairly simple. But even for these simple test cases testers sometimes avoid writing the JMeter scripts themselves and often pair up with developers. Coming from a background of record &amp; playback tools, testers find it inconvenient to write JMeter scripts. Fortunately, <a href="http://www.badboysoftware.biz" target="_blank">Badboy</a> solves most of the problems which are stated above. Testers can simply record the flow in Badboy and export the script as JMeter script which can be easily run in JMeter.Here in this blog, using a simple example I'll explain how we can record the flow in Badboy, export and run in JMeter.<!--more--></p>
<p>Let's take a scenario of analyzing performance for logging in gmail, and logging out. If we try to write this in JMeter, it could be bit complicated as we have to add some parameters, change the request type (to POST) and other things such as we cannot access the pages directly by typing the URL. But in Badboy it is fairly easy, simply press record button, navigate to different pages as a normal user and finally export the test as JMeter. So lets look at this step by step:-</p>
<p><strong>1.</strong> Download the latest version of Badboy from <a href="http://www.badboy.com.au/download" target="_blank">here</a> and install.</p>
<p><strong>2.</strong> Record the flow in Badboy, by clicking on the record button, typing the url and navigating through different pages.
<a href="http://xebee.xebia.in/wp-content/uploads/2010/08/1.jpg"><img class="alignleft size-large wp-image-4481" title="Record Flow in BadBoy" src="http://xebee.xebia.in/wp-content/uploads/2010/08/1-1024x424.jpg" alt="" width="679" height="297" /></a></p>
<p><strong>3.</strong> Export to JMeter and save the .jmx script<a href="http://xebee.xebia.in/wp-content/uploads/2010/08/2.jpg"><img class="alignleft size-large wp-image-4501" title="Export to JMeter" src="http://xebee.xebia.in/wp-content/uploads/2010/08/2-1024x640.jpg" alt="" width="677" height="248" /></a></p>
<p><strong>4. </strong>Open the Exported Script in JMeter. Sometimes the latest version (jmeter 2.4 ) fails to import the exported file, so import the file in an older version of JMeter (jmeter 2.3)</p>
<p><strong>5.</strong> Set the Thread Properties in Thread Group. Click on Thread Group in Left Panel and set the number of threads, ramp up period &amp; loop count.<a href="http://xebee.xebia.in/wp-content/uploads/2010/08/3.jpg"><img class="alignleft size-large wp-image-4503" title="Thread Group" src="http://xebee.xebia.in/wp-content/uploads/2010/08/3-1024x533.jpg" alt="" width="684" height="373" /></a></p>
<p><strong>6.</strong> Add Graphs by right clicking on Thread Group &amp; adding a listener.<a href="http://xebee.xebia.in/wp-content/uploads/2010/08/4.jpg"><img class="alignleft size-large wp-image-4506" title="Graph Result" src="http://xebee.xebia.in/wp-content/uploads/2010/08/4-1024x537.jpg" alt="" width="681" height="376" /></a></p>
<p><strong>7. </strong>Run the JMeter Test.</p>
<p><strong>8.</strong> Click the Graph Results to view the graphs and analyze the performance of the application under test.
<a href="http://xebee.xebia.in/wp-content/uploads/2010/08/5.jpg"><img class="alignleft size-large wp-image-4512" title="Graph Results" src="http://xebee.xebia.in/wp-content/uploads/2010/08/5-1024x639.jpg" alt="" width="679" height="447" /></a></p>
<p>Unfortunately Jscripts &amp; data sources created in Badboy are not supported when exported to JMeter. But again these features are for advanced users who can create the complicated scripts in JMeter itself. Also ,we can do a performance testing in Badboy but Badboy does not have very good performance analysis capability which is there in JMeter. Different types of listeners then can be added in JMeter (such as Graphs, Aggregate Report, etc) to get a good performance report after exporting a script from Badboy.</p>

## Comments

**[Nitin More](#4748 "2011-01-06 14:59:51"):** Thanks....Harsh.!!! You are simply removed the HTTPS problem...hahhahaha..:) & showing the simple way to record the script ....with out frustrating abt the HTTPS.!!!! Keep it up!!!! Thanks, Nitin

**[http://chs75.chs.harvard.edu/chswiki/index.php?title=User:Glossyritual75](#5948 "2011-09-28 15:59:29"):** **Related information...** Here's some more information about the issue...

**[pret implant dinte](#8771 "2012-05-13 12:32:12"):** **...Check this out...** [...]The overall look of your website is excellent, let neatly as the content material![...]...

**[Rts](#9154 "2012-07-12 13:04:05"):** Nice Article dude.. Thnaks

