---
layout: post
header-img: img/default-blog-pic.jpg
author: vgupta
description: 
post_id: 2297
created: 2009/12/02 08:03:48
created_gmt: 2009/12/02 03:03:48
comment_status: open
---

# Domain Driven Design : An Introduction

**Introduction**

**The primary purpose of most software projects is to add value to the customer's business. This is done by abstracting the complex web of thoughts inside a human mind in the form of a software program. Most software projects cater to a particular domain, and in many software projects, the domain of a software project is not technical. Unfortunately, majority of the technical people do not give enough importance to the domain of the software, and hence, they lack the required domain knowledge. This lack of domain knowledge leads to a situation where **What**(to do?) takes over **Why**(to do?). Generally, in such a scenario, it might be possible to deliver a usable product, but the primary purpose of adding business value is often neglected.**

The above mentioned situation is quite common and there is a continual influx of thoughts, which intend to provide a solution, by various experts. [Eric Evans][1] is one of such experts, who based on his experience, gained by working on complex projects, epitomized his findings in the form of tips, suggestions, patterns and guidelines, and coined a term **Domain Driven Design**, better known as **DDD**. In this blog, I intend to present various aspects of DDD and how it suggests to tackle the complexity of the domain.  **What is DDD?**

**![picture_1000_words][2] **

A picture is worth a thousand words and, sometimes, is better suited to convey a message. Similarly, a domain model is a simplification of the domain of the software and it abstracts the elements of domain which are relevant to the problem the software intends to solve. A domain model is representation agnostic, i.e., it can be represented as an UML diagram, as written code, as simply plain English, or as a simple diagram. The decision to choose an option depends upon the situation and complexity of the problem. The process of creating, maintaining and enhancing highly expressive domain model by applying objected oriented principles, design patterns is known as **Domain Driven Design(DDD)**. One important point to understand is that DDD is not a one time activity. It is an iterative process and model evolves with the project.

We all know that the process of creating domain model by abstracting from the complexities of a domain is not an easy job. Experienced programmers/thought leaders often compare this job with that of an artist. Few would disagree with them. But, with any art, there are certain principles and practices, which make the process of identifying and representing abstractions simpler. DDD also suggests to employ certain principles and practices, which we may already be doing intuitively, to work with highly expressive domain models. Let's have a look at some of the principles and practices of DDD.

**Getting Started with DDD** There might be several questions, which may be taking shape in the mind right now. Let's see which DDD principles and practices applies to specific question

**Q:** I don't have the domain knowledge. How will I communicate effectively with the domain experts? **A:** In most projects, there is an interface between the technical people and the domain experts. We may call this expert a Business Analyst, a Product Owner, or with some other name. Sometimes, this interface is not effective to transform the messages between two parties and in the process of message transformation, some relevant details are missed. This leads to a situation where **WHAT** takes over **WHY**.

To overcome this problem, DDD emphasizes to have a close relationship between the domain experts and the developers. Even when a modeler is working with domain experts(DE), he should be hands-on enough to think about design issues while brainstorming with the DE. DDD also suggests DDD developers to pick up key concepts while having conversation with the DE and also educate them about their terminology. This leads to the development of a **Ubiquitous Language(UL)** which forms the basis of development domain model. UL is basically a medium of communication which is used to develop a domain model. It is also used to validate a domain model periodically. The development of UL is an iterative process and is based on mutual agreement between DE and technical people.

**Q:** How will I represent my domain model? ** A:** Domain model should be represented in a way which best communicates the intent of the domain knowledge. It can be in the form of a diagram, a UML diagram, plain English, or simply code. Now, when there is freedom to represent something in multiple forms, how to maintain the sanctity of the domain model. I think that's the job of the skilled developers or domain modelers. It is difficult to provide a rule for everything and we all know that real world is like that. Sometimes we have to break the rules and take pragmatic decisions. DDD realizes that and promotes developers to take pragmatic decisions instead of being bound by the rules.

**Q:** How will I layout the architecture of the application with DDD? ** A:** The focus of DDD is to create domain models that are rich in behavior. Typically, an application has several other issues as well which are not concerned with the domain. To work effectively with DDD, we need to isolate those issues from the domain modelling. In order to do so, we can use layered architecture to isolate and group the problems.

![layering][3]In a layered architecture, each layer is aware of the layers below it. In general, a layer at the lower level cannot make calls to the layer above it.

**Q:** How will I represent concepts in a domain model? ** A:** Most concepts in domain can be classified into 

  * Something which has an identity: **Entities**. Entities, which are essentially objects, identified by an identity, which does not change. The identity can be a surrogate key or a composite key.
  * Something which has an important meaning in larger picture but does not have an identity of it's own: **Value Objects(VOs)**. VOs represent meaningful concepts and abstracts the details of those concepts but does not have their own entity. For example, a phone number is one such thing, which represent certain information, but does not make any sense if not associated with a person, place, etc.
  * Something which relate various concepts with each other: **Cardinality**. Cardinality represents relationship between various entities. DDD advocates to have minimum cardinality between entities.
  * Behavior common to various concepts: **Services**. Services are components which provides behavior which is not specific any particular entity. These services are different from the application service, in that, they provide services to the domain layer and not callable from the application layer.
Apart from the above, DDD also introduces the concept of **Aggregates**, which are essentially small group of related objects. Aggregates have a common root and act as an entry point for outer world to the constituents of the aggregate. This helps in managing the complex web of associations in a domain model.

   [1]: http://domaindrivendesign.org/about
   [2]: http://xebee.xebia.in/wp-content/uploads/2009/12/picture_1000_words1-300x300.jpg (picture_1000_words)
   [3]: http://xebee.xebia.in/wp-content/uploads/2009/12/layering.png (layering)