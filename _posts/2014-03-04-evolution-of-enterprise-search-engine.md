---
layout: post
header-img: img/default-blog-pic.jpg
author: Gagan
description: 
post_id: 18127
created: 2014/03/04 12:39:18
created_gmt: 2014/03/04 07:39:18
comment_status: open
---

# Evolution of Enterprise Search Engine

Today we can not even think of creating an application whether web or standalone without a search feature. Wherever I presented this topic the first question people asked me is

“If users know Google search then why do we need another search feature””

So before beginning a deep discussion on enterprise search, I would like to mention the differences between web search applications and enterprise search..  **Definitions from Wikipedia**

**Web search engine** A web search engine is a software system that is designed to search for information on the World Wide Web

**Enterprise search engine** “Enterprise search is the practice of making content from multiple enterprise-type sources, such as databases and intranets, searchable to a defined audience.”

Some of the important differences between web search engine and enterprise search engine are:

  1. Enterprise search has limited scope, only to a particular website/application. 
  2. User is interested in certain specific information from enterprise search whereas in web search user looks for concepts and relevant information on some topic.
  3. Security features such as access control are very important for enterprise search engine.
  4. Enterprise search generally uses faceted search to help in proper navigation and filtering.
  5. Enterprise search engine has multiple sources of information such as database, document repositories etc.

Enterprise search and web search both share the same objective of showing results for the search query but having different purposes and use cases.

**Evolution of Enterprise search**

Let us discuss how the market of enterprise search engine evolved and the current developments. I have divided enterprise search evolution into five generations. Below is the enterprise search evolution diagram which shows its five generations.  
![Evolution_of_ES][1]

Let’s understand each generation in detail and its outcome.

**Gen 1** This was the time when the application used to fire search (select) queries to get data out of relational databases to show in search result. Data size was not big and this solution worked perfectly fine. Applications used to have custom search solutions to list results which were not stored in the databases. We can call this age as early age in the evolution of Enterprise search. In this age no special focus was given to enterprise search and these components were developed like other modules of the application. Not many tools were available in the market.

**Gen 2** In late 90's and early 2000, a lot of work was done in the field of Information Retrieval and industry got many new enterprise search tools in this era. 

Important deliverable of this generation was Lucene, which is very light weight open source indexing and search library. This was only 4-5 kbs of jar and provides you the features of a search engine. Although Lucene doesn't provide features like crawling, HTML parsing etc. But its indexing and searching capabilities were simply awesome. Developers used to write java code in order to use Lucene and this was a bit cumbersome. Projects like solr extended lucene capabilities and provided xml based solution on top of lucene and exposed web services to access data.

Apart from open source tools like lucene and solr, various enterprise paid solutions also came to market such as Endeca, Microsoft Sharepoint.

Open source communities were evolving at that point of time. People were afraid of getting open source solutions into production because of lacking community support.

**Gen 3** Majorly this age was to stabilize and enrich the work been done in the second generation Some more out of the box features such as type ahead, fuzzy search, faceted search, Real time indexing, fast data retrieval etc. were delivered in this age.

The another major focus in this age was to make integration of these products very easy and to make these products technology independent.

Google launched “Google search appliance” to provide google like search solution for enterprise needs. Solution included everything like crawling, indexing, searching. This is an expensive solution but good google support was a plus.

Similarly from open source side Solr was in great demand. Open source communities were growing and delivering new enriched features to the market and their solutions were not less than any commercial solutions.

Although we discussed difference between enterprise search and web search in the starting but most of the features in enterprise search were influenced by web search engines like google, yahoo etc. And the trend continues.

**Gen 4** The amount of data, applications used to have in 2nd and 3rd generation were almost of same size. This was the period of late 90s and early 2000. In this generation people started talking about big data and multi nodes (cluster) setups. No solution was available till date which can scale to multiple nodes.

Hadoop brought distributed and horizontal scalability to the market and based on these solutions people started demanding distributed enterprise search solutions.

With growing data size, an enterprise search engine becomes a must have for any enterprise application.

Elasticsearch was the first distributed enterprise search solution available in market. Later Solr cloud and cloudera search and many more distributed search solutions were released.

All these solutions were developed using lucene behind the scene and their distributed solution techniques on top of that and provide very much similar capabilities. Now people started comparing these solutions based on distributed computing features such as shard rebalancing, scaling, real time indexing etc. Which clearly shows the shift in expectations. Now people want to search in terabytes of data and want results very quickly like in a few seconds.

**Gen 5** Personalized search is new concept which is gaining very popularity these days. Simple example of personalize search is that an e-commerce websites shows user products on the top of the search results which match to her tastes and results will be different for different users for the same query.

Another development in this direction is cloud based search-as-a-service based solutions. Although these applications were not able to please because of security concerns but now after the announcements of these services from big giants like Amazon (Cloud search) and Microsoft azure (Lucid imagination), market of these services is picking up.

New philosophy Unified Information Access is also gaining popularity where large volumes of unstructured, semi-structured, and structured information is integrated into a unified environment for processing, analysis and decision-making. Unified Information Access platforms are built on the search backbone for fast data/ information retrieval. Many companies like Endeca, Semantic are working on these types of solutions.

**Conclusion** Enterprise search engine is now top priority feature of any enterprise applications. Solutions are now available to index and search massive amount of structured, semi structured and unstructured data. People started using these capabilities beyond the imagination of the developers of these solutions. I saw one company trying solr for a recommendation engine and generating decent results. Unified Information access platforms will surely change the definition of search engines. So think beyond and a lot more things can be done with these new generation search solutions.

See Xebia's offering in Big Data [here][2].

   [1]: http://xebee.xebia.in/wp-content/uploads/2014/03/Evolution_of_ES.jpg
   [2]: http://www.xebia.in/big-data.html