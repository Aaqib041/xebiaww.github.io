---
layout: post
header-img: img/default-blog-pic.jpg
author: Priyanka
description: 
post_id: 12454
created: 2012/03/02 11:35:02
created_gmt: 2012/03/02 06:35:02
comment_status: open
---

# Five best practices for effective mobile QA

In today’s world, ever increasing inclination towards mobile devices and applications has provided great prospects for enterprises to move their business in mobile world. As mobile technology lets the customers stay connected for almost all the time, enterprises are moving beyond the desktop world to keep attuned to their customer needs. But development of mobile applications with an impeccable user interface and functionality for highly fragmented mobile market is in itself an ordeal. Thus quite evidently mobile application development has become a booming field in the IT industry. Few days back, I met with a guy who is mobile QA from long time. I discussed with him few of the best practices which shall be followed for functional testing of mobile applications. So through this blog I want to share few of those practices which should be followed by mobile QA while doing testing for any mobile application.

**1\. Emulators can be used for limited functionality testing only**

Emulators are just mimic to actual devices in terms of functionality. They can never be treated as substitutes for actual devices. For a start, device emulators and simulators can act as an stepping stone in testing mobile applications but totally relying on them can lead to catastrophe. One of the prime examples where emulators can be used is testing design level implementations, for instance if the desired icons are on a specific screen or not. The biggest limitation of emulators is that they do not work on real network and are not prone to real world interrupts and for this reason, we need real devices to test applications. For instance emulators can be used in location based apps by simulating a route, but actual interrupts occurring on these routes can only be tested using real devices.

**2\. Field testing is uber important**

Mobile field testing provides testing assurance of the mobile device’s workability in the actual field. This will ensure that the device works not only in the Static field environment but also in the dynamic field environment. Few years back, there was one research conducted in Helsinki where users were given 10 different tasks to perform on application at different locations. Although that application was already tested before in their laboratories but during field testing users found many different functional bugs due to signal discrepancies and other interrupts.

**3\. Never assume one application works for all**

Market is full of devices having different configurations, operating systems, features and assuming if things are working fine on one device  then they will be fine on every device is not a good practice at all. Every device has its own characteristics due to which some features of application may fit well in one device and may not in other device. For example:- Samsung and Nokia might come up with any native application solution. Though the basic functionality is the same for both the apps but the look and feel, form factor of the applications might be largely different from each other.

**4\. Keep interrupts in mind while mobile app testing**

Wireless networks are prone to functional and network interrupts. Lets take an example where user is disbursing using his debit card and in between that transaction there is a signal loss for fraction of seconds. In that case application should be able to intimate user about signal loss or should be able to resume from where signal was lost then his transaction should be completed after this interrupt is over. This was just one example of interrupt. There can be other types of interrupts like receiving message, receiving calls which are uncontrollable. The concept of interrupts is important in every type of applications and they should be tested with extra care specially in case of financial applications like mobile banking.

**5\. Give special focus on usability**

Good user interface can play crucial role in software industry.Any software product can be made with provided set of functionality but an intelligent user interface can make a product hit. Intelligent user  interface means it should be easily used by end-users. There are unique features in usability of a mobile application when compared to normal desktop application. Some of the examples are mentioned below:-

  1. Mobile devices are designed for both left handed and right handed people. In that case, QA should focus on convenience perspective so that mobile applications can be used by both set of people.

  2. Mobile applications should be tested according to phone orientation as mobile applications can be viewed either in landscape or portrait format.  Here, QA should focus on testing on different screen resolution as well as different orientation format.

  3. In addition to usability testing in different orientation,  usability should also be tested for touchscreen screens, qwerty keypads,etc. Example : sufficient usability testing should be performed to ensure that the keyboard is visible only when it is required while inputting text, numbers, etc.

## Comments

**[Pravat](#7837 "2012-03-02 17:50:08"):** Hi , I’m truly appreciated by reading your blog and love to find some new and updated technologies blog in near future…. You have just enhanced the real learning desire of all internet readers by showcasing your informative blog. However I have bookmarked your site, and hope you will post some informative blog quickly …………. Keep it up !

**[Priyanka](#7859 "2012-03-04 21:47:19"):** Hi Pravat, Thanks for reading my blog. Yes, I will surely come up with more informative blogs on mobile testing. Thanks, Priyanka

**[Bhavesh Panchal](#7986 "2012-03-21 11:07:36"):** Hi Priyanka, Nice Blog.... Please update some more content :) If you now , Please share name of the company Which works for Field testing... Many thanks Br... Bhavesh Panchal

