---
layout: post
header-img: img/default-blog-pic.jpg
author: Gagan
description: 
post_id: 16650
created: 2013/06/13 19:52:38
created_gmt: 2013/06/13 14:52:38
comment_status: open
---

# Understanding and Debugging Lucene Index.

<p>In my last project we used lucene for search functionality. Initially we struggled a lot in handling following issues:
<ol>
    <li>Convincing testing team about search result.</li>
    <li>Deciding boost factor to alter search results ordering.</li>
    <li>Analyzing and understanding the impact of using different analyzers.</li>
    <li>Analyzing the result of different query patterns.</li>
    <li>Confirming whether all the terms are indexed or not. The fields which are marked as NOT_ANALYZED should not be indexed, and marked Stored.YES should be stored.</li>
    <li>Basically we need to write a lot of code to examine the lucene index and query explanations.</li>
</ol>
<b>Note</b> This is not a beginner blog for lucene. I am assuming that readers of this blog are aware of lucene and its functionalities.
<!--more-->
We could handle all these scenarios by writing code using lucene's exposed APIs, but that idea seems to be very cumbersome. Finally we found one tool name “Luke”. Luke is an elegant lucene index browser which makes our life very simple.</p>
<p><b>Luke</b>
Luke is a desktop based application which is very light open source library. This is available as small jar file. We can simply run it like executable jar using “java -jar” command from command line or by simply double clicking in gui. It provides very good user interface. It provides overall index statistics like number of docs, terms in a particular doc and what not? You can browse entire index, fire queries, change analyzers, update indexes, delete documents etc. We can also see the explanation of query and results.</p>
<p><b>Installing Luke</b>
As we discussed above to install Luke we just need to download Luke jar from this <a title="luke" href="http://code.google.com/p/luke/" target="_blank">link</a>. The latest version at this time is 4.0.0_ALPHA which works on lucene 4.0.0-ALPHA. Simply go to this link and download lukeall-4.0.0-ALPHA.jar jar file. Run it using below command.</p>
<p>[code]
Java -jar lukeall-4.0.0-ALPHA.jar
[/code]</p>
<p>To illustrate Luke functionality with example let's first create simple lucene index for below documents. Assuming that readers are aware of lucene and can simply add below documents to lucene index.</p>
<p>[code]
document 1: {docNumber:”doc-1”, contents:”Gagan works at Xebia.”}
document 2: {docNumber:”doc-1”, contents:”There is an IT company in Gurgaon named Xebia , xebia is a good company.”}
document 3: {docNumber:”doc-1”, contents:”People in xebia are satisfied with xebia's work culture.”}
[/code]</p>
<p><b>How Luke GUI looks like</b>
When we run Luke using above command it prompts with a below screen. Which asks you for index directory.</p>
<p>[caption id="attachment_16666" align="aligncenter" width="300"]<a href="http://xebee.xebia.in/wp-content/uploads/2013/05/blog1.png"><img class="size-medium wp-image-16666" alt="Luke Startup screen." src="http://xebee.xebia.in/wp-content/uploads/2013/05/blog1-300x233.png" width="300" height="233" /></a> Luke Startup screen.[/caption]</p>
<p>Once you enter the index directory path Luke will load index and show you the following screen.</p>
<p>[caption id="attachment_16697" align="aligncenter" width="300"]<a href="http://xebee.xebia.in/wp-content/uploads/2013/06/blog2.png"><img class="size-medium wp-image-16697" alt="Luke overview tab screen" src="http://xebee.xebia.in/wp-content/uploads/2013/06/blog2-300x234.png" width="300" height="234" /></a> Luke overview tab screen[/caption]</p>
<p>Now the above screen has four tabs at top.</p>
<p><b>Overview</b>
This tab is opened by default and can be seen in the above screen as well. As name suggested it shows the overall statistics of lucene index like number of documents, top terms, number of deleted document in index etc.</p>
<p><b>Documents</b>
We can analyze particular document in this tab. You can browse different terms of a document. You can edit or add documents. Below is the screen where I am editing one document with content “Gagan works at Xebia” and used lucene's StandardAnalyzer so “at” is removed because of English language stop word and place is shown as null_1, while editing this document. Which is very difficult to see without luke.</p>
<p>[caption id="attachment_16699" align="aligncenter" width="300"]<a href="http://xebee.xebia.in/wp-content/uploads/2013/06/blog4.png"><img class="size-medium wp-image-16699" alt="Luke documents tab" src="http://xebee.xebia.in/wp-content/uploads/2013/06/blog4-300x236.png" width="300" height="236" /></a> Luke documents tab[/caption]</p>
<p><b>Search</b>
This is very important tab here we can run our search queries and analyze the order of documents listed in result. We can use QueryParser with different options like with/without wild characters etc. We can also change the similarity setting and analyze the results.
If I search “xebia” in lucene index that includes 3 documents that we listed above. Below is the screen showing result of this search.</p>
<p>[caption id="attachment_16700" align="aligncenter" width="300"]<a href="http://xebee.xebia.in/wp-content/uploads/2013/06/Blog5.png"><img class="size-medium wp-image-16700" alt="Luke search tab" src="http://xebee.xebia.in/wp-content/uploads/2013/06/Blog5-300x235.png" width="300" height="235" /></a> Luke search tab[/caption]</p>
<p>We can see document level explain to see why doc1 is given weight “0.3562” in comparison to “0.3148” for document 2.</p>
<p>Below screens show the explain of first two documents.</p>
<p>[caption id="attachment_16701" align="aligncenter" width="300"]<a href="http://xebee.xebia.in/wp-content/uploads/2013/06/blog-new.png"><img class="size-medium wp-image-16701" alt="Luke search screen with explain plan" src="http://xebee.xebia.in/wp-content/uploads/2013/06/blog-new-300x238.png" width="300" height="238" /></a> Luke search screen with explain plan[/caption]</p>
<p>Here field norm is different for search term “xebia” in two documents because as per lucene logic in accordance with the number of tokens of this field in the document, so that shorter fields contribute more to the score that's why doc-1 ranked 1.</p>
<p><b>Commits</b>
Commits screen list all the commits happened on this particular index along with list of all files generated by lucene. This tab provides the facility to open a particular commit point.</p>
<p>[caption id="attachment_16703" align="aligncenter" width="300"]<a href="http://xebee.xebia.in/wp-content/uploads/2013/06/Blog8.png"><img class="size-medium wp-image-16703" alt="Luke commit screen." src="http://xebee.xebia.in/wp-content/uploads/2013/06/Blog8-300x234.png" width="300" height="234" /></a> Luke commit screen.[/caption]</p>
<p><b>Plugin</b>
In this tab you can add available/self developed luke plugins. Some plugins are already installed like Hadoop plugin to support hdfs file system to read lucene index.</p>
<p>[caption id="attachment_16702" align="aligncenter" width="300"]<a href="http://xebee.xebia.in/wp-content/uploads/2013/06/blog9.png"><img class="size-medium wp-image-16702" alt="Luke plugin screen" src="http://xebee.xebia.in/wp-content/uploads/2013/06/blog9-300x238.png" width="300" height="238" /></a> Luke plugin screen[/caption]</p>
<p>This is very basic example to show the use of luke, but you can play and dissect your lucene index with this powerful tool. This tool helped us a lot while developing search functionality using lucene.</p>