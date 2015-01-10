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

**Why another blog on Service builder ?** Since you have landed on this post, we know that you are looking for a good reference for Liferay Service Builder, and by now you might be frustrated by the minimal documentation from Liferay, or are enervated by scrolling down the liferay forums, or you have been hopping from one blog to other since long. There is not even a single sufficient article/blog which explains service builder in detail. I and my colleague [Pratik][1], faced the same problem some time ago, and we then decided to write a series of blogs which will help you in getting a good understanding of Service Builder.

We are using Liferay 5.2.3 for developing a web based collaboration portal with a very huge user base. Portal requires development of many custom portlets, which means handling CRUDs around old and new entities. We then started searching for some tools which could increase our pace of development of portlets and we came across Liferay Service Builder. This tool has great power but its effective usage requires good level of understanding. So here is the series of upcoming blogs which will introduce the liferay service builder concepts. We will start with "Service Builder Basics" and then dive deeper into advanced topics. Here is a snapshot of different spaces. 

  1. Service Builder Basics
  2. Service Builder Finders
  3. Advantages , Disadvantages , Tips and Tricks
  4. Remote Services
  5. Caching
  6. Transactions
  7. Testing
**What is service builder ?** Liferay provides a code generation utility called Service Builder, created with an intention to get rid of most boilerplate work during portlet development. Liferay internally uses Spring and Hibernate, which basically means if you are not using service builder, you will end up writing the service layers, pojo’s , dao’s , sql scripts and configuration files if not using annotations. Service Builder does all this with little instructions. To get a good hang of service builder lets walk through an example and run though the various stages starting with concept of an entity and ending with the deployment of that entity inside a portlet with suitable views.

**Email Example ** Lets say we need an entity of “Email” which has attributes such as from , to , cc, body, date and text.

**Configuration** Create a service.xml file in your portlet as location - "portlet/docroot/WEB-INF/service.xml".  service.xml should stick to [this][2] schema definitions. Add entry for Email entity in service.xml.

[sourcecode language="xml"] <entity name="Email" local-service="true" remote-service="false" table="Email"> <column name="id" type="long" primary="true"> <column name="to" type="String"> <column name="from" type="String"> <column name="subject" type="String"> <column name="content" type="String"> <column name="date" type="Date">
    
    
     &lt;!--  Finders --&gt;
     &lt;finder name=&quot;To&quot; return-type=&quot;Collection&quot;&gt;
         &lt;finder-column name=&quot;to&quot;&gt;&lt;/finder-column&gt;
          &lt;/finder&gt;
     &lt;finder name=&quot;From&quot; return-type=&quot;Collection&quot;&gt;
         &lt;finder-column name=&quot;from&quot;&gt;&lt;/finder-column&gt;
         &lt;/finder&gt;
     &lt;finder name=&quot;Subject&quot; return-type=&quot;Collection&quot;&gt;
         &lt;finder-column name=&quot;subject&quot;&gt;&lt;/finder-column&gt;
         &lt;/finder&gt;
    &lt;finder name=&quot;Date&quot; return-type=&quot;Collection&quot;&gt;
         &lt;finder-column name=&quot;date&quot;&gt;&lt;/finder-column&gt;
         &lt;/finder&gt;
    

</entity> [/sourcecode] 

  * entity refers to a new model which we want to create
  * name attribute refers to the name of the entity
  * column tag represents the different fields in entity or columns in table
  * type refers to the data-type . Please note you cannot establish one-to-many or many-to-many relationships using this.
  * finder tag is used to create find utility methods
**Code Generation** Run "ant build-service" from the command (will work only if liferay bin is included in your path). Below is the explanation of all the classes that will be generated. Liferay strictly follows program to interface design so every impl will have its corresponding interface.

**Generated classes.** Service Builder generates three types of classes i.e. Model, Service and Persistence

** **

**Model Objects**

These objects are nothing but the data/value objects.

![][3]Following is the list of model classes generated when you run service builder 

  * **BaseModel** \- This is the Base class provided by liferay which is extended by every model/entity object. This is analogous to object class in Java.** **
  * **BaseModelImpl **\- Basic implementation provided by liferay for BaseModel.** **
  * **EmailModel **\- Interface for the getters/setters of all the fields that are specified in the service.xml for this entity.** **
  * **Email **\- This is interface for Email entity.** **
  * **EmailModelImpl** \- Implementation for the EmailModel methods and couple of other utility methods like clone , toString etc.
  * **EmailImpl** \- By Default this class is empty. The purpose of this class is to provide a placeholder to have custom methods on the entity (which are not generated by default) . For instance, if you want to make this entity rich by having  methods like validate “to” field , you can add method something like this [sourcecode language="java"] … <pre>public boolean validateToAddress(){ //Add code to validate this.to } … [/sourcecode]

Now once you add this method in the Impl , you need to have this inside a corresponding interface also which is Email. To add this method signature just run the "build-service" target again and method will be added to EmailInterface. Please note that the method cannot be added inside the EmailModelImpl because in case if we change the entity in future or just run the ant build-service, those custom written methods will get deleted.
** **

**Service Objects**

** ** Once you have you model objects, you need services to operate on them, and service builders generates these as well 

![][4] Following are the classes generated by service builder for service layer

  * **EmailLocalService **: Interface containing all the crud operations that can be performed on the Email entity. for e.g add/delete/get/update/find** **

   [1]: http://xebee.xebia.in/author/pgarg/
   [2]: http://www.liferay.com/dtd/liferay-service-builder_5_2_0.dtd%20
   [3]: http://xebee.xebia.in/wp-content/uploads/2012/02/email_model_1.png (email_model_1)
   [4]: http://xebee.xebia.in/wp-content/uploads/2012/02/email_service.png (email_service)

## Comments

**[Aarti Grover](#7590 "2012-02-17 08:38:25"):** Awesome post.. Well done Robin and Pratik !! waiting for rest of the post in the series

**[Muzakir Khan](#9149 "2012-07-11 16:43:01"):** Wowlicious job Dude.. Keep it Up!..

**[Varun](#8499 "2012-04-19 15:24:02"):** i am waiting for next article please include explanation of one to one mapping between entities specified in same service.xml.

**[Muddassar](#7999 "2012-03-23 01:44:06"):** Excellent article on Service Builders

**[sukdev garai](#8029 "2012-03-24 20:18:19"):** Its a nice and detail

**[Stepa](#7786 "2012-02-26 20:05:33"):** Great work. Can't wait for the next post!

**[Dimitris](#9244 "2012-07-27 02:31:15"):** I'm trying to implement inheritance using the Service Builder. Up to now i'm stack with no answers or clues. I've found articles regarding relationships only. Please Robin, can you give me some insights how to do that? Your article is great! Thank you

