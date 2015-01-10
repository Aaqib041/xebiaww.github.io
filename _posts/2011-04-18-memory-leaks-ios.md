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

<p>I recently started working in iPhone (iOS) development. Before that I was having a happy life while working on Java-Flex technology. It is a big shift from wider to smaller space if you compare Java with Objective-C.</p>
<p>Java guy doesn’t need to know much about the memory-management because he has a very good friend in form of Mr. Garbage-Collector who is always present to take care about memory worries. Now when you move to iOS dev, you are surrounded by the complex world of Retain-Release cycle. Yes you guessed it right, I am talking about Manual Memory Management (MMM). The bigger consequences of not implementing it right way or taking it into account results in a worthless app and embarrassing moment. For instance, you are giving a demo of your app to the customer, which you wrote working hard and wrote a lot of unit test cases, but to your shock it suddenly CRASHES in the demo.
<!--more--></p>
<p>[<strong><em>Note</em></strong><em>: </em><em>In MacOSXv10.5 and later, you can use automatic memory management by using Garbage Collection but garbage collection is not available on <strong>iOS</strong>.]</em></p>
<p>Application crashes mainly happen due to memory-leaks which is not very common scenario for a Java developer.</p>
<p>So it's important to take some memory management lessons while shifting from Java to Objective-C programmer.</p>
<p>Lets move to learn the basic concept of MMM (Retail-Release) in Objective C. The fundamental rule is to “<em>take care the objects you create</em>”. When you use <em>new</em>, <em>alloc</em> operators or <em>copy</em> for creating object or you use <em>retain</em> then you should use <em>release</em> or <em>autorelease</em> call to that object. In Objective-C, memory-management works on simple reference count mechanism, which says if object's retain count is ZERO, the object is destroyed (means memory is freed). Each object has a property called retain-count which tracks the number of references for that object. When you use <em>alloc</em>, <em>new</em>, <em>copy</em>, <em>retain</em>, the retain count for that object is increased by ONE. When you use "release" its retain-count gets decremented by ONE. The focus here should be to maintain a Zero balance of reference-count for an object.</p>
<p>Let's take a look at a sample <strong>Objective C </strong> code</p>
<p><span style="color: #800080;"><em>MyClass *mClass = [[MyClass alloc] init];</em></span></p>
<p><span style="color: #800080;"><em>…</em></span></p>
<p><span style="color: #800080;"><em>(use this mClass anywhere but remember to free its memory when done)</em></span></p>
<p><span style="color: #800080;"><strong><em>[mClass  release];</em></strong></span>
<em> </em></p>
<p>In above sample code, <em>new</em> operator is used to create a new dynamic object at heap-space and <em>release</em> is used to deallocating the memory.
Now a java developer tends to forget the release part of code so for them here comes the savoir from the Apple it is called <em>Analyze</em>. It is a static code analyzer tool that helps developer to find Memory leaks in your code before the code is run. Although <em>Apple </em>has lot of powerful performance and memory monitoring tools for e.g. <em>Instruments</em>, but we will discuss about a simple Analyze tool, which is very easy to use and solves your big problem (for Java developer) very easily.</p>
<p>I will give you a simple example that will show you how intuitive are the tool in letting you find the memory leaks.</p>
<p>Consider the code above that you have written in Xcode (for me version 4) and as a java developer you forgets to release the memory (very common in initial days).</p>
<p><a rel="attachment wp-att-8506" href="http://xebee.xebia.in/2011/04/18/memory-leaks-ios/xcode1-3/"><img class="aligncenter size-full wp-image-8506" title="xcode" src="http://xebee.xebia.in/wp-content/uploads/2011/04/xcode12.png" alt="xcode" width="669" height="466" /></a></p>
<p>Now in your Xcode (version 3.2 or higher) Go to <strong><em>Product-&gt;Analyze</em></strong> then you will see something like this</p>
<p><a rel="attachment wp-att-8507" href="http://xebee.xebia.in/2011/04/18/memory-leaks-ios/xcode2-3/"><img class="aligncenter size-full wp-image-8507" title="xcode" src="http://xebee.xebia.in/wp-content/uploads/2011/04/xcode22.png" alt="Xcode Analyze" width="669" height="466" /></a></p>
<p>This gives you a clear picture that something you have forget please rectify, so now we add the release code and it is DONE.</p>
<p><a rel="attachment wp-att-8508" href="http://xebee.xebia.in/2011/04/18/memory-leaks-ios/xcode3-3/"><img class="aligncenter size-full wp-image-8508" title="xcode" src="http://xebee.xebia.in/wp-content/uploads/2011/04/xcode32.png" alt="xcode" width="669" height="466" /></a></p>
<p><strong><em>Note</em></strong><em>: This a basic tutorial for checking memory leaks. Memory leaks can be surfaced for other reasons that can be profiled using Product-&gt;Profile tool.</em></p>