---
layout: post
header-img: img/default-blog-pic.jpg
---

# Developing Application on facebook : Facebook basic setup

You just need to follow some basic setup to communicate with Facebook. This setup will include, loading of Facebook sdk. checking status such as login, logout, authorization etc. There are several ways provided to achieve this, but here we are focusing on **Javascript-sdk** and **Jquery**. Just use the below code as a reference: 

    1. **Javascript-SDK**
    
    
    window.fbAsyncInit = function() {
    FB.init({
    appId : 'YOUR_APP_ID', // _App ID from the app dashboard_
    status : true, // _Check Facebook Login status_
    cookie : true, // _Enable cookie_
    xfbml : true // _Look for social plugins on the page_
    });
    FB.Event.subscribe('auth.logout', function() {
    notAuthored();//_Method to be called if call is not authorized by Facebook_
    });
    FB.Event.subscribe('auth.login', function(response) {
    if (response.status === 'connected') {
    afterAuthorization(response);//_Method to be called if call is authorized by Facebook_
    }
    });
    FB.getLoginStatus(function(response) {
    if (response.status === 'connected') {
    afterAuthorization(response);
    } else if (response.status === 'not_authorized') {
    notAuthored();
    } else {
    notAuthored();
    }
    });
    };
    // _Load the SDK Asynchronously_
    (function(d) {
    var js, id = 'facebook-jssdk', ref = d.getElementsByTagName('script')[0];
    if (d.getElementById(id)) {
    return;
    }
    js = d.createElement('script');
    js.id = id;
    js.async = true;
    js.src = "//connect.facebook.net/en_US/all.js";
    ref.parentNode.insertBefore(js, ref);
    }(document));

    1. **JQuery**
    
    
    $(document).ready(function() {
    $.ajaxSetup({ cache: true });
    $.getScript('//connect.facebook.net/en_UK/all.js', function(){
    FB.init({
    appId: 'YOUR_APP_ID',
    status : true, // _Check Facebook Login status_
    cookie : true, // _Enable cookie_
    xfbml : true // _Look for social plugins on the page_
    });
    FB.Event.subscribe('auth.logout', function() {
    notAuthored();//_Method to be called if call is not authorized by Facebook_
    });
    FB.Event.subscribe('auth.login', function(response) {
    if (response.status === 'connected') {
    afterAuthorization(response);//_Method to be called if call is authorized by Facebook_
    }
    });
    FB.getLoginStatus(function(response) {
    if (response.status === 'connected') {
    afterAuthorization(response);
    } else if (response.status === 'not_authorized') {
    notAuthored();
    } else {
    notAuthored();
    }
    });
    });
    });

you can write the logic for your own login button as : 
    
    
    function onClickLoginButton(){
    FB.login(function(response) {
    if (response.authResponse) {												afterAuthorization(response);//_logic after authorization_
    } else {
    console.log('User cancelled login or did not fully authorize.');}},
    {scope : 'user_about_me,user_birthday,user_hometown,user_location' //_Used to get access token for the folling permissions_
    });});
    }

Now test this at the following link : [Basic Setup](http://facebook.w3villa.com/basicSetup.html) For more information on Xebia's offerings on Enterprise web apps, [Click here](http://www.xebia.in/enterprise-mobile.html)