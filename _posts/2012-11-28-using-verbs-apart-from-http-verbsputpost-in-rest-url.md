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

As per REST, the URLs should make use of HTTP verbs to expose their REST based services via HTTP. (i.e GET/PUT/POST/DELETE) 

So, a resource would be something like 

[sourcecode language="java"] GET ../foods/1 would get you the food with id 1. PUT ../foods/2 would update the food with id 2. POST ../foods will add a new food. DELETE ../foods/1 will delete the food resource with id 1.</span> [/sourcecode]

But in a real life complex application, we are faced with exposing many services such as approve, reject where it becomes inevitable to add verbs to the URL. What should we do? Should we just have the URLs like ../foods/1/approve ? 

What would go wrong if we use verbs in REST URL. Whether there is some rationale behind it or it just REST dogma.. ** Apparently, there is** :

To understand, we would need to get into the details of HTTP and understand how we could make use of browser capabilities (caching..) which are made in consideration with [HTTP specifications][1].

**1.  GET :** As per the specification ; HTTP GET command should not change the state of the resource. It means that HTTP GET command has to be with no side effect, suppose we want to do a 

GET on /approve and then return a boolean which would return success/false.

It would be incorrect as it is changing the state of resource on which approve is called.

HTTP GET is cached by the browser and many other proxy servers like [CDNs][2]. (provided some conditions as per HTTP specifications are met.) 

And in order to leverage these caching capabilities HTTP specifications must be adhered. 

 

**2\. PUT :** The specification says that apart from POST all HTTP verbs have to be idempotent.

Idempotent means that the result of the action is independent of the number of times it is executed; i.e a request sent n number of times would still return the same result. So if a browser is unable to complete a PUT request or does not get back acknowledgement it safely resends the request again and again, without worrying. 

If we have a verb in a URL like /increment or /run or /stop ; executing them again and again will not result in the same result; (they might as well return an error.) (try running startup.sh on a running tomcat )

In case there is an issue with concurrent modification of the resource and there is a fear that the cached GET response is stale or might have got changed by some other thread, HTTP lays down the specification for conditional PUTs and POSTs with the help of HTTP header tags.([ETag)][3]

 

**3\. POST :** we could do a POST on a verb, it will not harm anything. But, It would be not self explanatory and would defeat the whole purpose of simple,self explanatory REST urls.

 

**4\. DELETE:** this would not make any sense on /increment or ../start

**5\. **Also, when if we have verbs different from HTTP verbs we would be removing the intuitiveness of the URL , 2 verbs in a URL certainly looks ambiguous.

So, still if we get stuck in a situation where we have to use a verb, what could be our options. Below are the alternatives :

**1.** Use a "method" Query param - Use a Query param method=approve So our approach would be:

[sourcecode] PUT .../foods/1/?method=approve&user=10134 [/sourcecode]

(flicker uses this approach ;example http://api.flickr.com/services/rest/?method=flickr.test.echo&name=value.)

**2. **Send a JSON Form object

[sourcecode] POST .../foods/1 JSON: {method:approve} [/sourcecode]

**3.** Use noun instead of verb. Instead of using verb "approve" we would use the noun "approval" and make this a domain object. The approval would have association with the domain objects which are required to be approved.

for example, to approve a food, we would have two additional domain objects.

    1. ApprovalRequest     2. Approval (or Approved)

To submit a food for approval We would do:

[sourcecode] POST ../foods/1/approvalRequests -- >foodId :1 is added into ApprovalRequest [/sourcecode]

To approve this food, we would do

[sourcecode] POST ../foods/1/approval -- > foodId :1 is added to Approval The flow hence becomes , Food --- > ApprovalRequest ---> Approval GET on ../foods/approval will give all approved foods and a GET on ../foods/approvalRequests will give all pending foods. doing a DELETE ../foods/1/approvalRequests -- >will reject the food. [/sourcecode]

(This is how facebook handles the states of friend requests.) 

So, the basic idea is to model in such a way that all our verbs become states if want them to be exposed via HTTP. This looks a very RESTful and neat approach.

   [1]: http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html
   [2]: http://en.wikipedia.org/wiki/Content_delivery_network
   [3]: http://en.wikipedia.org/wiki/HTTP_ETag

## Comments

**[Vijay Rawat](#9300 "2012-11-29 14:57:51"):** Good blog. Please share code for ApprovalRequest and Approval. (If possible). Thanks.

