---
layout: post
header-img: img/default-blog-pic.jpg
author: prachinagpal
description: 
post_id: 11237
created: 2012/02/08 15:25:13
created_gmt: 2012/02/08 10:25:13
comment_status: open
---

# Semantic based web search engines- Changing the world of Search

<p>An important quality that the majority of search engines functional today lack is the ability to take into account the intention of the user behind the overall query. Basing the matching of web pages on keyword frequency and a ranking metric such as <a target="_blank" href="http://en.wikipedia.org/wiki/PageRank">Pagerank </a>return various results that may be of high ranking but still irrelevant to the users intended context. Therefore I explored with my friend Sumedha and realized that there  is a need to add <a target="_blank" href="http://en.wikipedia.org/wiki/Semantic_search">semantics</a> to the web search.</p>
<p>There are some semantic search engines that have already come up in the markets eg. Hakia, Swoogle, Kosmix, etc. that takes a semantic based approach which is different from the traditional search engines. I really liked their idea of  implementing and adding semantics to the web search. This provoked me to do more research in this field and tried to think of different ways to add semantics. <!--more--></p>
<p>Following is an <i>Algorithm </i>that can be used in <i>Semantic Based Web Search Engines</i> :-</p>
<p>So, to find web pages on the internet that match a user’s query based not only on the important keywords in a user query, but also based on the intention of the user, behind that query, first the user’s entered query is expanded using <a target="_blank" href="http://en.wikipedia.org/wiki/WordNet">WordNet</a> ontology.</p>
<p>This Algorithm focuses on work that uses the <i>Hypernym</i>/<i>Hyponymy </i>and<i> Synset </i>relations in WordNet for query expansion algorithm. A set of words that are highly related to the words in the user query, determined by the frequency of their occurrence in the <i>Synset</i> and <i>Hyponym</i> tree for each of the user query terms is created.  This set is now refined using the same relations to get a more precise and accurate expanded query.
<h1><span style="font-size: small"><b>Query Expansion Using Semantics</b></span></h1>
Let the user input query consisting of a set of query terms Q<sub>i </sub>be:</p>
<p>Q = {q<span style="font-size: xx-small">1</span>, q<span style="font-size: xx-small">2</span>,...................q<span style="font-size: xx-small">t</span>}                                          (1)</p>
<p>The next step is to find the set <i>EQ</i> consisting of the semantically expanded user Query <i>Q</i>.</p>
<p>EQ = {eq<span style="font-size: xx-small">1</span>, eq<span style="font-size: xx-small">2</span>, .........eq<span style="font-size: xx-small">m</span>}                                         (2)</p>
<p>The set <i>EQ</i> can then be used to return a list of web pages that match the expanded query and are ranked according to highest relevance first.
<h2><span style="font-size: small"><b><i>Query Preprocessing</i></b></span></h2>
When the search engine is initially begun, a HashSet of commonly used English words is created globally, to be used for stop word removal each time a user query is inputted. This helps in elimination of noise data and reducing redundancy during context creation.</p>
<p>On the search engine, a user is asked to enter a String Query, <i>Q</i>. This query is now tokenized into separate <i>‘t’</i> Query terms by removing white spaces to get (1). We now perform stop word removal using the hashset of common English words defined during the backend processing to get a refined set of <i>‘n’</i> query terms,</p>
<p>Q' = {q'<span style="font-size: xx-small">1</span>, q'<span style="font-size: xx-small">2</span>, ............... q'<span style="font-size: xx-small">n</span>}                                     (3)</p>
<p>This set <i>Q </i>is now used for semantic query expansion.
<h2><span style="font-size: small"><b><i>Algorithm </i></b></span></h2>
The complete algorithm used for Query Expansion is:</p>
<p><i> </i></p>
<p><i>Algorithm</i> : QueryExpansion</p>
<p><b><i>Input</i></b>: User Query</p>
<p><b><i>Output</i></b>: Refined Expanded Query</p>
<p>1.     Input<i> Q = {q<sub>1</sub>, q<sub>2</sub>, …, q<sub>t</sub>}</i> from user.</p>
<p>2.     Perform tokenization and stop word removal to get refined query <i>Q’ = {q’<sub>1</sub>, q’<sub>2</sub>, … q’<sub>n</sub>}</i></p>
<p>3.     Use synset <i>S(EQ)</i> and hyponymy relationships to get expanded query <i>EQ</i>.</p>
<p>4.     Create a hierarchical structure of terms <i>eq<sub>i</sub></i> in expanded query to get context string <i>C</i>.
<h1><span style="font-size: small"><b>Finding Content for user Context</b></span></h1>
To find web pages that are relevant to the users’ intention, a directory of the web that categorizes web pages according to their content is needed. For this purpose, the <a target="_blank" href="http://www.dmoz.org/%20">Open Directory Project</a>, which is a human-edited directory of the web can be used. Similarly, to find text content that is related to the users’ intention, content can be extracted from the DBpedia.org Project (<a target="_blank" href="http://dbpedia.org/About">DBpedia</a>). It allows extraction of structured information from <a target="_blank" href="http://www.wikipedia.org/%20">Wikipedia</a>.
<h2><span style="font-size: small"><b><i>Retrieval of Relevant URLs</i></b></span></h2>
The <a target="_blank" href="http://www.dmoz.org/rdf.html">RDF dump</a> of the ODP provides a list of all the possible categories found within it . The Context String found after the QueryExpansion algorithm is matched to all the categories within the ODP to retrieve the top ten category matches. These top ten matches are then displayed to the user using our user-interface and he/she is asked to select the category that according to them is the closest to their intention. This is called <i>CAT.</i></p>
<p>Once we have the correct category, we can create an online connection to the ODP to retrieve all the URLs that fall within that category. This can be done by tokenizing the CAT string and passing it as a query to the ODP search box.
<h2><span style="font-size: small"><b><i>Retrieval of Relevant Content</i></b></span></h2>
Now to display some relevant, concise text content to the user, along with the URLs, so that the user has immediate access to some information related to his user query. For this, the CAT string is tokenized into keywords and the ‘Keyword Search’ feature of DBpedia is used to retrieve a list of Uniform Resource Identifiers (URI) that matches this tokenized CAT string. These URIs are sorted according to maximum match the keywords by DBpedia. The first URL in the list returned by DBpedia is now used for content retrieval.</p>
<p>The requirement now is to extract some text content from this DBpedia resource that can be used to display to the user. For this, use of automated queries written in the <a target="_blank" href="http://www.w3.org/TR/rdf-sparql-query/">SPARQL</a> Query Language for RDF (SPARQL) is made. Using this automated query, extract a short abstract, written in English language that describes the resource in a brief manner is extracted and stored.
<h2><span style="font-size: small"><b><i>Semantics Based Search Engine</i></b></span></h2>
To display the above results to the user, a user interface called the Semantics Based Search Engine (SBSE) is created in the form of a website.</p>
<p>The home page of the website asks the user to enter his query. Once the user enters his query and presses the ‘Search’ button, he is redirected to a page that shows him a dropdown list of the top ten matching ODP categories that were found after comparison with the context string <i>C. </i>Once the user selects the most relevant category, he is directed to a page that uses frames to display the short abstract retrieved from DBpedia in the left frame and a list of relevant URLs in the right frame.</p>
<p><span style="font-size: small"><b>Conclusion</b></span></p>

## Comments

**[Pankaj Dhawan](#8657 "2012-05-02 14:31:29"):** Nice Job Prachi.........

**[Shruti Khattar](#7469 "2012-02-08 15:47:00"):** Good job Prachi... Keep Posting :)

**[Gagandeep Singh](#7472 "2012-02-08 17:30:55"):** Good article however I couldn't see any practicability of this in real world scenarios. Few points that I could see are, 1\. Very few websites provide RDF support. Even this blog don't provide that. Facebook trying to do that with open graph but that too is very limiting. 2\. Users never going to select categories because they need convenience. You also have to make auto-categorization of query or display small pool of result from say top 5 categories. 3\. IMO, stop words are major determiner of query context and non stop words are determiner of query category. So, I am hoping that you will take these points into consideration. And, will make better usable search engine for people like me. :)

**[Prachi Nagpal](#7473 "2012-02-08 18:53:13"):** Thank You Shruti.

**[Prachi Nagpal](#7474 "2012-02-08 19:26:48"):** @Gagandeep : Thanq for your informative points. This is one way of doing this which is being tried to portray in this blog. Well, 1\. There are some difficulties with the semantics of RDF, which are currently being resolved by the RDFCore working group since benefit of using RDF wrt efficiency is obvious. 2\. Many heuristic algorithms are being worked upon to compute the min-cost categorization. 3\. If the sets of data are similar, the removal of stopwords may not be important to the search. But if the results or the categories aren’t similar, the stopword may be considered important and shouldn’t be removed from the query.

