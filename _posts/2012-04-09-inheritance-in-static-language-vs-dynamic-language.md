---
layout: post
header-img: img/default-blog-pic.jpg
author: gagrawal
description: 
post_id: 13086
created: 2012/04/09 19:27:11
created_gmt: 2012/04/09 14:27:11
comment_status: open
---

# Inheritance in Static Language vs Dynamic Language

<p>Inheritance is generally meant to be used when a group of classes belong to same family and share common properties and methods apart from having their own specialized ones. However at times it is seen that classes are extended just to avoid writing boiler plate code, even though they might not necessarily belong  to same family. This might be fine at times in static languages like Java as there are less options available. However dynamic languages like Groovy offer better alternatives than forcing inheritance for such scenarios.<!--more--></p>
<p>Let’s elaborate more on this with an example of an e-commerce application.</p>
<p>Suppose in an e-commerce application, we consider following entities.
<ul>
    <li>User – Contains user details like username, address, email etc.</li>
    <li>CreditCard – For billing via credit card</li>
    <li>BankAccount – For billing via bank account</li>
</ul>
Now here, CreditCard and BankAccount represents Billing Details and will have some properties in common. E.g account number, owner etc. Also since both represent billing details of a user, they can be considered to belong to same family i.e. Billing Details. Over here, inheriting CreditCard and BankAccount entities from BillingDetails makes perfect sense as shown below.
<p style="text-align: center;"><a rel="attachment wp-att-13091" href="http://xebee.xebia.in/2012/04/09/inheritance-in-static-language-vs-dynamic-language/1-5/"><img src="http://xebee.xebia.in/wp-content/uploads/2012/04/1.png" title="Billing Details Class Diagram" class="size-full wp-image-13091 alignleft" /></a></p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
Now since instances of CreditCard and BankAccount will be saved in database, both of them will have a database identifier i.e. “id” field. Also there would also be certain operations in common like save(), delete() etc. So we can put all such properties and methods in BillingDetails so that they will be inherited by CreditCard and BankAccount. But wait a minute. What about User entity? Wouldn’t User have “id” attribute and “save”, “delete” methods too? If we just put them in BillingDetails, we will have to duplicate it in User entity as well. So what should be done?</p>
<p>A typical solution one might think of doing is, create a common class “BaseEnity” and extend all other entities in the system from it as shown below.</p>
<p><img height="348" width="423" src="http://xebee.xebia.in/wp-content/uploads/2012/04/21.png" title="Billing Details Class Diagram" class="size-full wp-image-13118 alignleft" />
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
<p style="text-align: center;">&nbsp;</p>
So now we have all the attributes and methods common to entire application in BaseEntity and every other entity will extend from BaseEntity. However in this design we have User and BillingDetails entities extending same class BaseEntity even though they don’t belong to same family. One might argue that User and BillingDetails both are entity(domain) classes and share common structure for database operations and hence can be considered belonging to same family. However this is more of technical similarity that they share.  From an e-commerce domain perspective they do not belong to same family and hence should not extend same class.</p>
<p>Apart from this, it is also possible that certain common properties are required in  completely different category of classes. E.g “log” property is generally required in all types of classes. From a simple web application perspective there could be 3 different layers of classes.
<ul>
    <li>Controllers</li>
    <li>Services</li>
    <li>Domains(Entities)</li>
</ul>
Now in order to make log property available in all types of classes, I am sure nobody would like to extend all these classes from a base class.</p>
<p>So what we want is a solution which can make our design cleaner and also avoid duplicating code. From the above mentioned example perspective, User and BillingDetails entities should have “id” attribute and save(), delete() methods available to them automatically, but without extending same class. Let’s see how this can be achieved via Dynamic Language.</p>
<p>Dynamic Languages provide a great way to extend a class definition by adding new fields or methods at compile time or run time. This feature can be a wonderful way of adding common properties and methods in classes instead of extending them from a base class. And let the inheritance be applied only when it makes sense.</p>
<p>Let us elaborate more on this with dynamic language “Groovy” and its web framework “Grails”.</p>
<p>Groovy allows to extend class definition at runtime via ExpandoMetaClass and at compile time via AST Transformations(Global or Local). On top of it, Grails is a web framework for Groovy which works on "Convention over Configuration" concept. There are mainly 3 layers in Grails i.e. controller, services and domains. As per this concept, Grails auto injects lots of utility fields and methods in various artefacts at compile time and run time. So common properties like “log” are available in classes across layers and layer specific properties are available in respective layer classes.  For instance all domain classes have various database operation methods e.g save(), delete() etc. and some required fields like id and version. All controller classes have access to bindData() method for binding request parameters to objects and so on.</p>
<p>Hence with Groovy it is possible to auto inject common properties and methods across a set of classes and use inheritance only when it makes sense. One should keep in mind this important feature of dynamic language and should make most out of it.</p>