---
layout: post
header-img: img/default-blog-pic.jpg
author: sohil
description: 
post_id: 5524
created: 2010/10/12 01:40:52
created_gmt: 2010/10/11 20:40:52
comment_status: open
---

# Flex :  Increase your productivity while working on a bigger application

<p>Once the project starts growing you repeatably spend your time performing same actions e.g. Login ,  fill form,etc . Lets consider following use case  I have to add two new validation in the current form which takes more than 5-6 inputs fields. Also to open the form i will have to perform 4-5 steps like  login , go to settings page , click on link , click on create button and wait for the form to open , fill all the fields and once you are done with all the required fields fill the two new fields and validate.<br /> Whoops there is a error, something didn't work while saving the form try again :) .This approach can be pretty nasty as you spend a lot of developer time doing the repetitive steps<!--more--></p>

<p><p>One of the approach is you hard code field (username , password) the values inside the form and once the application starts click on appropriate buttons and continue with you test. This approach might work for some but is a little dangerous, last thing you wanna know is that the username and password is committed into the code.</p> <p>We had a similar condition , while working we had to go deep down to a particular container follow the same inputs repeatably and then continue our work.<br /> This is where flex conditional compiling came to rescue.Using flex conditional compiling we could add in all the code that we did not want<br /> to go to production and still have it in the Application. In the example below we will see a simpler way to add flex conditional compiling code.</p> <p>In Flex , you can pass additional constants during compile time using the  -define as  compiler argument. Following is the syntax you add to your compiler</p> <pre>[sourcecode language="java"]</p>
<p>-define=NAMESPACE::variable,value</p>
<p>[/sourcecode]</pre> <p>Where the namespace can be anything like DEBUG , RELEASE and value of the variable can be Boolean, String, or Number, or an expression that can be evaluated in  ActionScript at compile  time.In the below example we are going to create a namespace DEBUG and pass the variable insertCode as BooleanOnce you have passed the variable as compiler argument this can be further used in the flex code using&lt;/p&gt;</p> <pre>[sourcecode language="java"]DEBUG::insertCode[/sourcecode]</pre> <p>In flex to allow a block</p> <pre>[sourcecode language="java"]
    { </p>
<pre><code>}
</code></pre>
<p>[/sourcecode]</pre> <p>to be complied or not is decided by the  evaluating expression before the block. If the expression evaluated to be true the block is add to the compile code</p> <pre>[sourcecode language="java"]</p>
<p>expression{</p>
<p>}</p>
<p>[/sourcecode]</pre> <p>Below is the example showing how I use conditional compiling to speedup the application development. Click on this link  <a href="http://xebee.xebia.in/wp-content/uploads/2010/10/ConditionalCompiling.zip">ConditionalCompiling</a> to download the project and check it out.</p> <p>Following is the video demonstrating the use of conditional compiling</p> <p>Video tutorial <a href="http://xebee.xebia.in/wp-content/uploads/2010/10/Flex_Conditional_Compiling.swf">Flex_Conditional_Compiling</a></p> <p>Please do let me know your reviews for the blogs.</p></p>

## Comments

**[Abbas](#3021 "2010-10-12 22:45:45"):** Nice post

**[Sohil](#3024 "2010-10-13 10:15:03"):** Thanks Abbas :)

