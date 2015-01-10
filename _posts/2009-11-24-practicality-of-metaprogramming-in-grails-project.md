---
layout: post
header-img: img/default-blog-pic.jpg
author: sunil.inteti
description: 
post_id: 2075
created: 2009/11/24 18:49:52
created_gmt: 2009/11/24 13:49:52
comment_status: open
---

# Practicality of MetaProgramming in Grails Project

Grails is a powerful technology to build web applications very rapidly. Groovy provides so many enhancements to Java objects. [Metaprogramming][1] capability is one of them. Grails uses this [Metaprogramming capability of Groovy][2] heavily to accomplish things faster the way it does.

This blog discusses the the _Practicality of this Metaprogramming in the Grails projects_ based on my experiences.

Let me explain briefly how we can achieve Metaprogramming in Groovy in a brief way.

Every method invocation and property access in Groovy is done through **[metaclass**][3]. This metaclass contains the properties and methods of the class. Using this metaClass we can add,ovverirde methods to an object and thereby change object behaviour at runtime. For example :

we can add a method to an object using  its metaClass and a [closure][4]

[sourcecode language="java"]class Module { String name } Module.metaClass.album = { //add the logic for this dynamic method album }[/sourcecode]

Some of the scenarios where we can use Metaprogramming in my Grails project(indirectly) are

**1) Using Dynamic methods for domain objects provided by GORM** [sourcecode language="java"]Module.findAllByNameLike("%Groovy%"); def module = new Module(name:'groovy',url:'someurl').save()[/sourcecode] In controllers and Services we can use these dynamic methods and it makes life very simple.

**2) Groovy has added the some methods to final classes like java,lang.Integer, java,lang.String.**

Groovy added closure methods to metaClass of these classes. [sourcecode language="java"]3.times { //put your business logic here }[/sourcecode] We can use this helpful methods of Groovy in our application. The readability is increased here and syntax is simple.

Consider a situation if we have to add these dynamic methods to business objects in our application.  Yes we can very well add these methods dynamically and do our business logic but it comes at huge cost.

I strongly feel that using these metaClass mechanism in our business logic has more bad consequesnces than good. It makes code impossible to read and understand (Groovy is already a bit difficult than Java )and difficult to manage later on. I think its better to have appropriate design of domain objects and services rather use this metaClass stuff at all for having dynamic method behavior for object. Practically i don't see any advatage to use metaClass to add dynamic methods in our application even if we have a use case because we keep changing our application code frequently unlike the Grovvy library class that we use. I also wonder what kind of applications heavily needs this change of behaviour of objects.

One area where I think we can use this metaClass mechanism is in Unit testing your Grails application. We can ovveride the existing method of dependant object using the metaclass and mock the behaviour of any object. (Only if you are bored with normal mocking mechanism and if you fancy using metaClass)

**Conclusion :**

Metaprogramming sounds so cool and you would be tempted to use it some how in your project(at least i was). I feel that we have to use this very judiciously so that we don't  make our codebase 'hardToUnderstand' and 'hardToManage' and 'hardToTest'. **If you(who used Grails in projects ) disagree with me, please feel free to put the counter argument and  usecase where you used the metaClass to add dynamic methods in your Grails application**

   [1]: http://en.wikipedia.org/wiki/Metaprogramming (Metaprogramming)
   [2]: http://groovy.codehaus.org/ExpandoMetaClass (Metaprogramming capability of Groovy)
   [3]: http://groovy.codehaus.org/api/groovy/lang/MetaClass.html
   [4]: http://groovy.codehaus.org/Closures