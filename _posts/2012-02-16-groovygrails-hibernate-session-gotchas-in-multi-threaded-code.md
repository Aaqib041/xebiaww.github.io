---
layout: post
header-img: img/default-blog-pic.jpg
---

# Groovy/Grails : Hibernate Session Gotchas in multi-threaded code

Recently I came across a situation where I had to delegate some portion of work from main thread to a new thread. I did not want user to keep waiting and hence returned response as soon as the desired work was completed and triggered supplementary work in new thread. However while doing so, I faced many challenges with respect to hibernate session in new thread. In this blog I will share my experiences with hibernate session in multi-threaded code and possible ways of handling it. Since currently I am working on Groovy/Grails, my example is modeled around it. But same concepts can be used in any framework.

Initially I used default implementation of Executor framework provided by Java to delegate supplementary work in separate thread. The advantage of using Executor framework is that we don't have to worry about Thread management. (You can read this [blog](http://insidethemachine.wordpress.com/2008/11/08/the-executor-framework/) to get quick insight on Executor framework.). Following is the code snippet for initializing and submitting task in executorService.

[sourcecode lang="groovy"]<br /> def executorService = Executors.newFixedThreadPool(10)<br /> executorService.submit(closure)<br /> [/sourcecode]

The problem with above implementation is that, as soon as we submit closure(task) in the executorService, the hibernate session used in current thread is not available in closure submitted in thread pool. As a result when we start executing database related logic with in the closure, all sorts of hibernate sessions related problems start coming. Let me elaborate more on this.  
Suppose we have a web portal, in which when user registers himself, he will receive welcome email from Portal. So basically when user hits "Register" button after providing his information on form, behind the scene two actions are performed.

Action1 : Validate and persist user information in database

Action2 : Send welcome email to user

Action 1 must be performed in the main thread in which user's request is getting served. However once validations are done and related information is saved in database, it will not be a great idea to keep user waiting while email is getting sent out. It requires network communication with Email Server and might take some time to complete the process. Hence it makes more sense to trigger email sending code in separate thread. In the below mentioned example, "register" method of UserService will be called in main thread and "sendEmail" in separate thread.

[sourcecode lang="groovy"]<br /> class UserService{</p> <p> def executorService = Executors.newFixedThreadPool(10)</p> <p> void register(User user){<br /> if(user.validate()){<br /> user.save()<br /> executorService.submit{<br /> sendEmail(user)<br /> }<br /> }else{<br /> //throw Exception to report errors<br /> }</p> <p> }</p> <p> void sendEmail(User user){<br /> EmailRequest emailRequest = new EmailRequest(user : user)<br /> try{<br /> //Email Code to send updated user information<br /> emailRequest.status = &quot;SUCCESS&quot;<br /> }catch(Exception e){<br /> emailRequest.status = &quot;FAILURE&quot;<br /> }<br /> emailRequest.save()<br /> }<br /> }<br /> [/sourcecode]

Now here, we first validate the user object and if it is valid, we save it in db. After saving user information, we submit a closure to send email in executorService which assigns the required task to one of the threads in pool. So basically "sendEmail" method is executed in separate thread and user get's instant response without waiting for email to be sent. When "sendEmail" method is invoked in separate thread, it will create a EmailRequest and try to send email to user. If it is successful, will save emailRequest with success status else with failure status for reporting purpose. Now to perform db operations a hibernate session is required. For the "register" method of UserService, hibernate session is automatically attached to main thread by Grails. However for "sendEmail", there won't be any hibernate session attached since we have not injected executorService implementation and have rather created on our own. As a result there is all sort of possibility of getting error "No hibernate session found" in sendEmail thread. To avoid this error a hibernate session either needs to be manually created and attached with new thread or should be injected by Grails. No doubt second option is desirable and in order to achieve that one must make use of Grails executor [plugin ](http://grails.org/plugin/executor)which takes care of injecting required hibernate session in threads created by Executor Framework. So now executorService will get auto injected and I no longer need to initialize it as mentioned below. And task submitted to executorService will have hibernate session attached to it's thread.

[sourcecode lang="groovy"]</p> <p>class UserService{</p> <p>def executorService</p> <p>...<br /> }</p> <p>[/sourcecode

## Comments

**[Nitin Khattar](#7582 "2012-02-16 20:27:59"):** Gagan, Very well explained blog. I wasn't aware of such hibernate gotchas in multi-threaded environment. Will be of Great Help...

**[Sunil Prakash Inteti](#7994 "2012-03-22 18:28:12"):** Good blog Gagan. Useful info wrt Executor service and the hibernate session. Personally I wouldn't use a background job at all which runs for every 15 mins thats a complete overhead. I would just push a JMS message on to the queue and later the message will be processed to send an email. Managing the jobs is always a pain.

**[Vijay Rawat](#7605 "2012-02-18 01:14:14"):** Thanks for pointing this. A good read.

