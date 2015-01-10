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

<p>As part of "Improve the Usability" drive, we decided to get away with the multiple logins for the user when user hops from one application to another, basically have SSO in place for different cross domain sites. After analyzing the existing architecture/applications we required the following things to be in place to implement the SSO functionality.</p>
<p>1)  Have SSO across the various applications (including cross domain).
 2)  Have Customized login (In UI Terms) pages on CAS server for different applications. We needed this as the existing applications also have different login pages but uses the same database.
 3)  Ability to post login data over HTTP by using the embedded login page which customises the CAS login page. (User won't be redirected to CAS   login page in this case).</p>
<p>Given above requirements me and my colleague <a href="http://xebee.xebia.in/author/pgarg/">Pratik</a>  did spikes on these frameworks <a href="JOSSO - Java Open Single Sign-On Project Home - Atricore">Josso</a> , <a href="http://www.forgerock.com/openam.html">OpenAM</a> and <a href="http://www.jasig.org/cas">CAS</a> and at end of the day we decided to go with CAS. 
<!--more--></p>
<p>To implement SSO using CAS is a straight forward thing.All steps are well documented. The problem occurs when we have to customize the login jsp as per the individual site theme. This blog aims to address this issue and to show a way to customise the login.jsp for every individual site.</p>
<p>The first requirement is straightforward (you need little configurations except OpenAM, which is a very cumbersome tool to understand and we will never recommend it). Second requirement requires little modification on SSO server but the challenge on the way was third requirement. Let's discuss the different approaches that can be taken to handle that.</p>
<p><span style="font-size: large;"><strong>CAS Overview</strong></span>
Not going into too much details, we require below 5 fields to successfully login into CAS using HTTP POST , and it will not matter whether you come from CAS login page or you custom login/embedded page.</p>
<p>1) username -  user will enter
2) password  - user will enter
3) lt (login ticket) - should come from CAS
4) execution (webflow id) - should come from CAS
5) jsessionid (cookie for cas server) - between browser and CAS server.</p>
<p>CAS server generated the login ticket and execution id when browser requests the CAS login page and sends them as hidden fields. CAS expects to receive them back along with the username/password. Also it sends the jsessionid as a cookie to the browser and all these three fields are tied together. So the trick now is to get handle of all these three fields , if you are going to use your own login page.</p>
<p><span style="font-size: large;"><strong>Proxies : </strong></span> Proxies can be used , When we cannot do a POST (Full Page / Ajax) to a CAS server because of network limitations (cross domain / others) , so we will make a Full page /ajax call to proxy at the server side with the username/credentials , which will call then CAS  , Now here were don't have the loginticket/executionId/jsessionId . so we will have to use the restFul API of the CAS server.  First step is to get the TGT ticket and then the service ticket. The java proxy code for both the steps can be downloaded from here <a href="https://wiki.jasig.org/display/CASUM/RESTful+API">download proxy code</a>. 
Now once you receive the response , pass it on as the ajax response/complete redirect and to the client side based on success/failure response from CAS.
<em>Note</em> :- The only problem with the approach is we won't be able to achieve the single sign on , since we can't store the cookies on the browser side related to CAS via any means.</p>
<p><span style="font-size: large;"><strong>iFrame : </strong></span> You can embed an iframe in your custom login page which is loading your CAS login page. This way it will generate the jsessionid and you will get lt and execution as the content of your iFrame. Now user will submit the iframe from the custom login page. 2 things will happen now.
1) Credentials didn't match , so user will see the same login page with error message.
2) Successful authentication , will result in a success page in iframe. In this case iframe will load the serviceURL  page(please see the iframe code below) . We will have to put a check on "src" attribute of iframe using a javascript bind method and once we get to know about the successful authentication , we can do post/redirect/refresh as per the application requirement.</p>
<p>[sourcecode language="xml"]
&lt;div &gt;
    &lt;iframe id=&quot;iFrame&quot; name=&quot;iFrame&quot; src=&quot;https://CasHost:9443/cas/login?service=http://localhost:7080/mywebapp/protected/&quot;&gt;
    &lt;/iframe&gt;
&lt;/div&gt;
[/sourcecode]</p>
<p><em>Here service represents the url to which the CAS server will redirect after successful login.</em></p>
<p>Limitations:
1) Some older versions of browsers may not render iframes properlly. So it can create some browser specific issues.
2) There can be some usability issues &lt;&lt;&gt;&gt;</p>
<p><span style="font-size: large;"><strong>Javascript : </strong></span> The issues that we faced during implementation using iframe can be eliminated by slight modification in the CAS server code base. We can get lt and execution from the CAS server as a javascript parameters. jsessionid will get created when you hit the CAS server to get these attributes as javascript.
Add following code to your custom login jsp.</p>
<p>login.jsp:
[sourcecode language="xml"]
&lt;script language=&quot;Javascript&quot; src=&quot;https://CasHost:9443/cas/login?</p>
<p>service=http://myhost:7080/mywebapp/protected/&amp;nonCASLogin=true&quot; /&gt; 
[/sourcecode]
Also keep following hidden fields also in login.jsp:</p>
<p>[sourcecode language="xml"]
&lt;body onload=&quot;init()&quot;&gt;
    &lt;form id=&quot;fm1&quot; class=&quot;fm-v clearfix&quot; action=&quot;https://CasHost:9443/cas/login?nonCASLogin=true&quot; method=&quot;post&quot;&gt;</p>
<pre><code>    &amp;lt;input type=&amp;quot;hidden&amp;quot; name=&amp;quot;lt&amp;quot; id=&amp;quot;lt&amp;quot; value=&amp;quot;&amp;quot; /&amp;gt;
    &amp;lt;input type=&amp;quot;hidden&amp;quot; name=&amp;quot;execution&amp;quot; id=&amp;quot;execution&amp;quot; value=&amp;quot;&amp;quot; /&amp;gt;
    &amp;lt;input type=&amp;quot;hidden&amp;quot; name=&amp;quot;_eventId&amp;quot; value=&amp;quot;submit&amp;quot; /&amp;gt;
    &amp;lt;input type=&amp;quot;hidden&amp;quot; name=&amp;quot;authenticate&amp;quot; value=&amp;quot;true&amp;quot; /&amp;gt;
    &amp;lt;input class=&amp;quot;btn-submit&amp;quot; name=&amp;quot;submit&amp;quot; accesskey=&amp;quot;l&amp;quot; value=&amp;quot;LOGIN&amp;quot; tabindex=&amp;quot;4&amp;quot; type=&amp;quot;submit&amp;quot; onclick = &amp;quot;submitForm()&amp;quot;/&amp;gt;
    &amp;lt;input id=&amp;quot;username&amp;quot; name=&amp;quot;username&amp;quot;  type=&amp;quot;text&amp;quot;/&amp;gt;
    &amp;lt;input id=&amp;quot;password&amp;quot; name=&amp;quot;password&amp;quot; type=&amp;quot;password&amp;quot;/&amp;gt;
&amp;lt;/form&amp;gt;
</code></pre>
<p>&lt;/body&gt;
[/sourcecode]</p>
<p>Changes required at the CAS server:</p>
<p>casLoginView.jsp:</p>
<p>Add the following scriptlet at the top of casLoginView.jsp. This will check if the request is having  nonCASLogin parameter which is true. If this condition is met then it will return lt and execution as a javascript variables.</p>
<p>[sourcecode language="xml"]
&lt;c:if test=&quot;${nonCASLogin eq 'true'}&quot;&gt;
    var lt=&quot;${loginTicket}&quot;;
    var execution=&quot;${flowExecutionKey}&quot;;<br />
 &lt;/c:if&gt;
&lt;c:if test=&quot;${nonCASLogin ne 'true'}&quot;&gt;
    Entire CAS login page will reside here.
&lt;/c:if&gt;
[/sourcecode]</p>
<p><span style="font-size: large;"><strong>Conclusion : </strong></span>  Considering the requirements to be fulfilled by the SSO solution, we finally decided to proceed with the approach using javascript to hit CAS server. Using this approach we were able to implement Cross domain SSO using custom login page. As this requires a simple javascript include from CAS server url, there is no chances of any browser specific issues unlike iFrame. </p>

## Comments

**[jaycverg](#9311 "2013-01-29 10:09:28"):** Hi, I just want to ask how did you handle credential errors here? Are we just letting it go through the actual CAS login page and display the errors there?

