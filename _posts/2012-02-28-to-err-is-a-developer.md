---
layout: post
header-img: img/default-blog-pic.jpg
author: Shruti Khattar
description: 
post_id: 12140
created: 2012/02/28 13:05:13
created_gmt: 2012/02/28 08:05:13
comment_status: open
---

# To Err is a Developer

<p style="text-align: left;" align="CENTER"><span style="color: #000000; font-family: arial, helvetica, sans-serif; font-size: small;" color="#000000"><span face="Arial, sans-serif"><span size="2">There is nothing like “flawless” in this world. Even the renowned or self proclaimed “Perfectionists” are not immaculate. Errors, mistakes, bugs are part of life of a developer. They can be equated to the quadrants of the 'Johari Window'.  In terms of design, code, testing, security measures, providing access and information, there can be both technical as well as non technical mistakes. I would like to discuss some of the common mistakes and loopholes that the developers are prone to commit even if they score very high on the proficiency level.</span></span></span></p>

<p align="CENTER"><span style="color: #000000; font-family: arial, helvetica, sans-serif; font-size: small;" color="#000000"><span face="Arial, sans-serif"><span size="2"><!--more--></span></span></span></p>

<ul>
    <li><strong><span style="font-family: arial, helvetica, sans-serif; font-size: small;">In a design, individual elements do not matter, big picture does: </span></strong><span style="font-family: arial, helvetica, sans-serif; font-size: small;">It can be difficult to separate an eye for design with the ability to design. A captivating design is simple and attractive and not. Because of prejudice and bias, designers are not able to distinguish what should be included in the existing design and what should not be. They should not be tempted to use anything that does not integrate well with the existing design. It is absolutely not worth over committing the details and wasting time on it</span></li>
</ul>

<ul>
    <li><strong><span style="color: #000000; font-family: arial, helvetica, sans-serif; font-size: small;" color="#000000"><span face="Arial, sans-serif"><span size="2">Demanding Perfection in first attempt: </span></span></span></strong><span style="font-family: arial, helvetica, sans-serif; font-size: small;">Too many people want everything perfect before they go live. While the coder makes all the efforts to get everything perfect, the Internet changes, someone presents  the same idea. “Getting there first” is often more important than “Getting there perfect” Apply the 80-20 rule to projects instead. It is better to wait to spend 80% of your resources polishing that last 20% once you've proven the stability of your site.</span></li>
</ul>

<ul>
    <li><strong><span style="color: #000000; font-family: arial, helvetica, sans-serif; font-size: small;" color="#000000"><span face="Arial, sans-serif"><span size="2">Not enough Unit Testing : </span></span></span></strong><span style="font-family: arial, helvetica, sans-serif; font-size: small;">It is the confidence biased towards the self written code that refrains the developer. It becomes hard to think of the scenarios and inputs that can break their code. Thus, the developer is not able to enhance his code to make it more rich and robust. Also, they get complacent when their code has to be reviewed by someone else and then is functionally checked by the QA.  A developer has to get into a different mindset keeping in consideration that all checks cannot be and ideally should not be incorporated into the code.  The method(s) has to be as generic as possible to allow extensibility  and it should be tested with respect to all the specifications. Ideally, even for a minor bug fix, a unit test case should be written.</span></li>
</ul>

<ul>
    <li>
<p align="LEFT"><span style="color: #000000; font-family: arial, helvetica, sans-serif; font-size: small;" color="#000000"><span face="Arial, sans-serif"><span size="2"><b>Exceptions</b></span></span></span></p>
<p align="LEFT"><em style="font-family: arial, helvetica, sans-serif; font-size: small;"><span color="#000000"><span face="Arial, sans-serif"><span size="2">Surpassing     and suppressing exceptional conditions</span></span></span></em></p>
<p align="LEFT"><span style="font-family: arial, helvetica, sans-serif; font-size: small;">Sometimes, the programmer is not specific enough in the type of exception they catch. Catching too general an exception type means that they may be inadvertently dealing with particular exceptions that would be best left to other code, higher up the call chain. Tune your exception catch blocks to be as specific as possible. The second mistake is more detrimental-the programmer doesn't want any exceptions leaving their code and so catches them all and ignores them. This is known as the empty catch block. Also,don't absorb exceptions with no logging and operation. Ignoring exceptions will save that moment but will create a chaos for maintainability later</span></p>
<p align="LEFT"><em style="font-family: arial, helvetica, sans-serif; font-size: small;"><span color="#000000"><span face="Arial, sans-serif"><span size="2"><span face="'times new roman', times" size="3">Inter wining Exception Handling and Business Logic</span></span></span></span></em></p>
<p align="LEFT"><span style="font-family: arial, helvetica, sans-serif; font-size: small;">Don't manage business logic with exceptions. Use conditional statements instead. If a control can be done with if-else statement clearly, don't use exceptions because it reduces readability and performance  (e.g. null control, divide by zero control).eg. Null control with conditionals is not an alternative to catching NullPointerException. If a method may return null, control it with if-else statement. If a return may throw NullPointerException, it should be caught.</span></p>
</li>
</ul>

<ul>
    <li><span style="font-family: arial, helvetica, sans-serif; font-size: small;"><strong><span style="color: #000000;" color="#000000"><span face="Arial, sans-serif"><span size="2"><b>Closing   the source: </b></span></span></span></strong></span><span style="font-family: arial, helvetica, sans-serif; font-size: small;">One of the trickiest challenges for any company is determining how much to share with the people who use the software. Often the code itself grows more modular and better structured as others recompile the program and move it to other platforms. Just opening up the code forces you to make the info more accessible, understandable, and thus better. As they make the small tweaks to share the code, they feed the results back into the code base. Due to the trade off, developers are mostly indecisive of it.</span></li>
</ul>

<ul>
    <li><span style="color: #000000; font-family: arial, helvetica, sans-serif; font-size: small;" color="#000000"><span face="Arial, sans-serif"><span size="2"><b>Playing     it fast and loose: </b></span></span></span><span style="font-family: arial, helvetica, sans-serif; font-size: small;">Even though the developers are well aware of the possible security breaches, still not much is done to prevent them. Some of them are:</span></li>
</ul>

<p><span color="#000000" style="font-family: arial, helvetica, sans-serif; font-size: small;"><span face="Arial, sans-serif"><span size="2">1. <i>SQL  injection or OS injection</i></span></span></span></p>
<p><span style="font-family: arial, helvetica, sans-serif; font-size: small;"><span color="#000000"><span face="Arial, sans-serif"><span size="2">2. <i style="font-family: 'times new roman', times; font-size: medium;">Cross-site   scripting</i></span></span></span><span color="#000000" style="color: #000000;"><span face="Arial, sans-serif"><span size="2"> : By not validating the input that is given dynamically, the     developer might invite these problems like compromising on data     integrity, cookies can be set and read, user input can be   intercepted. Malicious scripts can be executed by the client in     the context of the trusted source.</span></span></span></span></p>

## Comments

**[Pooja](#7808 "2012-02-28 14:43:42"):** Nicely Written Shruti!! :)

**[Rahul Vidhani](#7809 "2012-02-28 15:52:10"):** Good work shruti.

**[Nitin](#7812 "2012-02-29 05:49:45"):** Good Job!!!

