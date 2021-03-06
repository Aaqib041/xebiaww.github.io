---
layout: post
header-img: img/default-blog-pic.jpg
author: anubhava
description: 
post_id: 13319
created: 2012/04/24 17:47:14
created_gmt: 2012/04/24 12:47:14
comment_status: open
---

# DojoX form validation with custom error messages

I was following [this tutorial][1] and got stuck when I wanted to show my custom error messages. I did not want to use the Dijit-based form widgets because I may change the JavaScript framework in future or in other words decoupled HTML form and validation. If I use Dijit-based form then it is no hassel to show my custom error but as I said that I do not want to use Dijit-based form for simplicity and modularity.

Here is an example of how you could print your custom error message if you used the Dijit-based form:

[sourcecode language="html"] <input id="email" name="email" data-dojo-type="dijit.form.ValidationTextBox"  
data-dojo-props="validator:dojox.validate.isEmailAddress, invalidMessage:'This is not a valid email!'" /> [/sourcecode]

but to decouple my HTML structure with the validation I chose to create a normal HTML form and applied Dojo valiations on it.

Here is the HTML:

[sourcecode language="html"] <div id='errors'></div> <form name='register' id='register-form' method='POST' action="/register/"> <label for='remail'>Email</label><input type='text' value='' name='email' id='remail'/> <label for='rpassword'>Password</label><input type='password' value='' name='password' id='rpassword'/> <label for='rrepassword'>Re-type Password</label><input type='password' value='' name='repassword' id='rrepassword'/> <input type='submit' class='button' id='submit-register' value='Create my account' /> </form> [/sourcecode]

**Assumption**: I am assuming that you know how to add dojo.js to your source, if not look [here][2].

Now, lets create the submit handler, form profile and the validator:

[sourcecode language="javascript"] dojo.require('dojox.validate.web'); dojo.require('dojox.validate.check');

var profile;

function validateForm(form){ var results = dojox.validate.check(form, profile); errors = []; if (!results.isSuccessful()){ missing = results.getMissing(); invalid = results.getInvalid();
    
    
       // see the rest of the blog to find the code
    
    }
    return errors;
    

}

dojo.ready(function(){
    
    
    profile = {
        trim: [&quot;email&quot;],
        required: [&quot;email&quot;, &quot;password&quot;, &quot;repassword&quot;],
        constraints : {
            email: [dojox.validate.isEmailAddress, false, true],
            password: dojox.validate.isText,
            repassword: dojox.validate.isText
        },
        confirm: {
            &quot;repassword&quot;: &quot;password&quot;
        }
    }
    
    // check onsubmit event and show errors, if any
    var f = dojo.query(&quot;#register-form&quot;)[0];
    f.onsubmit = function(e){
        dojo.stopEvent(e);
        errors = validateForm(f);
        if (!errors){
            // no errors found, submit your form
        } else {
            var msgs = &quot;&lt;ul&gt;&quot;;
            for(var key in errors){
                msgs += &quot;&lt;li&gt;&quot;+errors[key]+&quot;&lt;/li&gt;&quot;;
            }
            msgs += &quot;&lt;/ul&gt;&quot;;
            dojo.query('#errors')[0].innerHTML = msgs;
        }
    };
    

}); [/sourcecode]

The above code still doesn't give you a way to add your custom messages. I added the following two arrays in my form 'profile' ( key names are self-explanatory):

[sourcecode language="javascript"] //final form profile profile = { trim: ["email"], required: ["email", "password", "repassword"], constraints : { email: [dojox.validate.isEmailAddress, false, true], password: dojox.validate.isText, repassword: dojox.validate.isText }, confirm: { "repassword": "password" }, // please be more creative with error messages invalid: { email : "Invalid email", password: "Invalid password", repassword: "Invalid confirm password" }, missing: { email: "Missing email", password: "Missing password", repassword: "Missing confirm password" } } [/sourcecode]

Next, we will update our validateForm() function so that we could grep the correct error messages, save them in the errors array and return it to the submit handler.

_**PS: The getMissing() and getInvalid() returns the 'name' of the element and not the 'id'. Please be careful when you define your own 'invalid' and 'missing' items for form profile, make sure you use the field names and not the ids.**_

Here is the complete validationForm() function, which returns your custom error messages defined in the form 'profile' variable:

[sourcecode language="javascript"] function validateForm(form){ var results = dojox.validate.check(form, profile); errors = []; if (!results.isSuccessful()){ missing = results.getMissing(); invalid = results.getInvalid(); for(var i in missing){ // missing[i] contains the name of the element msg = profile['missing'][missing[i]]; if (msg) { errors.push(msg); } } for(var i in invalid){ // invalid[i] contains the name of the element msg = profile['invalid'][invalid[i]]; if (msg) { errors.push(msg); } } } return errors; } [/sourcecode]

**Summary:** 1\. Create a normal HTML form 2\. Use dojo's onsubmit handler to handle the default event 3\. Add 'invalid' and 'missing' arrays to the form profile ( use field names and not ids for array keys ) 4\. In your validator function get all the missing and invalid field names 5\. Iterate over both missing and invalid array and fetch the custom error message from the form profile

That's it folks...

   [1]: http://dojotoolkit.org/documentation/tutorials/1.7/validation/
   [2]: http://dojotoolkit.org/documentation/tutorials/1.7/hello_dojo/