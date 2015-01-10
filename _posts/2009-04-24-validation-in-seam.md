---
layout: post
header-img: img/default-blog-pic.jpg
author: rgoyal
description: 
post_id: 1081
created: 2009/04/24 04:41:37
created_gmt: 2009/04/24 03:41:37
comment_status: open
---

# Validation in Seam

<p>Validations are a huge part of any software development. We need to validate that the data being entered from the UI is correct i.e of correct type or we are not leaving a notNull/notEmpty fields to be blank. There are so many ways to fulfill these requirements and we can have UI level validations or persistence level validations.</p>

<!--more-->

<p>First lets look at the validations that we can have.</p>

<p><strong>Hibernate Validations</strong>: By using annotations, it is very easy to apply validations on an entity e.g we can have @NotNull annotation which basically means that the fields cannot be Not Null. We can also have @Length and specify the min and max length for a particular field. So lets say that we have an entity User which looks like</p>

<blockquote>
<pre lang="java5">@Entity
public class User {
    private String userName;
    private String passwordHash;

    @NotEmpty
    @Length(min=1,max=5)
    public String getUserName() {
        return userName;
    }

    @NotNull
    public String getPasswordHash() {
        return passwordHash;
    }
....
}</pre>
</blockquote>

<p>Now after the User entity parameters have been updated and they are ready to persist, hibernate validation will be invoked to assert that the fields userName, and passwordHash are not null/empty or that the length of the field userName is indeed between 1 and 5.</p>

<p><strong>JSF Validations:</strong> JSF provides the UI level validations and they are done  in the Process Validation phase of the JSF life cycle. These are done to ensure that the data is valid before updating the model data so that when the Invoke Application phase is called , we can be sure of the validity of the data. With JSF , we can do the same validations as above (i.e on User Entity) on the UI. Lets look at the JSF code for the field userName</p>

<blockquote>
<pre lang="xml">
  <h:outputLabel for="name">
      Name:<span> *</span>
  </h:outputLabel>
  <span class="value">
    <h:inputText id="userName" value="#{User.userName}" required="true">
      <f:validateLength maximum="5" minimum="1"/>
    </h:inputText>
  </span>
  <span class="errors">
      <h:message for="name" styleClass="errors"/>
  </span>

</pre>
</blockquote>

<p>Having  required="true" in the form specifies that the corresponding component must have a value. JSF tag &lt;f:validateLength&gt; ascertains that the length of value entered by the user for the field userName is between 1 and 5. Here each field must be validated explicitly.</p>

<p><strong>How Seam binds these Validations together</strong></p>

<p>Seam brings both Hibernate and JSF validations together to perform double checks on every data feld. It does this by first invoking UI Validations (JSF)  and then annotation based Hibernate Validations.  Seam uses ModelValidator to invoke the hibernate Validations after the JSF validations have been successfully executed. So in Seam framework, Hibernate validations are applied twice. First is when the user enters any data and the framework validates it and the second when the successfully validated data is being persisted.</p>

<p>Seam has introduced  new tags &lt;s:validate/&gt; and &lt;s:validateAll/&gt; which invoke the Hibernate Validator for the input component.  It registers a Hibernate Validator-JSF Validator bridge which helps in performing the Hibernate Validations from the UI to provide a feedback to the user. &lt;s:validate&gt; tag is required for each individual input component for which we want to do validations but we can easily just put all the components within one &lt;s:validateAll&gt; tag to invoke the validations on all the input fields. Lets see how our Seam-JSF code will look now.</p>

<blockquote>
<pre lang="xml">
  <s:validateAll>
    <h:inputText  id="username"   value="#{user.userName}"  required="true"/>

    <h:inputSecret  id="password" redisplay="true" required="true" size="45" maxlength="45"
        value="#{user.passwordHash}"/>
  </s:validateAll>
</pre>
</blockquote>

<p>Here <s:validationAll> tag eliminates the need for <f:validateLength> tag as it uses the annotation on the corresponding model class property to do the hibernate validations.</p>

<p><strong>Some weird Behaviour</strong></p>

<p>Even though Seam integrates JSF and Hibernate validations very nicely, there is a little weird behaviour that I came across while investigating how validations worked in Seam.</p>

<p>Specifying @NotNull  on the model does not eliminate the requirement for required="true" to appear in the JSF.  This is due to a limitation of the JSF validation architecture.<strong> If required="true" is specified on a tag in JSF, and the field is not inputted any value then the Hibernate Validations are not activated as the JSF framework first looks at the value of the "required" field. JSF activates hibernate validatations only if  we input any value.</strong> . So lets consider a scenario where we have 3 conditions:</p>

<p>1.  required="true" is not specified in JSF<br />
2. @NotNull annotation on the field in the entity and @Length(max=4)<br />
3. No value is inputted and submitted the form.</p>

<p>In this scenario, JSF will not activate hibernate validations and and an empty string will be passed to the application. Now, the second level hibernate validations will be activated and since @NotNull check will pass as it has a not empty string , the field will be persisted to the database. @NotEmpty annoatation is a good replacement for @NotNull if required="true" is not specified. If we add the required = "true" in JSF with @NotNull annotation, then Hibernate Validations will be bypassed and "value is required" error message will added to the Faces Context by the JSF framework. Second level validations will still be done while persisting the data.</p>

<p><strong>AJAX Validations</strong>:  Seam provides a great AJAX support and it is especially beneficial for the validations. In our  previous example, all the validations were  performed on the click of the submit button. But with Ajax, it is possible to get instant validation upon losing the focus on the input field. Ajax4jsf  provides a tag  which can be used to send an Ajax request from the JSF page. So the seam-jsf code with Ajax support will look like</p>

<blockquote>
<pre lang="xml">
  <s:decorate id="usernameDecorate" template="layout/edit.xhtml">
    <ui:define name="label">Username</ui:define>
    <h:inputText  id="username"  required="true" value="#{user.userName}" >
      <a:support event="onblur" reRender="userOutputPanel" ajaxSingle="true" />
    </h:inputText>
  </s:decorate>
</pre>
</blockquote>

<p>Now, if we consider the same scenario as above, i.e<br />
1. required="true" is not specified in JSF<br />
2. @NotNull annotation on the field in the entity and @Length(max=4)<br />
3. No value is inputted and focus is moved out of the field</p>

<p>Here again,  hibernate Validations will not get invoked by the JSF. Also, the second level hibernate validations will not be done as the form has not been submitted so <strong>no error message will be displayed.</strong> Similarly, if required="true" is added to the JSF, JSF framework will check the required flag and add the error message to the context.</p>

<p>We can also do custom validations using Ajax. Let us consider a simple example of registering a user for your application, we can have a field for username, password and confirm password.  We can easily validate that the value inputted in the confirm Password field is the same in password field using Ajax.  The JSF code for the confirm Password field will look something like this</p>

<blockquote>
<pre lang="xml">
  <s:decorate id="passwordConfirmDecorate" template="layout/edit.xhtml">
    <ui:define name="label">Password Confirm</ui:define>
    <h:inputSecret id="passwordConfirm" redisplay="true" required="true" size="45" maxlength="45"
         value="#{registerAction.passwordConfirm}">
      <a:support event="onblur"
                      action="#{registerAction.validatePassword()}"
                      reRender="userOutputPanel"
                      ajaxSingle="true" />
    </h:inputSecret>
  </s:decorate>
</pre>
</blockquote>