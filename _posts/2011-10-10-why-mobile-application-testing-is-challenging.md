---
layout: post
header-img: img/default-blog-pic.jpg
author: Priyanka
description: 
post_id: 9900
created: 2011/10/10 23:23:09
created_gmt: 2011/10/10 18:23:09
comment_status: open
---

# Why mobile application testing is challenging?

During the last decade, there has been tremendous growth in the number of mobile users and mobile devices. Total number of mobile users have grown more than 852 million in India and 2.6 billion worldwide. This tremendous growth of mobile users has opened up a new market which is flooded with various mobile platforms and devices like Blackberry, iPhone, Android devices, other smart phones and tablets, and thousands of applications to run on them. Gradually, businesses are shifting focus towards mobile because users are more interested in browsing the web on their pocket size devices which are more portable. This is the reason why mobile applications and websites are becoming increasingly important for enterprises and they are progressively looking towards mobile while looking to extend their business solutions to reach a wider audience.

The diversity of mobile environment, small size of mobile devices and complexity of mobile hardware and software bring in several challenges in quality assurance which demand advanced testing strategies. Below are some points which explain in brief that what the real challenges are in mobile testing, and why such challenges arise?

**1) Diversity in Mobile Device platforms:-**

Android, Blackberry, Nokia’s Symbian and Apple’s iPhone have together grabbed a large part of the Smartphone market, but these are not the only mobile platforms; there are many other mobile platforms that are in use like BREW, BREWMP, Windows 7, etc. While testing any multiplatform mobile application, it would be required to test it on each platform while carrying out  UI testing, functionality testing,etc. It is bit difficult and challenging because there might be chances of inconsistency in terms of functionality or UI across multiple platforms as there are thousands of mobile platforms in market and every platform may have some limitations.It is simply not possible to test mobile app exhaustively on each type device available in market.

In addition, there are few other testing areas that needs to be taken care for each and every platform. I am explaining few additional testing aspects for some commonly used platforms below:-

_**Apple:-**_ While making iphone applications we need to keep in mind few things like:- 

  1. UI Guidelines from Apple need to be adhered to when making applications/websites for IOS.
  2. They should have Backward OS compatibility
  3. Only one application should be allowed to run at a time.
_**Android:-**_ We need to keep in mind some additional testing aspects while working for android platform which are :- 

  1. Android allows to run multiple applications in background
  2. Application gets normally minimized on exiting.
_**Blackberry:-**_ Some additional testing aspects for Blackberry applications are given below:- 

  1. It  allows running multiple applications in background
  * Device Reboot is needed for clearing cached data
  * Device Reboot is needed for uninstalling/installing any application.
  * Should be Signed/certified by RIM
**2) Diversity of the Mobile Devices:-**

There is a huge variety of mobile devices available  in market with different screen sizes , different input methods like touchscreen, qwerty keypads, trackball and each of them having different hardware capabilities. We know the fact that testing on every device is not possible and feasible because we cannot buy every handset and test on each. We know that exhaustive testing of user interfaces is very important to ensure compatibility of the application and verifying UI consistency on every device is important which is really challenging and time consuming task. It seems that the diversity in handsets is a big challenge for testing. We know that mobile devices also have different application runtimes like Binary Runtime Environment for Wireless Java and embedded visual basic runtime, etc which means applications should be tested specific to runtime also.

**3) Diversity in Hardware Configuration:-**

Apart from diversity in platform and mobile devices, there is diversity in their hardware also. There are many drawbacks of diverse hardware configurations. Some of the drawbacks are discussed here like Mobile environment provides lesser memory and processing power for computing as compared to PC which results in limitations in processing speed and variations in performance of applications. Therefore, testing should ensure that the applications deliver optimum performance for all desired configurations of hardware. Some mobile devices communicate through WAP and some use HTTP for communication. Applications should be tested for their compatibility with WAP-enabled as well as HTTP-enabled devices.

**4) Diversity in Network**

We know there is always unpredictability in network latency when applications communicate over network boundaries, leading to inconsistent data transfer speeds. It demands testing to measure the performance of applications for various network bandwidths of various service providers. Wireless network use data optimizers like gateways to deliver content and it may result in decreased performance in case of heavy traffic.  Therefore, testing should be performed to determine the network traffic level at which gateway capabilities will impact the performance of the mobile application.

There are few solutions which can be used to overcome few of  the challenges mentioned above while testing mobile application. I would recommend mobile testers to use a combination of the below mentioned techniques along with testing applications on real devices on real network: - 

  * Using Test automation to increase the test coverage. Some automation tools available for mobile apps testing are TestComplete, Test Quest Pro, Robotium, Sikuli, Deviceanywhere, FoneMonkey(iPhone), Eggplant(iPhone), TestiPhone(For iPhoneMobileWeb), IBM® Rational® Performance Tester (RPT), MITE(A Mobile content testing and validation tool for Mobile Web app).
  * Testing on Mobile Emulators mainly for Unit and Integration testing. The emulators could be used to conduct performance, GUI and compatibility testing also but the testing using actual devices is preferred.
  * Testing on Remote Real devices using Remote Device Access.
  * Testing on cloud-based platforms, like the Perfecto Mobile Handset Cloud which helps QA teams to test their widgets, applications and mobile content on variety of handsets.

## Comments

**[Nikhil Mehta](#6246 "2011-11-23 12:58:15"):** Priyanka Substantial information on mobile testing. Very nice

**[Vikrant Taneja](#6015 "2011-10-12 15:26:25"):** Hi Priyanka, I think the effort you gave and the knowledge you shared is appreciable. I believe it will be very beneficial for people who are new to mobile testing. Thanks, Vikrant

**[Surajit Purkayastha](#6140 "2011-11-07 18:40:49"):** Hi Priyanka, This text is super helpful for testers who are naive in this field. I would appreciate if you could exemplify a test case / test scenario for, lets say a android application. Surajit

**[Devendra Acharya](#7918 "2012-03-14 16:14:10"):** Hey Priyanka, Really this is very useful information..

