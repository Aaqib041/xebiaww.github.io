---
layout: post
header-img: img/default-blog-pic.jpg
author: vgupta
description: 
post_id: 2961
created: 2009/12/30 17:36:33
created_gmt: 2009/12/30 12:36:33
comment_status: open
---

# Domain Driven Design : Querying Domain Objects

<p><em>After having discussed about <a href="http://xebee.xebia.in/2009/12/23/domain-driven-design-creating-domain-objects/">object creation</a></em><em> in the previous post, I would like to discuss the challenges of querying objects in a domain model.</em></p>
<p>There is a well designed domain model with a rich set of associations between objects. How to get to a particular entity or value object inside the domain model? Since the domain model is well connected, if you have a reference to a root entity, you can get to the other members to which this root is associated. But, how to get to this root? Well, we can have a global root which contains the collections of all the root of the entities. For example, in a sales order application, you can have a global root containing the collection of order and customer objects.</p>
<p>One drawback of the above solution is that it will perform badly in a distributed system. For example, when a client asks the global root to return the list of customers, and there are millions of customers, we have to do a database query and it will return millions of records. This will cause a lot of data transfer on the network. With multiple clients, this solutions would not work at all.</p>
<p>In most situations, you would need only a subset of customers which meet a particular set of conditions. So, what are the options that we have to return a subset of customers to the client in a way which performs well and does swamp the domain model with the complexity of the query logic. The options that we have are<!--more-->
<ul>
    <li>Using a SQL query</li>
    <li>Query Objects</li>
    <li>Repository Objects</li>
</ul>
Let's look at these options in detail.</p>
<p><strong>Using an SQL query </strong></p>
<p>An SQL query is a powerful tool to get what we want from the database. Using an SQL query involves processing the resultset returned from the database and converting into object format. This data access code if coded inside the domain objects can result in obfuscating the domain model. Also, there is a tight coupling between domain objects and database.</p>
<p>This problem of translating between object world and the database world can partially be solved by using <a href="http://martinfowler.com/eaaCatalog/metadataMapping.html">metadata mapping</a>. Meta data mapping reduces the overhead of translating between java objects and sql resultsets. But, the problem of separating data access code from the domain model is still there if sql queries are employed.</p>
<p><strong>Query Objects</strong></p>
<p>A Query Object is a structure of objects that can form itself into a SQL query. You can create this query by referring to classes and fields rather than tables and columns. In this way those who write the queries can do so independently of the database schema and changes to the schema can be localized in a single place. A Query object is based on <a href="http://en.wikipedia.org/wiki/Interpreter_pattern">Interpreter</a> pattern. Hibernate, a leading framework for Object Relational mapping provides API for query Objects. Following is an example of data Access Code written using query objects</p>
<p>[sourcecode language="java"]
List cats = sess.createCriteria(Cat.class)
    .add( Restrictions.like(&quot;name&quot;, &quot;Fritz%&quot;) )
    .add( Restrictions.between(&quot;weight&quot;, minWeight, maxWeight) )
    .list();
[/sourcecode]</p>
<p>The advantages of Query Object come with more sophisticated needs: keeping database schemas encapsulated, supporting multiple databases, supporting multiple schemas, and optimizing to avoid multiple queries. Some projects with a particularly sophisticated data source team might want to build these capabilities themselves, but most people who use Query Object do so with a tool. There are a lot of tools available like Hibernate, Toplink, iBatis which provide APIs for creating queries using object oriented code.</p>
<p><strong>Repository Objects</strong></p>
<p>Query Objects help in cleaning up the domain model from the data access code. But, in most scenarios, it is advisable to separate the data access code from the domain model into a separate layer. A <strong>Repository</strong> mediates between the domain and data mapping layers, acting like an in-memory domain object collection.</p>
<p>Repository presents a simple interface. Clients create a criteria object specifying the characteristics of the objects they want returned from a query. For example, to find person objects by name we first create a criteria object, setting each individual criterion like so: criteria.equals(Person.LAST_NAME, "Gupta"), and criteria.like(Person.FIRST_NAME, "V"). Then we invoke repository.matching(criteria) to return a list of domain objects representing people with the last name Gupta and a first name starting with V. Under the covers, Repository combines Metadata Mapping with a Query Object to automatically generate SQL code from the criteria. An example involving repository is shown below</p>
<p>[sourcecode language="java"]
public class Person { //domain object
   public List dependents() {
      return Registry.personRepository().dependentsOf(this);
   }
}</p>
<p>public class PersonRepository extends Repository {
   public List dependentsOf(aPerson) {
      Criteria criteria = new Criteria();
      criteria.equal(Person.parent, aPerson);
      return matching(criteria);
   }
}</p>
<p>abstract class Repository {
   private RepositoryStrategy strategy;
   protected List matching(aCriteria) {
      return strategy.matching(aCriteria);
   }
}</p>
<p>public class RelationalStrategy implements RepositoryStrategy {
   protected List matching(Criteria criteria) {
      Query query = new Query(myDomainObjectClass())
      query.addCriteria(criteria);
      return query.execute();
   }
}
[/sourcecode]</p>
<p>Because Repository's interface shields the domain layer from awareness of the data source, we can refactor the implementation of the querying code inside the Repository without changing any calls from clients. Indeed, the domain code needn't care about the source or destination of domain objects. In we want to change the data to be picked up from the memory instead of the database, we can simply change the RepositoryStrategy from relational to in memory by creating an in memory relational strategy.</p>
<p>After having discussed about various methods of querying the domain objects, we can conclude that on a large project with complex domain we should not use queries directly from the domain model. Now, as far as query objects and repositories are concerned, I think they both resolve a separate problem. Query Objects help in abstracting the data access code in an object oriented way, whereas, Repository is used for separating the data access code from the query objects and providing for additional sophistication, id required. Moreover, if you are using sophisticated tools like Hibernate, they provide the APIs which implement these patterns.</p>
<p><em>So far in this series, we have discussed the basics of domain driven design. In the next part of this series, I will discuss about Specification pattern.</em></p>