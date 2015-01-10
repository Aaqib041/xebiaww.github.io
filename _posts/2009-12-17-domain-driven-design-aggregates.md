---
layout: post
header-img: img/default-blog-pic.jpg
author: vgupta
description: 
post_id: 2912
created: 2009/12/17 00:16:13
created_gmt: 2009/12/16 19:16:13
comment_status: open
---

# Domain Driven Design : Aggregates

<p>Every object has a lifecycle. Some objects take birth and die in a single method while others have a more complex and longer life. Typically, <strong>Domain Objects</strong> falls into the second category. The lifecycle of a domain object is shown below</p>
<p><img class="aligncenter size-medium wp-image-2911" title="lifecycle" src="http://xebee.xebia.in/wp-content/uploads/2009/12/lifecycle-300x192.png" alt="lifecycle" width="300" height="192" /></p>
<p>As the lifecycle of a domain model is complex, it is advisable to separate the domain logic from the lifecycle management logic. Furthermore, sometimes, grouping related objects to manage them as an entity is easier than managing each of them separately. <strong>Domain Driven Design(DDD)</strong> suggests to group related objects as <strong>Aggregates</strong> and to use <strong>Factories</strong> and <strong>Repositories</strong> to manage the lifecycle of the object. In this blog, I will discuss about the <strong>Aggregates</strong> and their design considerations.<!--more--></p>
<p><strong>Aggregates</strong></p>
<p>In a software system, a common requirement can be to maintain the history of the changes to a user's payment information, like credit card number, cvv number, etc. Typically, this is done by creating an object for user's history and persisting it. This can be done as shown in the code below</p>
<p>[sourcecode language="java"]
void updatePaymentInformation(params...) { //service class  method
  User user = User.get(id);</p>
<p>UserHistory userHistory = new UserHistory(user); //copy constructor to copy auditable fields
  userHistory.save();</p>
<p>user.setCreditCardNumber();
  user.setCvvNumber();
  user.save();
}
[/sourcecode]</p>
<p>Above code seemingly easy, but logically, UserHistory does not have use without a user. The true identity of the UserHistory is the associated user. So, it is better to manage the UserHistory from the User object itself as shown in the code below.</p>
<p>[sourcecode language="java"]
class User {
  Set&lt;UserHistory&gt; userHistories;
  void updatePaymentInformation(params..) { //user class method
    this.setCreditCardNumber(...);
    this.setCvvNumber(...)
    userHistories.add(new UserHistory(this));
  }
}
[/sourcecode]</p>
<p>In above example, we can see that UserHistory cannot exist without User. So, we can enforce the access to UserHistory through User object only. This will minimize the dependencies between the classes and reduce complexity. The example mentioned in here in trivial, but in real world, there can be many other scenarios where one entity can also exist without another entity, like an Order item cannot exist without an Order, a Book cannot exist without a page. One important thing to note here is that in these examples, the reverse relation is also important, i.e., an Order does not have a meaning without a Order item and nobody will read a book without pages. Here, Book and Order represent a cluster of associated objects, which model something meaningful as a whole but lose importance when separated from each other.</p>
<p>In DDD, such cluster of associated objects is known as a <strong>Aggregate</strong>. Each aggregate has a <strong>root</strong> and a <strong>conceptual boundary</strong>. Root is a specific entity in the aggregate and is the only member of the aggregate visible(allowed to get referred from other objects) to rest of the world. Entities other than the Root entity are local entities, which have identities to distinguish them within the aggregate.</p>
<p>Other than managing the access to local entities of the aggregate, root of the aggregate can also enforce the <strong>invariants</strong>, which are consistency rules which needs to be maintained whenever data changes. For example, in most Purchase Orders(PO) there is a approval limit. So, whenever a new PO item is added to the PO, we can check whether adding the item will make PO exceed it's approval limit.</p>
<p>Since aggregates model a concept which represent a meaningful whole, consistency of aggregates are very important. To maintain consistency of aggregates, we need to apply a set of rules, which are
<ul>
    <li>The root entity should be the only window to the aggregate to the outside world.</li>
    <li>All local entities within the aggregate should be accessed by traversing the tree from the root entity.</li>
    <li>Invariants, if any, should be applied only by the root entity. Invariants should be applied at all times, i.e, whenever a non root member of the aggregate is inserted, update or deleted, all applicable invariants should be satisfied.</li>
    <li>Any object which is outside the aggregate should not have references to entity or value objects which are member of the aggregate. The only exception to this rule is root entity.</li>
    <li>Any update to the non root member should we through root entity as it would help in satisfying the aggregates.</li>
</ul>
In this blog, we discussed about the Aggregates and the set of rules to be applied while designing them. Although, an aggregate is a cluster of different objects, they represent a business concept as a unit. On the face of it, Aggregates seem to reduce the complexity of the model and make the model clean. But, we should avoid overuse of aggregates. We should avoid overuse of aggregates because
<ul>
    <li>Overuse would result in deep nested trees and this would require a longer traversal time to the local entities.</li>
    <li>In a distributed environment, serialization and de-serialization would take time if we have deeply nested structures.</li>
</ul>
Modelling aggregates judiciously would reduce the complexity and maintain consistency of the domain model.</p>