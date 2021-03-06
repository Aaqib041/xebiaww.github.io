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

Twitter provides APIs for integrating with web applications using OAuth. By integrating Twitter within our web application we can show the tweets posted by a particular user, his/her friends, their tweets, post new tweets etc. Also we can provide “Sign in with Twitter Login” functionality in the web application. So users do not need to have a separate user id, instead they can use their Twitter Login to sign in to our web application.

OAuth (Open Authentication) provides our web application a way to access the resources owned by the resource owners without knowing their credentials. For more information on OAuth, you may want to visit the following link: [http://hueniverse.com/oauth][1]

The first thing that we are going to need for integrating Twitter is Twitter Account. You will be also need a web server with public IP so that Twitter can connect to the same. We will discuss later why we need this. We will be using TwitterOAuth library (written in PHP) for integrating the Twitter with our web application. This library can be downloaded from the following link: <https://github.com/abraham/twitteroauth/downloads>

Before getting our hands dirty, let us discuss our approach.

We will create one Twitter application, and Twitter will provides us Consumer Key and Consumer Secret for the same. We will use the consumer key and consumer secret to connect to Twitter in order to get request token. Then we will provide this request token to the Twitter and take user to the Twitter Login where he/she can enter his/her credentials on the Twiiter Website and in return we will receive access token. This access token will be enable our web application to use his Twitter Account(in a limited way). So by this access token our web application can show his Tweets, friends' tweets, post new tweets etc.

Now let us move on to the coding part.

Goto the URL : <https://dev.twitter.com> and sign in with your twitter account. The page you see is meant for developers. After you have successfully logged in, hover onto your username on the top-right corner of the screen. You will see a “My Application” link. Click on that link and then create new application there.

Put the name and description to whatever you wish. In the Callback Url field, provide a url which is publicly accessible. For our demo application, we are setting it to [http://210.7.69.29/twoth/authenticate.php]. Click create and your application is ready. Scroll down the page and copy the Consumer Key and Consumer Secret to a safe place for future reference.

Now goto the application settings and change the access to Read and Write. This is required if we need to post new tweets from the logged in user's account. Till here we are done with Twitter settings.

Lets move on to the TwitterOAuth library. I assume that you have downloaded the archive from the [link][2] mentioned above in the blog. Extract the archive and copy the directory named twitteroauth. Inside this directory there will be two files namely twitteroauth.php and oauth.php. So we will be using these two files only from the library.

Our demo application directory structure will be as below:

So you can create the above shown directory structure and save it in the webroot directory of the web server.

Firstly, we will create our configuration file, in this file we will define the Consumer Key and Consumer Secret for the Twitter Application. So Copy the Consumer key and consumer secret from the application page on the twitter website and paste the same in the config file as shown below:

[sourcecode language="php"] /*_ * twoth/config/config.php * Configuration file. * Used for defining the Consumer key and Consumer Secret of Twitter * Application. _/

define('CONSUMER_KEY','**_*_****_*_****_*_****'); define('CONSUMER_SECRET','**_*_****_*_****_*_****_*_****_*_****_*_*****'); [/sourcecode]

Lets move on to the index.php file. In this file we will import the following three files: 

  * lib/twitteraouth/twitteroauth.php
  * lib/twitteraouth/oauth.php
  * config/config.php

The library file twitteroauth.php provides us a class TwitterOAuth which we will be using extensively to connect to Twitter. The source code for the index.php is shown below:

[sourcecode language="php"] require_once 'lib/twitteroauth/OAuth.php'; require_once 'lib/twitteroauth/twitteroauth.php'; require_once 'config/config.php';

session_start();

/_ * Instantiating the TwitterOAuth class with the Consumer key and consumer * secret as defined in config.php file. _/ $twitterOAuth = new TwitterOAuth(CONSUMER_KEY, CONSUMER_SECRET);

/_ * calling getRequestToken() method to get the request token. * the request token will be used later to compare it with the token * returned from the Twitter again on the callback URL (as specified) * in the application settings. * In our case it is http://210.7.69.29/twoth/authenticate.php * As I have mentioned before, this callback url should be publicly accessible. _/ $requestToken = $twitterOAuth->getRequestToken();

/_ * saving the request token in the session for future use _/ $_SESSION['request_token'] = $requestToken['oauth_token']; $_SESSION['request_token_secret'] = $requestToken['oauth_token_secret'];

/_ * Redirecting the user to the Twitter Sign in page where he/she will * provide his/her user credentials. _/ header ('Location: ' . $twitterOAuth->getAuthorizeURL($requestToken)); [/sourcecode]

Now if user has successfully entered his/her credentials, he will redirected to the callback url. Twitter will pass one parameter named "oauth_token" in the url itself. This parameter will be compared to the request token saved in the session, both of these parameters should be same.

Below is the source code for the authenticate.php file:

[sourcecode language="php"] require_once 'lib/twitteroauth/OAuth.php'; require_once 'lib/twitteroauth/twitteroauth.php'; require_once 'config/config.php';

session_start();

/_ * Comparing the request token with the token passed by the Twitter in the * url as the parameter named "oauth_token" _/

if($_SESSION['request_token'] == $_GET['oauth_token']) {
    
    
    /*
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
    $accessToken = $twitterOAuth-&gt;getAccessToken();
    if(isset($accessToken['user_id']) &amp;&amp; is_numeric($accessToken['user_id'])){
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
    

} else {
    
    
    /*
     *If token retured from the Twitter to the callback url doesn't
     * match with the token which is saved already in the session, then
    

   [1]: http://hueniverse.com/oauth/
   [2]: https://github.com/abraham/twitteroauth/downloads

## Comments

**[Varun Jalandery](#8800 "2012-05-16 13:51:13"):** @SMile If we do that, then the user would have to pass his/her credentials on our website, which is just against the purpose of OAuth, OAuth is designed in such a way that user should pass his/her credentials on the original website to which his/her account belongs. So we never play with username and password with OAuth. But there is a way for accessing the Twitter account without logging in, but this works for the applcation owner (the developer who has created the Twitter App). You can simply copy the access tokens as shown in the App settings page on the Twitter Site and use it in the php code to access the Twitter account.

**[smile](#8789 "2012-05-14 22:10:24"):** HI, Is it possible to pass the user credentials to the twitter url page through the php program itself , rather than prompting the user to enter it from index.php page and redirect to callback php. We are trying to modify this to meet our use case? Thanks SMile

