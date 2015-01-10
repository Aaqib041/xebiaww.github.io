---
layout: post
header-img: img/default-blog-pic.jpg
author: saket_vishal
description: 
post_id: 222
created: 2007/04/30 12:03:11
created_gmt: 2007/04/30 10:03:11
comment_status: open
---

# Do You Log from JavaScript? – Part I

<p class="MsoNormal" style="text-align: justify">Why do so many<span lang="EN-GB"> Java web applications write log messages from the Java code?</span><span lang="EN-GB"> </span><span lang="EN-GB">A normal answer would be, to capture the information about operation of an application. This captured information can further be used for debugging, troubleshooting and auditing<strong>. JavaScript continues to prosper as top scripting language and AJAX has acted as icing on the cake</strong>.

<!--more-->

JavaScript is also a popular scripting language to be used with Java web applications. At times the JavaScript becomes too complex and messy as its contribution increases in an application, making it difficult to assimilate. Things like debugging and troubleshooting get bit difficult and time consuming.</span> The paramount question on this situation to come up would be “<span lang="EN-GB">How many of these Java web applications write log messages from the JavaScript</span>?” I will leave the last question unanswered and instead I will raise some more questions. How do you feel when you see an application throwing a JavaScript error in production or even during development phase? Isn’t it annoying? Doesn’t it hurt? Many times we are unaware of solutions that exist for a problem which can make our life so much easier. Is there exists a simple solution to log from a JavaScript code? In the following part of this blog I will try to answer some of these questions.</p>

<p class="MsoNormal" style="text-align: justify"></p>

<p class="MsoNormal" style="text-align: justify">Many developers are not even aware of the availability of JavaScript loggers. However, various tools available for debugging JavaScript such as plugins for Firefox are used heavily by JavaScript developers these days. Are these tools really suppressing the need for JavaScript Loggers? I have tried to cover up the various techniques used by the available loggers for JavaScript followed up by their Pros & Cons.</p>

<p class="MsoNormal"></p>

<p class="MsoNormal" style="text-align: justify"><strong># Alert boxes</strong></p>

<p class="MsoNormal" style="text-align: justify">Many developers will argue on including this as a logging technique but as this is the most frequently used way for JavaScript debugging, so I have to mention it. This involves simply placing a number of alert boxes to view the status of a variable or to act as a notification.</p>

<p class="MsoNormal" style="text-align: justify"></p>

<blockquote>
<p class="MsoNormal" style="text-align: justify">+ Very easy to use, suitable for simple scripts.
-  No control of output.
-  Requires multiple clicks on alert boxes which can be annoying.</blockquote>

<p class="MsoNormal" style="text-align: justify"></p>

<p class="MsoNormal" style="text-align: justify"></p>

<p class="MsoNormal" style="text-align: justify"><strong># Plugins and Browser included tools</strong></p>

<p class="MsoNormal" style="text-align: justify">Another technique which is currently very popular in the developer community is the various plugins available for browsers like JavaScript console for Firefox and others. These plugins or add-ons generally get integrated with the browser and provide useful information related to our code.</p>

<p class="MsoNormal" style="text-align: justify"></p>

<p class="MsoNormal" style="text-align: justify"></p>

<blockquote>
<p class="MsoNormal" style="text-align: justify">+ Quick overview of errors.
-  Time required in learning, some debuggers can be very complex.
-  Portability issues as these tools are browser dependent.
-  Fail to provide the knowledge of flow of the code.
-  Lacks the ability to log from the code.</blockquote>

<p class="MsoNormal" style="text-align: justify"><strong># Logging by appending to the DOM</strong></p>

<p class="MsoNormal" style="text-align: justify">A nice replacement to the alert boxes is logging by appending messages to DOM in the HTML page. There are tons of JavaScript loggers of this breed offering logging with certain levels with indicating colors (using css). The common working pattern of these loggers involves a JavaScript function that appends log messages to a predefined HTML element in the page or by creating an element by itself.</p>

<p class="MsoNormal" style="text-align: justify"></p>

<p class="MsoNormal" style="text-align: justify"></p>

<blockquote>
<p class="MsoNormal" style="text-align: justify">+ Very helpful in development phase.
-  Messages by this logger can’t be persisted.</blockquote>

<p class="MsoNormal" style="text-align: justify"><strong> # Logging with AJAX pattern</strong></p>

<p class="MsoNormal">Loggers of this breed use the well known AJAX pattern to solve the log persistence problem raised by DOM manipulators or appenders, in this the error messages are communicated to the server through JavaScript where they can be persisted for further use.</p>

<blockquote>+ Allows persistence of error messages.
+ Allows gathering of client-side usage statistics, this can prove to be valuable information for the websites with rich user interfaces.
-  Raises an issue of user privacy as this technique can easily be quoted as used for spying on users.</blockquote>

<p class="MsoNormal" style="text-align: justify"></p>

<p class="MsoNormal" style="text-align: justify"></p>

<p class="MsoNormal" style="text-align: justify">Unaware of existence of such JavaScript loggers a developer gets a natural inclination for writing one of their own, but why to reinvent the wheel? There are logging libraries such as log4javascript and log4js which have nearly all the above stated techniques incorporated and depending upon his needs a developer can use one or many of them. I will try to cover one such library in my next blog and will show how beneficial in can be. Till then Happy Logging!</p>

<p><span style="font-size: 12pt" /></p>