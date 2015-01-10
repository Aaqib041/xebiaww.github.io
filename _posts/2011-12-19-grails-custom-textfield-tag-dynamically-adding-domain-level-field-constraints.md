---
layout: post
header-img: img/default-blog-pic.jpg
author: nkhattar
description: 
post_id: 10527
created: 2011/12/19 16:31:55
created_gmt: 2011/12/19 11:31:55
comment_status: open
---

# Grails Custom TextField Tag: Dynamically adding Domain level Field Constraints

<p>Recently in my project, I came across a requirement to add a custom grails textfield in the application. The application is primarily a Web based application with multiple domain classes having numerous nested level associations with each other. Corresponding to the domain classes, there are multiple view pages like Create, Edit, Show, Search, etc.</p>
<p>The requirement was to add field constraints like ‘<strong>maxSize</strong>’ to both domain classes as well as their respective view pages. Adding such constraints at the domain level is quite easier via ‘<a href="http://www.grails.org/doc/1.3.7/guide/7.%20Validation.html#7.2 Validating Constraints" target="_blank">static constraints block</a>’ within each domain class, but reflecting the same on the view page isn’t easy enough.</p>
<p>For e.g. Adding <strong>maxsize </strong>constraint for a field in a domain class is equivalent to adding <strong>maxlength </strong>property in a textbox corresponding to that field on the view page. So, if the maxSize of a field ‘Short Name’ is say 5, then similarly we can add maxlength equal to 5 in the textfield corresponding to ‘Short Name’. <em>The solution sounds simple &amp; straightforward, but it is neither scalable nor maintainable</em>.  Adding maxlength property to ‘Short Name’ textfield in every view page, where ‘Short Name’ is used, is a time-taking task and also manually updating the maxlength property in every textfield (on change of maxSize constraint at the domain level) is not at all efficient.</p>
<p>So, the question is How to fulfill this requirement, while taking into account the <strong>scalability </strong>&amp; <strong>maintainability </strong>of the solution.
<!--more--></p>
<p>One definite solution is to <em>create a custom grails textfield which in some way reads the ‘maxSize’ constraint value from the domain class and populates the ‘maxLength’ property of the textfield</em>. In this manner, one need not to manually add or update the maxLength value on every view page, rather one just need to add or update maxSize constraint value at the domain level.</p>
<p>So the next thing to identify was How to create such a Smart Tag? Then, I came across this link "<a href="http://www.javathinking.com/2008/03/maxlength-html-attribute-for-domain-objects/" target="_blank">MaxLength HTML attribute for domain objects</a>" hosted on the official <a href="http://www.grails.org/Smarter+grails+tags" target="_blank">Grails Smart Tag webpage</a>. The solution is exactly what we discussed in the above lines.</p>
<p>[sourcecode lang="javascript"]
def maxlength = object.constraints.&quot;${attrs.name}&quot;.getMaxSize()
if(maxlength) {
attrs.put('maxlength', maxlength)
}
attrs.put('value', fieldValue(bean:object,field:attrs.name)) ...
[/sourcecode]</p>
<p>In the above mentioned code, the key thing to look for is</p>
<p>“<strong>object.constraints."${attrs.name}".getMaxSize()</strong>”</p>
<p>**'<span style="text-decoration: underline;">constraints.field</span>’ snippet in the above line returns an object of type ‘<strong><a href="http://grails.org/doc/1.3.x/api/org/codehaus/groovy/grails/validation/ConstrainedProperty.html" target="_blank">ConstrainedProperty</a></strong>’ which in turn has a list of all applied constraints for that field. ConstrainedProperty Class has a vast api exposed for almost every type of <a href="http://grails.org/doc/1.3.x/api/org/codehaus/groovy/grails/validation/Constraint.html" target="_blank">Constraint</a> like <em>maxSize</em>, <em>blank</em>, <em>minSize</em>, <em>email</em>, etc.</p>
<p>But, the moment I tried implementing the same solution in my case, <span style="color: #ff0000;">it just didn’t worked</span>. The reason is that the solution is not compliant to <strong>nested level domain class associations</strong>. What I mean by Nested level associations is that suppose Class A has a field ‘b’ which is of type Class B, which in turn has a field ‘c’ of type String. Then referring to the above mentioned solution, the maxlength value of the textfield pertaining to field ‘b.c’ of Class A will be as follows:
<p style="text-align: center;"><strong> “a.constraints.b.c.getMaxSize()”</strong></p>
Hey, but<span style="color: #ff0000;"> this will not work</span>, because maxSize constraint for ‘c’ is defined within Class B &amp; not in Class A. The above statement should be something like this:
<p style="text-align: center;"><strong> “a.b.constraints.c.getMaxSize()”</strong></p>
Again, for the above statement to execute, ‘b’ must be initialized in order to access its constraints, but in a view page like Create for Entity A, you cannot make sure to initialize ‘b’. Therefore, the above solution is not foolproof.
<p style="text-align: left;"><strong>What should be done in such a case? What could be a generic solution for this problem?</strong></p>
<p style="text-align: left;">An important thing to notice here is that ‘constraint block/closure in domain classes’ is a static block, so one need not to worry about whether the instance is initialized or not. The thing that one should worry about is how to determine the Class from the field name. The simplest way to determine Class from field name is using <strong><em><a href="http://docs.oracle.com/javase/6/docs/api/java/lang/Class.html" target="_blank">java.lang.Class</a>. </em></strong>‘Class.java’ has multiple getter methods out of which following are of great help:</p></p>
<ul>
    <li>Field[] <strong>getFields</strong>() - Returns an array containing <strong><a href="http://docs.oracle.com/javase/6/docs/api/java/lang/reflect/Field.html" target="_blank">java.lang.reflect.</a></strong><code><strong><a href="http://docs.oracle.com/javase/6/docs/api/java/lang/reflect/Field.html" target="_blank">Field</a></strong></code> objects reflecting all the accessible public fields of the class or interface represented by this <code>Class</code> object.</li>
    <li>Field <strong>getField</strong>(String name) : Returns a <code>Field</code> object that reflects the specified public member field of the class or interface represented by this <code>Class</code> object.</li>
    <li>Field <strong>getDeclaredField</strong>(String name) : Returns a <code>Field</code> object that reflects the specified declared field of the class or interface represented by this <code>Class</code> object.</li>
</ul>

<p>Using <strong>getDeclaredField(String name)</strong> function, our objective can be easily achieved. ‘<em>getDeclaredField(String name)</em>’ function returns a Field Object for the selected field, which in turn has a property ‘type’ that returns you the Class of the selected field. From the returned Class you can then access the static constraints block. That's all and you are done!!!</p>
<p><strong>Let’s implement the above solution using a real world example:</strong></p>
<p><strong>"</strong>There is a <em>Car </em>domain class which has two fields - String companyName &amp; CarDetails cardDetails. <em>CarDetails </em>class in turn has a field – String color. ‘companyName’ field has a <em>maxSize </em>constraint of 20 characters &amp; ‘color’ has a maxSize constraint of 10. You need to design a ‘<em>create</em>’ view page in order to allow user to add new car entries into the database. The webpage create.gsp will have two textfields: Company Name &amp; Car Color with respective <em>maxLength constraints</em><strong>"</strong></p>
<p>Solution:-</p>
<p><strong>Domain Classes</strong> -</p>
<p>[sourcecode lang="java"]
class Car {
    CarDetails carDetails
    String companyName
    static constraints = {
        companyName maxSize: 20
    }
}</p>
<p>class CarDetails {
    String color
    static constraints = {
        color maxSize: 10
    }
}
[/sourcecode]</p>
<p><strong>Custom TagLib</strong> -</p>
<p>[sourcecode lang="java"]
class CustomTagLib {
    static namespace = 'custom'
    def textField = {attrs -&gt;
        attrs.type = &quot;text&quot;
        attrs.tagName = &quot;textField&quot;</p>
<pre><code>    def bean = attrs.bean
    String field = attrs.name
    if (bean) {
        Class parentBeanClass = bean.class;
        String fieldName = field
        if (field.indexOf('.')) {
</code></pre>

## Comments

**[Sunil Prakash Inteti](#6418 "2011-12-19 18:19:37"):** I recommend not to use 'def' If you ready know the type of object in a Grails project. This would improve the performance a bit as there is no runtime look up for the class type for every property/method invocation. Its a good practice in Groovy/Grails projects. BTW, Nice demonstration to show the dynamism of grails and code reuse using the tag libraries.

**[Nitin Khattar](#6422 "2011-12-20 08:00:31"):** @Sunil Thanks a lot for your useful suggestion. Yes, you are right 'runtime lookup' feature of grails should be used to a limited extent till it boosts the flexibility of the code. Will definitely keep it in mind in future...

