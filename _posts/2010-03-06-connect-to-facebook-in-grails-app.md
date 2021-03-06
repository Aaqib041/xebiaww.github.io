---
layout: post
header-img: img/default-blog-pic.jpg
author: sunil.inteti
description: 
post_id: 3209
created: 2010/03/06 12:16:01
created_gmt: 2010/03/06 07:16:01
comment_status: open
---

# Connect to Facebook in Grails App

Recently we integrated the Facebook connect plugin in our Grails project. In this post we will see how to connect to facebook  from our application. So that we can allow facebook users to use our Application.First step would be to get the facebook connect button on to our home page. The code for this is below. ** Getting the facebook connect button on our home page and its functionality **

[sourcecode language="html"] <fb:login-button onlogin="facebook_onlogin();"></fb:login-button> [/sourcecode]

Ofcourse we need to add the facebook namespace to our gsp/html page.

[sourcecode language="html"] xmlns:fb="http://www.facebook.com/2008/fbml" [/sourcecode]

1) fb:login-button is an XFBML tag, we need some Facebook-specific JavaScript to actually render it. This line does the trick.

[sourcecode lang="html"]<script type="text/javascript" src="http://static.ak.connect.facebook.com/js/api_lib/v0.4/FeatureLoader.js.php"></script>[/sourcecode]

2) Create a cross domain receiver file, which is necessary for secure [cross-domain communication][1] with Facebook

create a file with a name xd_receiver.htm and put the following contents into it.

[sourcecode lang="html"]<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd"> <html xmlns="http://www.w3.org/1999/xhtml" > <body> <script src="http://static.ak.connect.facebook.com/js/api_lib/v0.4/XdCommReceiver.js" type="text/javascript"></script> </body> </html> [/sourcecode]

3) Every connect website is a facebook application and an API key is required. Fortunately we can easily generate an API key from[ here][2] , fill in details and submit the application.

4) We need to tell facebook about our API key. We need to put this code in the <body> tag.

[sourcecode lang="html"]<script type="text/javascript">FB.init("3f0ba1d70537bbf9381bca2bbb9f9a93","xd_receiver.htm");</script>[/sourcecode]

This tells the facebook about out API key and the location of the out cross domain receiver file. The above points are [here][3] in detail.

By this piece of code we can get the fbConnect button on our home page. But when you click it nothing happens as we need to add a the functionality for the function **facebook_onlogin()**

[sourcecode lang="java"] function facebook_onlogin() { var uid = FB.Facebook.apiClient.get_session().uid ; var sessionkey = FB.Facebook.apiClient.get_session().session_key;FB.Facebook.apiClient.users_getInfo(uid, new Array('first_name','last_name','email','pic_square_with_logo'), function(result, ex) { var firstname = result[0]['first_name'] var lastname = result[0]['last_name'] var pic = result[0]['pic_square_with_logo'] document.location.href="${params.lang}/facebook/subscription?firstname="+firstname+"&lastname="+lastname+"&fbuid="+uid+"&sessionkey="+sessionkey+"&pic="+pic;}); }[/sourcecode]

The code is self explainatory. To be brief, here we are using the javascript library **FeatureLoader.js.php **to contact the Facebook and get details of a facebook user like his first name, last name, his profile picture etc.

We also get his facebook id using **var uid = FB.Facebook.apiClient.get_session().uid ;**

If the user doesnt have a facebook session already it prompts the popup to log on to facebook. **If you want to know how it is done **look through the sourcecode [here][4]. If he is logged in we can get the details of the facebook user like facebook id, first name, last name, email etc.

The trick here is to pass on the details to our custom controller, lets name it FacebookController. More precisely this is the code that is doing that.

[sourcecode lang="java"]document.location.href="${params.lang}/facebook/subscription?firstname="+firstname+"&lastname="+lastname+"&fbuid="+uid+"&sessionkey="+sessionkey+"&pic="+pic;});[/sourcecode]

The action name in facebook controller is subscription and in here we can choose to authenticate him in to our application by connecting him to an existing user. This is to say, user doesn't need to log in to grails application by his username and password but rather use his facebook credentials. I will explain how this can be done in my next blog.

   [1]: http://wiki.developers.facebook.com/index.php/Cross_Domain_Communication
   [2]: http://www.facebook.com/developers/
   [3]: http://wiki.developers.facebook.com/index.php/Facebook_Connect_Tutorial1
   [4]: http://static.ak.connect.facebook.com/js/api_lib/v0.4/FeatureLoader.js.php

## Comments

**[Stephane](#5554 "2011-05-05 13:47:03"):** Hi, In your line var sessionkey = FB.Facebook.apiClient.get_session().session_key;FB.Facebook.apiClient.users_getInfo(uid, new Array('first_name','last_name','email','pic_square_with_logo'), function(result, ex) { you list the email. But I think it is not retrieved. And I cannot see it later used in your code. Are you sure you can retrieve it that way ? If so, then try displaying it later oin in the code.. Cheers,

**[Free BlackBerry](#5688 "2011-07-08 10:08:13"):** A good article Thank you!

