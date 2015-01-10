---
layout: post
header-img: img/default-blog-pic.jpg
author: pgupta
description: 
post_id: 17593
created: 2013/11/20 15:25:38
created_gmt: 2013/11/20 10:25:38
comment_status: open
---

# Developing Application on facebook : Facebook basic setup

<p>You just need to follow some basic setup to communicate with Facebook. This setup will include, loading of Facebook sdk. checking status such as login, logout, authorization etc.</p>
<p>There are several ways provided to achieve this, but here we are focusing on <b>Javascript-sdk</b> and <b>Jquery</b>. Just use the below code as a reference:
<ol>
<ol>
    <li><strong>Javascript-SDK</strong></li>
</ol>
</ol>
<pre>window.fbAsyncInit = function() {
FB.init({
appId : 'YOUR_APP_ID', // <em>App ID from the app dashboard</em>
status : true, // <em>Check Facebook Login status</em>
cookie : true, // <em>Enable cookie</em>
xfbml : true // <em>Look for social plugins on the page</em>
});
FB.Event.subscribe('auth.logout', function() {
notAuthored();//<em>Method to be called if call is not authorized by Facebook</em>
});
FB.Event.subscribe('auth.login', function(response) {
if (response.status === 'connected') {
afterAuthorization(response);//<em>Method to be called if call is authorized by Facebook</em>
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
// <em>Load the SDK Asynchronously</em>
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
}(document));</pre>
<ol>
<ol>
    <li><strong>JQuery</strong></li>
</ol>
</ol>
<pre>$(document).ready(function() {
$.ajaxSetup({ cache: true });
$.getScript('//connect.facebook.net/en_UK/all.js', function(){
FB.init({
appId: 'YOUR_APP_ID',
status : true, // <em>Check Facebook Login status</em>
cookie : true, // <em>Enable cookie</em>
xfbml : true // <em>Look for social plugins on the page</em>
});
FB.Event.subscribe('auth.logout', function() {
notAuthored();//<em>Method to be called if call is not authorized by Facebook</em>
});
FB.Event.subscribe('auth.login', function(response) {
if (response.status === 'connected') {
afterAuthorization(response);//<em>Method to be called if call is authorized by Facebook</em>
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
});</pre>
you can write the logic for your own login button as :
<pre>function onClickLoginButton(){
FB.login(function(response) {
if (response.authResponse) {                                                afterAuthorization(response);//<em>logic after authorization</em>
} else {
console.log('User cancelled login or did not fully authorize.');}},
{scope : 'user_about_me,user_birthday,user_hometown,user_location' //<em>Used to get access token for the folling permissions</em>
});});
}</pre>
Now test this at the following link : <a href="http://facebook.w3villa.com/basicSetup.html" title="Basic setup" target="_blank">Basic Setup</a></p>
<p>For more information on Xebia's offerings on Enterprise web apps, <a href="http://www.xebia.in/enterprise-mobile.html">Click here</a></p>