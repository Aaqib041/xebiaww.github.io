---
layout: post
header-img: img/default-blog-pic.jpg
author: srirangan
description: 
post_id: 2817
created: 2009/12/11 17:17:25
created_gmt: 2009/12/11 12:17:25
comment_status: open
---

# Enterprise adoption: Does HTML5 stand a chance against Flash technologies?

I had been following up on HTML5 for the last few months and Abhay's XKE session a couple of days ago really helped better understand the scope and capabilities of HTML5 as a technology. But having worked with Flex for nearly three years and being familiar with what the Flash platform does offer, there was not much in HTML5 that got me really excited.

In fact I am going to stick my neck out and make a series of claims and eventually conclude that the benefits of choosing Adobe Flex today completely overshadow HTML5. ![Content for Flash Player reaches 99% of Internet-enabled desktops][1]

## Flash/Flex reaches 99% of internet enabled desktops

I am not making this claim, Adobe is. And they have backed it up with real data including the [Milward Brown survey][2] conducted as recently as September 2009 showing Flash runtime as the clear leader in this segment. In addition the [Ubiquity survey for September 2009][3] shows that in average 93.5% of users in mature markets (i.e. U.S., Canada, Europe, Japan, Australia and New Zealand) use the latest version for the Flash plugin which is version 10.

Comparing this with HTML5, which is only partially supported by only the latest versions of Chrome, Opera, Safari and Firefox (all of which put together don't command a majority in the browser market share) is a no-contest. I'm with all of you and I use Chrome myself (with the Flash plugin!) and I am all for open standards prevailing over proprietary technologies, however when making a business decision personal preferences are to be put aside and market realities are to be acknowledged.

More people can view your web application if developed in Flash/Flash than in HTML5. I write this in December 2009 and I think this would hold true at least for the next 18 to 24 months. 

## Greater interoperability with your existing systems is achieved with Flash/Flex

Both HTML5 and Flash/Flex allow for asynchronous and persistent communications with server side layers. However Flex has been around longer than HTML5, hence its support for interoperability has matured significantly and provides for a lot more. Open technologies such as BlazeDS can be used to integrate Flex easily with your Java investments. I don't want to make this post read like an ad for Adobe so I am not mentioning Adobe LiveCycle but if you're looking for greater enterprise support, that would be the way to go. .Net too isn't unfamiliar territory for Flex with inbuilt support for SOAP based web services.

Other popular server side languages such as PHP and Python also have popular AMF libraries -- [AMFPHP][4], [AMFast ][5]and [PYAMF ][6]just to name a few.

Offering a technology stack including Adobe Flex, Python and AMFast deployed on Google infrastructure (via the Google App Engine) would give us a solution where in development time is low (Python and Flex are popular for rapid app development) while the solution itself is secure, well backed up and highly scalable. 

## Do more with Flash/Flex

I am yet to see a good enough data visualization application in HTML5 that competes with everything that Flex offers i.e. the chart components, the simple and advanced data grids and OLAP data grids as well. 

Data visualization being just one example. There is a lot more Flex can natively provide for that HTML5 simply does not at this point of time.  Not to mention the scores of open source and proprietary third party libraries available for ActionScript/Flex. [BirdEye][7], [PaperVision3D][8] to name a couple, and [this][9] is a good place to get started.

Capability-wise HTML5 / Javascript libraries are just not there yet. 

## Finding developers

[http://www.google.co.in/search?q="flex+developer"][10] returns 1,060,000 results [http://www.google.co.in/search?q="html5+developer"][11] returns 863 results

Not claiming this to be a scientific poll but you get my point! :)

Flash/Flex has been out there for many years now, good reference material is widely available, training programmes and certification exams are conducted and well supported by Adobe and third parties. Meanwhile HTML5 is still a "draft specification". 

## **Conclusion**

**As a developer I am excited about HTML5 and I would love to see an open technology succeed. But it would just not be practical to opt for HTML5 over Flex at the moment. The silver lining for HTML5 would be that it has an opportunity in the mobile devices segment where Adobe Flash does lag behind.**

   [1]: http://xebee.xebia.in/wp-content/uploads/2009/12/stats_432x309.gif (Content for Flash Player reaches 99% of Internet-enabled desktops)
   [2]: http://www.adobe.com/products/player_census/flashplayer/
   [3]: http://www.adobe.com/products/player_census/flashplayer/version_penetration.html
   [4]: http://www.amfphp.org/
   [5]: http://code.google.com/p/amfast/
   [6]: http://pyamf.org/
   [7]: http://birdeye.googlecode.com/svn/branches/ng/examples/demo/BirdEyeExplorer.html
   [8]: http://blog.papervision3d.org/category/demos/
   [9]: http://code.google.com/search/#q=flex
   [10]: http://www.google.co.in/search?q=%22flex+developer%22
   [11]: http://www.google.co.in/search?q=%22html5+developer%22