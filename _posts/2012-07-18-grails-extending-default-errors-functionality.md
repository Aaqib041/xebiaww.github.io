---
layout: post
header-img: img/default-blog-pic.jpg
author: nkhattar
description: 
post_id: 14680
created: 2012/07/18 10:22:57
created_gmt: 2012/07/18 05:22:57
comment_status: open
---

# Grails: Extending default 'Errors' functionality

<p>Recently in my Grails project, we came across a requirement where we need to<strong> inject a Warning type of Error</strong> in errors collection of every domain class. The application is primarily a Web based application with multiple domain classes having view pages like Create, Edit, Show, Search, etc. The domain objects generated from the view are passed through a set of validations and if successfully passed, domain object is saved into the database. In case any of the validation fails, then the corresponding errors are saved into database for further reporting.</p>
<p>Now, the requirement was to handle a different type of Validation, named <b>Soft Validation</b> which results in adding a Warning or Soft Error against the domain object but never fails the execution workflow. There could have been two ways to handle the same:-
<ol>
    <li>To create a separate Warnings Collections for keeping track of all the warnings and injecting the same in every Domain Class using Grails <a href="http://www.ibm.com/developerworks/java/library/j-grails09159/index.html" target="_blank">Custom Plug-in</a>.</li>
    <li>To make use of the existing Error Handling mechanisms provided by Grails Framework, by default in every domain class and extend it to handle the warnings or Soft Errors.</li>
</ol>
<b>Option 1</b> is a much cleaner way without any dependencies involved, but the major problem is that one needs to <b>reinvent the wheel</b>. We need to do the same handling for warnings as already done by Grails Framework for Errors Object, while validating or saving the domain object.</p>
<p>In that case it is better <b>to extend the existing Error Handling mechanism</b> present in every Grails Domain Class.
<!--more--></p>
<p>Before going further on implementing Option 2, Lets understand the <strong>importance of injecting Errors Object</strong> in every Grails Domain Class and how that is done.</p>
<p>“The <b><i>errors</i></b> property on domain classes is an instance of the Spring <strong><a href="http://static.springsource.org/spring/docs/3.0.x/javadoc-api/org/springframework/validation/Errors.html" target="_blank">Errors</a> </strong>interface. It stores and exposes information about data-binding and validation errors for a specific object. The Errors interface provides multiple methods to navigate the validation errors and also retrieve the original values. The errors property is used by Grails during <a href="http://grails.org/doc/latest/guide/single.html#dataBinding" target="_blank">Data Binding</a> to store type conversion errors and during <a href="http://grails.org/doc/latest/guide/single.html#validation" target="_blank">Validation</a> when calling the ‘<a href="http://grails.org/doc/latest/ref/Domain%20Classes/validate.html">validate</a>’ or ‘<a href="http://grails.org/doc/latest/ref/Domain%20Classes/save.html">save</a>’ methods. One can also add one’s own errors using the ‘<em>reject</em>’ and ‘<em>rejectValue</em>’ methods.”</p>
<p>The Errors Interface provide various useful API methods for insertion &amp; retrieval of Validation Errors, like <i>addAllErrors, reject, rejectValue, getAllErrors, getErrorCount, hasErrors, etc. </i>Spring framework provides separate abstractions for handling Field Level or Global Level<i> </i>errors with corresponding api methods for the same.</p>
<p>In <b>Grails pre-2.0 </b>versions, ‘errors’ property is injected in every domain class at runtime using Groovy <strong><a href="http://groovy.codehaus.org/Dynamic+Groovy" target="_blank">Runtime Metaprogramming</a></strong>, whereas in <b>Grails post-2.0</b> versions Groovy <strong><a href="http://groovy.codehaus.org/Compile-time+Metaprogramming+-+AST+Transformations" target="_blank">Compile-time Metaprogramming</a></strong> via AST Transformation is used to inject ‘errors’ property. <a href="http://grails.org/doc/2.0.4/api/org/codehaus/groovy/grails/compiler/injection/ASTValidationErrorsHelper.html" target="_blank">ASTValidationErrorsHelper</a> class is the AST Transformation class used for the same.</p>
<p>Now, let’s <em>implement the above mentioned solution</em> where we will extend the existing Error Handling Mechanism to handle and manipulate Soft Errors. There are mainly 3 steps required for this:</p>
<p><span style="text-decoration: underline;">Step 1 – <b>Add field “Error Type” in the Error Class</b></span></p>
<p>As mentioned earlier, Spring Framework provides separate abstraction for handling Global level errors as <a href="http://static.springsource.org/spring/docs/3.0.x/javadoc-api/org/springframework/validation/ObjectError.html" target="_blank">ObjectError</a> which encapsulates an object error, that is, a global reason for rejecting an object. Field level errors are handled via <a href="http://static.springsource.org/spring/docs/3.0.x/javadoc-api/org/springframework/validation/FieldError.html" target="_blank">FieldError</a> Class which inherits from ObjectError along with properties encapsulating field level information.</p>
<p>What we need to do is to inject a field named ‘<b>errorType</b>’ in ObjectError Class or individually in FieldError in order to <strong>differentiate between Error &amp; Warning</strong>. Since these classes are pre-compiled and shipped in Spring framework plug-in, one cannot get access to the source code of these classes. Therefore, in order to add a custom field in these pre-compiled classes, we can make use of <strong>Groovy Metaclass Injection mechanism</strong>. Metaclass Injection provides a way to add new fields, methods, constructors or modify the body of existing methods in a class. Complete information around Groovy Metaclass Injection can be found <strong><a href="http://groovy.codehaus.org/JN3525-MetaClasses" target="_blank">here</a></strong>.</p>
<p>[sourcecode lang="groovy"]
ObjectError.metaClass.errorType = ErrorType.HARD_ERROR
[/sourcecode]</p>
<p><span style="color: #ff0000;"><b>*</b></span>ErrorType is a user-defined enum having two values - HARD_ERROR, SOFT_ERROR</p>
<p>Here I have injected errorType field into ObjectError Class whose default value is HARD_ERROR. Adding a field in the above manner exposes a default getter-setter method at the runtime, so one need not to expose any getter-setter method explicitly.</p>
<p><span style="text-decoration: underline;">Step 2 – <b>Adding a new method in the Class implementing Spring Errors Interface which accepts an additional argument for errorType</b></span></p>
<p>The most important prerequisite of this step is to<em> identify the <strong>implementation class of Errors Interface</strong> in Grails pre 2.0 &amp; post 2.0</em> versions. Spring framework provides <a href="http://static.springsource.org/spring/docs/3.0.x/javadoc-api/org/springframework/validation/BeanPropertyBindingResult.html" target="_blank">BeanPropertyBindingResult</a> Class as the implementation class of Errors Interface which is referenced directly in Grails pre 2.0 versions, whereas in post 2.0 versions, Grails provide its own class <a href="http://grails.org/doc/latest/api/grails/validation/ValidationErrors.html" target="_blank">ValidationErrors</a> extending BeanPropertyBindingResult Class, which provides two methods (<i>getAt, putAt</i>) for easy retrieval and addition of field errors.</p>
<p>Once this is done, next task is to <b>inject a new method in the implementation class</b> that adds an error with supplied ‘errorType’. The signature of new overloaded method will be very much similar to that of existing methods for adding error, with only difference that it will accept an <b>additional argument of type <em>ErrorType</em></b>.</p>
<p>[sourcecode lang="groovy"]
BeanPropertyBindingResult.metaClass.rejectValue = {
      String field, String code, Object[] args, String defaultMsg, ErrorType errorType-&gt;
      String[] codes = [code]
      FieldError fieldError = new FieldError(delegate.objectName,field,
      null,false,codes,args,defaultMsg)
      fieldError.errorType = errorType
      delegate.addError(fieldError)
}
[/sourcecode]</p>
<p><span style="color: #ff0000;">*</span>For injecting this new overloaded method, we have again used Grails Metaclass Injection mechanism.</p>