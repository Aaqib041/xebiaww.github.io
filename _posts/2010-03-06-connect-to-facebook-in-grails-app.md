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

<p>Recently we integrated the Facebook connect plugin in our Grails project. In this post we will see how to connect to facebook  from our application. So that we can allow facebook users to use our Application.First step would be to get the facebook connect button on to our home page. The code for this is below.
<strong> Getting the facebook connect button on our home page and its functionality
</strong>
<!--more--></p>
<p>[sourcecode language="html"] &lt;fb:login-button onlogin=&quot;facebook_onlogin();&quot;&gt;&lt;/fb:login-button&gt;
[/sourcecode]</p>
<p>Ofcourse we need to add the facebook namespace to our gsp/html page.</p>
<p>[sourcecode language="html"] xmlns:fb=&quot;http://www.facebook.com/2008/fbml&quot; [/sourcecode]</p>
<p>1) fb:login-button is an XFBML tag, we need some Facebook-specific JavaScript to actually render it. This line does the trick.</p>
<p>[sourcecode lang="html"]&lt;script type=&quot;text/javascript&quot; src=&quot;http://static.ak.connect.facebook.com/js/api_lib/v0.4/FeatureLoader.js.php&quot;&gt;&lt;/script&gt;[/sourcecode]</p>
<p>2) Create a cross domain receiver file, which is necessary for secure <a href="http://wiki.developers.facebook.com/index.php/Cross_Domain_Communication">cross-domain communication</a> with Facebook</p>
<p>create a file with a name xd_receiver.htm and put the following contents into it.</p>
<p>[sourcecode lang="html"]&lt;!DOCTYPE html PUBLIC &quot;-//W3C//DTD XHTML 1.0 Strict//EN&quot; &quot;http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd&quot;&gt; &lt;html xmlns=&quot;http://www.w3.org/1999/xhtml&quot; &gt; &lt;body&gt; &lt;script src=&quot;http://static.ak.connect.facebook.com/js/api_lib/v0.4/XdCommReceiver.js&quot; type=&quot;text/javascript&quot;&gt;&lt;/script&gt; &lt;/body&gt; &lt;/html&gt;  [/sourcecode]</p>
<p>3) Every connect website is a facebook application and an API key is required. Fortunately we can easily generate an API key from<a href="http://www.facebook.com/developers/" target="_blank"> here</a> , fill in details and submit the application.</p>
<p>4) We need to tell facebook about our API key. We need to put this code in the &lt;body&gt; tag.</p>
<p>[sourcecode lang="html"]&lt;script type=&quot;text/javascript&quot;&gt;FB.init(&quot;3f0ba1d70537bbf9381bca2bbb9f9a93&quot;,&quot;xd_receiver.htm&quot;);&lt;/script&gt;[/sourcecode]</p>
<p>This tells the facebook about out API key and the location of the out cross domain receiver file. The above points are <a href="http://wiki.developers.facebook.com/index.php/Facebook_Connect_Tutorial1" target="_blank">here</a> in detail.</p>
<p>By this piece of code we can get the fbConnect button on our home page. But when you click it nothing happens as we need to add a the functionality for the function <strong>facebook_onlogin()</strong></p>
<p>[sourcecode lang="java"]
function facebook_onlogin() {
var uid = FB.Facebook.apiClient.get_session().uid ;
var sessionkey = FB.Facebook.apiClient.get_session().session_key;FB.Facebook.apiClient.users_getInfo(uid, new  Array('first_name','last_name','email','pic_square_with_logo'), function(result, ex) {
var firstname = result[0]['first_name']
var lastname = result[0]['last_name']
var pic = result[0]['pic_square_with_logo']
document.location.href=&quot;${params.lang}/facebook/subscription?firstname=&quot;+firstname+&quot;&amp;lastname=&quot;+lastname+&quot;&amp;fbuid=&quot;+uid+&quot;&amp;sessionkey=&quot;+sessionkey+&quot;&amp;pic=&quot;+pic;});
}[/sourcecode]</p>
<p>The code is self explainatory. To be brief, here we are using the javascript library <strong>FeatureLoader.js.php </strong>to contact the Facebook and get details of a facebook user like his first name, last name, his profile picture etc.</p>
<p>We also get his facebook id using  <strong>var uid = FB.Facebook.apiClient.get_session().uid ;</strong></p>
<p>If the user doesnt have a facebook session already it prompts the popup to log on to facebook. <strong>If you want to know how it is done </strong>look through the sourcecode <a href="http://static.ak.connect.facebook.com/js/api_lib/v0.4/FeatureLoader.js.php" target="_blank">here</a>. If he is logged in we can get the details of the facebook user like facebook id, first name, last name, email etc.</p>
<p>The trick here is to pass on the details to our custom controller, lets name it FacebookController. More precisely this is the code that is doing that.</p>
<p>[sourcecode lang="java"]document.location.href=&quot;${params.lang}/facebook/subscription?firstname=&quot;+firstname+&quot;&amp;lastname=&quot;+lastname+&quot;&amp;fbuid=&quot;+uid+&quot;&amp;sessionkey=&quot;+sessionkey+&quot;&amp;pic=&quot;+pic;});[/sourcecode]</p>
<p>The action name in facebook controller is subscription and in here we can choose to authenticate him in to our application by connecting him to an existing user. This is to say, user doesn't need to log in to grails application by his username and password but rather use his facebook credentials. I will explain how this can be done in my next blog.</p>

## Comments

**[Stephane](#5554 "2011-05-05 13:47:03"):** Hi, In your line var sessionkey = FB.Facebook.apiClient.get_session().session_key;FB.Facebook.apiClient.users_getInfo(uid, new Array('first_name','last_name','email','pic_square_with_logo'), function(result, ex) { you list the email. But I think it is not retrieved. And I cannot see it later used in your code. Are you sure you can retrieve it that way ? If so, then try displaying it later oin in the code.. Cheers,

**[Free BlackBerry](#5688 "2011-07-08 10:08:13"):** A good article Thank you!

