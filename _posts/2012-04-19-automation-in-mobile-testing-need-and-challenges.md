---
layout: post
header-img: img/default-blog-pic.jpg
author: Priyanka
description: 
post_id: 12900
created: 2012/04/19 21:36:52
created_gmt: 2012/04/19 16:36:52
comment_status: open
---

# Automation in Mobile Testing: Need and Challenges

Automation testing helps testers to reduce burden of repetitive tasks in manual testing. Many software companies are launching applications in industry and to keep pace with this fast moving mobile industry they have test and release new mobile applications or same application with more functionality rapidly. To meet this challenge, software companies are moving towards automation testing in mobile. We know that mobile testing is evolving and so is mobile automation testing. It is not completely matured like in case of website automation testing.

There are few tools available in market for mobile automation testing. It is true that mobile testing is still evolving and there is always increased pressure on companies to adopt to changing mobile operating system or in other words diversified nature of mobile phones. There are challenges in mobile automation testing which are mentioned below:-

1) **Functional Testing of multiple Mobile Screens:-**

There is no universal record/playback based automation tool available for different mobile screens, browser type, operating system, etc. For the applications that support various devices having resolution changes, like the iPhone and some Android, Blackberry and Windows Mobile models and etc. Testers need to make sure to include lots of testing where you rotate the device from portrait to landscape display, and vice versa, on all of the pages within your application. Automation can help in that case but there is lack of universal record/playback automation tool.

2)  **Testing with Actual Devices:-**

We know that Mobile devices support Application different types of input methods like external touchscreen or external keypad or internal touchscreen or internal keypad and etc. Therefore there is a need automation tool which can test not only on emulators but also on mobiles and should react according to "inputs given from mobile".

3) **Testing of Interrupts:-**

Everybody knows real networks introduce interrupts and ability of automation tools to keep check on network/software/network interrupts can help automation testers a lot. Lets see few of the test scenarios below for any application where we want to see how it will react before, during, and after the call:

  1. Your application is interrupted by an incoming call, originator/terminator hangs up the call

  2. Your application is interrupted by placing an outgoing call, originator/terminator hangs up the call

5) **Network Message Logging:-**

In spite of  interrupts, Mobiles working on real network are prone to signal loss, hand offs, weak signals also. For those reasons, "Automation tool needs to do recording and logging of network messages transmitted and received during mobile app usage".

For the applications that are going to be used on devices in various locations with various network connection speeds, it is important to plan testing coverage for the following scenarios:

  1. With no SIM card in the device

  2. In Airplane mode (or all connections disabled)

  3. intermittent network scenarios that a user might encounter in the real world like Walking out of Wi-Fi range so the connection automatically switches to 3G/2G (for example, in a large building like a Corporate Offices)

5) **Single tool for all Platforms:-**

There are several mobile operating systems in market and each day more advanced are coming in market. We do not have any single tool which can work for all platforms (Microsoft, Symbian, iPhone,android,etc.). There is need of automation tool which can work with all the platforms rather than a different tool for each platform.

6) **Proper End to End Testing:-**

There is also need of automation tools/frameworks which can perform proper End to End Testing where testers are able to perform connectivity from the device to the automated testing equipment (typically a PC).

## Comments

**[Priyanka](#8867 "2012-05-25 17:27:54"):** Hi Shraddha, There are many tools available in market for mobile automation like Robotium(which can be used to test android based application), sikuli( but your test cases might not work in different resolutions),seetest tool is also there. you can also use selenium for android mobile application testing.

**[Shraddha](#8801 "2012-05-16 14:18:46"):** Hi Priyanka, I am searching automation tool for mobile application testing. If you have any information about this then plz let me know.... :) Thanks, -Shraddha

