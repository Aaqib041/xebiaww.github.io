---
layout: post
header-img: img/default-blog-pic.jpg
author: sgupta
description: 
post_id: 11841
created: 2012/02/16 16:21:28
created_gmt: 2012/02/16 11:21:28
comment_status: open
---

# Tune your postgresql server with index on Text column

I am working on postgresql from last couple of weeks. In this blog, I will share my experience with tuning of postgresql server. After working on streaming replication, I got a chance to find the optimal write performance of postgresql with index and without index.

As application is financial so we need to stick with data consistency and thats why postgresql is running in "synchronized" replication mode. In the application, we are required to insert 40 million records, which would come from multiple sources in the form of text or csv files, in postgresql database with index on or off.

Without index the performance was very good (4000 to 4200 writes per second) but when I created index on text column then the performance reduced to 1/8th (500 to 550 writes per second). The reason is as the data grows the size of index also grows which takes huge amount of RAM, which results in an increased number of hits to the database for read and write operations.

To increase the performance I tweaked configuration of postgresql server which is defined in postgresql.conf. After changing the configuration the performance almost reached to previous level (3800 to 4000 writes per second). 

** **

**Test Environment**

Master and Standby postgresql servers were running on same system in synchronized streaming replication mode. System had 4 GB RAM and 8 core CPU with Centos 6 operating system. I inserted 40 million records in the database and each row size was approximately 2.5 KB.

** **

**Configuration parameters**

Following are the parameters which I changed in configuration file:

  1. **SHARED_BUFFERS** The first parameter that I changed in the configuration file was shared_buffers. Shared buffers defines a block of memory that PostgreSQL will use to hold requests that are awaiting attention from the kernel buffer and CPU. The default value is 32 MB which is very low for write-heavy applications. This buffer is allocated while initializing the database. The value can not be assigned greater than  the value defined in shmmax file . Shmmax is in /proc/sys/kernel directory. If the value of shared_buffers is greater than value defined in shmmax and you try to start the postgresql server then server will throw an exception on the screen. So first update the value in shmmax and then start the server again. Value is in bytes in shmmax file. It should be sufficient enough to handle load on database server. Otherwise PostgreSQL will start pushing data to file and it will hurt the performance overall. This value should be set based on the: Size of data in database server + available RAM. Recommended value is 25 percent of system RAM.
  2. **MAINTENANCE_WORK_MEM** This is the working memory for larger operations than just regular sorting e.g. VACUUM, CREATE INDEX, ALTER TABLE ADD FOREIGN KEY. The approximate value of this parameter should be 50 MB of maintainance_work_mem per GB of server RAM.
  3. **WAL_BUFFER** The default value of this parameter is 64 KB. This setting should be large enough to hold the amount of WAL data for a transaction. Otherwise data is written to disk every time which reduce the performance of server.  For write-heavy application the optimal value is atleast 1 MB or more. If you set the value as -1 then server automatically calculate and set the value.
  4. **CHECKPOINT_SEGMENT** PostgreSQL creates WAL segment in database files for each new transactions that are 16MB in size. By default a checkpoint segment value is 3. This value should be atleast 10 if you are not running very small application. For more write-heavy systems, values are used  between 32 (checkpoint every 512MB) to 256 (every 4GB).  Very large value will increase the disk size as well as time to recover the database. Normally the large settings (>64/1GB) are only used for bulk loading.
  5. **CHECKPOINT_COMPLETION_TARGET** If you have increased the value of checkpoint_segment atleast 10 then this value should be increased from default value 0.5 to 0.9. This provides the checkpoint spreading without overhead on I/O.
  6. **EFFECTIVE_CACHE_SIZE** This parameter generally used for query tuning. This cache is used by planner before executing the query to create different plans. Setting effective_cache_size to 1/2 of total memory would be a normal conservative setting, and 3/4 of memory is a more aggressive but still reasonable amount.

**Conclusion**

Following are the values that I used in postgresql.conf

  1. Shared_buffer: 500MB
  2. Checkpoint_segment: 64
  3. Checkpoint_completion_target: 0.9
  4. Maintenance_work_mem: 50 MB
  5. Wal_buffer: 16MB
  6. Effective_cache_size: 500MB

These all parameters that I tweaked in configuration file to get the best performance. This was all about my experience. If you also had similar issues/challenges or if you want to share your finding on the same, then please share it as a comment.

## Comments

**[sashbeer](#8429 "2012-04-12 09:13:16"):** Hi i m using dell R710 with 64 GB Ram, 8Core proces, and I have install postgres 8.2 on the server please help me to tuniing postgresql for better performance .

**[Saurabh Gupta](#8431 "2012-04-12 13:50:29"):** 1.Shared_buffer: 25% of your RAM i.e. 16GB 2.Checkpoint_segment: If your application is write heavy then use 256 3.Checkpoint_completion_target: 0.9 4.Maintenance_work_mem: 50 MB per GB of your RAM i.e. 3-3.2 GB 5.Wal_buffer: -1 System will identify the correct value according to your RAM 6.Effective_cache_size: 32GB I think these settings can improve the performance.

