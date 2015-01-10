---
layout: post
header-img: img/default-blog-pic.jpg
author: anirudh.xebia
description: 
post_id: 15335
created: 2012/11/28 21:10:15
created_gmt: 2012/11/28 16:10:15
comment_status: open
---

# Using verbs apart from HTTP verbs(PUT/POST..) in REST URL

<p><span style="font-family: verdana,geneva; font-size: small;">As per REST, the URLs should make use of HTTP verbs to expose their REST based services via HTTP. (i.e GET/PUT/POST/DELETE)
</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">So, a resource would be something like
</span></p>
<p>[sourcecode language="java"]
GET ../foods/1 would get you the food with id 1.
PUT ../foods/2 would update the food with id 2.
POST ../foods will add a new food.
DELETE ../foods/1 will delete the food resource with id 1.&lt;/span&gt;
[/sourcecode]</p>
<p><span style="font-family: verdana,geneva; font-size: small;">But in a real life complex application, we are faced with exposing many services such as approve, reject where it becomes inevitable to add verbs to the URL. What should we do? Should we just have the URLs like ../foods/1/approve ?
</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">What would go wrong if we use verbs in REST URL.</span>
<span style="font-family: verdana,geneva; font-size: small;"> Whether there is some rationale behind it or it just REST dogma..</span>
<span style="font-family: verdana,geneva; font-size: small;"><strong> Apparently, there is</strong> :</span>
<span style="font-family: verdana,geneva; font-size: small;"> <!--more--></span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">To understand, we would need to get into the details of HTTP and understand how we could make use of browser capabilities (caching..) which are made in consideration with <a href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html" target="_blank">HTTP specifications</a>.</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">
</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;"><strong>1.  GET :</strong> As per the specification ; HTTP GET command should not change the state of the resource.</span>
<span style="font-family: verdana,geneva; font-size: small;"> It means that HTTP GET command has to be with no side effect, suppose we want to do a </span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">GET on /approve and then return a boolean which would return success/false.</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">It would be incorrect as it is changing the state of resource on which approve is called.</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">HTTP GET is cached by the browser and many other proxy servers like <a href="http://en.wikipedia.org/wiki/Content_delivery_network" target="_blank">CDNs</a>. (provided some conditions as per HTTP specifications are met.) </span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">And in order to leverage these caching capabilities HTTP specifications must be adhered.
</span></p>
<p>&nbsp;</p>
<p><span style="font-family: verdana,geneva; font-size: small;"><strong>2. PUT :</strong> The specification says that apart from POST all HTTP verbs have to be idempotent.</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">Idempotent means that the result of the action is independent of the number of times it is executed; i.e a request sent n number of times would still return the same result. So if a browser is unable to complete a PUT request or does not get back acknowledgement it safely resends the request again and again, without worrying.
</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">If we have a verb in a URL like /increment or /run or /stop ; executing them again and again will not result in the same result; (they might as well return an error.)</span>
<span style="font-family: verdana,geneva; font-size: small;"> (try running startup.sh on a running tomcat )</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">In case there is an issue with concurrent modification of the resource and there is a fear that the cached GET response is stale or might have got changed by some other thread, HTTP lays down the specification for conditional PUTs and POSTs with the help of HTTP header tags.(<a href="http://en.wikipedia.org/wiki/HTTP_ETag" target="_blank">ETag)</a>
</span></p>
<p>&nbsp;</p>
<p><span style="font-family: verdana,geneva; font-size: small;"><strong>3. POST :</strong> we could do a POST on a verb, it will not harm anything. But, It would be not self explanatory and would defeat the whole purpose of simple,self explanatory REST urls.</span></p>
<p>&nbsp;</p>
<p><span style="font-family: verdana,geneva; font-size: small;"><strong>4. DELETE:</strong> this would not make any sense on /increment or ../start</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">
</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;"><strong><span style="font-family: verdana,geneva; font-size: small;">5. </span></strong><span style="font-family: verdana,geneva; font-size: small;">Also, when if we have verbs different from HTTP verbs we would be removing the intuitiveness of the URL</span> , 2 verbs in a URL certainly looks ambiguous.<span style="font-family: verdana,geneva; font-size: small;">
</span></span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">
</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">So, still if we get stuck in a situation where we have to use a verb, what could be our options.</span>
<span style="font-family: verdana,geneva; font-size: small;"> Below are the alternatives :</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">
</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;"><strong>1.</strong> Use a "method" Query param - Use a Query param method=approve</span>
<span style="font-family: verdana,geneva; font-size: small;"> So our approach would be:</span></p>
<p>[sourcecode]
PUT .../foods/1/?method=approve&amp;user=10134
[/sourcecode]</p>
<p><span style="font-family: verdana,geneva; font-size: small;"> (flicker uses this approach ;example http://api.flickr.com/services/rest/?method=flickr.test.echo&amp;name=value.)</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">
</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;"><strong>2. </strong>Send a JSON Form object</span></p>
<p>[sourcecode]
 POST .../foods/1 JSON: {method:approve}
[/sourcecode]</p>
<p><span style="font-family: verdana,geneva; font-size: small;"><strong>3.</strong> Use noun instead of verb. Instead of using verb "approve" we would use the noun "approval" and make this a domain object.</span>
<span style="font-family: verdana,geneva; font-size: small;"> The approval would have association with the domain objects which are required to be approved.</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">for example, to approve a food, we would have two additional domain objects.</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">    1. ApprovalRequest</span>
<span style="font-family: verdana,geneva; font-size: small;">    2. Approval (or Approved)</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">To submit a food for approval We would do:</span></p>
<p>[sourcecode]
POST ../foods/1/approvalRequests -- &gt;foodId :1 is added into ApprovalRequest
[/sourcecode]</p>
<p><span style="font-family: verdana,geneva; font-size: small;"> To approve this food, w</span><span style="font-family: verdana,geneva; font-size: small;">e would do</span></p>
<p>[sourcecode]
POST ../foods/1/approval -- &gt; foodId :1 is added to Approval The flow hence becomes ,
Food --- &gt; ApprovalRequest ---&gt; Approval
GET on ../foods/approval will give all approved foods
and a
GET on ../foods/approvalRequests will give all pending foods.
doing a DELETE ../foods/1/approvalRequests -- &gt;will reject the food.
[/sourcecode]</p>
<p><span style="font-family: verdana,geneva; font-size: small;">(This is how facebook handles the states of friend requests.)
</span></p>
<p><span style="font-family: verdana,geneva; font-size: small;">So, the basic idea is to model in such a way that all our verbs become states if want them to be exposed via HTTP.</span>
<span style="font-family: verdana,geneva; font-size: small;"> This looks a very RESTful and neat approach.</span></p>

## Comments

**[Vijay Rawat](#9300 "2012-11-29 14:57:51"):** Good blog. Please share code for ApprovalRequest and Approval. (If possible). Thanks.

