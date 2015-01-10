---
layout: post
header-img: img/default-blog-pic.jpg
author: sarora
description: 
post_id: 3424
created: 2010/05/03 21:22:59
created_gmt: 2010/05/03 16:22:59
comment_status: open
---

# Implementing Command Pattern using Spring injection

<p>Recently I was working on a user story and after the initial design and a bit of implementation, I noticed that the code is not organised nicely and also making my life hard from the unit testing standpoint.
The business problem my code addressed was building a rule engine (built with a set of validation rules)  that is applied on the input from the client , generate warning, error messages and validating the input.So, this is how I designed my implementation at first</p>
<p>[sourcecode language="java"]
class Validator {
 ValidationResult result;</p>
<p>public ValidationResult validate(input){
   rule1();
   rule2();
   rule3();</p>
<hr />
<hr />
<p>}</p>
<p>private rule1(){
   doSomething...
 }
 private rule2(){
   doSomething...
 }
 ...and so on.
}
[/sourcecode]</p>
<p>At first when I read the user story documentation, I had to apply 4 rules in all, but think of a scenario when the user has a couple of more rules to be applied on the input now and wants to take out a few which we implemented in the first place , what would u do now?</p>
<!--more-->

<p>Add 2 more private methods named rule5 and rule6 to Validator and add a call to both in the validate method , also take out method rule1(since that rule has become obsolete now)
Now, because I had only one method exposed from Validator,the unit test case(called ValidatorTest) also broke and I had to capture the scenarios covered by the two new rules added and remove the obsolete one.</p>
<!--more-->

<p>Looks clumsy , doesn't it?, I thought of a better design approach from the clean code and unit testing perspective and came up with the following design</p>
<p>Validator now would only have one method called validate and no private methods inside it , since all the rules take input as a parameter and return validation result as the output, created the following interface</p>
<p>[sourcecode language="java"]
interface ValidationRule {
   ValidationResult validate(input,result);
}
[/sourcecode]</p>
<p>I pulled out all the private methods from Validator and created a class for each one of those with the following implementation</p>
<p>Note : result is an instance of ValidationResult only</p>
<p>[sourcecode language="java"]
class Rule1 implements ValidationRule{
   public void validate(input,result){
    [business logic here .....];
   }
}</p>
<p>class Rule2 implements ValidationRule{
   public void validate(input,result){
     [business logic here .....];
   }
}
[/sourcecode]</p>
<p>and so on ...</p>
<p>Lets plugin these into Validator ..I used spring's dependency injection in following way
My Validator class would look like</p>
<p>[sourcecode language="java"]
class Validator {
ValidationResult result;
List&lt;ValidationRule&gt; rules;</p>
<pre><code>public void setRules(List&amp;lt;ValidationRule&amp;gt; rules){
 this.rules=rules;
}
public ValidationResult validate(input){
   for(ValidationRule rule:rules){
     rule.validate(input,this.result);
}
</code></pre>
<p>}
}
[/sourcecode]</p>
<p>In the application-context.xml I added following</p>
<p>[sourcecode language="xml"]
&lt;bean id=&quot;validator&quot;&gt;
 &lt;property name=&quot;rules&quot;&gt;
   &lt;list&gt;
     &lt;bean class=&quot;com.xebia.business.rules.Rule1&quot;/&gt;
     &lt;bean class=&quot;com.xebia.business.rules.Rule2&quot;/&gt;
     &lt;bean class=&quot;com.xebia.business.rules.Rule3&quot;/&gt;
     -----
     -----
   &lt;/list&gt;
 &lt;/property&gt;
&lt;/bean&gt;
[/sourcecode]</p>
<p>I added unit test cases for all the rules as Rule1Test, Rule2Test e.g., independent of one another.</p>
<p>Benefits that I see with this design are following</p>
<ol>
<li>
<p>Cleaner code , Validator is now acting as a proxy for the rules engine and doesn't contain any business logic.</p>
</li>
<li>
<p>Enhanced testability , all the rule classes can be tested independent of each other.</p>
</li>
<li>
<p>In future if a rule becomes obsolete , I'll just remove that from the &lt;list&gt; tag in the context file and similarly if new rules need to be added, just add a rule class without touching either Validator and any of the Rule classes and put an entry in the list tag in the context file.</p>
</li>
</ol>

## Comments

**[PSG](#6022 "2011-10-14 00:49:06"):** Thanks for sharing your idea...for long number of days I was looking for this type of clean code example with a real life problem..... Thanks ......

