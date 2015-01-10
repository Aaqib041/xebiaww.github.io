---
layout: post
header-img: img/default-blog-pic.jpg
author: anubhava
description: 
post_id: 13578
created: 2012/05/03 12:29:30
created_gmt: 2012/05/03 07:29:30
comment_status: open
---

# Form Validation With Custom Modules In Dojo 1.7

<p>In the last blog, <a href="http://xebee.xebia.in/2012/04/24/dojox-form-validation-with-custom-error-messages/">DojoX form validation with custom error messages</a>, I tried to explain how to validate a form with custom error messages using dojox.validate, but the drawback with that approach was that <strong>it was not modular</strong>. This blog is just an attempt to create your own custom <strong>form</strong> and <strong>error</strong> modules and use them to validate your form(s) without re-writing the validation code and doing things in Dojo way.
<!--more--></p>
<h2>Overview</h2>

<p>Defining modules in Asynchronous Module Definition (AMD) format helps the developers to develop the code and debug easily. The syntax for module identifier has changed in Dojo 1.7 from my.firstmodule.Rocks to my/firstmodule/Rocks, which also looks like actual paths too. Another thing to note is that a package contain modules so in our case 'my' is a package and firstmodule is a module. For detailed information, look "<a href="http://dojotoolkit.org/documentation/tutorials/1.7/modules/">Dojo 1.7 Defining Modules</a>", while you are at it please also look at the difference between <em>Requiring Modules</em> and <em>Defining Modules</em>.</p>
<h2><u>Create custom form validation module</u></h2>

<h3>HTML</h3>

<p>Firstly, let's create a very simple HTML that would contain an error container, a registration form and a login form:</p>
<p>[sourcecode language="html"]
&lt;div id='errors'&gt;&lt;/div&gt;
&lt;form name='register' id='register-form' method='POST' action=&quot;/register/&quot;&gt;
    &lt;label for='remail'&gt;Email&lt;/label&gt;&lt;input type='text' value='' name='email' id='remail'/&gt;
    &lt;label for='rpassword'&gt;Password&lt;/label&gt;&lt;input type='password' value='' name='password' id='rpassword'/&gt;
    &lt;label for='rrepassword'&gt;Re-type Password&lt;/label&gt;&lt;input type='password' value='' name='repassword' id='rrepassword'/&gt;
    &lt;input type='submit' class='button' id='submit-register' value='Create my account' /&gt;
&lt;/form&gt;
&lt;form name='login' id='login-form' method='POST' action=&quot;/login/&quot;&gt;
    &lt;label for='lemail'&gt;Email&lt;/label&gt;&lt;input type='text' value='' name='email' id='lemail'/&gt;
    &lt;label for='lpassword'&gt;Password&lt;/label&gt;&lt;input type='password' value='' name='password' id='lpassword'/&gt;
    &lt;input type='submit' class='button' id='submit-login' value='Login' /&gt;
&lt;/form&gt;</p>
<p>&lt;!-- The contents of these two files are included at the end on this blog--&gt;
&lt;script type='text/javascript' src=&quot;app/resources/static/js/validateRegister.js'&gt;&lt;/script&gt;
&lt;script type='text/javascript' src=&quot;app/resources/static/js/validateLogin.js'&gt;&lt;/script&gt;
[/sourcecode]</p>
<h3>Directory Structure</h3>

<p>Next, create the following two modules in 'my' package:</p>
<p>[sourcecode language="text"]
static
   js/
     dijit/
     dojo/
     dojox/
     my/
        error/
            _base.js
        form/
            _base.js
            helper.js
        error.js
        form.js
     util/</p>
<p>form.js : is the actual module file
form/ : contain files for the form module
          _base.js : a class to represent a form
          helper.js : a helper class that attaches the onsubmit event to the current scoped form 
                      and other helper functions to print out the error messages</p>
<p>error.js: is the actual module file
error/ : contain files for the error module
         _base.js : a class to represent errors, provides a constructor and a function 
                    that prints out the form errors in the error container 
                    (which we provide during form set-up, more on this later)</p>
<p>[/sourcecode]</p>
<p>It is completely up to you where you want to save your package or modules but you have to configure the Dojo loader to locate the modules correctly. The following shows how to enable you package so that the loader can locate your modules (read <a href="http://dojotoolkit.org/documentation/tutorials/1.7/dojo_config/">here</a> to find out more about dojoConfig):</p>
<h3>Configure Package</h3>

<p>[sourcecode language="javascript"]
&lt;script&gt;
            var dojoConfig = {
                baseUrl: &quot;'js/&quot;,
                tlmSiblingOfDojo: false,
                packages: [
                    { name: &quot;dojo&quot;, location: &quot;dojo&quot; },
                    { name: &quot;dijit&quot;, location: &quot;dijit&quot; },
                    { name: &quot;dojox&quot;, location: &quot;dojox&quot; },
                    { name: &quot;util&quot;, location: &quot;util&quot; },
                    { name: &quot;my&quot;, location: &quot;my&quot; }
                ],
                async: true,
                aliases: [
                    [&quot;ready&quot;, &quot;dojo/ready&quot;],
                    [&quot;xvalidate&quot;, &quot;dojox/validate&quot;],
                    [&quot;xcheck&quot;, &quot;dojox/validate/check&quot;],
                    [&quot;xweb&quot;, &quot;dojox/validate/web&quot;],
                    [&quot;dom&quot;,&quot;dojo/dom&quot;],
                    [&quot;connect&quot;,&quot;dojo/_base/connect&quot;],
                    [&quot;event&quot;,&quot;dojo/_base/event&quot;],
                    [&quot;declare&quot;, &quot;dojo/_base/declare&quot;],
                    [&quot;lang&quot;, &quot;dojo/_base/lang&quot;],
                    [&quot;dom-class&quot;, &quot;dojo/dom-class&quot;],
                ]
            };
&lt;/script&gt;
[/sourcecode]</p>
<p>In the package array, the <strong>name</strong> could be anything you like and the <strong>location</strong> should be the actual path to you package (baseUrl/location). From now on all our modules wiould be accessible via 'my', Eg: require["my/form"]..., dojo loader would look for form.js in the js/my/ directory. </p>
<h3>Form Module</h3>

<p>The <strong>my/form.js</strong> module file is a very simple file that just defines or loads the required form object ( define callback always return a value, unlike require ).</p>
<p>[sourcecode language="javascript"]
define([&quot;./form/helper&quot;], function(form){
    return form;
});
[/sourcecode]</p>
<p>But the main juice lies within the helper.js file in the "my/form/" directory. The helper class is nothing but an interface to my/form/_base.js that also makes use of error module to print the errors in the error container when a form fails to get validated.</p>
<p>[sourcecode language="javascript"]
define([&quot;lang&quot;, &quot;declare&quot;, &quot;./_base&quot;, &quot;dom&quot;, &quot;event&quot;, &quot;connect&quot;, &quot;../error/_base&quot;], 
function(lang, declare, BaseFormValidator, dom, event, connect, myError){
    var FormValidator = declare(null, {
        form: null,
        profile: null,
        errorContainer: 'errors',
        baseFormValidator: null,</p>
<pre><code>    constructor: function(/*Object*/ args){
        declare.safeMixin(this, args);
    },

    getErrorsAsHTMLList : function(errors){
        // print errors as list
        var html = &amp;quot;&amp;quot;;
        if (errors){
            html = &amp;quot;&amp;lt;ul&amp;gt;&amp;quot;;
            for(var key in errors){
                html += &amp;quot;&amp;lt;li&amp;gt;&amp;quot;+errors[key]+&amp;quot;&amp;lt;/li&amp;gt;&amp;quot;;
            }
            html += &amp;quot;&amp;lt;/ul&amp;gt;&amp;quot;;
        }
        return html;
    },

    submitOrPrintErrors: function() {
        // validates form and if an error occurred then display the errors in the
        // error container
        var status = this.baseFormValidator.validateForm();
        if (status) {
            this.baseFormValidator.submitForm();
        } else {
            var html = this.getErrorsAsHTMLList(this.baseFormValidator.getErrors())
            var eObj = new my.Error({errorContainer: this.errorContainer, errorHTML: html});
            eObj.animateErrors();
        }
    }
});

var helper = lang.getObject(&amp;quot;my.form.helper&amp;quot;, true);

helper.attachOnSubmitEvent = function(formId, profile, errorContainer){
    var f = dom.byId(formId);
</code></pre>