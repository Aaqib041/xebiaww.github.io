---
layout: post
header-img: img/default-blog-pic.jpg
author: rnagpal
description: 
post_id: 11999
created: 2012/02/16 22:52:51
created_gmt: 2012/02/16 17:52:51
comment_status: open
---

# Liferay Service Builder

<p><strong>Why another blog on Service builder ?</strong>
Since you have landed on this post, we know that you are looking for a good reference for Liferay Service Builder, and by now you might be frustrated by the minimal documentation from Liferay, or are enervated by scrolling down the liferay forums, or you have been hopping from one  blog to other since long. There is not even a single sufficient article/blog  which explains service builder in detail. I and my colleague <a href="http://xebee.xebia.in/author/pgarg/">Pratik</a>, faced the same problem some time ago, and we then decided to write a series of blogs which will help you in getting a good understanding of Service Builder.</p>
<!--more-->

<p>We are using Liferay 5.2.3 for developing a web based collaboration portal with a very huge user base. Portal requires development of many custom portlets, which means handling CRUDs around old and new entities. We then started searching for some tools which could increase our pace of development of portlets and we came across Liferay Service Builder. This tool has great power but its effective usage requires good level of understanding. So here is the series of upcoming blogs which will introduce the liferay service builder concepts. We will start with "Service Builder Basics" and then dive deeper into advanced topics.  Here is a snapshot of different spaces.
<ol>
    <li>Service Builder Basics</li>
    <li>Service Builder Finders</li>
    <li>Advantages , Disadvantages , Tips and Tricks</li>
    <li>Remote Services</li>
    <li>Caching</li>
    <li>Transactions</li>
    <li>Testing</li>
</ol>
<strong>What is service builder ?</strong>
Liferay provides a code generation utility called Service Builder, created with an intention to get rid of most boilerplate work during portlet development. Liferay internally uses Spring and Hibernate, which basically means if you are not using service builder, you will end up writing the service layers, pojo’s , dao’s , sql scripts and configuration files if not using annotations. Service Builder does all this with little instructions. To get a good hang of service builder lets walk through an example and run though the various stages starting with concept of an entity and ending with the deployment of that entity inside a portlet with suitable views.</p>
<p><span style="font-size: large;"><strong>Email Example </strong></span>
Lets say we need an entity of “Email” which has attributes such as  from , to , cc, body, date and text.</p>
<p><span style="font-size: large;"><strong>Configuration</strong></span>
Create a service.xml file in your portlet as location - "portlet/docroot/WEB-INF/service.xml".  service.xml should stick to <a href="http://www.liferay.com/dtd/liferay-service-builder_5_2_0.dtd%20">this</a> schema definitions. Add entry for Email entity in service.xml.</p>
<p>[sourcecode language="xml"]
&lt;entity name=&quot;Email&quot; local-service=&quot;true&quot; remote-service=&quot;false&quot; table=&quot;Email&quot;&gt;
     &lt;column name=&quot;id&quot; type=&quot;long&quot; primary=&quot;true&quot;&gt;
     &lt;column name=&quot;to&quot; type=&quot;String&quot;&gt;
     &lt;column name=&quot;from&quot; type=&quot;String&quot;&gt;
     &lt;column name=&quot;subject&quot; type=&quot;String&quot;&gt;
     &lt;column name=&quot;content&quot; type=&quot;String&quot;&gt;
     &lt;column name=&quot;date&quot; type=&quot;Date&quot;&gt;</p>
<pre><code> &amp;lt;!--  Finders --&amp;gt;
 &amp;lt;finder name=&amp;quot;To&amp;quot; return-type=&amp;quot;Collection&amp;quot;&amp;gt;
     &amp;lt;finder-column name=&amp;quot;to&amp;quot;&amp;gt;&amp;lt;/finder-column&amp;gt;
      &amp;lt;/finder&amp;gt;
 &amp;lt;finder name=&amp;quot;From&amp;quot; return-type=&amp;quot;Collection&amp;quot;&amp;gt;
     &amp;lt;finder-column name=&amp;quot;from&amp;quot;&amp;gt;&amp;lt;/finder-column&amp;gt;
     &amp;lt;/finder&amp;gt;
 &amp;lt;finder name=&amp;quot;Subject&amp;quot; return-type=&amp;quot;Collection&amp;quot;&amp;gt;
     &amp;lt;finder-column name=&amp;quot;subject&amp;quot;&amp;gt;&amp;lt;/finder-column&amp;gt;
     &amp;lt;/finder&amp;gt;
&amp;lt;finder name=&amp;quot;Date&amp;quot; return-type=&amp;quot;Collection&amp;quot;&amp;gt;
     &amp;lt;finder-column name=&amp;quot;date&amp;quot;&amp;gt;&amp;lt;/finder-column&amp;gt;
     &amp;lt;/finder&amp;gt;
</code></pre>
<p>&lt;/entity&gt;
[/sourcecode]
<ul>
    <li>entity refers to a new model which we want to create</li>
    <li>name attribute refers to the name of the entity</li>
    <li>column tag represents the different fields in entity or columns in table</li>
    <li>type refers to the data-type . Please note you cannot establish one-to-many or many-to-many relationships using this.</li>
    <li>finder tag is used to create find utility methods</li>
</ul>
<span style="font-size: large;"><strong>Code Generation</strong></span>
Run "ant build-service" from the command (will work only if liferay bin is included in your path). Below is the explanation of all the classes that will be generated. Liferay strictly follows program to interface design so every impl will have its corresponding interface.</p>
<p><span style="font-size: large;"><strong>Generated classes.</strong></span>
Service Builder generates three types of classes i.e. Model, Service and Persistence</p>
<p><span style="font-size: medium;"><strong> </strong></span></p>
<p><span style="font-size: medium;"><strong>Model Objects</strong></span></p>
<p>These objects are nothing but the data/value objects.</p>
<p><a href="http://xebee.xebia.in/2012/02/16/liferay-service-builder/email_model_1/" rel="attachment wp-att-12036"><img src="http://xebee.xebia.in/wp-content/uploads/2012/02/email_model_1.png" title="email_model_1" class="aligncenter size-full wp-image-12036" width="650" height="400" /></a>Following is the list of model classes generated when you run service builder
<ul>
    <li><strong>BaseModel</strong> - This is the Base class provided by liferay which is extended by every model/entity object. This is analogous to object class in Java.<strong> </strong></li>
    <li><strong>BaseModelImpl </strong>- Basic implementation provided by liferay for BaseModel.<strong> </strong></li>
    <li><strong>EmailModel </strong>- Interface for the getters/setters of all the fields that are specified in the service.xml for this entity.<strong> </strong></li>
    <li><strong>Email </strong>- This is interface for Email entity.<strong> </strong></li>
    <li><strong>EmailModelImpl</strong> - Implementation for the EmailModel methods and couple of other utility methods like clone , toString etc.</li>
    <li><strong>EmailImpl</strong> - By Default this class is empty. The purpose of this class is to provide a placeholder to have custom methods on the entity (which are not generated by default) . For instance, if you want to make this entity rich by having  methods like validate “to” field , you can add method something like this [sourcecode language="java"] …
&lt;pre&gt;public boolean validateToAddress(){
//Add code to validate this.to
}
…
[/sourcecode]</p>
<p>Now once you add this method in the Impl , you need to have this inside a corresponding interface also which is Email. To add this method signature just run the "build-service" target again and method will be added to EmailInterface. Please note that the method cannot be added inside the EmailModelImpl because in case if we change the entity in future or just run the ant build-service, those custom written methods will get deleted.</li>
</ul>
<span style="font-size: medium;"><strong> </strong></span></p>
<p><span style="font-size: medium;"><strong>Service Objects</strong></span></p>
<p><span style="font-size: medium;"><strong> </strong></span> Once you have you model objects, you need services to operate on them, and service builders generates these as well
<div width="100%"><a href="http://xebee.xebia.in/2012/02/16/liferay-service-builder/email_service/" rel="attachment wp-att-12018"><img src="http://xebee.xebia.in/wp-content/uploads/2012/02/email_service.png" title="email_service" class="aligncenter size-full wp-image-12018" width="650" height="283" /></a>
Following are the classes generated by service builder for service layer</div>
<ul>
    <li><strong>EmailLocalService </strong>:  Interface containing all the crud operations that can be performed on the Email entity. for e.g add/delete/get/update/find<strong> </strong></li></p>

## Comments

**[Aarti Grover](#7590 "2012-02-17 08:38:25"):** Awesome post.. Well done Robin and Pratik !! waiting for rest of the post in the series

**[Muzakir Khan](#9149 "2012-07-11 16:43:01"):** Wowlicious job Dude.. Keep it Up!..

**[Varun](#8499 "2012-04-19 15:24:02"):** i am waiting for next article please include explanation of one to one mapping between entities specified in same service.xml.

**[Muddassar](#7999 "2012-03-23 01:44:06"):** Excellent article on Service Builders

**[sukdev garai](#8029 "2012-03-24 20:18:19"):** Its a nice and detail

**[Stepa](#7786 "2012-02-26 20:05:33"):** Great work. Can't wait for the next post!

**[Dimitris](#9244 "2012-07-27 02:31:15"):** I'm trying to implement inheritance using the Service Builder. Up to now i'm stack with no answers or clues. I've found articles regarding relationships only. Please Robin, can you give me some insights how to do that? Your article is great! Thank you

