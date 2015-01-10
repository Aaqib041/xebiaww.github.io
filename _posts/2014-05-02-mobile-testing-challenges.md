---
layout: post
header-img: img/default-blog-pic.jpg
author: Shankar Garg
description: 
post_id: 18303
created: 2014/05/02 10:05:28
created_gmt: 2014/05/02 05:05:28
comment_status: open
---

# Mobile Testing Challenges

<p><span style="font-family: verdana, geneva;font-size: 12px">Before we understand what are the challenges faced in Mobile Testing, Lets first focus on what is the scope of Mobile Testing and then we will dig deeper in the world of Mobile Testing.</span>
<p class="clean alert"><span style="text-decoration: underline;color: #0000ff;font-family: verdana, geneva;font-size: 12px">Scope of Mobile Testing</span></p>
<span style="font-family: verdana, geneva;font-size: 12px">Mobile Testing covers following Apps:</span>
<ul>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Web Apps: Web applications opened on mobile browsers (chrome, safari, opera etc.)</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Native Apps: application program that has been developed for use on a particular platform or device (android, iOS etc.)</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Hybrid Apps : Native Apps + Web Apps</span></li>
</ul>
<span style="text-decoration: underline;font-family: verdana, geneva;font-size: 12px"><span style="color: #0000ff;text-decoration: underline">Challenges in Mobile Testing</span></span></p>
<p><span style="font-family: verdana, geneva;font-size: 12px">The objective of testing is same for both website and mobile i.e. if the AUT meets the quality criteria or not, but there are some issues/challenges/factors that are only relevant to Mobiles and are of no importance to traditional Website Testing.</span></p>
<p><span style="font-family: verdana, geneva;font-size: 12px">                     Here in my blog, I have tried to list down possible challenges we face in Mobile Testing and the factors that constitute that challenge. So it becomes very important to specially deal with these challenges to come up with an effective Mobile Test Strategy.</span>
<h2><span style="color: #3366ff"><span style="font-family: verdana, geneva;font-size: 12px"> </span><span style="font-family: verdana, geneva;font-size: 12px"><i>Hardware Challenge</i></span></span></h2>
<span style="font-family: verdana, geneva;font-size: 12px">Mobile application testing is difficult because you need to test for various hardware configurations which is a result of different combinations of the factors mentioned below:</span>
<ul>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Various Manufacturers (Samsung, Apple etc.)</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Various Screen Sizes (Phone and Tablet etc.)</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Keypad – (Virtual or hardware)</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Variations in RAM, CPU, OS utilization etc.</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Touch Screen Responsiveness.</span></li>
</ul>
<span style="font-family: verdana, geneva;font-size: 12px">Did you know: - <span style="color: #ff0000">There<span style="text-decoration: underline"> is <b><i>No Guarantee</i></b> that application tested on one device will run successfully run on other device also.</span></span></span>
<h3><span style="font-family: verdana, geneva;font-size: 12px;color: #3366ff"><i>Software Challenge</i></span></h3>
<span style="font-family: verdana, geneva;font-size: 12px">Mobile application testing is difficult because you need to test for various software configurations which is a result of different combinations of the factors mentioned below:</span>
<ul>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Various OS (iOS, android, Windows etc.)</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Various Versions of OS (iOS 4 to iOS 7.1, android 4.2.2 to android 4.4 etc.)</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Also need to test on upgraded OS versions( i.e. from iOS 6 to iOS 7)</span></li>
</ul>
<span style="font-family: verdana, geneva;font-size: 12px">Did you know:-<span style="text-decoration: underline"><span style="color: #ff0000;text-decoration: underline">OS like iOS does not allow the device to be downgraded to lower iOS once upgraded to higher iOS.</span> <span style="color: #ff0000;text-decoration: underline">So if do not keep this in mind, it can exponentially increase the testing cost. </span></span></span></p>
<p><em><span style="color: #3366ff"><strong><span style="font-family: verdana, geneva;font-size: 12px">Network Challenge</span></strong></span></em></p>
<p><span style="font-family: verdana, geneva;font-size: 12px">Mobile application testing is difficult because you need to test for various Network configurations which is a result of different combinations of the factors mentioned below:</span>
<ul>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Network Carrier (Airtel , Vodafone etc.)</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Bandwidth (2G, 3G, Broadband etc.)</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px">No Network, Airplane Mode</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Network Connectivity (low, high etc.)</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Replicate diverse Geo-graphical locations?</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Test GPS-based Operations at real Site?</span></li>
</ul>
<span style="font-family: verdana, geneva;font-size: 12px">Did you know:- <span style="color: #808000"><i><span style="text-decoration: underline">Network Simulators can be used for creating artificial network of varying speeds 2G,3G etc.</span></i></span></span>
<h2><span style="color: #3366ff"><em><strong><span style="font-family: verdana, geneva;font-size: 12px">Interrupts Challenge</span></strong></em></span></h2>
<span style="font-family: verdana, geneva;font-size: 12px">This is one area which is quite often missed by testing teams until the alpha or beta testing and this is one area which is also most often missed by developers also. So as soon as you start testing for interrupts, be ready for some crazy behavior.</span></p>
<p><span style="font-family: verdana, geneva;font-size: 12px">But creating the real life scenarios for this kind of testing is a real head-ache for the testing team. So you need to innovative in creating real life scenarios for:</span>
<ul>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Phone Calls</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Text Messages</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Push Notifications from other apps</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Push Notifications from AUT</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Battery/OS level interrupts</span></li>
</ul>
<span style="font-family: verdana, geneva;font-size: 12px">Did you know:-<span style="color: #ff0000"><i><span style="text-decoration: underline">While accessing internet if a calls comes than Internet connection is interrupted.</span></i></span></span></p>
<p><span style="color: #3366ff"><strong><span style="font-family: verdana, geneva;font-size: 12px"><i>Data Security</i></span></strong></span></p>
<p><span style="font-family: verdana, geneva;font-size: 12px">This is also one area that is quite often not given the importance by testing teams and also not understood fully also. Data Security is a big challenge for the testing teams because now a days all sorts of user data is present on Mobile Handsets so security of that data is big daunted task.</span>
<ul>
    <li><span style="font-family: verdana, geneva;font-size: 12px">Confidentiality: Does your app keep your private data private?</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px"> Integrity: Can the data from your app be trusted and verified?</span></li>
    <li><span style="font-family: verdana, geneva;font-size: 12px"> Authentication: Does your app verify you are who you say you are?</span></li></p>