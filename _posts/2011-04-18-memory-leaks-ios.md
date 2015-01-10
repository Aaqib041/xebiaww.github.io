---
layout: post
header-img: img/default-blog-pic.jpg
author: psuneja
description: 
post_id: 8467
created: 2011/04/18 22:09:08
created_gmt: 2011/04/18 17:09:08
comment_status: open
---

# iPhone Memory leaks – An Introduction

I recently started working in iPhone (iOS) development. Before that I was having a happy life while working on Java-Flex technology. It is a big shift from wider to smaller space if you compare Java with Objective-C.

Java guy doesn’t need to know much about the memory-management because he has a very good friend in form of Mr. Garbage-Collector who is always present to take care about memory worries. Now when you move to iOS dev, you are surrounded by the complex world of Retain-Release cycle. Yes you guessed it right, I am talking about Manual Memory Management (MMM). The bigger consequences of not implementing it right way or taking it into account results in a worthless app and embarrassing moment. For instance, you are giving a demo of your app to the customer, which you wrote working hard and wrote a lot of unit test cases, but to your shock it suddenly CRASHES in the demo. 

[**_Note_**_: __In MacOSXv10.5 and later, you can use automatic memory management by using Garbage Collection but garbage collection is not available on **iOS**.]_

Application crashes mainly happen due to memory-leaks which is not very common scenario for a Java developer.

So it's important to take some memory management lessons while shifting from Java to Objective-C programmer.

Lets move to learn the basic concept of MMM (Retail-Release) in Objective C. The fundamental rule is to “_take care the objects you create_”. When you use _new_, _alloc_ operators or _copy_ for creating object or you use _retain_ then you should use _release_ or _autorelease_ call to that object. In Objective-C, memory-management works on simple reference count mechanism, which says if object's retain count is ZERO, the object is destroyed (means memory is freed). Each object has a property called retain-count which tracks the number of references for that object. When you use _alloc_, _new_, _copy_, _retain_, the retain count for that object is increased by ONE. When you use "release" its retain-count gets decremented by ONE. The focus here should be to maintain a Zero balance of reference-count for an object.

Let's take a look at a sample **Objective C ** code

_MyClass *mClass = [[MyClass alloc] init];_

_…_

_(use this mClass anywhere but remember to free its memory when done)_

**_[mClass  release];_** _ _

In above sample code, _new_ operator is used to create a new dynamic object at heap-space and _release_ is used to deallocating the memory. Now a java developer tends to forget the release part of code so for them here comes the savoir from the Apple it is called _Analyze_. It is a static code analyzer tool that helps developer to find Memory leaks in your code before the code is run. Although _Apple _has lot of powerful performance and memory monitoring tools for e.g. _Instruments_, but we will discuss about a simple Analyze tool, which is very easy to use and solves your big problem (for Java developer) very easily.

I will give you a simple example that will show you how intuitive are the tool in letting you find the memory leaks.

Consider the code above that you have written in Xcode (for me version 4) and as a java developer you forgets to release the memory (very common in initial days).

![xcode][1]

Now in your Xcode (version 3.2 or higher) Go to **_Product->Analyze_** then you will see something like this

![Xcode Analyze][2]

This gives you a clear picture that something you have forget please rectify, so now we add the release code and it is DONE.

![xcode][3]

**_Note_**_: This a basic tutorial for checking memory leaks. Memory leaks can be surfaced for other reasons that can be profiled using Product->Profile tool._

   [1]: http://xebee.xebia.in/wp-content/uploads/2011/04/xcode12.png (xcode)
   [2]: http://xebee.xebia.in/wp-content/uploads/2011/04/xcode22.png (xcode)
   [3]: http://xebee.xebia.in/wp-content/uploads/2011/04/xcode32.png (xcode)