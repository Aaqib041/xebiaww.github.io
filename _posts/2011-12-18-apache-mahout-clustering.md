---
layout: post
header-img: img/default-blog-pic.jpg
author: pranjan
description: 
post_id: 10517
created: 2011/12/18 22:55:17
created_gmt: 2011/12/18 17:55:17
comment_status: open
---

# Introducing Apache Mahout

Nowadays almost every application is dealing with a descently big amount of data. Irrespective of whether the application is of e-commerce, travel, learning, finance or social networking domain, they all have a good amount of customer data, feedback data, financial records etc. The origin of frameworks in nosql and distributed computing/storage field have also boosted the applications urge to store all the data it can. Nowadays, servers running on commodity hardware can very easily support storage of billions of records. So, its not at all a big deal for any application to store whatever it can.

The result of all this is a huge heap of records persisted on hard disks on various clouds hosted all around the world. And the world is happy keeping it over there. To add more and more of data every passing day. Apache Mahout can help them increase their happiness.

Apache Mahout is a machine learning framework. Don't get scared by hearing machine learning. Its simple. There is no need for you to go through all the statistical, algorithmic, mathematical formulas to use Apache Mahout. Apache Mahout implements everything for you.** Tells you which algorithm can help in which business scenario, and gives you java classes and CLI's ( Command Line Invocators) to run the algorithms.**

Now, lets see what all it can do.

**Recommendation:**

You have an application which sells books. The application stores data of who bought which book. This data will anyway be available as the customers and purchase records would be available.

Now, generally readers have similar preferences. A person buying Flex books might be interested in HTML 5 book. A person buying book on Hadoop might be interested in a book on HBase. But how would you know that?

Here's where Apache Mahout's recommendation algorithms pitch in. **Mahout has a number of recommendation algorithms which can find out similarity between users. **You just need to provide it all the customer purchases and it would find out recommendations for customers by finding out similar customers. There are algorithms which can also use user ratings to further improve the recommendations.

But, you say you have 1 billion records of customer data. Would Mahout be able to process it?

Yes, another good thing about Apache Mahout is that it is built on top of Hadoop. So, if needed, you can run it on a Hadoop Cluster. If it finds Hadoop Configuration parameters like Hadoop home etc on the server, then it will automatically use the cluster and process data on it. It uses HDFS to read/write records and results when running on Hadoop.

It does not mean that it can run only on Hadoop. Each algorithm is also having an sequential implementation. You have an option to run it on a distributed system or locally. If you have only some hundreds of thousands of records, then why go for a cluster. Just run it on your standalone machine.

**Clustering:**

You have a social networking site where people share articles and thoughts. Wouldn't you like to provide users the option to read similar articles? Someone reading an article on tomcat configuration might be interested in articles on other web containers. But how would you find similar articles if there was no user information ever stored. The application does not keep the track of which user read what.

This can be done by using Apache Mahout's Clustering Algorithms. **Apache Mahout's clustering algorithms help in finding groups containing similar items.**

How it works? Think of a n-dimensional ( for simplicity, you can also assume a 2-dimensional ) space. Now, plot co-ordinates (0,0), (0,1), (0,1), (1,1) and (5,5), (5,4),(4,5), (4,4) on it. You can very easily see two groups of points on the x-y (2 dimensional) space. One group around (0,0) and another group around (4,4). These groups can also be called as clusters of points.

Apache Mahout's Clustering Algorithms helps in finding out clusters in a vector space. Suppose you are able to express an article in a n-dimensional vector, where each dimension of the vector is representing number of occurrences of a particular word in the article i.e. Suppose in another article, Number of occurrences of "server" = 5, "tomcat"=6, "port"=7 etc. Similarly, all articles having lots of these keywords can be of interest to the reader of Tomcat article (mentioned in the beginning of this section).

Articles is just an example, you can use any of your product's features to find out similar groups. Mahout implements a number of Clustering algorithms, and it can run sequentially as well as on a Hadoop Cluster.

**Classification:**

**﻿**When we talked about Clustering in the previous section, that was an example of unsupervised learning. What is unsupervised learning? When you don't have any prior information about the data and relation between them, and you try to extract some information/relation from it. We tried to do that using Clustering.

**﻿Another type of machine learning is supervised learning. In supervised learning, you need to train the algorithm with some prior information.**

Lets take the example of a bank which wants to classify its customers into premium, average and useless category. It can pick out some features/properties of customers like balance, number of transactions, credit rating, age of account, number of accounts etc, and figure out premium, average and useless category of users manually for around 1% of the data.

This information needs to be fed into the training algorithm. Now, the algorithm is supervised. It knows which type of user can belong to which category by analyzing the features of the other users. It can now instantly categorize the users into one of the categories.

Classification algorithms generally take a sample input with predefined results and produce/train a model. The model then takes other records and classifies into categories.

This was an overview of what all is in Apache Mahout. In future posts, I will be explaining how to use different features of Mahout.

## Comments

**[kiran](#7868 "2012-03-06 17:20:39"):** Good tutorial, pls share the links where would i get more info on the Mahout. Thanks

