---
layout: post
header-img: img/default-blog-pic.jpg
author: jagmeet2003
description: 
post_id: 11582
created: 2012/02/15 15:48:01
created_gmt: 2012/02/15 10:48:01
comment_status: open
---

# Infinispan – The Path Covered

Last month I attended JUDCon – 2012 (JBoss Users and Developers Conference). One thing that attracted me most were sessions about Infinispan especially by Manik Surtani. I have tried to compile the information gained in a concise form.

**What is Infinispan?**

[Infinispan][1] is an opensource (LGPL) data grid platform written in Java and Scala. It is a distributed key/value store which is transactional, in-memory (optionally persisted to disk) and feature rich.

It supports 2 modes: Embedded and Client – Server

Let's feature some of the key points for Infinispan: 

  * API – It is _java.util.Map _like key/value store with an additional asynchronous API which can be utilized for asynchronous operations. It also features a CDI (Contexts and Dependency Injection) API. There is also an upcoming JPA layer using [Hibernate OGM][2].
    
        <li>Transactions  –     Infinispan supports JTA and XA compliant transactions. It supports   different levels of XA integrations namely; simple synchronization,     full XA and full XA with recovery. It also supports optimistic and      pessimistic transactions.</li>
    <li>Persistent  Storage –  Inifinispan is not just an in-memory data store  with    support for write  through and write behind. It also supports   Passivation (spillover to  disk) and warm starts (preloading from   disk). It is shipped with  implementations for file system based    BTree (BerkelyDB, JDB), JDBC,  Cloud Storage (via JClouds)</li>
    <li>Replication –   It uses two clustered modes, namely: Replication (Full replication)     and Distribution (Partial replication).
    

    * Full replication – It attracts fast reads at the cost of expensive writes, however it has scalability limitations.
    * Distribution – It is linear and far more scalable with access to very large heaps. Remote Gets can be expensive, hence need for L1 cache (near cache in general) with attributes such as limited lifespan for entries and smart invalidating algorithms for invalidating entries in cache.
  * Code Execution – Infinispan also features concept of moving logic to data instead of vice - versa. This is implemented using java concurrency enhancements: executors and callable interfaces and APIs. It also has support for Map/Reduce for compressing calculation results.
An open, language independent wired protocol, Hot Rod is also developed for client – server communication with attributes like smart routing and build-in failover and load balancing capabilities.

Apart from Hot Rod, memcached and REST protocols can be used for communicating for client – server.

**Why use Infinispan?**

Despite of above features that Infinispan posses, do we really need it? What could be use cases for using cache or more specifically distributed caches?

There could be many use cases such as: 

  * To cache data that is expensive to retrieve or calculate. For example, database calls can be expensive. There could be some data which is an outcome of complex time taking calculations used multiple times.
  * The need for fast, low – low latency data access for performance and time sensitive applications. Distributes caches are mostly used in industries like Financial industry, Telcos or Highly scalable e-commerce industries.
  * Data grids as clustering toolkits – Data grids can also act as toolkits for introducing high availability and failover to opensource, commercial or in-house frameworks or reusable architectures. It may also aide in delegating all state management to data grid which in turns helps frameworks to be stateless and hence elastic which could be very important for cloud.
  * Data grids used as cloud storage (NoSQL key/value store) – Traditional databases are hard to deal with short lived clouds. As all cloud components are expected to be elastic and highly available, Infinispan fits in appropriately.
To summarize, Infinispan is an opensource, in-memory, highly available and elastic data grid platform which can be used as: 
  * A fast, powerful local cache to boost performance.
  * A fast, available, distributed and elastic data grid.
  * A combination of Infinispan caches and data grids can be used to create high performance and scalable near caches.
Watch out this space for some hands-on Infinispan.

For more information, visit [http://www.infinispan.org][3]

   [1]: http://www.infinispan.org
   [2]: http://www.hibernate.org/subprojects/ogm.html
   [3]: http://www.infinispan.org/