---
layout: post
header-img: img/default-blog-pic.jpg
---

# Mobile Testing Challenges

Before we understand what are the challenges faced in Mobile Testing, Lets first focus on what is the scope of Mobile Testing and then we will dig deeper in the world of Mobile Testing.

Scope of Mobile Testing

Mobile Testing covers following Apps:

  * Web Apps: Web applications opened on mobile browsers (chrome, safari, opera etc.)
  * Native Apps: application program that has been developed for use on a particular platform or device (android, iOS etc.)
  * Hybrid Apps : Native Apps + Web Apps
Challenges in Mobile Testing The objective of testing is same for both website and mobile i.e. if the AUT meets the quality criteria or not, but there are some issues/challenges/factors that are only relevant to Mobiles and are of no importance to traditional Website Testing.                      Here in my blog, I have tried to list down possible challenges we face in Mobile Testing and the factors that constitute that challenge. So it becomes very important to specially deal with these challenges to come up with an effective Mobile Test Strategy.

##  _Hardware Challenge_

Mobile application testing is difficult because you need to test for various hardware configurations which is a result of different combinations of the factors mentioned below:

  * Various Manufacturers (Samsung, Apple etc.)
  * Various Screen Sizes (Phone and Tablet etc.)
  * Keypad – (Virtual or hardware)
  * Variations in RAM, CPU, OS utilization etc.
  * Touch Screen Responsiveness.
Did you know: - There is **_No Guarantee_** that application tested on one device will run successfully run on other device also.

### _Software Challenge_

Mobile application testing is difficult because you need to test for various software configurations which is a result of different combinations of the factors mentioned below:

  * Various OS (iOS, android, Windows etc.)
  * Various Versions of OS (iOS 4 to iOS 7.1, android 4.2.2 to android 4.4 etc.)
  * Also need to test on upgraded OS versions( i.e. from iOS 6 to iOS 7)
Did you know:-OS like iOS does not allow the device to be downgraded to lower iOS once upgraded to higher iOS. So if do not keep this in mind, it can exponentially increase the testing cost.  _**Network Challenge**_ Mobile application testing is difficult because you need to test for various Network configurations which is a result of different combinations of the factors mentioned below:

  * Network Carrier (Airtel , Vodafone etc.)
  * Bandwidth (2G, 3G, Broadband etc.)
  * No Network, Airplane Mode
  * Network Connectivity (low, high etc.)
  * Replicate diverse Geo-graphical locations?
  * Test GPS-based Operations at real Site?
Did you know:- _Network Simulators can be used for creating artificial network of varying speeds 2G,3G etc._

## _**Interrupts Challenge**_

This is one area which is quite often missed by testing teams until the alpha or beta testing and this is one area which is also most often missed by developers also. So as soon as you start testing for interrupts, be ready for some crazy behavior. But creating the real life scenarios for this kind of testing is a real head-ache for the testing team. So you need to innovative in creating real life scenarios for:

  * Phone Calls
  * Text Messages
  * Push Notifications from other apps
  * Push Notifications from AUT
  * Battery/OS level interrupts
Did you know:-_While accessing internet if a calls comes than Internet connection is interrupted._ **_Data Security_** This is also one area that is quite often not given the importance by testing teams and also not understood fully also. Data Security is a big challenge for the testing teams because now a days all sorts of user data is present on Mobile Handsets so security of that data is big daunted task.

  * Confidentiality: Does your app keep your private data private?
  *  Integrity: Can the data from your app be trusted and verified?
  *  Authentication: Does your app verify you are who you say you are?