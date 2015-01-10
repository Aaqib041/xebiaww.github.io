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

## **Maven Lucene Plugin**

Maven Lucene Plugin is an open source maven plugin for Apache Lucene developed by[ Xebia India IT Architects Ltd][1] . The project is hosted on SourceForge and can be found [here][2]. It is released under GNU General Public License (GPL).

The artifact is uploaded at Central Maven Repository <http://repo1.maven.org/maven2/com/xebia/>. You can browse the artifact [here][3].

**maven-lucene-plugin** creates a Lucene index from a file source. The structure of the lucene index i.e. fields, analyzers, indexLocation, fileSourceLocation, store etc can be configured in a configuration file lucene.xml.

**lucene.xml** contains all information regarding the lucene index and the data source (from which the index is created). The Maven Lucene Plugin looks for lucene.xml file in the project root directory (adjacent to pom.xml) and creates the lucene index from the file source mentioned in lucene.xml based on the index configuration provided.

Apache Lucene Version

The maven-lucene-plugin uses Lucene version 3.0.3. The index can be opened using Luke 1.0.1. Luke can be downloaded from [here][4].

### **Configuring plugin in pom.xml**

Configuring the **maven-lucene-plugin** in **pom.xml** is very simple. All information about index configuration needs to be written in **lucene.xml**. Index can be created by running “**createIndex**” goal in the plugin. It can be done by running the goal during any phase as shown in the example plugin configuration. OR it can be called by executing “**createIndex**” goal from command line by “**mvn lucene:createIndex**”.

The “**createIndex**” goal looks for a file “**lucene.xml**” in the project root directory (adjacent to pom.xml). The **lucene.xml** file needs to have all the information related to the configuration of index. 

#### Example pom.xml is shown below:
    
    
    **<build>
     <plugins>
      <plugin>
        <groupId>com.xebia</groupId>
        <artifactId>maven-lucene-plugin</artifactId>
        <version>1.0</version>
        <executions>
         <execution>
          <phase>install</phase>
          <goals>
           <goal>createIndex</goal>
          </goals>
         </execution>
        </executions>
      </plugin>
     </plugins>
    </build>**

### Example lucene.xml

**maven-lucene-plugin** looks for a file **lucene.xml** in the project root directory (adjacent to pom.xml). The **maven-lucene-plugin** will not work without **lucene.xml**. lucene.xml contains all the information about the lucene index.

#### Example lucene.xml is provided below and each tag is explained in detail after that:
    
    
    **<index>
      <indexLocation>C:\\index</indexLocation>
      <sourceFileLocation>C:\\file\\data.txt</sourceFileLocation>
      <overWrite>true</overWrite>
      <separator>;</separator>
      <field>
        <name>names</name>
        <type>string</type>
        <analyzer>WhiteSpaceAnalyzer</analyzer>
        <store>true</store>
      </field>
      <field>
        <name>hobby</name>
        <type>string</type>
        <analyzer>SimpleAnalyzer</analyzer>
        <store>true</store>
      </field>
    </index>**

The parent tag is index. All other information is encapsulated inside it. 

  1.     **<indexLocation>C:\index</indexLocation>**

indexLocation tag points to the directory on your disk where the index will be created after the goal:createIndex is executed.
  2.     **<sourceFileLocation>C:\file\data.txt</sourceFileLocation>**

sourceFileLocation tag points to the file on your disk which will be indexed.
  3.     **<overWrite>true</overWrite>**

overWrite tag specifies whether the existing index present at the indexLocation (if any) should be deleted or should be overWritten
  4.     **<separator>;</separator>**

separator tag specifies the delimiter between the field names and the data to be indexed in it. The default separator is ; (Please refer to Sample File Source Format described in the next section for more detailed information).
  5.     **<field>
     <name>names</name>
     <type>string</type>
     <analyzer>WhiteSpaceAnalyzer</analyzer>
     <store>true</store>
    </field>**

A <field> specifies a Lucene Field. <index/> can have one or many <field/>.

A <field> should contain the following tags :

Following analyzers are supported by maven-lucene-plugin in release 1.0. The default is the WhitespaceAnalyzer. 

  * ** "WhitespaceAnalyzer"**
  * ** "SimpleAnalyzer"**
  * ** "KeyWordAnalyzer"**
  * ** "StopAnalyzer"**
  * ** "StandardAnalyzer"**
6\. <store> specifies whether this field will be stored in the index or not. 

### Source File Format

This is an example of file source ( file containing data to be indexed). The data in the file source should be written in the format explained below. 

  * The top row represents the field names separated by the separator.
  * The separator is the defined in lucene.xml.
  * The default separator is “;”.
  * The rows other than the top row contains values for the field.
  * The field name should be same as that specified in the <field><name>fieldName</name></field> tag in lucene.xml.
The source file can be seen as a set of columns with the element at the top as the field name. Technically, each row other than the top row (name of fields) is considered as a lucene document.

For eg. in the example file data shown below, names and hobby are the field names.

“names” field has values “albert einstein” and “Leonardo da Vinci”. “hobby” field has values “invention and discovery” and “Writing Painting”Example File 
    
    
    **names;hobby
    albert einstein;invention and discovery
    Leonardo da Vinci;Writing Painting**

## Maven Lucene Search

maven-lucene-search is a dependency which can be used to search lucene indexes without doing any Lucene specific coding.

It can be added by using dependency 
    
    
    **<dependency>
     <groupId>com.xebia</groupId>
     <artifactId>maven-lucene-search</artifactId>
     <version>1.0</version>
    </dependency>**

   [1]: http://www.xebiaindia.com/ (Xebia India)
   [2]: https://sourceforge.net/projects/lucene-plugin/ (Maven Lucene Plugin)
   [3]: https://oss.sonatype.org/index.html#nexus-search;quick~maven-lucene-plugin (OSS Sonatype Repository)
   [4]: http://luke.googlecode.com/files/lukeall-1.0.1.jar (Download Luke)

## Comments

**[Abhinav Kumar](#5327 "2011-03-01 08:02:11"):** Great. Well Done.

