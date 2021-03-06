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

In the last blog, [DojoX form validation with custom error messages][1], I tried to explain how to validate a form with custom error messages using dojox.validate, but the drawback with that approach was that **it was not modular**. This blog is just an attempt to create your own custom **form** and **error** modules and use them to validate your form(s) without re-writing the validation code and doing things in Dojo way. 

## Overview

Defining modules in Asynchronous Module Definition (AMD) format helps the developers to develop the code and debug easily. The syntax for module identifier has changed in Dojo 1.7 from my.firstmodule.Rocks to my/firstmodule/Rocks, which also looks like actual paths too. Another thing to note is that a package contain modules so in our case 'my' is a package and firstmodule is a module. For detailed information, look "[Dojo 1.7 Defining Modules][2]", while you are at it please also look at the difference between _Requiring Modules_ and _Defining Modules_.

## _Create custom form validation module_

### HTML

Firstly, let's create a very simple HTML that would contain an error container, a registration form and a login form:

[sourcecode language="html"] <div id='errors'></div> <form name='register' id='register-form' method='POST' action="/register/"> <label for='remail'>Email</label><input type='text' value='' name='email' id='remail'/> <label for='rpassword'>Password</label><input type='password' value='' name='password' id='rpassword'/> <label for='rrepassword'>Re-type Password</label><input type='password' value='' name='repassword' id='rrepassword'/> <input type='submit' class='button' id='submit-register' value='Create my account' /> </form> <form name='login' id='login-form' method='POST' action="/login/"> <label for='lemail'>Email</label><input type='text' value='' name='email' id='lemail'/> <label for='lpassword'>Password</label><input type='password' value='' name='password' id='lpassword'/> <input type='submit' class='button' id='submit-login' value='Login' /> </form>

<!-- The contents of these two files are included at the end on this blog--> <script type='text/javascript' src="app/resources/static/js/validateRegister.js'></script> <script type='text/javascript' src="app/resources/static/js/validateLogin.js'></script> [/sourcecode]

### Directory Structure

Next, create the following two modules in 'my' package:

[sourcecode language="text"] static js/ dijit/ dojo/ dojox/ my/ error/ _base.js form/ _base.js helper.js error.js form.js util/

form.js : is the actual module file form/ : contain files for the form module _base.js : a class to represent a form helper.js : a helper class that attaches the onsubmit event to the current scoped form and other helper functions to print out the error messages

error.js: is the actual module file error/ : contain files for the error module _base.js : a class to represent errors, provides a constructor and a function that prints out the form errors in the error container (which we provide during form set-up, more on this later)

[/sourcecode]

It is completely up to you where you want to save your package or modules but you have to configure the Dojo loader to locate the modules correctly. The following shows how to enable you package so that the loader can locate your modules (read [here][3] to find out more about dojoConfig):

### Configure Package

[sourcecode language="javascript"] <script> var dojoConfig = { baseUrl: "'js/", tlmSiblingOfDojo: false, packages: [ { name: "dojo", location: "dojo" }, { name: "dijit", location: "dijit" }, { name: "dojox", location: "dojox" }, { name: "util", location: "util" }, { name: "my", location: "my" } ], async: true, aliases: [ ["ready", "dojo/ready"], ["xvalidate", "dojox/validate"], ["xcheck", "dojox/validate/check"], ["xweb", "dojox/validate/web"], ["dom","dojo/dom"], ["connect","dojo/_base/connect"], ["event","dojo/_base/event"], ["declare", "dojo/_base/declare"], ["lang", "dojo/_base/lang"], ["dom-class", "dojo/dom-class"], ] }; </script> [/sourcecode]

In the package array, the **name** could be anything you like and the **location** should be the actual path to you package (baseUrl/location). From now on all our modules wiould be accessible via 'my', Eg: require["my/form"]..., dojo loader would look for form.js in the js/my/ directory. 

### Form Module

The **my/form.js** module file is a very simple file that just defines or loads the required form object ( define callback always return a value, unlike require ).

[sourcecode language="javascript"] define(["./form/helper"], function(form){ return form; }); [/sourcecode]

But the main juice lies within the helper.js file in the "my/form/" directory. The helper class is nothing but an interface to my/form/_base.js that also makes use of error module to print the errors in the error container when a form fails to get validated.

[sourcecode language="javascript"] define(["lang", "declare", "./_base", "dom", "event", "connect", "../error/_base"], function(lang, declare, BaseFormValidator, dom, event, connect, myError){ var FormValidator = declare(null, { form: null, profile: null, errorContainer: 'errors', baseFormValidator: null,
    
    
        constructor: function(/*Object*/ args){
            declare.safeMixin(this, args);
        },
    
        getErrorsAsHTMLList : function(errors){
            // print errors as list
            var html = &quot;&quot;;
            if (errors){
                html = &quot;&lt;ul&gt;&quot;;
                for(var key in errors){
                    html += &quot;&lt;li&gt;&quot;+errors[key]+&quot;&lt;/li&gt;&quot;;
                }
                html += &quot;&lt;/ul&gt;&quot;;
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
    
    var helper = lang.getObject(&quot;my.form.helper&quot;, true);
    
    helper.attachOnSubmitEvent = function(formId, profile, errorContainer){
        var f = dom.byId(formId);
    

   [1]: http://xebee.xebia.in/2012/04/24/dojox-form-validation-with-custom-error-messages/
   [2]: http://dojotoolkit.org/documentation/tutorials/1.7/modules/
   [3]: http://dojotoolkit.org/documentation/tutorials/1.7/dojo_config/