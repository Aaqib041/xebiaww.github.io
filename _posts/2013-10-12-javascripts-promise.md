---
layout: post
header-img: img/default-blog-pic.jpg
author: jpuri
description: 
post_id: 17364
created: 2013/10/12 14:36:42
created_gmt: 2013/10/12 09:36:42
comment_status: open
---

# Javascript's Promise

<p><span style="line-height: 1.5em;">This blog is about Javascript's Promise Object. Writing asynchronous functions with complex callbacks can at times result in lot of boilerplate and ugly code. Javascript Promise give the promise to simplify these.</span></p>
<p><span style="line-height: 1.5em;">Promises are outcome of an asynchronous method calls for example I/O calls or Remote calls. At any given point a promise can be in one of 3 states:</span>
<ol>
    <li><span style="line-height: 1.5em;">Pending, if asynchronous call is executing.</span></li>
    <li><span style="line-height: 1.5em;">Resolved, if asynchronous call is successfully completed.</span></li>
    <li><span style="line-height: 1.5em;">Rejected, if asynchronous call failed.</span></li>
</ol>
We can attach callbacks to Promise which will be executed as the promise is Rejected or Resolved, or we can attach any other arbitrary callback to the Promise object which will be executed as soon as invoked.
<!--more-->
Lets look into jQuery's implementation of Promise called Deferred. Suppose I have a function save which I call on my model objects and it saves the objects remotely. I can implement it using Deferred Objects as:
<p style="padding-left: 90px;">ModelObject.save = function({key: value}){
<span style="padding-left: 20px;">var deferred = new $.Deferred();</span>
<span style="padding-left: 20px;">MyUtil.saveRemotely({key: value}, function(){</span>
<span style="padding-left: 40px;">success: function(){</span>
<span style="padding-left: 60px;">deferred.resolve();</span>
<span style="padding-left: 40px;">};</span>
<span style="padding-left: 40px;">failure: function(){</span>
<span style="padding-left: 60px;">deferred.reject()</span>
<span style="padding-left: 40px;">};</span>
<span style="padding-left: 20px;">});</span>
<span style="padding-left: 20px;">return deferred.promise();</span>
}</p>
Now as user calls function save() he will get as return value a promise object. The user can then register call backs on this promise object to be invoked as promise is resolved or rejected.
<p style="padding-left: 90px;">var myPromiseObject = myModelObject.save({'id': 1});</p>
<p style="padding-left: 90px;">myPromiseObject.done(fuction(){
<span style="padding-left: 20px;">console.log('Saved successfully');</span>
});</p>
<p style="padding-left: 90px;">myPromiseObject.fail(fuction(){
<span style="padding-left: 20px;">console.log('Error while trying to save');</span>
});</p>
<p style="padding-left: 90px;">myPromiseObject.always(fuction(){
<span style="padding-left: 20px;">console.log('Save method completed.');</span>
});</p>
These call backs can even be chained. There is another call back then which takes 3 functions, 1st will be invoked in case of success, 2nd in case of failure and 3rd will be executed always.
<p style="padding-left: 90px;">MyPromiseObject.then(
<span style="padding-left: 20px;">fuction(){</span>
<span style="padding-left: 40px;">console.log('Saved successfully');</span>
<span style="padding-left: 20px;">},</span>
<span style="padding-left: 20px;">fuction(){</span>
<span style="padding-left: 40px;">console.log('Error while trying to save');</span>
<span style="padding-left: 20px;">},</span>
<span style="padding-left: 20px;">fuction(){</span>
<span style="padding-left: 40px;">console.log('Save method completed.');</span>
<span style="padding-left: 20px;">}</span>
}</p>
Thus a complex pyramid of call backs like:
<p style="padding-left: 90px;">function1(value, function(value1){
<span style="padding-left: 20px;">function2(value1, function(value1){
<span style="padding-left: 40px;">function3(value2, function(value2){
<span style="padding-left: 60px;">function4(value3, function(value3){
<span style="padding-left: 60px;">})
<span style="padding-left: 40px;">})
<span style="padding-left: 20px;">})
} )</span></span></span></span></span></span></p>
can be reduced to:
<p style="padding-left: 90px;"><span style="line-height: 1.5em;">function1(value).</span>
then(function2).
then(function3).
then(function4)</p>
Promises are now being increasingly used in various JavaScript Api and Frameworks. The are great tools in next generation of JavaScript design patterns.</p>