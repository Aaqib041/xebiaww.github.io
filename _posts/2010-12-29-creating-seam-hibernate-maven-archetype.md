---
layout: post
header-img: img/default-blog-pic.jpg
author: Rajdeep
description: 
post_id: 6531
created: 2010/12/29 16:23:58
created_gmt: 2010/12/29 11:23:58
comment_status: open
---

# Creating Seam-Hibernate Maven archetype

<p>It all started when me and <a href="http://xebee.xebia.in/author/vaibhav sehgal/">Vaibhav</a> were getting our feet wet in a new project. The project was based on <a href="http://www.seamframework.org">Seam</a> and <a href="http://www.hibernate.org">Hibernate</a>. While trying to create sample projects involving Seam and Hibernate, we found most of the constructs were using <a href="http://ant.apache.org/">Ant</a> as build tool, thus limiting stuff like reporting, standard build lifecycle. Further they used seam-gen to generate startup projects. Also, available maven archetypes used EJB's (not suited to our purpose). So we thought of creating our own maven archetype based on Seam and Hibernate.</p>
<!--more-->

<p>We went through the following steps to create the archetype:
<ul>
    <li>First we created the structure of the project that we wanted to be generated from the archetype.</li>
</ul>
<ul>
    <li>We then used the command  <strong>mvn archetype:create-from-project</strong> (in root directory). An important file to be considered after this step is <strong>archetype-metadata.xml</strong> (found in target/generated-sources/archetype/src/main/resources/META-INF/maven) which contains meta information about the directories and files that will be included in the archetype. We can further enrich this file by marking required properties, giving them default values or even creating new properties. For example
[sourcecode language="xml"]
      &lt;requiredProperties&gt;
        &lt;requiredProperty key=&quot;groupId&quot;&gt;
          &lt;defaultValue&gt;com.xebia&lt;/defaultValue&gt;
        &lt;/requiredProperty&gt;
      &lt;/requiredProperties&gt;
[/sourcecode]</li>
    <li>We now ran <strong>mvn install </strong>(after navigating to target/generated-sources/archetype) to finally generate the required archetype.</li>
</ul>
This method allowed us to use an IDE to generate the startup project and automatically convert it into the the archetype by running few commands.</p>
<p>The archetype uses seam, hibernate to provide the core functionality and spring,  dbunit are used in test cases. The project generated from archetype has user registration, login-logout functionality implemented.
The structure of our project was :</p>
<p>[sourcecode language="xml"]</p>
<p>|--pom.xml
<code>-- src
|-- main
|   |-- java
|   |</code>-- com
|   |       <code>-- xebia
|   |</code>-- seam
|   |               |-- components
|   |               |   |-- Login.java
|   |               |   <code>-- Register.java
|   |</code>-- domain
|   |                   <code>-- User.java
|   |-- resources
|   |   |-- hibernate.cfg.xml
|   |   |-- hibernate.properties
|   |</code>-- seam.properties
|   <code>-- webapp
|       |-- home.xhtml
|       |-- index.jsp
|       |-- login.xhtml
|       |-- META-INF
|       |</code>-- MANIFEST.MF
|       |-- register.xhtml
|       |-- template.xhtml
|       <code>-- WEB-INF
|           |-- components.xml
|           |-- faces-config.xml
|           |-- lib
|           |-- pages.xml
|</code>-- web.xml
<code>-- test
|-- java
|</code>-- com
|       <code>-- xebia
|</code>-- seam
|               |-- components
|               |   <code>-- LoginTest.java
|</code>-- databaseSetup
|                   <code>-- IDataMethods.java</code>-- resources
|-- dataset.xml
`-- hibernate-test.cfg.xml
[/sourcecode]</p>
<p>It was fun learning how maven can be used to do stuff that cannot be done with the help of other build tools which are pretty limited in the build scope.
The archetype source is <a href="http://xebee.xebia.in/wp-content/uploads/2010/12/archetype.zip">here</a> , just unzip it and run mvn install to install the archetype in local repository.Then use <strong>mvn</strong> <strong>archetype:generate -DarchetypeCatalog=local</strong> to generate new project. We plan to release it to the maven repositories soon. Watch this blog for more updates.</p>