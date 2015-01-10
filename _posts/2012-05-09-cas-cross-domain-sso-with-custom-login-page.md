---
layout: post
header-img: img/default-blog-pic.jpg
author: cgoel
description: 
post_id: 13762
created: 2012/05/09 18:38:14
created_gmt: 2012/05/09 13:38:14
comment_status: open
---

# CAS cross domain SSO with custom login page.

As part of "Improve the Usability" drive, we decided to get away with the multiple logins for the user when user hops from one application to another, basically have SSO in place for different cross domain sites. After analyzing the existing architecture/applications we required the following things to be in place to implement the SSO functionality.

1) Have SSO across the various applications (including cross domain). 2) Have Customized login (In UI Terms) pages on CAS server for different applications. We needed this as the existing applications also have different login pages but uses the same database. 3) Ability to post login data over HTTP by using the embedded login page which customises the CAS login page. (User won't be redirected to CAS login page in this case).

Given above requirements me and my colleague [Pratik][1] did spikes on these frameworks [Josso][2] , [OpenAM][3] and [CAS][4] and at end of the day we decided to go with CAS. 

To implement SSO using CAS is a straight forward thing.All steps are well documented. The problem occurs when we have to customize the login jsp as per the individual site theme. This blog aims to address this issue and to show a way to customise the login.jsp for every individual site.

The first requirement is straightforward (you need little configurations except OpenAM, which is a very cumbersome tool to understand and we will never recommend it). Second requirement requires little modification on SSO server but the challenge on the way was third requirement. Let's discuss the different approaches that can be taken to handle that.

**CAS Overview** Not going into too much details, we require below 5 fields to successfully login into CAS using HTTP POST , and it will not matter whether you come from CAS login page or you custom login/embedded page.

1) username - user will enter 2) password - user will enter 3) lt (login ticket) - should come from CAS 4) execution (webflow id) - should come from CAS 5) jsessionid (cookie for cas server) - between browser and CAS server.

CAS server generated the login ticket and execution id when browser requests the CAS login page and sends them as hidden fields. CAS expects to receive them back along with the username/password. Also it sends the jsessionid as a cookie to the browser and all these three fields are tied together. So the trick now is to get handle of all these three fields , if you are going to use your own login page.

**Proxies : ** Proxies can be used , When we cannot do a POST (Full Page / Ajax) to a CAS server because of network limitations (cross domain / others) , so we will make a Full page /ajax call to proxy at the server side with the username/credentials , which will call then CAS , Now here were don't have the loginticket/executionId/jsessionId . so we will have to use the restFul API of the CAS server. First step is to get the TGT ticket and then the service ticket. The java proxy code for both the steps can be downloaded from here [download proxy code][5]. Now once you receive the response , pass it on as the ajax response/complete redirect and to the client side based on success/failure response from CAS. _Note_ :- The only problem with the approach is we won't be able to achieve the single sign on , since we can't store the cookies on the browser side related to CAS via any means.

**iFrame : ** You can embed an iframe in your custom login page which is loading your CAS login page. This way it will generate the jsessionid and you will get lt and execution as the content of your iFrame. Now user will submit the iframe from the custom login page. 2 things will happen now. 1) Credentials didn't match , so user will see the same login page with error message. 2) Successful authentication , will result in a success page in iframe. In this case iframe will load the serviceURL page(please see the iframe code below) . We will have to put a check on "src" attribute of iframe using a javascript bind method and once we get to know about the successful authentication , we can do post/redirect/refresh as per the application requirement.

[sourcecode language="xml"] <div > <iframe id="iFrame" name="iFrame" src="https://CasHost:9443/cas/login?service=http://localhost:7080/mywebapp/protected/"> </iframe> </div> [/sourcecode]

_Here service represents the url to which the CAS server will redirect after successful login._

Limitations: 1) Some older versions of browsers may not render iframes properlly. So it can create some browser specific issues. 2) There can be some usability issues <<>>

**Javascript : ** The issues that we faced during implementation using iframe can be eliminated by slight modification in the CAS server code base. We can get lt and execution from the CAS server as a javascript parameters. jsessionid will get created when you hit the CAS server to get these attributes as javascript. Add following code to your custom login jsp.

login.jsp: [sourcecode language="xml"] <script language="Javascript" src="https://CasHost:9443/cas/login?

service=http://myhost:7080/mywebapp/protected/&nonCASLogin=true" /> [/sourcecode] Also keep following hidden fields also in login.jsp:

[sourcecode language="xml"] <body onload="init()"> <form id="fm1" class="fm-v clearfix" action="https://CasHost:9443/cas/login?nonCASLogin=true" method="post">
    
    
        &lt;input type=&quot;hidden&quot; name=&quot;lt&quot; id=&quot;lt&quot; value=&quot;&quot; /&gt;
        &lt;input type=&quot;hidden&quot; name=&quot;execution&quot; id=&quot;execution&quot; value=&quot;&quot; /&gt;
        &lt;input type=&quot;hidden&quot; name=&quot;_eventId&quot; value=&quot;submit&quot; /&gt;
        &lt;input type=&quot;hidden&quot; name=&quot;authenticate&quot; value=&quot;true&quot; /&gt;
        &lt;input class=&quot;btn-submit&quot; name=&quot;submit&quot; accesskey=&quot;l&quot; value=&quot;LOGIN&quot; tabindex=&quot;4&quot; type=&quot;submit&quot; onclick = &quot;submitForm()&quot;/&gt;
        &lt;input id=&quot;username&quot; name=&quot;username&quot;  type=&quot;text&quot;/&gt;
        &lt;input id=&quot;password&quot; name=&quot;password&quot; type=&quot;password&quot;/&gt;
    &lt;/form&gt;
    

</body> [/sourcecode]

Changes required at the CAS server:

casLoginView.jsp:

Add the following scriptlet at the top of casLoginView.jsp. This will check if the request is having nonCASLogin parameter which is true. If this condition is met then it will return lt and execution as a javascript variables.

[sourcecode language="xml"] <c:if test="${nonCASLogin eq 'true'}"> var lt="${loginTicket}"; var execution="${flowExecutionKey}";  
</c:if> <c:if test="${nonCASLogin ne 'true'}"> Entire CAS login page will reside here. </c:if> [/sourcecode]

**Conclusion : ** Considering the requirements to be fulfilled by the SSO solution, we finally decided to proceed with the approach using javascript to hit CAS server. Using this approach we were able to implement Cross domain SSO using custom login page. As this requires a simple javascript include from CAS server url, there is no chances of any browser specific issues unlike iFrame. 

   [1]: http://xebee.xebia.in/author/pgarg/
   [2]: JOSSO - Java Open Single Sign-On Project Home - Atricore
   [3]: http://www.forgerock.com/openam.html
   [4]: http://www.jasig.org/cas
   [5]: https://wiki.jasig.org/display/CASUM/RESTful+API

## Comments

**[jaycverg](#9311 "2013-01-29 10:09:28"):** Hi, I just want to ask how did you handle credential errors here? Are we just letting it go through the actual CAS login page and display the errors there?

