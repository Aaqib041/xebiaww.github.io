---
layout: post
header-img: img/default-blog-pic.jpg
author: Manish Kumar Thakur
description: 
post_id: 17334
created: 2013/10/16 18:01:12
created_gmt: 2013/10/16 13:01:12
comment_status: open
---

# JSF Validations without validators

<p>One of the most time consuming and repetitive task in a web application development is applying validations on various fields of the form. For this task most developers find themselves writing their own custom validators. JSF simple syntax and tweaks can be used to eradicate the need of any custom validators which include implementing an interface like Validator and overriding a method such as validate(). Just about any validation can be done in JSF form without writing much of java code. We'll see some of the most common field validations that occurs in most web applications. First we'll start with phone number field validation. The validation we need are:</p>
<p>It is mandatory.</p>
<p>The length should be 10 digits.</p>
<p>It should not contain any characters.</p>
<p>The data type we can use is primitive “long” in the entity class as all 10 digit numbers remains with in the range of long. The problem with long data type is that it gets initialized with 0 and the form when rendered shows that 0 by default. Now to overcome this developers prefers to use String data type as String is initialized with “null” value and the field when rendered remains blank. But they now need a custom validator to validate the length of the field and parse it to validate if it can be converted to a number. If conversion fails a NumberFormatException occurs and the developer in turn throws a ValidationException with a message “Number can't contain any characters”. We need to do this all because of that zero. If zero is a problem to you there is workaround for that. Instead of primitive “long” used the corresponding wrapper class “Long”. An object remains “null” if not initialized and your form will show blank when rendered. Now lets see how to apply validation on the field. For mandatory validation we use the attribute required for the input field and requiredMessage to display a message.</p>
<p><span style="font-family: courier new,courier;">required="true” requiredMessage="Please fill in your phone number"</span></p>
<p>For valid phone number JSF will try to convert the field value to the corresponding entity field. If conversion fails it will throw an exception and a default JSF message will be displayed on the form. We can use the attribute converterMessage to show our custom message.</p>
<p><span style="font-family: courier new,courier;">converterMessage="Only digits are allowed"</span></p>
<p>The best way to validate the maximum digits allowed in a phone number is not to allow the user enter more than the required number of digits in the first place. Making maxlength="10" will allow the user to enter only 10 digits. To validate minimum length we will use the namespace f with function validateLength</p>
<p><span style="font-family: courier new,courier;">&lt;f:validateLength minimum="10" maximum="10" /&gt;</span></p>
<p>The complete field with validation is now :</p>
<p><span style="font-family: courier new,courier;">&lt;h:inputText id="phoneNumber" value="#{userController.newUserBeingCreated.mobileNumber}" maxlength="10" required="true" requiredMessage="Please fill in your mobile number" validatorMessage="Should be 10 digits" converterMessage="Only digits are allowed" &gt;</span></p>
<p><span style="font-family: courier new,courier;">&lt;f:validateLength minimum="10" maximum="10" /&gt;</span></p>
<p><span style="font-family: courier new,courier;">&lt;/h:inputText&gt;</span></p>
<p><span style="font-family: courier new,courier;">&lt;h:message for="phoneNumber" errorClass="error" infoClass="info"/&gt;</span></p>
<p>This shows a simple way to validate a phone number field without any java code. Similarly other validation can also be applied as per the need.</p>
<p>Next we see validating a drop-down field.Sometimes we need to identify the user based on their gender. We show a drop-down box on the form to select a value from 'M' or 'F'. Showing a default value out of these two would be problem as user might overlook the field and proceed. It is better to show a text such as “Select” and ask the user to fill out their gender when no selection is made. So we can have 3 options as 'Select', 'M' and 'F' of which 'Select' is by default selected when the form is rendered and corresponds to an invalid selection. Now most developers implement a custom validator to check the value of drop-down box. If it is “Select” this means user has not made any selection and accordingly a message is shown to the user. But custom validator can be avoided by making use of the attribute “noSelectionOption” for the “Select” option which makes it corresponds to no selection.</p>
<p><span style="font-family: courier new,courier;">noSelectionOption="true"</span></p>
<p>Make the field mandatory with required="true" and put a message as</p>
<p><span style="font-family: courier new,courier;">requiredMessage="Please select a value"</span></p>
<p>The complete field with validation is</p>
<p><span style="font-family: courier new,courier;">&lt;h:selectOneMenu id ="gender" value="#{userController.newUserBeingCreated.gender}" required="true" requiredMessage="Please select a value"&gt;</span></p>
<p><span style="font-family: courier new,courier;">&lt;f:selectItem itemValue='' itemLabel="Select" noSelectionOption="true" /&gt;</span></p>
<p><span style="font-family: courier new,courier;">&lt;f:selectItem itemValue='M' itemLabel="M" /&gt;</span></p>
<p><span style="font-family: courier new,courier;">&lt;f:selectItem itemValue='F' itemLabel="F" /&gt;</span></p>
<p><span style="font-family: courier new,courier;">&lt;/h:selectOneMenu&gt;</span></p>
<p><span style="font-family: courier new,courier;">&lt;h:message for="gender" errorClass="error" infoClass="info" /&gt;</span></p>
<p>Next we'll see how to validate an email field. Most of you will agree the best way to validate an email field is to match it against a regular expression. The compiler uses a regular expression to generate a sequence of symbols. If the email follows that pattern it is considered valid. With the namespace f there is no need of any custom validators as the function validateRegex can be used directly to match the entered email against a regular expression. Use the attribute validatorMessage to provide a custom message when the validation fails.</p>
<p><span style="font-family: courier new,courier;">validatorMessage="Email is not valid"</span></p>
<p>The accuracy of validation lies in the regular expression that you write. Now an email can have characters in small or upper case and digits from 0 to 9 as well as some special characters like underscore, hyphen followed by mandatory '@' symbol and then a domain name. A sample regular expression would be like</p>
<p><span style="font-family: courier new,courier;">“([A-Za-z0-9._-$]|-)+@([A-Za-z0-9.]|-)+.[A-Za-z]{2,4}”</span></p>
<p>This regular expression allows some alphabets and digits and special characters like dot, underscore, hyphen and dollar symbol. Then it is followed by mandatory '@' symbol. Again some characters and lastly a dot followed by some more characters with minimum length 2 and maximum length 4. That means .n, .orgni are invalid. Valid ones are .in, .us, .org.</p>
<p>The regular expression is quite specific to the needs of your application. We have just seen a sample expression.</p>
<p>The complete field with validations is :</p>
<p><span style="font-family: courier new,courier;">&lt;h:inputText id="email" value="#{userController.newUserBeingCreated.email}" required="true"</span></p>
<p><span style="font-family: courier new,courier;">requiredMessage="Please fill in your email address" validatorMessage="Email is not valid"&gt;</span></p>
<p><span style="font-family: courier new,courier;">&lt;f:validateRegex pattern="([A-Za-z0-9._-$]|-)+@([A-Za-z0-9.]|-)+.[A-Za-z]{2,4}" /&gt;</span></p>
<p><span style="font-family: courier new,courier;">&lt;/h:inputText&gt;</span></p>
<p><span style="font-family: courier new,courier;">&lt;h:message for="email" errorClass="error" infoClass="info" /&gt;</span></p>