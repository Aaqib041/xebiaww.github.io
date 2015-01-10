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

Symfony is a one of the best open-source PHP web application frameworks with a huge and comprehensive documentation. Symfony comes with default inbuilt testing framework for functional and unit testing. I recently started working on Symfony functional test cases. I personally found the testing framework very strong and useful.

When I was writing functional test cases for my recent application, I came across with various scenarios where Symfony's documentation lacks or very limited information available on web directly. Here, I am sharing my experiences and solutions for the various scenarios of functional testing.

Testing post request with forms: Let's take an example if you have any user registration feature and you want to test it (here is a simple code that I used to test):

Assuming that you have registration functionality working with Symfony framework and its URL is http://www.xyz.com/register and it shows a form:

[sourcecode language="html"] <form action="/register" method="post" > <tr> <th><label for="signup_username">User Name</label></th> <td><input type="text" name="signup[username]" id="signup_username" /></td> </tr> <tr> <th><label for="signup_email">Email</label></th> <td><input type="text" name="signup[email]" id="signup_email" /></td> </tr> <tr> <th><label for="signup_password">Password</label></th> <td><input type="password" name="signup[password]" id="signup_password" /></td> </tr> <tr> <th><label for="signup_password_confirmation">Confirm Password</label></th> <td><input type="password" name="signup[password_confirmation]" id="signup_password_confirmation" /></td> </tr> </form> <pre>[/sourcecode]

I am assuming you have a fair idea of writing functional test cases in Symfony. Now to create a functional test case. Create a test file RegisterTest.php in your project's test/functional directory, you can do it manually or if you have used Symfony CLI to generate your module, it will automatically generate the test file and it will look like the following by default:

[sourcecode language="php"] include(dirname(**FILE**).'/../../bootstrap/functional.php');

$browser = new sfTestFunctional(new sfBrowser());

$browser-> get('/register')-> with('request')->begin()-> isParameter('module', 'UserRegistration')-> isParameter('action', 'index')-> end()-> with('response')->begin()-> isStatusCode(200)-> checkElement('body', '!/This is a temporary page/')-> end(); [/sourcecode]

To test general success of the registration page, if it  is properly loaded in the browser, update the above code with the following:

[sourcecode language="php"] $browser-> get('/register')-> with('request')->begin()-> isParameter('module', 'UserRegistration')-> isParameter('action', 'index')-> end()-> with('response')->begin()-> isStatusCode(200)-> checkElement('form input[type="text"][name="signin[username]"]',true)-> checkElement('form input[type="text"][name="signin[password]"]',true)-> end(); [/sourcecode]

I am just checking only 2 fields of the form but you can check as many fields as you want. The above code will make a GET request to '/register' through a mock browser object '$browser' and check the response if the form have those required elements in it.

[sourcecode language="php"] input[type="text"][name="signin[username]"] ~ <input type="text" name="signup[username]" id="signup_username" /> [/sourcecode]

When you run the above test case via Symfony command line tool:

[sourcecode language="bash"] symfony test:functional yourapp RegisterTest [/sourcecode]

it will give you a result something like:

[sourcecode language="php"] # get /register ok 1 - status code is 200 ok 2 - response selector form input[type=text][name=signin[username]] exists ok 2 - response selector form input[type=text][name=signin[password]] exists [/sourcecode]

Now to extend the test case to test the functionality for actually registering the user with the form, use the code below:

[sourcecode language="php"] $browser-> get('/register')-> with('request')->begin()-> isParameter('module', 'UserRegistration')-> isParameter('action', 'index')-> end()-> with('response')->begin()-> isStatusCode(200)-> checkElement('form input[type="text"][name="signin[username]"]',true)-> checkElement('form input[type="text"][name="signin[password]"]',true)-> end()-> with('form')->begin()-> setField('signup[username]', 'abc')-> setField('signup[email_address]', 'abc@xyz.com')-> setField('signup[password]', 'xyz')-> setField('signup[password_confirmation]', 'xyz')-> click('save')-> end(); [/sourcecode]

Assuming the code redirects the user to some other page after successfully registration:

[sourcecode language="php"] with('form')->begin()-> setField('signup[username]', 'abc')-> setField('signup[email_address]', 'abc@xyz.com')-> setField('signup[password]', 'xyz')-> setField('signup[password_confirmation]', 'xyz')-> click('save')-> end()-> with('response')->begin()-> isRedirected()-> isStatusCode(302)-> followRedirect()-> end(); [/sourcecode]

Now there is a case in which the form had some errors while submitting, it will fail your test:

[sourcecode language="php"] isRedirected()-> isStatusCode(302)-> [/sourcecode]

and you have no clue why it is failing? What was the error on the form? or if the form had an error while submitting the request. Use the following code to check that:

[sourcecode language="php"] with('form')->begin()-> setField('signup[username]', 'abc')-> setField('signup[email_address]', 'abc@xyz.com')-> setField('signup[password]', 'xyz')-> setField('signup[password_confirmation]', 'xyz')-> click('save')-> end()-> with('form')->begin()-> hasErrors(false)-> end()-> with('response')->begin()-> isRedirected()-> isStatusCode(302)-> followRedirect()-> end(); [/sourcecode]

By using the above code at-least you can now know that the form had errors and you can use debug method of the $browser object to debug that error:

[sourcecode language="php"] with('form')->begin()-> hasErrors(false)-> debug()-> end()-> [/sourcecode]

This is how you can write and debug test cases for Form POST request testing scenarios.

There is another interesting scenario I came across for AJAX request testing in Symfony application. You will hardly find information on the web directly, which talks about AJAX requests testing, except a small paragraph on <http://www.symfony-project.org/jobeet/1_4/Doctrine/en/18>

The following simple test case includes both the scenarios: AJAX GET and AJAX POST request testing:

This is a simple scenario to add a new book in the books list in a library management system with AJAX. In this scenario we are testing that AJAX GET request should take a Book form in response with some basic fields and after putting the data in the form the page posts the data back to the server, if the book is added successfully it will send a redirection with Status code 302.

[sourcecode language="php"] $browser->setHttpHeader('X-Requested-With', 'XMLHttpRequest')-> get('/book/add')->

## Comments

**[Sarker](#8945 "2012-06-04 09:22:00"):** after running a functional test from CLI i want the result as a report (in a txt or word file). how can i do this?

**[Pankaj Sahni](#6834 "2012-01-11 15:33:34"):** best documentation i found on internet about symfony's functional testing. even better than jobeet tutorial. keep posting.

