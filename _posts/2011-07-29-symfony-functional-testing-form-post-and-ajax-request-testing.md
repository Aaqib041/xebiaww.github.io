---
layout: post
header-img: img/default-blog-pic.jpg
author: ramsharma
description: 
post_id: 9493
created: 2011/07/29 12:11:16
created_gmt: 2011/07/29 07:11:16
comment_status: open
---

# Symfony Functional Testing - Form POST and AJAX Request Testing

<p>Symfony is a one of the best open-source PHP web application frameworks with a huge and comprehensive documentation. Symfony comes with default inbuilt testing framework for functional and unit testing. I recently started working on Symfony functional test cases. I personally found the testing framework very strong and useful.</p>
<p>When I was writing functional test cases for my recent application, I came across with various scenarios where Symfony's documentation lacks or very limited information available on web directly. Here, I am sharing my experiences and solutions for the various scenarios of functional testing.</p>
<p>Testing post request with forms:
Let's take an example if you have any user registration feature and you want to test it (here is a simple code that I used to test):</p>
<p>Assuming that you have registration functionality working with Symfony framework and its URL is http://www.xyz.com/register and it shows a form:</p>
<p>[sourcecode language="html"]
&lt;form action=&quot;/register&quot; method=&quot;post&quot; &gt;
 &lt;tr&gt;
   &lt;th&gt;&lt;label for=&quot;signup_username&quot;&gt;User Name&lt;/label&gt;&lt;/th&gt;
   &lt;td&gt;&lt;input type=&quot;text&quot; name=&quot;signup[username]&quot; id=&quot;signup_username&quot; /&gt;&lt;/td&gt;
 &lt;/tr&gt;
 &lt;tr&gt;
   &lt;th&gt;&lt;label for=&quot;signup_email&quot;&gt;Email&lt;/label&gt;&lt;/th&gt;
   &lt;td&gt;&lt;input type=&quot;text&quot; name=&quot;signup[email]&quot; id=&quot;signup_email&quot; /&gt;&lt;/td&gt;
 &lt;/tr&gt;
 &lt;tr&gt;
   &lt;th&gt;&lt;label for=&quot;signup_password&quot;&gt;Password&lt;/label&gt;&lt;/th&gt;
   &lt;td&gt;&lt;input type=&quot;password&quot; name=&quot;signup[password]&quot; id=&quot;signup_password&quot; /&gt;&lt;/td&gt;
 &lt;/tr&gt;
 &lt;tr&gt;
   &lt;th&gt;&lt;label for=&quot;signup_password_confirmation&quot;&gt;Confirm Password&lt;/label&gt;&lt;/th&gt;
   &lt;td&gt;&lt;input type=&quot;password&quot; name=&quot;signup[password_confirmation]&quot; id=&quot;signup_password_confirmation&quot; /&gt;&lt;/td&gt;
 &lt;/tr&gt;
 &lt;/form&gt;
&lt;pre&gt;[/sourcecode]</p>
<!--more-->

<p></pre>
I am assuming you have a fair idea of writing functional test cases in Symfony. Now to create a functional test case. Create a test file RegisterTest.php in your project's test/functional directory, you can do it manually or if you have used Symfony CLI to generate your module, it will automatically generate the test file and it will look like the following by default:</p>
<p>[sourcecode language="php"]
 include(dirname(<strong>FILE</strong>).'/../../bootstrap/functional.php');</p>
<p>$browser = new sfTestFunctional(new sfBrowser());</p>
<p>$browser-&gt;
   get('/register')-&gt;
   with('request')-&gt;begin()-&gt;
     isParameter('module', 'UserRegistration')-&gt;
     isParameter('action', 'index')-&gt;
   end()-&gt;
   with('response')-&gt;begin()-&gt;
     isStatusCode(200)-&gt;
     checkElement('body', '!/This is a temporary page/')-&gt;
  end();
 [/sourcecode]</p>
<p>To test general success of the registration page, if it  is properly loaded in the browser, update the above code with the following:</p>
<p>[sourcecode language="php"]
 $browser-&gt;
   get('/register')-&gt;
   with('request')-&gt;begin()-&gt;
     isParameter('module', 'UserRegistration')-&gt;
     isParameter('action', 'index')-&gt;
   end()-&gt;
   with('response')-&gt;begin()-&gt;
     isStatusCode(200)-&gt;
     checkElement('form input[type=&quot;text&quot;][name=&quot;signin[username]&quot;]',true)-&gt;
     checkElement('form input[type=&quot;text&quot;][name=&quot;signin[password]&quot;]',true)-&gt;
 end();
 [/sourcecode]</p>
<p>I am just checking only 2 fields of the form but you can check as many fields as you want. The above code will make a GET request to '/register' through a mock browser object '$browser' and check the response if the form have those required elements in it.</p>
<p>[sourcecode language="php"]
 input[type=&quot;text&quot;][name=&quot;signin[username]&quot;] ~ &lt;input type=&quot;text&quot; name=&quot;signup[username]&quot; id=&quot;signup_username&quot; /&gt;
 [/sourcecode]</p>
<p>When you run the above test case via Symfony command line tool:</p>
<p>[sourcecode language="bash"]
 symfony test:functional yourapp RegisterTest
 [/sourcecode]</p>
<p>it will give you a result something like:</p>
<p>[sourcecode language="php"]
 # get /register
 ok 1 - status code is 200
 ok 2 - response selector form input[type=text][name=signin[username]] exists
 ok 2 - response selector form input[type=text][name=signin[password]] exists
 [/sourcecode]</p>
<p>Now to extend the test case to test the functionality for actually registering the user with the form, use the code below:</p>
<p>[sourcecode language="php"]
 $browser-&gt;
   get('/register')-&gt;
   with('request')-&gt;begin()-&gt;
     isParameter('module', 'UserRegistration')-&gt;
     isParameter('action', 'index')-&gt;
   end()-&gt;
   with('response')-&gt;begin()-&gt;
     isStatusCode(200)-&gt;
     checkElement('form input[type=&quot;text&quot;][name=&quot;signin[username]&quot;]',true)-&gt;
     checkElement('form input[type=&quot;text&quot;][name=&quot;signin[password]&quot;]',true)-&gt;
   end()-&gt;
   with('form')-&gt;begin()-&gt;
     setField('signup[username]', 'abc')-&gt;
     setField('signup[email_address]', 'abc@xyz.com')-&gt;
     setField('signup[password]', 'xyz')-&gt;
     setField('signup[password_confirmation]', 'xyz')-&gt;
   click('save')-&gt;
 end();
 [/sourcecode]</p>
<p>Assuming the code redirects the user to some other page after successfully registration:</p>
<p>[sourcecode language="php"]
 with('form')-&gt;begin()-&gt;
   setField('signup[username]', 'abc')-&gt;
   setField('signup[email_address]', 'abc@xyz.com')-&gt;
   setField('signup[password]', 'xyz')-&gt;
   setField('signup[password_confirmation]', 'xyz')-&gt;
   click('save')-&gt;
 end()-&gt;
 with('response')-&gt;begin()-&gt;
   isRedirected()-&gt;
   isStatusCode(302)-&gt;
   followRedirect()-&gt;
 end();
 [/sourcecode]</p>
<p>Now there is a case in which the form had some errors while submitting, it will fail your test:</p>
<p>[sourcecode language="php"]
 isRedirected()-&gt;
 isStatusCode(302)-&gt;
 [/sourcecode]</p>
<p>and you have no clue why it is failing? What was the error on the form? or if the form had an error while submitting the request. Use the following code to check that:</p>
<p>[sourcecode language="php"]
 with('form')-&gt;begin()-&gt;
   setField('signup[username]', 'abc')-&gt;
   setField('signup[email_address]', 'abc@xyz.com')-&gt;
   setField('signup[password]', 'xyz')-&gt;
   setField('signup[password_confirmation]', 'xyz')-&gt;
   click('save')-&gt;
 end()-&gt;
 with('form')-&gt;begin()-&gt;
   hasErrors(false)-&gt;
 end()-&gt;
 with('response')-&gt;begin()-&gt;
   isRedirected()-&gt;
   isStatusCode(302)-&gt;
   followRedirect()-&gt;
 end();
 [/sourcecode]</p>
<p>By using the above code at-least you can now know that the form had errors and you can use debug method of the $browser object to debug that error:</p>
<p>[sourcecode language="php"]
 with('form')-&gt;begin()-&gt;
   hasErrors(false)-&gt;
   debug()-&gt;
 end()-&gt;
 [/sourcecode]</p>
<p>This is how you can write and debug test cases for Form POST request testing scenarios.</p>
<p>There is another interesting scenario I came across for AJAX request testing in Symfony application. You will hardly find information on the web directly, which talks about AJAX requests testing, except a small paragraph on <a title="Symfony AJAX Request Testing" href="http://www.symfony-project.org/jobeet/1_4/Doctrine/en/18" target="_blank">http://www.symfony-project.org/jobeet/1_4/Doctrine/en/18</a></p>
<p>The following simple test case includes both the scenarios: AJAX GET and AJAX POST request testing:</p>
<p>This is a simple scenario to add a new book in the books list in a library management system with AJAX. In this scenario we are testing that AJAX GET request should take a Book form in response with some basic fields and after putting the data in the form the page posts the data back to the server, if the book is added successfully it will send a redirection with Status code 302.</p>
<p>[sourcecode language="php"]
 $browser-&gt;setHttpHeader('X-Requested-With', 'XMLHttpRequest')-&gt;
   get('/book/add')-&gt;</p>

## Comments

**[Sarker](#8945 "2012-06-04 09:22:00"):** after running a functional test from CLI i want the result as a report (in a txt or word file). how can i do this?

**[Pankaj Sahni](#6834 "2012-01-11 15:33:34"):** best documentation i found on internet about symfony's functional testing. even better than jobeet tutorial. keep posting.

