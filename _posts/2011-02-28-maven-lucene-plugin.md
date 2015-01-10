---
layout: post
header-img: img/default-blog-pic.jpg
author: pranjan
description: 
post_id: 7915
created: 2011/02/28 22:40:27
created_gmt: 2011/02/28 17:40:27
comment_status: open
---

<!--Maven Lucene Plugin is an open source maven plugin for Apache Lucene developed by Paritosh Ranjan of  Xebia India IT Architects Ltd . The project is hosted on SourceForge and can be found here. It is released under GNU General Public License (GPL).

-->

# Maven Lucene Plugin 1.0 : Documentation and Usage

<p style="text-align: justify;"></p>

<h2><span style="text-decoration: underline;"><strong>Maven Lucene Plugin</strong></span></h2>

<p style="text-align: justify;">Maven Lucene Plugin is an open source maven plugin for Apache Lucene developed by<a title="Xebia India" href="http://www.xebiaindia.com/" target="_blank"> Xebia India IT Architects Ltd</a> . The project is hosted on SourceForge and can be found <a title="Maven Lucene Plugin" href="https://sourceforge.net/projects/lucene-plugin/" target="_blank">here</a>. It is released under GNU General Public License (GPL).</p>

<p>The artifact is uploaded at Central Maven Repository <a title="Central Maven Repository" href="http://repo1.maven.org/maven2/com/xebia/" target="_blank">http://repo1.maven.org/maven2/com/xebia/</a>. You can browse the artifact <a title="OSS Sonatype Repository" href="https://oss.sonatype.org/index.html#nexus-search;quick~maven-lucene-plugin" target="_blank">here</a>.</p>
<p><strong>maven-lucene-plugin</strong> creates a Lucene index from a file source. The structure of the lucene index i.e. fields, analyzers, indexLocation, fileSourceLocation, store etc can be configured in a configuration file lucene.xml.</p>
<p><strong>lucene.xml</strong> contains all information regarding the lucene index and the data source (from which the index is created). The Maven Lucene Plugin looks for lucene.xml file in the project root directory (adjacent to pom.xml) and creates the lucene index from the file source mentioned in lucene.xml based on the index configuration provided.<!--more-->
<p style="text-align: justify;"><span style="text-decoration: underline;">Apache Lucene Version</span></p>
<p style="text-align: justify;">The maven-lucene-plugin uses Lucene version 3.0.3. The index can be opened using Luke 1.0.1. Luke can be downloaded from <a title="Download Luke" href="http://luke.googlecode.com/files/lukeall-1.0.1.jar" target="_blank">here</a>.</p></p>
<h3><span style="text-decoration: underline;"><strong>Configuring plugin in pom.xml</strong></span></h3>

<p style="text-align: justify;">Configuring the <strong>maven-lucene-plugin</strong> in <strong>pom.xml</strong> is very simple. All information about index configuration needs to be written in <strong>lucene.xml</strong>. Index can be created by running “<strong>createIndex</strong>” goal in the plugin. It can be done by running the goal during any phase as shown in the example plugin configuration. OR it can be called by executing “<strong>createIndex</strong>” goal from command line by “<strong>mvn lucene:createIndex</strong>”.</p>

<p>The “<strong>createIndex</strong>” goal looks for a file “<strong>lucene.xml</strong>” in the project root directory (adjacent to pom.xml). The <strong>lucene.xml</strong> file needs to have all the information related to the configuration of index.
<h4><span style="text-decoration: underline;">Example pom.xml is shown below:</span></h4>
<pre><strong>&lt;build&gt;
 &lt;plugins&gt;
  &lt;plugin&gt;
    &lt;groupId&gt;com.xebia&lt;/groupId&gt;
    &lt;artifactId&gt;maven-lucene-plugin&lt;/artifactId&gt;
    &lt;version&gt;1.0&lt;/version&gt;
    &lt;executions&gt;
     &lt;execution&gt;
      &lt;phase&gt;install&lt;/phase&gt;
      &lt;goals&gt;
       &lt;goal&gt;createIndex&lt;/goal&gt;
      &lt;/goals&gt;
     &lt;/execution&gt;
    &lt;/executions&gt;
  &lt;/plugin&gt;
 &lt;/plugins&gt;
&lt;/build&gt;</strong></pre>
<h3><span style="text-decoration: underline;">Example lucene.xml</span></h3>
<p style="text-align: justify;"><strong>maven-lucene-plugin</strong> looks for a file <strong>lucene.xml</strong> in the project root directory (adjacent to pom.xml). The <strong>maven-lucene-plugin</strong> will not work without <strong>lucene.xml</strong>. lucene.xml contains all the information about the lucene index.</p></p>
<h4>Example lucene.xml is provided below and each tag is explained in detail after that:</h4>

<pre><strong>&lt;index&gt;
  &lt;indexLocation&gt;C:\\index&lt;/indexLocation&gt;
  &lt;sourceFileLocation&gt;C:\\file\\data.txt&lt;/sourceFileLocation&gt;
  &lt;overWrite&gt;true&lt;/overWrite&gt;
  &lt;separator&gt;;&lt;/separator&gt;
  &lt;field&gt;
    &lt;name&gt;names&lt;/name&gt;
    &lt;type&gt;string&lt;/type&gt;
    &lt;analyzer&gt;WhiteSpaceAnalyzer&lt;/analyzer&gt;
    &lt;store&gt;true&lt;/store&gt;
  &lt;/field&gt;
  &lt;field&gt;
    &lt;name&gt;hobby&lt;/name&gt;
    &lt;type&gt;string&lt;/type&gt;
    &lt;analyzer&gt;SimpleAnalyzer&lt;/analyzer&gt;
    &lt;store&gt;true&lt;/store&gt;
  &lt;/field&gt;
&lt;/index&gt;</strong></pre>

<p>The parent tag is index. All other information is encapsulated inside it.
<ol>
    <li>
<pre><strong>&lt;indexLocation&gt;C:\index&lt;/indexLocation&gt;</strong></pre>
indexLocation tag points to the directory on your disk where the index will be created after the goal:createIndex is executed.</li>
    <li>
<pre><strong>&lt;sourceFileLocation&gt;C:\file\data.txt&lt;/sourceFileLocation&gt;</strong></pre>
sourceFileLocation tag points to the file on your disk which will be indexed.</li>
    <li>
<pre><strong>&lt;overWrite&gt;true&lt;/overWrite&gt;</strong></pre>
overWrite tag specifies whether the existing index present at the indexLocation (if any) should be deleted or should be overWritten</li>
    <li>
<pre><strong>&lt;separator&gt;;&lt;/separator&gt;</strong></pre>
separator tag specifies the delimiter between the field names and the data to be indexed in it. The default separator is ; (Please refer to Sample File Source Format described in the next section for more detailed information).</li>
    <li>
<pre><strong>&lt;field&gt;
 &lt;name&gt;names&lt;/name&gt;
 &lt;type&gt;string&lt;/type&gt;
 &lt;analyzer&gt;WhiteSpaceAnalyzer&lt;/analyzer&gt;
 &lt;store&gt;true&lt;/store&gt;
&lt;/field&gt;</strong></pre>
</li>
</ol>
A &lt;field&gt; specifies a Lucene Field. &lt;index/&gt; can have one or many &lt;field/&gt;.</p>
<p>A &lt;field&gt; should contain the following tags :</p>
<p>Following analyzers are supported by maven-lucene-plugin in release 1.0. The default is the WhitespaceAnalyzer.
<ul>
    <li><strong> "WhitespaceAnalyzer"</strong></li>
    <li><strong> "SimpleAnalyzer"</strong></li>
    <li><strong> "KeyWordAnalyzer"</strong></li>
    <li><strong> "StopAnalyzer"</strong></li>
    <li><strong> "StandardAnalyzer"</strong></li>
</ul>
6. &lt;store&gt; specifies whether this field will be stored in the index or not.
<h3><span style="text-decoration: underline;">Source File Format</span></h3>
This is an example of file source ( file containing data to be indexed). The data in the file source should be written in the format explained below.
<ul>
    <li> The top row represents the field names separated by the separator.</li>
    <li>The separator is the defined in lucene.xml.</li>
    <li>The default separator is “;”.</li>
    <li>The rows other than the top row contains values for the field.</li>
    <li>The field name should be same as that specified in the &lt;field&gt;&lt;name&gt;fieldName&lt;/name&gt;&lt;/field&gt; tag in lucene.xml.</li>
</ul>
The source file can be seen as a set of columns with the element at the top as the field name. Technically, each row other than the top row (name of fields) is considered as a lucene document.</p>
<p>For eg. in the example file data shown below, names and hobby are the field names.</p>
<p><address> “names” field has values “albert einstein” and “Leonardo da Vinci”.</address> <address> “hobby” field has values “invention and discovery” and “Writing Painting”</address>Example File
<pre><strong>names;hobby
albert einstein;invention and discovery
Leonardo da Vinci;Writing Painting</strong></pre>
<h2><span style="text-decoration: underline;">Maven Lucene Search</span></h2>
<p style="text-align: justify;">maven-lucene-search is a dependency which can be used to search lucene indexes without doing any Lucene specific coding.</p>
It can be added by using dependency
<pre><strong>&lt;dependency&gt;
 &lt;groupId&gt;com.xebia&lt;/groupId&gt;
 &lt;artifactId&gt;maven-lucene-search&lt;/artifactId&gt;
 &lt;version&gt;1.0&lt;/version&gt;
&lt;/dependency&gt;</strong></pre></p>

## Comments

**[Abhinav Kumar](#5327 "2011-03-01 08:02:11"):** Great. Well Done.

