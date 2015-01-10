---
layout: post
header-img: img/default-blog-pic.jpg
author: ankitk
description: 
post_id: 3568
created: 2010/05/24 14:44:45
created_gmt: 2010/05/24 09:44:45
comment_status: open
---

# Jump-start Lucene: Indexing and Search

<p style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;">This is a blog explaining what Apache Lucene is, how it works and some of its useful features. This information can be useful for someone who wants to quickly start on Lucene and also for someone who has used Lucene earlier but would like to gain from our experience on Lucene v2.4.1.</span></span></p>

<h1 style="margin-bottom: 0in;">Introduction</h1>

<p style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;">Apache Lucene is an open source Java based indexing and searching technology. It is a technology suitable for any application that requires </span></span><span style="color: #000080;"><span lang="zxx"><span style="text-decoration: underline;"><a href="http://en.wikipedia.org/wiki/Full_text_search"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;">full-text search</span></span></a></span></span></span><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;">, especially cross-platform. Lucene works by first indexing the data to be searched and then using the index for searching.</span></span></p>

<p style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;"><!--more--></span></span></p>

<p><img class="aligncenter size-medium wp-image-3570" title="lucenediagram" src="http://xebee.xebia.in/wp-content/uploads/2010/05/lucenediagram-291x300.jpg" alt="lucenediagram" width="291" height="300" />
<h1 style="margin-bottom: 0in;">Lucene Indexing Basics</h1>
<p style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;">Indexing is a process of converting text data into a format that facilitates rapid searching. Lucene stores the data in the form of an inverted index. An analogy is an index at the end of a book; the index points to the location of topics that appear in the book.</span></span></p>
<p style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;">There will be “Terms” created from the input which will point to the “Documents”. “Terms” are indexed and any search is performed on “Terms” and the related “Documents” are fetched.</span></span></p>
<p style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;">Lucene index is built over an implementation of the Directory class. Lucene supports multiple Directory implementations: - </span></span></p></p>
<ul>
    <li>
<div style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;"><span style="font-style: normal;"><em>RAMDirectory</em></span></span></span><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;">: Complete index is kept in memory.</span></span></div></li>
    <li>
<div style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;"><span style="font-style: normal;"><em>FSDirectory</em></span></span></span><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;">: Complete index is stored on file system. Some part of index is kept in cache.</span></span></div></li>
    <li>
<div style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;"><span style="font-style: normal;"><em>NIOFSDirectory</em></span></span></span><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;">: Supports multiple threads on FSDirectory.</span></span></div></li>
</ul>

<p style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;">The “Terms” to be created for the indexing can be governed by something called “Analyzers”. Lets see what they are.</span></span></p>

<h3 style="margin-bottom: 0in;"><span style="text-decoration: underline;">Analysis</span></h3>

<p style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;">The input to be indexed can be analyzed to extract the terms on which the searching can be done, like extracting the words, removing common words, changing words to lowercase, etc. Analyzers are used both at indexing time and search time. It is highly recommended to use the same analyzers both at index creation and searching time so that tokens created for searching are created in the same way the data was indexed.</span></span></p>

<p style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;">Analyzers internally use Tokenizers and Filters. Analyzers take a string as input and returns back a stream of tokens.</span></span></p>

<p style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;">Lucene provides some out of the box analyzers which come handy. In addition custom analyzers can also be created. Lets have a look at some of the Analyzers/Tokenizers/Filters provided by Lucene.</span></span></p>

<h3><span style="text-decoration: underline;">Useful Analyzers and Filters</span></h3>

<p style="margin-bottom: 0in; font-style: normal;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;"><strong>SimpleAnalyzer</strong></span></span></p>

<p style="font-weight: normal; margin-bottom: 0in; font-style: normal;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;">Tokenize the string to a set of words and converts them to lower case. </span></span></p>

<p style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;"><strong>StandardAnalyzer</strong></span></span></p>

<p style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;"><span style="font-style: normal;"><span style="font-weight: normal;">Tokenize the string to a set of words identifying acronyms, email addresses, host names, etc., discarding the basic English </span></span></span></span><em><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;"><span style="font-style: normal;"><span style="font-weight: normal;">stop words</span></span></span></span></em><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;"><span style="font-style: normal;"><span style="font-weight: normal;"> (a, an, the, to) and stemming the words. </span></span></span></span></p>

<p style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;"><strong>WhitspaceAnalyzer</strong></span></span></p>

<p style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;">Tokenize the data on white spaces.</span></span></p>

<p style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;"><strong>NgramTokenFilter (quite useful for fuzzy searches)</strong></span></span></p>

<p style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;">Tokenize the input into n-grams of the given size. It is used for fuzzy matching. This helps in matching when the search input text has spelling mistakes. For e.g. input text "Lucene" for ngram size 3 will have tokens "Luc","uce","cen" and "ene", if the user entered "Tucene" while searching still word "Lucene" will be shown as a suggestion since "Tucene" has similar tokens "uce","cen" and "ene" which point to the Document "Lucene" in the knowledge.</span></span></p>

<p style="margin-bottom: 0in;"><span style="font-family: Verdana, sans-serif;"><span style="font-size: x-small;"><strong>EdgeNGramTokenFilter</strong></span></span></p>