---
layout: post
header-img: img/default-blog-pic.jpg
author: rsharma
description: 
post_id: 3333
created: 2010/03/25 12:42:53
created_gmt: 2010/03/25 07:42:53
comment_status: open
---

# GigaSpaces: Space Based Architecture 

<!--        @page { margin: 2cm }       P { margin-bottom: 0.21cm } -->

<p style="margin-bottom: 0cm;" align="LEFT">Recently one of our clients asked to make a distributed application that can be scaled and managed easily and cost effectively. So we evaluated  <a href="http://java.sun.com/developer/technicalArticles/tools/JavaSpaces/">JavaSpaces</a>, a Space Based Architecture concept,  and one of its commercial implementation <a href="http://www.gigaspaces.com/">GigaSpaces </a>for the purpose. In this post I will try to describe in brief what the Space based Architecture approach is all about.</p>

<p style="margin-bottom: 0cm;" align="LEFT">Space Based Architecture(SBA) is a paradigm of co-located tiers. It combines the elements of SOA, event  driven architecture and grid computing. In SBA  we  would create tiers but would co-locate them with the messaging and data in-memory, at one single box. Now there are no network hop-overs as the data required is available in memory rather on an ESB or Database, hence there is minimum latency and thus maximum throughput. The whole box will now be called a <strong><em><span style="font-weight: normal;"><strong>PROCESSING UNIT</strong></span></em></strong><span style="font-weight: normal;">,</span> that will have an in memory bus known as the <em><strong>SPACE</strong>. </em><span style="font-style: normal;">For fail-over strategy we would eventually create  backups of this Processing Unit, with their own space having synchronous replication with primary space. </span></p>

<p style="margin-bottom: 0cm; font-style: normal;" align="LEFT"><span style="font-style: normal;">In order to create a highly scalable application we would like to employ the concept of </span><em><strong>PARTITIONING</strong></em><span style="font-style: normal;"> of space rather clustering. In partitioning we would have one single view of a space that is eventually distributed across different processing units(</span><em> it utilizes the shared dynamic memory of networked JVMs)</em><span style="font-style: normal;">. When ever some object is written to the space it will determine its location first and then will be routed to that Space partition. This eventually calls for the concept of </span><em><strong>DATA-AFFINITY</strong></em><span style="font-style: normal;">. It says that you should keep all the data that is attached to your primary object should be available in one single partitioned space. This would enable you not to query over other spaces and thus reduce the network calls.</span></p>

<p style="margin-bottom: 0cm;" align="LEFT"><img class="size-full wp-image-3335 alignright" title="SBA" src="http://xebee.xebia.in/wp-content/uploads/2010/03/SBA1.png" alt="SBA" width="450" height="318" /><span style="font-style: normal;">Now for scalable,high throughput applications we create </span><em><strong>SLA</strong></em><span style="font-style: normal;">s using partitioning that would instantiate new partitions or destroy old ones depending on the current load i.e. Scale-on-Demand. These SLAs are deployed along-with the application in an SLA-Driven container.</span></p>

<!--        @page { margin: 2cm }       P { margin-bottom: 0.21cm } -->

<p style="margin-bottom: 0cm;" align="LEFT"><span style="font-style: normal;">Applications also require persistent storage, since applications in SBA have in memory data grid(the </span><em>SPACE</em><span style="font-style: normal;">)  so they must persist data to DB. But DB writes are quite slow, so we would not like to interact with DB directly, here we have another new entity called </span><em><strong>MIRROR SERVICE</strong></em><span style="font-style: normal;">. Mirror service is usually a single space connected to an external storage(Files,DB etc). Your application space would write the data asynchronously to the Mirror, which would eventually persist the data. In SBA you do not read much data from persistent storage as most of the time data is available in the in memory grid, but at times there can be some data that is required to be read from the DB. In such cases a read-at-start-up strategy is employed where the space will load the entire data from the DB at start-up and will keep it in its in-memory grid.</span></p>

<p style="margin-bottom: 0cm;" align="LEFT">Here are  some of the implications of using SBA:</p>

<ul>
    <li>Scalability becomes predictable and linear</li>
    <li>Offers quite low latency thus a high throughput</li>
    <li>Lower Total Cost of Ownership</li>
    <li>Developers write code as if it was for a single server so eliminate efforts required to integrate tiers</li>
    <li>Simplistic API(read/write/take/update) with POJO based model</li>
    <li>Developers need to take consideration of Partitioning their data.</li>
    <li>Data in th DB that is being modified by some other application can not detected by the space application and thus can not be loaded.</li>
    <li>SLAs are often written in XML files with the application code(system administrators may not like that)</li>
</ul>

<p style="margin-bottom: 0cm;" align="LEFT"></p>

<p style="margin-bottom: 0cm;" align="LEFT"><span style="font-style: normal;">
</span></p>

<p style="margin-bottom: 0cm;" align="LEFT"></p>

<p style="margin-bottom: 0cm;" align="LEFT"><span style="font-style: normal;">
</span></p>

## Comments

**[vidya](#5683 "2011-07-07 16:26:38"):** Hi Rahul, I found this very useful to understand SBA. Do you have comparison between Javaspaces and Gigaspaces? Which one is better for scalability? What would it take if application written in Javaspaces is to be ported to Gigaspaces? Thanks, -Vidya

**[Rahul Sharma](#5685 "2011-07-07 22:16:32"):** HI Vidya, I do not have direct experience of Javaspaces but I think Gigaspaces uses the same thing underneath. Also there are couple of things that you should know when you scale with Gigaspaces, we found out this after using it couple of months : 1\. You can not scale beyond a point. For us the point was upto 6 servers after that the performance started diminishing. 2\. There is complete shift in how you will design your application. At the back it is a key-value store like a nosql db and would require the same concepts in order to scale better. regards Rahul

