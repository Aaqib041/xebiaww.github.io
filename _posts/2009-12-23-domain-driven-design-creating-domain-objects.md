---
layout: post
header-img: img/default-blog-pic.jpg
author: vgupta
description: 
post_id: 2928
created: 2009/12/23 08:02:39
created_gmt: 2009/12/23 03:02:39
comment_status: open
---

# Domain Driven Design : Creating Domain Objects

_In one of the previous posts, I discussed about [Aggregates][1] and their design considerations. In this part of the series, I will discuss the issues in object creation with domain objects. _

The normal way of creating an object by it's client is via it's public constructor. This works well with a simple objects. But, when complex objects like Aggregates are concerned, their construction can be complex and the process of creation can become overwhelming for an object to handle. As part of it's construction, an aggregate might have to 

  * create local entities within an aggregate.
  * check the invariant logic before adding a local entity.
Object creation has nothing to do with the domain. Moreover, with complex object creation, an object tends break the [Single Responsibility Principle][2](SRP). So, it is recommended to separate the object creation logic from the object. In Domain Driven Design(DDD), a program element whose primarily responsibility is to create complex domain objects and aggregates is called a Factory. Ideally, a factory should create the product atomically with all the safety checks, that is, with the all the invariants applied appropriately.

The DDD Factory pattern is different from the Gof Factory patterns, in that, the Gof patterns are on a technical level, whereas, DDD factory pattern is more on semantic domain model level. In fact, a DDD factory might make use of Gof [creational patterns][3] to create the domain objects. From hereon, I will refer DDD factory as a factory and indicate appropriately whenever a Gof factory is referred.

Factories have several benefits over constructors, which are 

  * Factories can tell about the objects they are creating, while constructors cannot. Let's take an example, an Order object can be created by passing params or by using an existing object to create a repeat order. So, in the Order class, two constructors for can be created as [sourcecode language="java"] class Order{ public Order(params) {} public Order(Order repearOrder) {} } [/sourcecode]

where, if we use factory, we can make code more expressive, as shown below

[sourcecode language="java"] class OrderFactory{ public static Order createOrder(params) {} public static Order createRepeatOrder(Order order) {} } [/sourcecode]
  * Whenever we call a constructor, a new object is created, whereas, we can control object creation by using factory. We can create a pool of objects and we might not have to create new objects every time. This is more applicable in case of Value Objects factories since VOs are mostly immutable we can keep a pool of VOs and avoid new object creation every time.
  * Factories are polymorphic, in that, they can return the object or any subtype of the object being created, whereas, constructors do not provide such flexibility.
  * In cases where the object being created has a lot of optional parameters, we can have a Builder object as the Factory. [Builder][4] is one of Gof's creational pattern which is used when an object has to be created in steps or when there are large number of optional parameters. For example, If an object has 2 mandatory parameters and 8 optional parameters, then it is not advisable to use constructors as providing constructors for all the combinations would swamp the object with constructors.
In this blog, we discussed about the Factory pattern and it's advantages over constructors as a means for creating objects. In case of complex objects or aggregates, factories encapsulate object creation logic and make model more expressive. However, in case of simple objects which do not have a lot of optional parameters, it is advisable to avoid factories and stick to constructors.

_Factories deal with early life of a domain object, but a domain object has a longer life cycle. A lot of time, we have to resurrect a domain object from storage. This part can be a little different from Factory and that's why there is a separate pattern for it, which is know as Repository pattern. I will present that in the next part of the series._

Let's look at the

   [1]: http://xebee.xebia.in/2009/12/17/domain-driven-design-aggregates/
   [2]: http://en.wikipedia.org/wiki/Single_responsibility_principle
   [3]: http://en.wikipedia.org/wiki/Creational_pattern
   [4]: http://en.wikipedia.org/wiki/Builder_pattern