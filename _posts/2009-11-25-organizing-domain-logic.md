---
layout: post
header-img: img/default-blog-pic.jpg
author: vgupta
description: 
post_id: 2031
created: 2009/11/25 13:28:58
created_gmt: 2009/11/25 08:28:58
comment_status: open
---

# Organizing Domain Logic

<p><strong>Introduction</strong></p>
<p>Most enterprise applications use Layering as the primary technique to break the complexity of the software projects. Layering involves breaking down the applications in various layers where each layer lie above the other and provide services to the higher layer. Layering provides following benefits
<ul>
    <li>You can understand about a layer without knowing about other layers. For example, if you know how <a href="http://en.wikipedia.org/wiki/Transmission_Control_Protocol">TCP</a> works you can create your own <a href="http://en.wikipedia.org/wiki/File_Transfer_Protocol">FTP</a> service without actually knowing about the internals of the ethernet.</li>
    <li>It is easy to substitute the layers with alternative implementation. For example, if DAO layer is designed properly, if should be easy to change the ORM solution from <a href="https://www.hibernate.org/">Hibernate</a> to <a href="http://ibatis.apache.org/">iBatis</a> easily.</li>
    <li>Layering also helps in standardization and extensibility.</li>
</ul>
But the downsides of layering applications include cascading changes, performance overhead of converting data from one representation into another and it is not often easy to decide the layer of a particular component. Considering the benefits of layering and it's applicability to almost any sort of computer application, it is used widely and successfully.</p>
<p>Three primary layers of an enterprise application can broadly be categorized into
<ol>
    <li>Presentation Layer: Information display, HTTP requests, command line invocations, batch API, etc.</li>
    <li>Domain Layer: Here lies the "Heart of the software"</li>
    <li>Datasource Layer: Provides access to external resources like databases, messaging, mail server, etc.</li>
</ol>
In this blog, we will discuss various patterns of organizing domain logic and specific pros and cons of each. We will also discuss how these approaches compare with each other.
<!--more-->
<strong>Domain Login Patterns</strong>
In his book, <a href="http://www.amazon.com/Patterns-Enterprise-Application-Architecture-Martin/dp/0321127420">PoEEA</a>, <a href="http://martinfowler.com/">Martin Fowler</a> has classified domain logic patterns into three primary patterns: Transaction Script, Domain Model and Table Module. He also discussed introducing an additional layer, Service Layer, which is useful in providing horizontal services. Let's look at each of these in detail.</p>
<p><strong>Transaction Script (TS)</strong>
Transaction script is essentially a procedure that takes input from the presentation, processes it with validations and calculations, stores data in the database, send emails, etc. It may also fetch the data and send to the UI. It's more like coding a C program in Java. Typically, in transaction script method of organizing domain logic, the code is segregated into various services and each service has methods that perform a particular business functionality. A method of a transaction service might look like this</p>
<p>[sourcecode language="java"]void tsmethod(params) {
   // get data
   // process data
   if () {
   } else if {
   } else {
   }
   // insert/update/delete data
   //send email
}[/sourcecode]</p>
<p>As you can see from the above, most of the above code is procedural. Here, the services act on the entities. A typical example would be an interaction between session bean and entity bean. Another similar pattern not mentioned in the PoEEA is <a href="http://martinfowler.com/bliki/AnemicDomainModel.html">Anemic Domain Model</a> (ADM) which works on the same principle where domain objects have attributes and relationships but does not have behaviour. Behaviour is provided by the services. The different between TS and ADM is that in former we do not have domain objects but rather have something which Fowler describe as <a href="http://martinfowler.com/eaaCatalog/tableDataGateway.html">Table Data Gateway</a>, which essentially is a wrapper for SQL queries.</p>
<p><strong>Domain Model (DM)</strong>
In domain model approach, both behaviour and data are encapsulated inside the domain objects. Domain model looks different from the database design and utilizes inheritance, strategies, and other design patterns to create complex webs of small interconnected objects. A typical domain model code might look like</p>
<p>[sourcecode lang="java"]class Bar {
  private BarAdmissionPolicy = new BarAdmissionPolicy();
  public void admit(Person person) {
     if (BarAdmissionPolicy.isAllowed(person)) {
         this.add(person)
     }
  }
}
public class BarAdmissionPolicy {
   public boolean isAllowed(Person person) {
     if (person.age &gt; 18) {
        return true;
     }
     return false;
   }
}[/sourcecode]</p>
<p>A well designed domain model provides an abstraction of the domain logic. It encapsulates complex business logic as interactions between domain objects.</p>
<p><strong>Table Module (TM)</strong>
It is essentially a singleton instance that handles the business logic for all the rows in a database table. There is no concept of identity in a Table Module. It is designed to operate on entire table data. A typical domain might look like</p>
<p>[sourcecode language="java"]class Product {
 void insert(name, type, etc) {
 }
 void insertAll(List) {
 }
 void calculateProductCost(List productIds) {
 }
}[/sourcecode]</p>
<p>As you can see, a table module encapsulates the data and behaviour of the entire table. It is most commonly used in places where UI widgets are data-aware like Oracle Forms or .Net environment which has Visual Studio, which provides tools like <a href="http://en.wikipedia.org/wiki/Recordset">RecordSet</a>.</p>
<p><strong>Service Layer (SL)</strong>
A Service Layer defines an application's boundary and its set of available operations from the perspective of interfacing client layers. It encapsulates the application's business logic, controlling transactions, applying security and coordinating responses in the implementation of its operations. Service Layer can be thick or thin depending upon whether you are using transaction scripts or domain model.</p>
<p>Now, we have understanding of the various domain logic patterns, let's discuss how they compare with each other.</p>
<p><strong>Comparison</strong>
Essentially, if you are using one of DM, TM or ADM pattern, the domain layer can be split into two layers: SL + DM/TM/ADM. SL is essential in providing horizontal services like security, transactions, etc. With TS in practice, it is difficult to identify the difference between SL and TS since TS can double up as a SL also.</p>
<p>Since, TM emphasizes one class per table, there are issues in implementing OO concepts while inheritance. There are no direct relationships between objects. For example, if you want to get a List of Products and Categories separately then it is easy to define where to put the code, but it is not clear where to put the code if you need a RecordSet which contains both Products and Categories.</p>
<p>TS is more like a service class which makes implementing domain logic difficult when the complexity of the application increases, but works well for simple applications.</p>
<p>There is a tough competition between ADM and DM. DM is essentially a Rich Domain Model (RDM). Common things between them includes data and relationships. But things specific to RDM is behaviour of the domain model. There has been a heated debate between designers to prove the dominance of one over the other. Suprisingly, the wiki page for ADM lists why should be avoid them. I reiterate them here
<ul>
    <li> Logic cannot be implemented in a truly object-oriented way unless wrappers are used, which hide the anemic data structure.</li>
    <li> Violation of the principals information hiding and encapsulation.</li>
    <li> Necessitates a separate business layer to contain the logic otherwise located in a domain model. It also means that domain model's objects cannot guarantee their correctness at any moment, because their validation and mutation logic is placed somewhere outside (most likely in multiple places).</li>
    <li> Necessitates a global access to internals of shared business entities increasing coupling and fragility.</li></p>