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

<p>I was following <a href="http://dojotoolkit.org/documentation/tutorials/1.7/validation/">this tutorial</a> and got stuck when I wanted to show my custom error messages. I did not want to use the Dijit-based form widgets because I may change the JavaScript framework in future or in other words decoupled HTML form and validation. If I use Dijit-based form then it is no hassel to show my custom error but as I said that I do not want to use Dijit-based form for simplicity and modularity.</p>
<!--more-->

<p>Here is an example of how you could print your custom error message if you used the Dijit-based form:</p>
<p>[sourcecode language="html"]
&lt;input id=&quot;email&quot; name=&quot;email&quot;  data-dojo-type=&quot;dijit.form.ValidationTextBox&quot;<br />
        data-dojo-props=&quot;validator:dojox.validate.isEmailAddress,
        invalidMessage:'This is not a valid email!'&quot;  /&gt;
[/sourcecode]</p>
<p>but to decouple my HTML structure with the validation I chose to create a normal HTML form and applied Dojo valiations on it.</p>
<p>Here is the HTML:</p>
<p>[sourcecode language="html"]
&lt;div id='errors'&gt;&lt;/div&gt;
&lt;form name='register' id='register-form' method='POST' action=&quot;/register/&quot;&gt;
    &lt;label for='remail'&gt;Email&lt;/label&gt;&lt;input type='text' value='' name='email' id='remail'/&gt;
    &lt;label for='rpassword'&gt;Password&lt;/label&gt;&lt;input type='password' value='' name='password' id='rpassword'/&gt;
    &lt;label for='rrepassword'&gt;Re-type Password&lt;/label&gt;&lt;input type='password' value='' name='repassword' id='rrepassword'/&gt;
    &lt;input type='submit' class='button' id='submit-register' value='Create my account' /&gt;
&lt;/form&gt;
[/sourcecode]</p>
<p><strong>Assumption</strong>: I am assuming that you know how to add dojo.js to your source, if not look <a href="http://dojotoolkit.org/documentation/tutorials/1.7/hello_dojo/">here</a>.</p>
<p>Now, lets create the submit handler, form profile and the validator:</p>
<p>[sourcecode language="javascript"]
dojo.require('dojox.validate.web');
dojo.require('dojox.validate.check');</p>
<p>var profile;</p>
<p>function validateForm(form){
    var results = dojox.validate.check(form, profile);
    errors = [];
    if (!results.isSuccessful()){
        missing = results.getMissing();
        invalid = results.getInvalid();</p>
<pre><code>   // see the rest of the blog to find the code

}
return errors;
</code></pre>
<p>}</p>
<p>dojo.ready(function(){</p>
<pre><code>profile = {
    trim: [&amp;quot;email&amp;quot;],
    required: [&amp;quot;email&amp;quot;, &amp;quot;password&amp;quot;, &amp;quot;repassword&amp;quot;],
    constraints : {
        email: [dojox.validate.isEmailAddress, false, true],
        password: dojox.validate.isText,
        repassword: dojox.validate.isText
    },
    confirm: {
        &amp;quot;repassword&amp;quot;: &amp;quot;password&amp;quot;
    }
}

// check onsubmit event and show errors, if any
var f = dojo.query(&amp;quot;#register-form&amp;quot;)[0];
f.onsubmit = function(e){
    dojo.stopEvent(e);
    errors = validateForm(f);
    if (!errors){
        // no errors found, submit your form
    } else {
        var msgs = &amp;quot;&amp;lt;ul&amp;gt;&amp;quot;;
        for(var key in errors){
            msgs += &amp;quot;&amp;lt;li&amp;gt;&amp;quot;+errors[key]+&amp;quot;&amp;lt;/li&amp;gt;&amp;quot;;
        }
        msgs += &amp;quot;&amp;lt;/ul&amp;gt;&amp;quot;;
        dojo.query('#errors')[0].innerHTML = msgs;
    }
};
</code></pre>
<p>});
[/sourcecode]</p>
<p>The above code still doesn't give you a way to add your custom messages. I added the following two arrays in my form 'profile' ( key names are self-explanatory):</p>
<p>[sourcecode language="javascript"]
    //final form profile
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
        },
        // please be more creative with error messages
        invalid: {
          email : &quot;Invalid email&quot;,
          password: &quot;Invalid password&quot;,
          repassword: &quot;Invalid confirm password&quot;
        },
        missing: {
            email: &quot;Missing email&quot;,
            password: &quot;Missing password&quot;,
            repassword: &quot;Missing confirm password&quot;
        }
    }
[/sourcecode]</p>
<p>Next, we will update our validateForm() function so that we could grep the correct error messages, save them in the errors array and return it to the submit handler.</p>
<p><em><strong>PS: The getMissing() and getInvalid() returns the 'name' of the element and not the 'id'. Please be careful when you define your own 'invalid' and 'missing' items for form profile, make sure you use the field names and not the ids.</strong></em></p>
<p>Here is the complete validationForm() function, which returns your custom error messages defined in the form 'profile' variable:</p>
<p>[sourcecode language="javascript"]
function validateForm(form){
    var results = dojox.validate.check(form, profile);
    errors = [];
    if (!results.isSuccessful()){
        missing = results.getMissing();
        invalid = results.getInvalid();
        for(var i in missing){
            // missing[i] contains the name of the element
            msg = profile['missing'][missing[i]];
            if (msg) {
                errors.push(msg);
            }
        }
        for(var i in invalid){
            // invalid[i] contains the name of the element
            msg = profile['invalid'][invalid[i]];
            if (msg) {
                errors.push(msg);
            }
        }
    }
    return errors;
}
[/sourcecode]</p>
<p><strong>Summary:</strong>
1. Create a normal HTML form
2. Use dojo's onsubmit handler to handle the default event
3. Add 'invalid' and 'missing' arrays to the form profile ( use field names and not ids for array keys )
4. In your validator function get all the missing and invalid field names
5. Iterate over both missing and invalid array and fetch the custom error message from the form profile</p>
<p>That's it folks...</p>