---
layout: post
header-img: img/default-blog-pic.jpg
author: varun_j
description: 
post_id: 13478
created: 2012/04/30 15:24:32
created_gmt: 2012/04/30 10:24:32
comment_status: open
---

# How to integrate Twitter into your web application with OAuth?

<p>Twitter provides APIs for integrating with web applications using OAuth. By integrating Twitter within our web application we can show the tweets posted by a particular user, his/her friends, their tweets, post new tweets etc. Also we can provide “Sign in with Twitter Login”  functionality in the web application. So users do not need to have a separate user id, instead they can use their Twitter Login to sign in to our web application.<!--more--></p>
<p>OAuth (Open Authentication) provides our web application a way to access the resources owned by the  resource owners without knowing their credentials. For more information on OAuth, you may want to visit the following link:
<a href="http://hueniverse.com/oauth/" target="_blank">http://hueniverse.com/oauth</a></p>
<p>The first thing that we are going to need for integrating Twitter is Twitter Account.
You will be also need a web server with public IP so that Twitter can connect to the same. We will discuss later why we need this.
We will be using TwitterOAuth library (written in PHP) for integrating the Twitter with our web application. This library can be downloaded from the following link:
<a href="https://github.com/abraham/twitteroauth/downloads" target="_blank">https://github.com/abraham/twitteroauth/downloads</a></p>
<p>Before getting our hands dirty, let us discuss our approach.</p>
<p>We will create one Twitter application, and Twitter will provides us Consumer Key and Consumer Secret for the same.
We will use the consumer key and consumer secret to connect to Twitter in order to get request token.
Then we will provide this request token to the Twitter and take user to the Twitter Login where he/she can enter his/her credentials on the Twiiter Website and in return we will receive access token.
This access token will be enable our web application to use his Twitter Account(in a limited way).
So by this access token our web application can show his Tweets, friends' tweets, post new tweets etc.</p>
<p>Now let us move on to the coding part.</p>
<p>Goto the URL : <a target="_blank" href="https://dev.twitter.com">https://dev.twitter.com</a> and sign in with your twitter account. The page you see is meant for developers. After you have successfully logged in, hover onto your username on the top-right corner of the screen. You will see a “My Application” link. Click on that link and then create new application there.</p>
<p>Put the name and description to whatever you wish. In the Callback Url  field, provide a url which is publicly accessible. For our demo application, we are setting it to [http://210.7.69.29/twoth/authenticate.php]. Click create and your application is ready. Scroll down the page and copy the Consumer Key and Consumer Secret to a safe place for future reference.</p>
<iframe src="https://skydrive.live.com/embed?cid=F9ECD7151D57C013&resid=F9ECD7151D57C013%21240&authkey=ANw3EGYDkoI9gBs" width="320" height="266" frameborder="0" scrolling="no"></iframe>

<p>Now goto the application settings and change the access to Read and Write. This is required if we need to post new tweets from the logged in user's account. Till here we are done with Twitter settings.</p>
<p>Lets move on to the TwitterOAuth library. I assume that you have downloaded the archive from the
<a target="_blank" href="https://github.com/abraham/twitteroauth/downloads">link</a> mentioned above in the blog. Extract the archive and copy the directory named twitteroauth. Inside this directory there will be two files namely twitteroauth.php and oauth.php. So we will be using these two files only from the library.</p>
<p>Our demo application directory structure will be as below:</p>
<iframe src="https://skydrive.live.com/embed?cid=F9ECD7151D57C013&resid=F9ECD7151D57C013%21243&authkey=ANTe7xqqGDq0N60" width="204" height="239" frameborder="0" scrolling="no"></iframe>

<p>So you can create the above shown directory structure and save it in the webroot directory of the web server.</p>
<p>Firstly, we will create our configuration file, in this file we will define the Consumer Key and Consumer Secret for the Twitter Application.
So Copy the Consumer key and consumer secret from the application page on the twitter website and paste the same in the config file as shown below:</p>
<p>[sourcecode language="php"]
/*<em>
 * twoth/config/config.php
 * Configuration file.
 * Used for defining the Consumer key and Consumer Secret of Twitter
 * Application.
 </em>/</p>
<p>define('CONSUMER_KEY','<strong><em>*</em></strong><strong><em>*</em></strong><strong><em>*</em></strong><strong>');
define('CONSUMER_SECRET','<strong><em>*</em></strong><strong><em>*</em></strong><strong><em>*</em></strong><strong><em>*</em></strong><strong><em>*</em></strong><strong><em>*</em></strong></strong>*');
[/sourcecode]</p>
<p>Lets move on to the index.php file. In this file we will import the following three files:
<ul>
<li>lib/twitteraouth/twitteroauth.php</li>
<li>lib/twitteraouth/oauth.php</li>
<li>config/config.php</li>
</ul></p>
<p>The library file twitteroauth.php provides us a class TwitterOAuth which we will be using extensively to connect to Twitter.
The source code for the index.php is shown below:</p>
<p>[sourcecode language="php"]
require_once 'lib/twitteroauth/OAuth.php';
require_once 'lib/twitteroauth/twitteroauth.php';
require_once 'config/config.php';</p>
<p>session_start();</p>
<p>/<em>
 * Instantiating the TwitterOAuth class with the Consumer key and consumer
 * secret as defined in config.php file.
 </em>/
$twitterOAuth = new TwitterOAuth(CONSUMER_KEY, CONSUMER_SECRET);</p>
<p>/<em>
 * calling getRequestToken() method to get the request token.
 * the request token will be used later to compare it with the token 
 * returned from the Twitter again on the callback URL (as specified)
 * in the application settings. 
 * In our case it is http://210.7.69.29/twoth/authenticate.php
 * As I have mentioned before, this callback url should be publicly accessible. 
 </em>/
$requestToken = $twitterOAuth-&gt;getRequestToken();</p>
<p>/<em>
 * saving the request token in the session for future use
 </em>/
$_SESSION['request_token'] = $requestToken['oauth_token'];
$_SESSION['request_token_secret'] = $requestToken['oauth_token_secret'];</p>
<p>/<em>
 * Redirecting the user to the Twitter Sign in page where he/she will
 * provide his/her user credentials.
 </em>/
header ('Location: ' . $twitterOAuth-&gt;getAuthorizeURL($requestToken));
[/sourcecode]</p>
<p>Now if user has successfully entered his/her credentials, he will redirected to the callback url. Twitter will pass one parameter named "oauth_token" in the url itself. This parameter will be compared to the request token saved in the session, both of these parameters should be same.</p>
<p>Below is the source code for the authenticate.php file:</p>
<p>[sourcecode language="php"]
require_once 'lib/twitteroauth/OAuth.php';
require_once 'lib/twitteroauth/twitteroauth.php';
require_once 'config/config.php';</p>
<p>session_start();</p>
<p>/<em>
 * Comparing the request token with the token passed by the Twitter in the 
 * url as the parameter named &quot;oauth_token&quot;
 </em>/</p>
<p>if($_SESSION['request_token'] == $_GET['oauth_token']) {</p>
<pre><code>/*
 * Instanitiating the TwitterOAuth class with the consumer token and
 * the request token
 */
$twitterOAuth = new TwitterOAuth(CONSUMER_KEY, CONSUMER_SECRET,
$_SESSION['request_token'],$_SESSION['request_token_secret']);

/**
 * Getting the access token from the Twitter.
 * This access token will be used by our web application.
 * It is only this access token which enables us to use the 
 * Twitter account of the user (in limited way).
 * By this access token we can read tweets, friends' tweets, post new
 * tweets etc. 
 */
$accessToken = $twitterOAuth-&amp;gt;getAccessToken();
if(isset($accessToken['user_id']) &amp;amp;&amp;amp; is_numeric($accessToken['user_id'])){
    /*
     * Saving the access token in the session
     */
    $_SESSION['access_token'] = $accessToken['oauth_token'];
    $_SESSION['access_token_secret'] = $accessToken['oauth_token_secret'];

    /*
     * Redirect the user to the success.php where we will show his/her
     * profile image and friends alongwith the tweets.
     */
    header('Location: ' . '/twoth/success.php');
}
</code></pre>
<p>} else {</p>
<pre><code>/*
 *If token retured from the Twitter to the callback url doesn't
 * match with the token which is saved already in the session, then
</code></pre>

## Comments

**[Varun Jalandery](#8800 "2012-05-16 13:51:13"):** @SMile If we do that, then the user would have to pass his/her credentials on our website, which is just against the purpose of OAuth, OAuth is designed in such a way that user should pass his/her credentials on the original website to which his/her account belongs. So we never play with username and password with OAuth. But there is a way for accessing the Twitter account without logging in, but this works for the applcation owner (the developer who has created the Twitter App). You can simply copy the access tokens as shown in the App settings page on the Twitter Site and use it in the php code to access the Twitter account.

**[smile](#8789 "2012-05-14 22:10:24"):** HI, Is it possible to pass the user credentials to the twitter url page through the php program itself , rather than prompting the user to enter it from index.php page and redirect to callback php. We are trying to modify this to meet our use case? Thanks SMile

