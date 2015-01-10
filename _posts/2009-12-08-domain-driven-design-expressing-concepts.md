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

<p><a href="http://en.wikipedia.org/wiki/Object-oriented_analysis_and_design">Object oriented analysis and design(OOAD)</a> is essentially an activity to model a system as an interaction between objects. Each object, which is an instance of a class and has state and behaviour, represents an entity with in the system. These entities invoke other entities to perform certain activities and are often associated with each other to model a system. <strong>Domain driven design(DDD)</strong> recommends guidelines which tend to optimize/enhance the process of identifying entities and defining relationship between them. It advocates to classify objects as <strong>Entities</strong> and <strong>Value objects</strong> based on certain criteria, to model <strong>Services,</strong> which abstract behaviour which is not specific to a particular entity, to divide application into <strong>Modules</strong>, and to minimize and constrain <strong>Associations</strong> between objects. In this blog, I will discuss these recommendations.
<!--more--> 
<strong>Associations</strong></p>
<p><strong><img class="aligncenter size-full wp-image-2720" title="association" src="http://xebee.xebia.in/wp-content/uploads/2009/12/association.png" alt="association" width="254" height="150" />
</strong></p>
<p>An association is basically a relationship between two entities. There can be a variety of relationships between objects. <strong>(Unified Modelling Language)UML </strong>specifies a notation for many of them. A succinct list of UML associations can be referred from <a href="http://www.shrinkrays.net/articles/uml-cheat-sheet.aspx">here</a>. DDD does not add any specific relationship of it's own but instead suggests to optimize them by
<ul>
    <li>Imposing a traversal direction</li>
    <li>Adding a qualifier to reduce multiplicity</li>
    <li>Eliminating non-essential associations</li>
</ul>
An order is placed for a customer, and over a period of time, a customer can have many orders. So, there is a bi-directional one-to-many relationship between a customer and an order. But, a deeper insight might reveal that most of the times we need to know the customer to whom a particular order belongs. But, only in some cases, we need to find out the orders of a customer. So, for simplicity, this relationship can be modelled as a uni-directional one-to-many relationship between a customer and an order and the traversal direction from order to customer. If we need the ability to find orders by customer, this can be done through a database query.</p>
<p>A brokerage account can have many investments. So, there is a one to many relationship between a brokerage account and it's investments. But most investment are against a stock. So, a one-to-many relationship can be reduced to a one-to-one relationship by adding a qualifier, which in this case would be a stock symbol. This is shown in the following code segment</p>
<p>[sourcecode language="java"]
public class BrokerageAccount {
  Set&lt;Investment&gt; Investments;
  public Set getInvestments() {
    return investments;
  }
}
[/sourcecode]</p>
<p>The above code can be changed to</p>
<p>[sourcecode language="java"]
public class BrokerageAccount {
  Map&lt;String stockSymbol, Investment&gt; investments;
  public Set getInvestment(String stockSymbol) {
    return (Investment) investments.get(stockSymbol);
  }
}
[/sourcecode]</p>
<p>The fact that an investment is against a stock symbol is made more explicit by adding a qualifier which in this case is a stock symbol.
<strong>
Entities</strong></p>
<p><strong><img src="http://xebee.xebia.in/wp-content/uploads/2009/12/identity.png" alt="identity" title="identity" width="123" height="160" class="aligncenter size-full wp-image-2721" /></strong>
An entity is something which has an identity and is clearly distinguishable from other entities of same or other type. It can be a person, place, ticket, or anything else which can be distinguished from other entities. The identity of an entity can either be a computer generated number or it can be a combination of the attributes.</p>
<p><strong>Value Objects</strong></p>
<p>Values object are something which represent concepts but does not have an identity of their own. When you care only about the attributes of an object, classify it as value objects. Value objects can
<ul>
    <li>be an assembly of other objects: For example, in a building plan, a wall consists of window. Both these objects does not have an identity of their own because it may not be required to identify them.</li>
    <li>refer to entities: For example, in an online map service, a route object consists of source and destination city, both of which may be an entity.</li>
</ul>
<img src="http://xebee.xebia.in/wp-content/uploads/2009/12/voexpr.png" alt="voexpr" title="voexpr" width="300" height="177" class="aligncenter size-full wp-image-2722" />
Values object makes domain model expressive. For example, the fact that Customer has an Address is made more explicit by modelling Address an a value object.</p>
<p>It is recommended to model VOs as immutable objects. This makes them thread safe and easily shareable. Making a value object immutable also open the doors for some optimization. We can use patterns like <a href="http://en.wikipedia.org/wiki/Flyweight_pattern">Flyweight</a>, which replaces large number of unshared objects with smaller number of shareable objects.</p>
<p>Making a VO immutable seems to be an obvious choice. However, if the value of the VO changes frequently and the creation of a new VO is expensive, then making it mutable seems to be a better choice. Despite all this, always design a VO as immutable and, if required, make it mutable.</p>
<p><strong>Services</strong></p>
<p><strong><img src="http://xebee.xebia.in/wp-content/uploads/2009/12/services.png" alt="services" title="services" width="140" height="140" class="aligncenter size-full wp-image-2724" /></strong></p>
<p>In some<strong> </strong>cases, a particular behaviour does not belong to a particular entity or value object. So, instead of bloating an object with the behaviour it does not intends to do, it is better to model it as a Service. A service provides a behaviour through an interface. The behaviour can be an action, or an activity. A service should not have a state of it's own so that it can be shared easily and it's behaviour can be reused easily.</p>
<p>A service can be classified as an <strong>application</strong> service which is involved with services like transactions, security, logging, etc., and a <strong>domain</strong> service which encapsulate behaviour not specific to any object. Although, application services provide very important services to an application, but in DDD, domain services are primary concerns while modelling behaviour.</p>
<p><strong>Modules</strong></p>
<p>Modules is a less talked about topic of DDD because the reasons to divide an application into modules are fairly obvious and we have been doing that for a long time. I will reiterate those reasons here once again
<ul>
    <li>Modules help in breaking down the complexity of an application.</li>
    <li>Modules acts as a communication aid because when we discuss a functionality by associating it with modules, the discussion context is narrowed down.</li>
    <li>Modules facilitate parallel development.</li>
</ul>
<strong>Conclusion</strong></p>
<p>In this blog, we discussed various elements of DDD which makes domain modelling expressive. Constrained associations reduces the complexity of the domain models, whereas  adding directions to relationships makes it more expressive. The idea of having immutable value objects seems to add expression to models by making concepts more expressive. Services take the load off the entities, and since they are interface based, we can change the implementation without affecting the domain object. Modules break the story of domain model into chapters. Applying these concepts judiciously will surely help in expressing a deep insight into the domain of the software.</p>