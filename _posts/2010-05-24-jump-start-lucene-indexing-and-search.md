---
layout: post
header-img: img/default-blog-pic.jpg
---

# Jump-start Lucene: Indexing and Search

This is a blog explaining what Apache Lucene is, how it works and some of its useful features. This information can be useful for someone who wants to quickly start on Lucene and also for someone who has used Lucene earlier but would like to gain from our experience on Lucene v2.4.1.

# Introduction

Apache Lucene is an open source Java based indexing and searching technology. It is a technology suitable for any application that requires [full-text search](http://en.wikipedia.org/wiki/Full_text_search), especially cross-platform. Lucene works by first indexing the data to be searched and then using the index for searching.

![lucenediagram](/wp-content/uploads/2010/05/lucenediagram-291x300.jpg)

# Lucene Indexing Basics

Indexing is a process of converting text data into a format that facilitates rapid searching. Lucene stores the data in the form of an inverted index. An analogy is an index at the end of a book; the index points to the location of topics that appear in the book.

There will be “Terms” created from the input which will point to the “Documents”. “Terms” are indexed and any search is performed on “Terms” and the related “Documents” are fetched.

Lucene index is built over an implementation of the Directory class. Lucene supports multiple Directory implementations: - 

  * _RAMDirectory_: Complete index is kept in memory.

  * _FSDirectory_: Complete index is stored on file system. Some part of index is kept in cache.

  * _NIOFSDirectory_: Supports multiple threads on FSDirectory.

The “Terms” to be created for the indexing can be governed by something called “Analyzers”. Lets see what they are.

### Analysis

The input to be indexed can be analyzed to extract the terms on which the searching can be done, like extracting the words, removing common words, changing words to lowercase, etc. Analyzers are used both at indexing time and search time. It is highly recommended to use the same analyzers both at index creation and searching time so that tokens created for searching are created in the same way the data was indexed.

Analyzers internally use Tokenizers and Filters. Analyzers take a string as input and returns back a stream of tokens.

Lucene provides some out of the box analyzers which come handy. In addition custom analyzers can also be created. Lets have a look at some of the Analyzers/Tokenizers/Filters provided by Lucene.

### Useful Analyzers and Filters

**SimpleAnalyzer**

Tokenize the string to a set of words and converts them to lower case. 

**StandardAnalyzer**

Tokenize the string to a set of words identifying acronyms, email addresses, host names, etc., discarding the basic English _stop words_ (a, an, the, to) and stemming the words. 

**WhitspaceAnalyzer**

Tokenize the data on white spaces.

**NgramTokenFilter (quite useful for fuzzy searches)**

Tokenize the input into n-grams of the given size. It is used for fuzzy matching. This helps in matching when the search input text has spelling mistakes. For e.g. input text "Lucene" for ngram size 3 will have tokens "Luc","uce","cen" and "ene", if the user entered "Tucene" while searching still word "Lucene" will be shown as a suggestion since "Tucene" has similar tokens "uce","cen" and "ene" which point to the Document "Lucene" in the knowledge.

**EdgeNGramTokenFilter**