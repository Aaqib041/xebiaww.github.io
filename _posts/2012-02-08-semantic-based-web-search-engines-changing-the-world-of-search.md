---
layout: post
header-img: img/default-blog-pic.jpg
---

# Semantic based web search engines- Changing the world of Search

An important quality that the majority of search engines functional today lack is the ability to take into account the intention of the user behind the overall query. Basing the matching of web pages on keyword frequency and a ranking metric such as [Pagerank ](http://en.wikipedia.org/wiki/PageRank)return various results that may be of high ranking but still irrelevant to the users intended context. Therefore I explored with my friend Sumedha and realized that there  is a need to add [semantics](http://en.wikipedia.org/wiki/Semantic_search) to the web search. There are some semantic search engines that have already come up in the markets eg. Hakia, Swoogle, Kosmix, etc. that takes a semantic based approach which is different from the traditional search engines. I really liked their idea of  implementing and adding semantics to the web search. This provoked me to do more research in this field and tried to think of different ways to add semantics.  Following is an _Algorithm _that can be used in _Semantic Based Web Search Engines_ :- So, to find web pages on the internet that match a user’s query based not only on the important keywords in a user query, but also based on the intention of the user, behind that query, first the user’s entered query is expanded using [WordNet](http://en.wikipedia.org/wiki/WordNet) ontology. This Algorithm focuses on work that uses the _Hypernym_/_Hyponymy _and_ Synset _relations in WordNet for query expansion algorithm. A set of words that are highly related to the words in the user query, determined by the frequency of their occurrence in the _Synset_ and _Hyponym_ tree for each of the user query terms is created.  This set is now refined using the same relations to get a more precise and accurate expanded query. 

# **Query Expansion Using Semantics**

Let the user input query consisting of a set of query terms Qi be: Q = {q1, q2,...................qt}                                          (1) The next step is to find the set _EQ_ consisting of the semantically expanded user Query _Q_. EQ = {eq1, eq2, .........eqm}                                         (2) The set _EQ_ can then be used to return a list of web pages that match the expanded query and are ranked according to highest relevance first. 

## **_Query Preprocessing_**

When the search engine is initially begun, a HashSet of commonly used English words is created globally, to be used for stop word removal each time a user query is inputted. This helps in elimination of noise data and reducing redundancy during context creation. On the search engine, a user is asked to enter a String Query, _Q_. This query is now tokenized into separate _‘t’_ Query terms by removing white spaces to get (1). We now perform stop word removal using the hashset of common English words defined during the backend processing to get a refined set of _‘n’_ query terms, Q' = {q'1, q'2, ............... q'n}                                     (3) This set _Q _is now used for semantic query expansion. 

## **_Algorithm _**

The complete algorithm used for Query Expansion is: _ _ _Algorithm_ : QueryExpansion **_Input_**: User Query **_Output_**: Refined Expanded Query 1.     Input_ Q = {q1, q2, …, qt}_ from user. 2.     Perform tokenization and stop word removal to get refined query _Q’ = {q’1, q’2, … q’n}_ 3.     Use synset _S(EQ)_ and hyponymy relationships to get expanded query _EQ_. 4.     Create a hierarchical structure of terms _eqi_ in expanded query to get context string _C_. 

# **Finding Content for user Context**

To find web pages that are relevant to the users’ intention, a directory of the web that categorizes web pages according to their content is needed. For this purpose, the [Open Directory Project](http://www.dmoz.org/%20), which is a human-edited directory of the web can be used. Similarly, to find text content that is related to the users’ intention, content can be extracted from the DBpedia.org Project ([DBpedia](http://dbpedia.org/About)). It allows extraction of structured information from [Wikipedia](http://www.wikipedia.org/%20). 

## **_Retrieval of Relevant URLs_**

The [RDF dump](http://www.dmoz.org/rdf.html) of the ODP provides a list of all the possible categories found within it . The Context String found after the QueryExpansion algorithm is matched to all the categories within the ODP to retrieve the top ten category matches. These top ten matches are then displayed to the user using our user-interface and he/she is asked to select the category that according to them is the closest to their intention. This is called _CAT._ Once we have the correct category, we can create an online connection to the ODP to retrieve all the URLs that fall within that category. This can be done by tokenizing the CAT string and passing it as a query to the ODP search box. 

## **_Retrieval of Relevant Content_**

Now to display some relevant, concise text content to the user, along with the URLs, so that the user has immediate access to some information related to his user query. For this, the CAT string is tokenized into keywords and the ‘Keyword Search’ feature of DBpedia is used to retrieve a list of Uniform Resource Identifiers (URI) that matches this tokenized CAT string. These URIs are sorted according to maximum match the keywords by DBpedia. The first URL in the list returned by DBpedia is now used for content retrieval. The requirement now is to extract some text content from this DBpedia resource that can be used to display to the user. For this, use of automated queries written in the [SPARQL](http://www.w3.org/TR/rdf-sparql-query/) Query Language for RDF (SPARQL) is made. Using this automated query, extract a short abstract, written in English language that describes the resource in a brief manner is extracted and stored. 

## **_Semantics Based Search Engine_**

To display the above results to the user, a user interface called the Semantics Based Search Engine (SBSE) is created in the form of a website. The home page of the website asks the user to enter his query. Once the user enters his query and presses the ‘Search’ button, he is redirected to a page that shows him a dropdown list of the top ten matching ODP categories that were found after comparison with the context string _C. _Once the user selects the most relevant category, he is directed to a page that uses frames to display the short abstract retrieved from DBpedia in the left frame and a list of relevant URLs in the right frame. **Conclusion**

## Comments

**[Pankaj Dhawan](#8657 "2012-05-02 14:31:29"):** Nice Job Prachi.........

**[Shruti Khattar](#7469 "2012-02-08 15:47:00"):** Good job Prachi... Keep Posting :)

**[Gagandeep Singh](#7472 "2012-02-08 17:30:55"):** Good article however I couldn't see any practicability of this in real world scenarios. Few points that I could see are, 1\. Very few websites provide RDF support. Even this blog don't provide that. Facebook trying to do that with open graph but that too is very limiting. 2\. Users never going to select categories because they need convenience. You also have to make auto-categorization of query or display small pool of result from say top 5 categories. 3\. IMO, stop words are major determiner of query context and non stop words are determiner of query category. So, I am hoping that you will take these points into consideration. And, will make better usable search engine for people like me. :)

**[Prachi Nagpal](#7473 "2012-02-08 18:53:13"):** Thank You Shruti.

**[Prachi Nagpal](#7474 "2012-02-08 19:26:48"):** @Gagandeep : Thanq for your informative points. This is one way of doing this which is being tried to portray in this blog. Well, 1\. There are some difficulties with the semantics of RDF, which are currently being resolved by the RDFCore working group since benefit of using RDF wrt efficiency is obvious. 2\. Many heuristic algorithms are being worked upon to compute the min-cost categorization. 3\. If the sets of data are similar, the removal of stopwords may not be important to the search. But if the results or the categories aren’t similar, the stopword may be considered important and shouldn’t be removed from the query.

