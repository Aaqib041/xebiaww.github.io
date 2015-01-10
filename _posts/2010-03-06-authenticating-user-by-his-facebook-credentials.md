---
layout: post
header-img: img/default-blog-pic.jpg
author: sunil.inteti
description: 
post_id: 3223
created: 2010/03/06 17:17:12
created_gmt: 2010/03/06 12:17:12
comment_status: open
---

# Authenticating user by his Facebook credentials

<p>In my previous <a href="http://xebee.xebia.in/2010/03/06/connect-to-facebook-in-grails-app/" target="_blank">post</a> I talked about how we can connect to Facebook from our application and get the user deatils. In this post we will discuss how we can authenticate the user based on his facebook user details. We will  use  acegi plugin of grails to allow uses our of web application to be authenticated by their facebook credentials . We dont need to aunthenticate them into our application like normal way. Let us see how that can be done!</p>
<!--more-->

<p>The prerequisite for this feature is to have the acegi plugin installed. In my last  <a href="http://xebee.xebia.in/2010/03/06/connect-to-facebook-in-grails-app/" target="_blank">post</a> , after the getting the facebook user details we redirect to facebook controller passing his details(<strong>facebook user id, sessionKey</strong> etc) as params  :</p>
<p>[sourcecode lang="java"]document.location.href=&quot;${params.lang}/facebook/subscription?firstname=&quot;+firstname+&quot;&amp;lastname=&quot;+lastname+&quot;&amp;fbuid=&quot;+uid+&quot;&amp;sessionkey=&quot;+sessionkey+&quot;&amp;pic=&quot;+pic;[/sourcecode]</p>
<p>Here we are passing the facebook user id as <strong>fbuid. </strong>Assuming the user class in the application is <strong>UserAccount, </strong>we can have a fbuid property on UserAccount. From the param <strong>fbuid</strong> we can get the existing user.We can also choose to connect <strong>fbuid</strong> to an existing user in our application. For  that we may need to authenticate user normally into our app and then updating his <strong>UserAccount.fbuid </strong>from the param fbuid. We can use a popup mechanism to give the user the option to connect his facebook uid to his existing account or create a new account with his facebook user id.</p>
<p>Lets assume that we already have an Useraccount and we have its correspondint fbuid attached to it. How can we create  his authentication object.  Here is the code which can be part of as service class(utilityService for eg.)</p>
<p>In <strong>FacebookController</strong></p>
<p>[sourcecode lang="groovy"]</p>
<p>subscription = {</p>
<p>def account = UserAccount.findByFbuid(params.fbuid)
   if (account) {
     // assuming user has list of authohorities or roles ...
     utilityService.authenticateFacebookUser(params.fbuid, params.sessionkey, account.authorities)
   }
}
[/sourcecode]</p>
<p>The idea here is that from <strong>fbuid </strong>we can get his account(Since user has a fbuid property). From that we can get his authority and then authenticate him using the following method in utilityService.</p>
<p>[sourcecode lang="java"]
public void authenticateFacebookUser(long uid, String sessionKey, GrantedAuthority[] authorities) {
    def token = new FacebookAuthenticationToken(authorities, uid, sessionKey)
    FacebookAuthenticationToken authentication = facebookAuthProvider.authenticate(token)
    SCH.context.authentication = authentication
}
[/sourcecode]</p>
<p>Here <strong>FacebookAuthenticationToken </strong>and <strong>facebookAuthProvider </strong>are the classes provided by acegi grails plugin &amp; SCH is the alias for Security Context Holder. By this way an user can be linked with his facebook id and authenticated.</p>
<p>How ever there are some changes that could be needed. In our existing application we are using acegi plugin and we use the AuthorizeTagLib.grrovy. We have to extend this to include the facebook authenticated users. Other wise when we use this existing logic we will get missing property exception on <strong>aunthentication </strong>object, as the authentication object is different in case of facebook and normal authentication object.</p>
<p>For example the isLoggedIn() method in AuthorizeTagLib.groovy can be extended like this.</p>
<p>[sourcecode lang="groovy"]</p>
<p>public boolean isLoggedIn() {
def authenticationObject = SCH?.context?.authentication
return (authenticationObject instanceof FacebookAuthenticationToken || authenticateService.isLoggedIn())</p>
<p>}[/sourcecode]</p>
<p>As the case may be other methods of the AuthorizeTagLib may have to be extended as well. This applies for classes facebookAuthenticationProvider and UserDetailsService of  Acegi plugin.By this the user of our application can be authenticated by his facebook credentials and he can use our site normally.</p>
<p><strong>Summary:</strong>In this post we basically talked about the idea of connecting a userAccount to user's facebook id, and authenticating   user by his facebook credentials also.</p>