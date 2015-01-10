---
layout: post
header-img: img/default-blog-pic.jpg
author: GauravS
description: 
post_id: 7934
created: 2011/02/28 23:46:12
created_gmt: 2011/02/28 18:46:12
comment_status: open
---

# Flex: Validator versus TextInput's restrict

<p>I came across a requirement where i need to restrict user from entering numeric values. The first solution that came into my mind was to simply restrict the input textfield, by using TextInput's restrict method.</p>
<p>During the sprint demo, a small suggestion came (from a usability perspective) - <em>Instead of restricting user, can't i alert him with some validation errors?</em>. It was a nice thought, as user can be shown with the reasons of this error, which is not possible if we simply 'restrict' the user. <!--more--></p>
<p>So after a small R&amp;D, i was able to write a validator for the same. <a href="http://xebee.xebia.in/wp-content/uploads/2011/03/ValidateAlphabets.zip">Attached</a> is the code. The end result was - The textfield will show validation errors if there are any numeric values found.</p>
<p>The main trick here was to override the 'doValidation()' method.</p>
<p>The highlights of this method are:
<ul>
    <li> Override this method in your custom validator class</li>
    <li> Contains the logic for validation.</li>
    <li> The result of validation is stored in _validationResults, which accepts objects of type ValidationResult.</li>
    <li> ValidationResult contains the error message to be displayed in the tooltip.</li>
</ul>
[code]
override protected function doValidation(value:Object):Array {
            var trimmedValue:String = StringUtil.trim(String(value));
            _validationResults = super.doValidation(trimmedValue);</p>
<pre><code>        if(_validationResults.length &amp;gt; 0 || trimmedValue.length == 0)
            return _validationResults;

        if(!CharacterUtil.validateCharacters(DECIMAL_DIGITS, trimmedValue)) {
            _validationResults.push(new ValidationResult(true, null, 
                            ApplicationConstants.INVALID_CHARACTER, 
                            _messageForInvalidCharacter));
            return _validationResults;
        }
        else {
            Alert.show(&amp;quot;No validation errors found&amp;quot;, &amp;quot;Alert&amp;quot;);
        }

        return _validationResults;
    }
</code></pre>
<p>[/code]</p>
<p>CharacterUtil is the class which encapsulates the logic to check for the numeric values in the entered characters. If you want to write your custom validator, just change the method written inside this class, with your checks and conditions.</p>

## Comments

**[Gaurav Srivastava](#5575 "2011-05-16 14:26:06"):** Agree. But as per the requirements, we had many custom validation rules. So in all cases, we have to override doValidation() to write any custom implementation.

**[Vijay Mareddy](#5567 "2011-05-11 01:07:44"):** You could use the NumberValidator which has a validateNumber function

