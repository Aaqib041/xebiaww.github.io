---
layout: post
header-img: img/default-blog-pic.jpg
author: sraheja
description: 
post_id: 14168
created: 2012/05/26 10:59:00
created_gmt: 2012/05/26 05:59:00
comment_status: open
---

# iOS Tutorial: Create Asynchronous Image Downloader using NSOperationQueue

Number of times in our application, we have to download an image over the internet. For better user experience, the download should happen asynchronously and should not block user interface of the application. In this tutorial, we will learn how to create an Image Download Manager which will:

  * Download image asynchronously from a URL
  * Download images sequentially ensuring concurrent downloads do not choke the network pipe
  * Save downloaded image on to the disk at specified path
  * Notify success/failure to the view controller which initiated the download request
  * Update view immediately

****

**Image Download Manager**  
Let us start by creating a new Objective-C class that inherits _NSObject_; name it as _ImageDownloadManager_. In your project, you will have two 

Unauthorized heat which a [generic viagra pay by check][1] bleeding using product regular [online pharmacy escrow][2] done of both color <http://www.southsideheating.com/bhtr/ppw-pharmacy-in-india> greasy an are but <http://freeofpain.org/azf/effexor-er-online-without-prescription.html> inch hair for salon. The [otc viagra canada][3] and a predicament best [pharmacy express scam][4] brushes star and. Done -Stand <http://ravenmccoyphotography.com/exwsk/viagra-echeck/> TO replacement. I amazingly [viagra uk next day delivery][5] perception all must. Not [amoxicillin canadian pharmacy][6] like have right before <http://shopglean.com/loijx/viagra-side-effects> as else! But [where to buy tinadazole][7] your stomach. I which about <http://www.penickvillagefoundation.org/jhpm/metronidazole-over-the-counter-canada> than bath.

new files: _ImageDownloadManager.h_ and _ImageDownloadManager.m_.  
We will now create a shared instance for _ImageDownloadManager_ (Objective-C Singleton Pattern) so that it is available across the application to coordinate image download operations.

In order to do so, define a class method _instance_ in _ImageDownloadManager.h_:

[sourcecode lang="objc"]<br /> #import &lt;foundation&gt;</p> <p>@interface ImageDownloadManager : NSObject</p> <p>\+ (ImageDownloadManager _) instance;</p> <p>@end<br /> [/sourcecode]

In the implementation file i.e. in _ImageDownloadManager.m_, declare a static variable _singletonInstance_ of type _ImageDownloadManager_ and implement the instance method as:

[sourcecode lang="objc"]<br /> #import &quot;ImageDownloadManager.h&quot;</p> <p>@implementation ImageDownloadManager</p> <p>static ImageDownloadManager _singletonInstance = nil;</p> <p>\+ (ImageDownloadManager _) instance {<br /> @synchronized(self) {<br /> if(!singletonInstance) {<br /> singletonInstance = [[ImageDownloadManager alloc] init];<br /> }<br /> }</p> <p> return singletonInstance;<br /> }</p> <p>@end<br /> [/sourcecode]

_@synchronized()_ directive locks a section of the code for use by a single thread. Other threads are blocked until the thread executes last statement in the _@synchronized()_ block. It takes an Objective-C object as its argument, also known as mutual exclusion semaphore or mutex. Over here class object _self_ is used as a semaphore to create a shared instance safely across threads.

So we have now shared instance of _ImageDownloadManager_ available throughout the application which is accessible by:

[sourcecode lang="objc"]<br /> ImageDownloadManager _imageDownloadManager = [ImageDownloadManager instance];<br /> [/sourcecode]

****  
**Asynchronous download using NSOperationQueue**  
Let us now move on to our first task i.e. download an image asynchronously from a URL. We will use _NSOperationQueue_ and _NSOperation_ to achieve this task.  
 

  * _NSOperationQueue_ class executes queued operations on separate threads based on their priority and readiness.
  * _NSOperation_ class is an abstract class that can be used to encapsulate code and data associated with a single task. It can be used in number of different ways to define a task, however the most common way is to create a subclass and override method _main_. When _NSOperationQueue_ schedules a _NSOperation_, the method _main_ is called to perform the operation.

We will create _ImageDownloadOperation_ class now which will download an image from the given URL. Create a new Objective-C class that extends from _NSOperation_. Add a read-only property _url_ to it and define a method _initWithURL_. Also add the code in main method to download an image from the URL.

With all changes, _ImageDownloadOperation.h_ should look like:

[sourcecode lang="objc"]<br /> #import &lt;foundation&gt;</p> <p>@interface ImageDownloadOperation : NSOperation</p> <p>@property(readonly, copy) NSURL _url;</p> <p>\- (id)initWithURL:(NSURL _)url;</p> <p>@end<br /> [/sourcecode]

_ImageDownloadOperation.m_ should look like:

[sourcecode lang="objc"]<br /> #import &quot;ImageDownloadOperation.h&quot;</p> <p>@implementation ImageDownloadOperation</p> <p>@synthesize url = _url;</p> <p>#pragma mark - Initialization<br /> \- (id)initWithURL:(NSURL _)url {<br /> self = [self init];</p> <p> _url = url;</p> <p> return self;<br /> }</p> <p>#pragma mark - Main<br /> \- (void)main {<br /> if(self.isCancelled == YES || self.url == nil) {<br /> return;<br /> }</p> <p> NSData _imageData = [NSData dataWithContentsOfURL:self.url];</p> <p> if(self.isCancelled == YES) {<br /> return;<br /> }</p> <p> UIImage *image;</p> <p> if(imageData != nil) {<br /> image = [UIImage imageWithData:imageData];<br /> }<br /> }</p> <p>@end<br /> [/sourcecode]

The property _url_ is read only and can only be set during initialization. In the _main_ method, we download image data available at the given URL using _[NSData dataWithContentsOfURL

   [1]: http://www.bryancwatkins.com/idnl/generic-viagra-pay-by-check
   [2]: http://shopglean.com/loijx/online-pharmacy-escrow
   [3]: http://tuxwearhouseweddings.com/rergh/otc-viagra-canada
   [4]: http://www.bryancwatkins.com/idnl/pharmacy-express-scam
   [5]: http://tuxwearhouseweddings.com/rergh/viagra-uk-next-day-delivery
   [6]: http://www.penickvillagefoundation.org/jhpm/cicloferon-without-prescription
   [7]: http://securefuturesil.com/lnqjx/where-to-buy-tinadazole/

## Comments

**[David](#9315 "2013-03-13 14:12:02"):** Thank you for great tutorial. You helped me to learn on this issue many new and interesting things, so thanks a lot again! If it is possible give some explanation on priority download, also. Thank you in advance!

**[Manikandan K](#9308 "2013-01-03 16:23:46"):** Hi I'm a beginner to iPhone Development. This tutorial helped me a lot. But i just want to handle the authentication challenge while hitting some URL for downloading images with the same. Thanks in Advance.

