---
layout: post
header-img: img/default-blog-pic.jpg
author: vgupta
description: 
post_id: 3013
created: 2010/01/27 17:50:44
created_gmt: 2010/01/27 12:50:44
comment_status: open
---

# Domain Driven Design : Specification Pattern

<p><em>In the previous post, I discussed about how to <a href="http://xebee.xebia.in/2009/12/30/domain-driven-design-querying-domain-objects/">query the domain objects.</a> That post completes the issues with domain object lifecycle management. In this post, I will discuss how to make implicit concepts explicit in order to make domain model more expressive by using Specification Pattern.</em></p>
<p>Sometimes small details of the design get lost in the code. We tend to consider small business rules and constraints as implicit and these details are not given enough importance as they should be given.They are often coded as a bunch of if-else blocks in the code and this makes the identification of these business rules difficult and more importantly makes design less effective. Let see some of the concepts/design patterns which can be used to make these implicit concepts more explicit. <!--more--></p>
<p><strong>Specification</strong></p>
<p>Most application have small rules which are evaluated as boolean value. Some of these rules can be simple. But, there are certain rules which are fairly complex. They are infact, a combination of one or more of such smaller business rules. Sometimes, these rules are applicable on multiple objects and can be complex enough to make their prescence felt and it is advisable to factor them out in a separate class or method. Such business rules can be termed as Specification as they test a object against a specified criteria. Let's see the example of a specification.</p>
<p>[sourcecode language="java"]
class OverdueInvoiceSpecification extends InvoiceSpecification {
   private Date currentDate;</p>
<p>public OverdueInvoiceSpecification(Date currentDate) {
      this.currentDate = currentDate;
   }</p>
<p>public boolean isSatisfiedBy(Invoice invoice) {
      int gracePeriod = invoice.customer().getPaymentGracePeriod();
      Date deadline = invoice.dueDate() + gracePeriod;</p>
<pre><code>  return currentDate.after(deadline);
</code></pre>
<p>}
}
[/sourcecode]</p>
<p>Now, we can simply apply the rules wherever, we want</p>
<p>[sourcecode language="java"]
class CustomerAccount {
  boolean isOverDue() {
   Specification overDueSpec = new OverdueInvoiceSpecification(new Date());
   Iterator&lt;Invoice&gt; listInvoices = customer.getInvoices().iterator();
   for (Invoice invoice : listInvoices) {
     if (overDueSpec.isSatisfiedBy(invoice)) return true;
   }
   return false;
  }
}
[/sourcecode]</p>
<p>There can be many places which warrant the use of specification pattern in an application. Mostly, they are used in
<ul>
    <li>Validations, as demonstrated above</li>
    <li>In filtering data, based on business rules. For doing this we can combine <a href="http://guptavikas.wordpress.com/2009/12/30/domain-driven-design-querying-domain-objects/">Repositories</a> with Specifications and filter data returned from the persistent storage. This filtering can also be done at database level, but that will make the application code less maintainable.</li>
    <li>In domain object security. We can check using specification whether a particular object is editable by a user with specified role. <a href="http://static.springsource.org/spring-security/site/">Spring security</a> provides a more sophisticated framework for this.</li>
</ul>
There is no doubt that factoring out methods and classes for these business rules makes design more expressive, but, there can be numerous such rules in the code and factoring a separate places for each of them can be quite cumbersome. So, how to decide when to create a new method or class for a business rule or constraint.  I think the key lies in the word business. If a condition affects a business decision, it is a business rule and we can model them separately in a method or class. Moreover, if the specification code in itself is bit involved and can be applied as multiple places, it is better to separate them out of the domain object.</p>