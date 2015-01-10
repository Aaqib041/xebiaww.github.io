---
layout: post
header-img: img/default-blog-pic.jpg
author: sohil
description: 
post_id: 7483
created: 2011/01/12 07:00:07
created_gmt: 2011/01/12 02:00:07
comment_status: open
---

# Swiz Framework 1.0 

<p>Swiz framework just announce 1.0 release , I have worked on other frameworks available in the market for development in Adobe Flex , after some research i found swiz the simplest , easiest and solves my purpose of using a framework without much hassle ,In this blog we are going to see two things why should i use Swiz framework and how to use it.</p>

<p><p><!--more--></p> <p>Following are the reasons that interest me for using the framework.</p> <ul> <li>You don't need to extend the framework specific classed</li> <li>Framework doesn't impose any design patterns</li> <li>No Boilerplate code</li> </ul> <p><strong>You don't need to extend the framework specific classed</strong><br /> I think this is a major reason why I would use swiz over the others. A lot of time while i m writing components i had to extend the events from my custom components. This made my custom components unusable for other projects. I had to often rewrite the events while reusing the components in another project. With Swiz while you are designing the custom components, events can be core flex event that extends Event class. Swiz provides a great flexibility of Event handling as well as Mediating. We will see in the code later how to handle events using Swiz</p> <p><strong>Framework doesn't impose any design patterns</strong><br /> This gives me a lot of flexibility in designing the application as I want. Earlier in one of the frameworks I had to implement the command pattern , etc. That did restrict me while designing the application. As whenever i had to design a functionality I had to take care of how I have to integrate with the underlying framework.</p> <p><strong>No Boilerplate code</strong><br /> Yet another added advantage. No more writing the same boiler plate code to add a new functionality every time. Actually this comes packaged when you have to impose the design patterns by the framework.</p> <p>A lot will be clearer in the video below. We are going to see the following things in the Video</p> <p>1. Swiz Framework<br /> 2. Configuring Swiz<br /> 2. How to use [Inject]<br /> 3. How to process events using Swiz<br /> 4. How to call server side service with one line of code<br /> 5. Dispatching the event without have to extend EventDispacher for non UI components.</p><object width="640" height="385"><param name="movie" value="http://www.youtube.com/v/FDVygVijpv8?fs=1&amp;hl=en_US"></param><param name="allowFullScreen" value="true"></param><param name="allowscriptaccess" value="always"></param><embed src="http://www.youtube.com/v/FDVygVijpv8?fs=1&amp;hl=en_US" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="640" height="385"></embed></object></p>

## Comments

**[Sohil](#5018 "2011-01-24 17:36:43"):** Hi Pushkar , can you specify a instance where parsley's IOC is better than swiz ? and what flexibility sdk 4.1 provides that is not provided by swiz ? Thanks for you inputs..

**[Sohil](#4880 "2011-01-12 19:16:10"):** Thank you !! @Age i have made the necessary changes

**[Age Mooij](#4877 "2011-01-12 14:47:38"):** It might be handy to mention somewhere in your post what this framework is actually for. There are some hints spread around here and there but until you google the Swiz framework or watch the video, you really have no idea this is a flex GUI framework.

**[Pushkar Singh](#4984 "2011-01-21 10:59:41"):** Swiz framework is one of the framework that can only be used particularly for smaller applications. Particularly for Application design and development and application with greater utilization framework like Parsley is very handy that offers 'IoC' capabilities with Flex 3.5. The Parsley M2 Spicelib Version is much more versatile and gives the much required Injection capabilities with very stronger binding. The latest Flex 4.1 SDK gives the much required flexibility which neither swiz nor carinogorm offers. Was just browsing through the blogs when came across yours'. Cheers!! Pushkar

**[Srirangan](#4869 "2011-01-12 07:53:39"):** Very cool. Bookmarked!

**[Brian Kotek](#4872 "2011-01-12 09:08:42"):** Very nice! The only correction I would note is that there is no longer an Outject metadata tag. It showed up in an early version of Swiz, but was then removed because it was confusing and wasn't actually very useful. Thanks for posting the video!

**[Sohil](#5304 "2011-02-17 10:10:32"):** @pggsb looks like you never took the pain to read the first paragraph :) Also video talks more about the fundamentals and how it works for someone who is new to swiz.

**[pggsb](#5288 "2011-02-13 03:58:56"):** For someone looking for more in-depth explanation of Swiz framework, this is very disappointing..It is basically the same as the QuickStart on Swiz's web site, just in point point/video form.. But thanks for the effort though..

