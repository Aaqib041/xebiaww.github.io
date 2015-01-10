---
layout: post
header-img: img/default-blog-pic.jpg
author: vashishtha
description: 
post_id: 800
created: 2008/11/16 20:48:38
created_gmt: 2008/11/16 18:48:38
comment_status: open
---

# Productive Tools on top of Flex Builder 3

<p>As I got introduced to Flex world some times back, I started using Flex Builder as an IDE, a product from Adobe on top of Eclipse platform. I assumed that it'll provide all the basic features available in Eclipse for Java, but I was wrong. Flex Builder is in nascent phase from tooling point of view. Just to refresh your memories, Flex Builder lacks some of the following Eclipse features.</p>
<!--more-->

<ol>
    <li>Support for generating getters-setters automatically.</li>
    <li>Support of templates for creating constants.</li>
    <li>Organize imports. If some type has not been included already, organize-import should do that.</li>
    <li>Better refactoring support. Right now refactoring is just limited to renaming and you start missing real sense of refactoring in terms of creating new methods etc.</li>
    <li>Sometimes you may want to create your own templates for creating ASDoc instead of doing it from the scratch all the time</li>
    <li>Formatting ActionScript/Flex files</li>
</ol>

<p>I did some research on internet and found some options to resolve these problems.</p>
<h3>Eclipse Monkey</h3>

<p>Eclipse Monkey currently is a part of <a href="http://www.eclipse.org/dash/">Eclipse Dash</a> project. It is <a href="http://www.eclipse.org/proposals/eclipse-monkey/">proposed</a> to be split from Dash to make it full-fledged project in itself. It's a very powerful tool. Instead of extending Eclipse platform through creating plugins, you can extend Eclipse functionality through some scripting. Eclipse Monkey uses the Mozilla <a href="http://www.mozilla.org/rhino/">Rhino Javascript interpreter</a>  at its core. You guessed it right, scripting couldn't have been easier for Eclipse than the way it does with Monkey using Javascript.</p>
<p>Eclipse Monkey can be used to create getters and setters in ActionScript in automated fashion. You may want to take a look on a <a href="http://eokyere.blogspot.com/2007/09/productivity-with-dash-in-eclipse.html">~eokyere blog</a> to see how Monkey is used to create getters and setters. As I searched some more available Monkey scripting resources, I found more examples and <a href="http://rs145gc2.rapidshare.com/files/119902972/Actionscript_Code_Generate_Scripts_1.0_by_Panel.zip">one of them</a> satisfied my problem no 2 also.</p>
<p>If you have already downloaded and executed Monkey scripts on your Eclipse platform, you may have realized how powerful this Scripting tool is for Eclipse. You can create a lot of scripting based tools to enhance productivity without creating specialized plugin.
<h3>FDT ActionScript Editor</h3>
For rest of the problems mentioned above, I tried many options but somehow they didn't provide desired outcome. However one ActionScript based editor <a href="http://fdt.powerflasher.com/">FDT</a> comes very close. It supports most of features mentioned in the list and is certainly a better choice from ActionScript editor point of view. However it's not free and sometimes I wonder why somebody will invest yet another €599 to buy this tool if he already has Flex Builder. Flex Builder is a defacto tool for Flex based development whereas FDT mainly focuses on ActionScript based programming. FDT doesn't support MXML yet.</p>
<p>No wonder that we come to conclude that Flex Builder has a lot to catch up in terms of enhancing its tooling for Flex developers. However the tools I just mentioned come a bit close in providing the required support.</p>