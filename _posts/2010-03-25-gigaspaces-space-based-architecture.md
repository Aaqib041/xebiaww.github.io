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

Recently one of our clients asked to make a distributed application that can be scaled and managed easily and cost effectively. So we evaluated [JavaSpaces][1], a Space Based Architecture concept, and one of its commercial implementation [GigaSpaces ][2]for the purpose. In this post I will try to describe in brief what the Space based Architecture approach is all about.

Space Based Architecture(SBA) is a paradigm of co-located tiers. It combines the elements of SOA, event driven architecture and grid computing. In SBA we would create tiers but would co-locate them with the messaging and data in-memory, at one single box. Now there are no network hop-overs as the data required is available in memory rather on an ESB or Database, hence there is minimum latency and thus maximum throughput. The whole box will now be called a **_**PROCESSING UNIT**_**, that will have an in memory bus known as the _**SPACE**. _For fail-over strategy we would eventually create backups of this Processing Unit, with their own space having synchronous replication with primary space. 

In order to create a highly scalable application we would like to employ the concept of _**PARTITIONING**_ of space rather clustering. In partitioning we would have one single view of a space that is eventually distributed across different processing units(_ it utilizes the shared dynamic memory of networked JVMs)_. When ever some object is written to the space it will determine its location first and then will be routed to that Space partition. This eventually calls for the concept of _**DATA-AFFINITY**_. It says that you should keep all the data that is attached to your primary object should be available in one single partitioned space. This would enable you not to query over other spaces and thus reduce the network calls.

![SBA][3]Now for scalable,high throughput applications we create _**SLA**_s using partitioning that would instantiate new partitions or destroy old ones depending on the current load i.e. Scale-on-Demand. These SLAs are deployed along-with the application in an SLA-Driven container.

Applications also require persistent storage, since applications in SBA have in memory data grid(the _SPACE_) so they must persist data to DB. But DB writes are quite slow, so we would not like to interact with DB directly, here we have another new entity called _**MIRROR SERVICE**_. Mirror service is usually a single space connected to an external storage(Files,DB etc). Your application space would write the data asynchronously to the Mirror, which would eventually persist the data. In SBA you do not read much data from persistent storage as most of the time data is available in the in memory grid, but at times there can be some data that is required to be read from the DB. In such cases a read-at-start-up strategy is employed where the space will load the entire data from the DB at start-up and will keep it in its in-memory grid.

Here are some of the implications of using SBA:

  * Scalability becomes predictable and linear
  * Offers quite low latency thus a high throughput
  * Lower Total Cost of Ownership
  * Developers write code as if it was for a single server so eliminate efforts required to integrate tiers
  * Simplistic API(read/write/take/update) with POJO based model
  * Developers need to take consideration of Partitioning their data.
  * Data in th DB that is being modified by some other application can not detected by the space application and thus can not be loaded.
  * SLAs are often written in XML files with the application code(system administrators may not like that)

   [1]: http://java.sun.com/developer/technicalArticles/tools/JavaSpaces/
   [2]: http://www.gigaspaces.com/
   [3]: http://xebee.xebia.in/wp-content/uploads/2010/03/SBA1.png (SBA)

## Comments

**[vidya](#5683 "2011-07-07 16:26:38"):** Hi Rahul, I found this very useful to understand SBA. Do you have comparison between Javaspaces and Gigaspaces? Which one is better for scalability? What would it take if application written in Javaspaces is to be ported to Gigaspaces? Thanks, -Vidya

**[Rahul Sharma](#5685 "2011-07-07 22:16:32"):** HI Vidya, I do not have direct experience of Javaspaces but I think Gigaspaces uses the same thing underneath. Also there are couple of things that you should know when you scale with Gigaspaces, we found out this after using it couple of months : 1\. You can not scale beyond a point. For us the point was upto 6 servers after that the performance started diminishing. 2\. There is complete shift in how you will design your application. At the back it is a key-value store like a nosql db and would require the same concepts in order to scale better. regards Rahul

