---
layout: post
header-img: img/default-blog-pic.jpg
author: vashishtha
description: 
post_id: 899
created: 2009/03/02 19:26:00
created_gmt: 2009/03/02 18:26:00
comment_status: open
---

# Refactoring Flex application from existing Adobe AIR codebase

<p>Adobe AIR is a great technology to provide platform-independent desktop RIA applications. Gone are the days when Windows operating system used to be ubiquitous in desktop market. That's the reason why Adobe AIR is considered as the future of desktop applications. Desktop applications are here to stay as they come with the power of providing rich features in efficient/optimal way compared to browser mode.</p>
<!--more-->

<p>Another side of the coin is - in the competitive age of software products with two many choices and with a lot of time-crunch, end-user might be a bit lazy in downloading and installing just to be able to look at the features of the application. On the other hand browser based applications are quick to experiment with. In terms of features and power, browser based application may be a subset of desktop application but still provides majority of features for most of the quick users.</p>
<p>We had a similar situation in our project where we had to refactor an existing AIR application to provide support for both browser and AIR versions. To accomplish this task, we found <a href="http://www.adobe.com/devnet/air/flex/articles/flex_air_codebase.html">a good article</a> which provided us a basic architectural approach to do it.</p>
<p>We started with refactoring existing AIR project (ProjectAIR) into two more projects, SharedProject and ProjectWeb (web project). SharedProject contained the common shared code between ProjectAIR and ProjectWeb.</p>
<p>Adobe AIR based applications use windows, file-system, SQL operations etc which needed to be replaced with their counterparts in web version. We abstracted out these functionalities using ActionScript (AS3) interfaces. In other words wherever AS3 code had to be implemented in different ways for AIR and Web version, we identified common abstraction in form of AS3 interfaces and  provided AIR (in ProjectAIR) and Web (in ProjectWeb) based implementations.</p>
<p>We used <a href="http://www.pranaframework.org/">Spring ActionScript</a>, an ActionScript dependency injection framework, to inject appropriate AS3 interface implementations based on applications we are using (different applicationContext.xml for ProjectAIR and ProjectWeb).</p>
<p>As AIR version is able to work on files (say images), we had to provide a similar functionality in web version also. It was just not possible in Flash 9 as it is a violation of sandbox security. Recently Flash 10 introduced a new feature of read/write files in flash player without any need to send anything back to the server. We used recently introduced Flex 3.2 SDK to access these features. Instead of storing data in file-system (possible in AIR), we used SharedObject (Flex cookie) to store the user-information on desktop.</p>
<p>One of the major issue comes when you have to reuse View (MXML). For some views where it was just not possible, we created different views (one each for web and AIR version). However for majority we could refactor and reuse them. We refactored the Views and taken out any AIR specific functionality. They became base version of view which could either be directly used in web version or needed to be extended for both web and AIR versions. We created a nomenclature something like <viewA>AIR and <viewA>Web (e.g. InvoiceViewAIR.mxml and InvoiceViewWeb.mxml which extend from InvoiceView.mxml). For instance InvoiceViewAIR.mxml (extends from InvoiceView.mxml) could be something like this.</p>
<pre lang="xml">
<?xml version="1.0" encoding="utf-8"?>
<invoiceView
    nativeDragEnter="onNativeDragEnter(event)"
    nativeDragDrop="onNativeDragDrop(event)"
    xmlns="com.xxx.product.*"
    xmlns:mx="http://www.adobe.com/2006/mxml">

    <mx:Script>
        <![CDATA[
            private function onNativeDragEnter(nativeDragEvent:NativeDragEvent):void
            {
                           ...
            }

            private function onNativeDragDrop(nativeDragEvent:NativeDragEvent):void
            {
                           ...
            }
           ]]