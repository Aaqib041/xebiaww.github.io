---
layout: post
header-img: img/default-blog-pic.jpg
author: vgupta
description: 
post_id: 2716
created: 2009/12/08 21:43:37
created_gmt: 2009/12/08 16:43:37
comment_status: open
---

# Domain Driven Design : Expressing Concepts

[Object oriented analysis and design(OOAD)][1] is essentially an activity to model a system as an interaction between objects. Each object, which is an instance of a class and has state and behaviour, represents an entity with in the system. These entities invoke other entities to perform certain activities and are often associated with each other to model a system. **Domain driven design(DDD)** recommends guidelines which tend to optimize/enhance the process of identifying entities and defining relationship between them. It advocates to classify objects as **Entities** and **Value objects** based on certain criteria, to model **Services,** which abstract behaviour which is not specific to a particular entity, to divide application into **Modules**, and to minimize and constrain **Associations** between objects. In this blog, I will discuss these recommendations.  **Associations**

**![association][2] **

An association is basically a relationship between two entities. There can be a variety of relationships between objects. **(Unified Modelling Language)UML **specifies a notation for many of them. A succinct list of UML associations can be referred from [here][3]. DDD does not add any specific relationship of it's own but instead suggests to optimize them by 

  * Imposing a traversal direction
  * Adding a qualifier to reduce multiplicity
  * Eliminating non-essential associations
An order is placed for a customer, and over a period of time, a customer can have many orders. So, there is a bi-directional one-to-many relationship between a customer and an order. But, a deeper insight might reveal that most of the times we need to know the customer to whom a particular order belongs. But, only in some cases, we need to find out the orders of a customer. So, for simplicity, this relationship can be modelled as a uni-directional one-to-many relationship between a customer and an order and the traversal direction from order to customer. If we need the ability to find orders by customer, this can be done through a database query.

A brokerage account can have many investments. So, there is a one to many relationship between a brokerage account and it's investments. But most investment are against a stock. So, a one-to-many relationship can be reduced to a one-to-one relationship by adding a qualifier, which in this case would be a stock symbol. This is shown in the following code segment

[sourcecode language="java"] public class BrokerageAccount { Set<Investment> Investments; public Set getInvestments() { return investments; } } [/sourcecode]

The above code can be changed to

[sourcecode language="java"] public class BrokerageAccount { Map<String stockSymbol, Investment> investments; public Set getInvestment(String stockSymbol) { return (Investment) investments.get(stockSymbol); } } [/sourcecode]

The fact that an investment is against a stock symbol is made more explicit by adding a qualifier which in this case is a stock symbol. ** Entities**

**![identity][4]** An entity is something which has an identity and is clearly distinguishable from other entities of same or other type. It can be a person, place, ticket, or anything else which can be distinguished from other entities. The identity of an entity can either be a computer generated number or it can be a combination of the attributes.

**Value Objects**

Values object are something which represent concepts but does not have an identity of their own. When you care only about the attributes of an object, classify it as value objects. Value objects can 

  * be an assembly of other objects: For example, in a building plan, a wall consists of window. Both these objects does not have an identity of their own because it may not be required to identify them.
  * refer to entities: For example, in an online map service, a route object consists of source and destination city, both of which may be an entity.
![voexpr][5] Values object makes domain model expressive. For example, the fact that Customer has an Address is made more explicit by modelling Address an a value object.

It is recommended to model VOs as immutable objects. This makes them thread safe and easily shareable. Making a value object immutable also open the doors for some optimization. We can use patterns like [Flyweight][6], which replaces large number of unshared objects with smaller number of shareable objects.

Making a VO immutable seems to be an obvious choice. However, if the value of the VO changes frequently and the creation of a new VO is expensive, then making it mutable seems to be a better choice. Despite all this, always design a VO as immutable and, if required, make it mutable.

**Services**

**![services][7]**

In some** **cases, a particular behaviour does not belong to a particular entity or value object. So, instead of bloating an object with the behaviour it does not intends to do, it is better to model it as a Service. A service provides a behaviour through an interface. The behaviour can be an action, or an activity. A service should not have a state of it's own so that it can be shared easily and it's behaviour can be reused easily.

A service can be classified as an **application** service which is involved with services like transactions, security, logging, etc., and a **domain** service which encapsulate behaviour not specific to any object. Although, application services provide very important services to an application, but in DDD, domain services are primary concerns while modelling behaviour.

**Modules**

Modules is a less talked about topic of DDD because the reasons to divide an application into modules are fairly obvious and we have been doing that for a long time. I will reiterate those reasons here once again 

  * Modules help in breaking down the complexity of an application.
  * Modules acts as a communication aid because when we discuss a functionality by associating it with modules, the discussion context is narrowed down.
  * Modules facilitate parallel development.
**Conclusion**

In this blog, we discussed various elements of DDD which makes domain modelling expressive. Constrained associations reduces the complexity of the domain models, whereas adding directions to relationships makes it more expressive. The idea of having immutable value objects seems to add expression to models by making concepts more expressive. Services take the load off the entities, and since they are interface based, we can change the implementation without affecting the domain object. Modules break the story of domain model into chapters. Applying these concepts judiciously will surely help in expressing a deep insight into the domain of the software.

   [1]: http://en.wikipedia.org/wiki/Object-oriented_analysis_and_design
   [2]: http://xebee.xebia.in/wp-content/uploads/2009/12/association.png (association)
   [3]: http://www.shrinkrays.net/articles/uml-cheat-sheet.aspx
   [4]: http://xebee.xebia.in/wp-content/uploads/2009/12/identity.png (identity)
   [5]: http://xebee.xebia.in/wp-content/uploads/2009/12/voexpr.png (voexpr)
   [6]: http://en.wikipedia.org/wiki/Flyweight_pattern
   [7]: http://xebee.xebia.in/wp-content/uploads/2009/12/services.png (services)