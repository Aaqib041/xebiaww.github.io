---
layout: post
header-img: img/default-blog-pic.jpg
author: Gagan
description: 
post_id: 17054
created: 2013/08/25 12:03:35
created_gmt: 2013/08/25 07:03:35
comment_status: open
---

# Build your own Recommendation Engine

Recommendation engine is a demand of every e-commerce and social website. A lot of work has already been done in this direction. In this blog we would try to understand the need of Recommendation Engine, concept behind computing recommendations and how to integrate developed recommendation engine with the existing application architecture. We will be using Hadoop based recommendation engine to cater big data volume as well.  Let's have an example of a typical e-commerce website where user can purchase items and technically application is using typical RDBMS to store user purchases and items. Now let's see how can we fit our recommendation engine to the existing system.

What is Recommendation?

Recommendation involves the prediction of what new items a user would like or dislike based on his purchase history. For example

A user, John , likes the following books 

  1. Computer Basic.
  2. Fundamentals of Computers & Programming in C.
  3. Internet & Java Programming.
Recommendation engine will predict which new books John, may like 
  1. Programming Language Pragmatics.
  2. Object Oriented Programming Concept & Implementations.
How Recommendation engine computed these results we will see in some time.

Amazon gave the concept of Item-to-Item collaborative-filtering. let's understand what it is? and How it works?

Collaborative filtering is a method of making predictions (filtering) about the interests of a user by collecting preferences or taste information from many users (collaborating).

Collaborative filtering works on the assumption that if a person A has the same opinion as a person B on an issue, A is more likely to have B's opinion on a different issue x than to have the opinion on x of a person chosen randomly.

**Item-to-Item Collaborative filtering.**

Below diagram depicts the user and their purchases made for certain items.

[caption id="attachment_17055" align="aligncenter" width="300"]![Item based filtering][1] Item based filtering[/caption]

In the above diagram user1 purchased grapes, watermelon and orange, user2 purchased grapes and watermelon and user3 purchases watermelon only. So if we need to recommend some item to user3, we can simply calculate that using the fact that the in our database users who purchased watermelon have also purchased grapes in common so simply we can recommend grapes to user3. We can visualize these calculations in the following diagram.

[caption id="attachment_17056" align="aligncenter" width="300"]![Similarity Matrix][2] Similarity Matrix[/caption]

**Similarity Matrix.** The above similarity matrix depicts that how many times two items purchased (in combination like how many times user purchased item1 and item3) together by some user. Like grapes (item1) and watermelon(item3) are purchased by two users (i.e. user1 and user2) so there is 2 value for item1 and item3 cell. Similarly Item1 has also been purchased by two users so there is 2 value in item1, item1 cell. This is symmetric matrix (rows and columns are same) this property helps a lot in doing calculations in hadoop job. So to maintain this property we keep calculations for same item (like how many times item1 purchased) as well which is shown along with the diagonal of the matrix. We can neglect these items in the result because there is no point in recommending the same item which user has already purchased.

**User purchase history.** This is very clear and showing 1 value only for the item which user has purchased.

**Recommendation Matrix.** Recommendation matrix is calculated simply by doing the dot product of above described two matrices and result is Item1. We should ignore item3 because user has already purchased this.

**Technical Solution.**

Many implementations for above described recommendation engine are already available. One of those implementations are Mahout based RecommenderJob, which provides both hadoop and non hadoop based implementation. This job simply accepts a input file in format (userId, itemId, rating) as we are using purchased history where user is not providing the product ratings so we can pass 1 here for all the records or just ignore. After processing Mahout based RecommenderJob provides the output in format (User id : [item1, item2, ..]).

**Application Architecture.**

  1. We can fit hadoop based recommendation engine to the existing typical e-commerce application. We need to perform following steps.
  2. Get data out of RDBMS where user purchase history is stored and keep it in HDFS from where hadoop based Mahout RecommenderJob will pick data and process. Sqoop is a tool which is available to help us here. Sqoop job can be written easily to get data out of RDBMS and put it in HDFS. Process Data using Mahout RecommenderJob.
  3. Put recommendation results back to the RDBMS. Again Sqoop can help us here.
[caption id="attachment_17057" align="aligncenter" width="250"]![Architecture][3] Architecture[/caption]

**Conclusion.** Amazon claims that this kind of recommendation system works well for them but some new websites which don't have big database like Amazon claims this is not so useful for them. This is static and batch job based recommendation system but today people need Real-time systems where recommendation can be manipulated on the fly based on user current behaviour on the website. We will discuss how Real time system works in the next blog so stay tuned.

**The objective of this blog is to just discuss how recommendation works and how easily can be fit to the existing application architecture. The Objective is not to provide code and working recommendation engine.

References : [Apache Mahout][4] [Apache Sqoop][5]

See Xebia's offering in Big Data [here][6].

   [1]: http://xebee.xebia.in/wp-content/uploads/2013/08/pic_blog1-300x276.jpg
   [2]: http://xebee.xebia.in/wp-content/uploads/2013/08/pic_blog2-300x160.jpg
   [3]: http://xebee.xebia.in/wp-content/uploads/2013/08/pic_blog3-250x300.jpg
   [4]: http://mahout.apache.org/ (Apache Mahout)
   [5]: http://sqoop.apache.org/ (Apache Sqoop)
   [6]: http://www.xebia.in/big-data-solutions.html (Big Data Solutions)

## Comments

**[Gagan Agrawal](#9438 "2013-09-03 10:00:28"):** Nice Post Gagan J. It does explain the recommendation process very well. However one thing I was interested to know is how will incremental updates be taken care of? After recommendation results have been pushed back to RDBMS and now users have further bought new items, will the whole process be repeated for new recommendations? If so, most likely the entire data set needs to be re-processed and hence will keep Hadoop nodes busy. Is it possible to do just process the incremental updates and not the entire data set at every run? This is something that can be done with Reco4j (Graph based recommender engine) + Hadoop to handle real time updates. But I wonder if this can be done with RDBMS and Hadoop?

**[Gagan Juneja](#9439 "2013-09-03 10:17:10"):** Very valid question Gagan A. This is not possible to achieve real time recommendations with RDBMS + Hadoop setup. But people are running hadoop jobs every hour or so to achieve this. The kind of setup I talked about, there generally Hadoop job is scheduled nightly to process entire data not only the incremental updates. There are other tools available Impala, Spark, Graph dbs like you mentioned to achieve real time behavior with Hadoop. Simple setup of Hadoop + Hive can also do that. Thanks!

