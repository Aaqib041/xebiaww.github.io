---
layout: post
header-img: img/default-blog-pic.jpg
author: Khyati
description: 
post_id: 18432
created: 2014/06/28 22:42:11
created_gmt: 2014/06/28 17:42:11
comment_status: open
---

# Fabricating Web Automation with Helium

There are many tools in the market widespread which tells how to automate web, web pages, web sites and all web activities which a user can do and an automation engineer wishes to automate.

**What automation testers desires ?**

An Automation tester wishes to make type of framework which mimics all the events which he/she wants to test. So the leading tool for this is Selenium, it is widely adopted by testers for functional GUI testing.

Another thing could be if he can make framework with less coding and he can do the same work in equivalent or even less time, as the other testers are taking.

**Why Helium?**

Helium is a tool which has the similar functioning as Selenium but in a more advanced and easier way. What Helium does is, it uses the API which Selenium offers in such a way that it lessen the code complexities to less than half.

  * It offers commands which are as good as we are writing simple English.
  * It is lightweight and is very simple to use.
  * It supports Java and Python, so you can apply OOPS concepts in your framework.
  * It supports tools like Eclipse for making projects so that you can manage your test scripts in IDE.
  * And last but not the least, it gives full support to Sauce Labs and Selenium, hence it is worth-working-with.

**How Helium does the magic?**

Basic set-up:

For setting up Helium, all you need to do is to download Helium jar from[ http://heliumhq.com/download][1]. Then unzip the zip file and extract it to a folder. And that's it, you are ready to make your test scripts in testing framework.

This is the folder structure which you will get when you extract the zipped file :

  1. **Examples** folder has a sample example of Google and you can run it with the **‘Demo -run me**’ batch file.
  2. **heliumlib** folder has all the libraries you need to make your own project via Helium.
  3. **runtime** will have all the logs, you will get after each execution.
  4. **webdrivers** has chrome and IE driver you require to run your scripts on these browsers.

![Helium.PNG][2]

**Setting up eclipse for the project creation**

For making your own project you need to make a ‘**_new project_**’ in eclipse. Then you have to add all the libraries placed in **heliumlib** folder to the **CLASSPATH** of project.

![ClassPAth.PNG][3]

And start making your own test scripts :)

**Helium API**

1\.  For every test case class you need to import helium API.

**_import static com.heliumhq.API.*;_**

2\. Start driver

To start an instance of a browser, you need to write start along with the name of browser you want to run your test scripts on.

![Start.png][4]

And then write the test steps you want to follow. For example, I have automated Xebia India first with Helium, here is the code for the same:

``` 
 package com.xebia.web.pages; import static com.heliumhq.API.*;

public class XebiaIndia { public static void main(String args[]) { startChrome("http://xebia.in/"); click("About Us"); click("Services"); click("Products"); click("Trainings"); click("Knowledge"); click("Careers"); click("Events"); killBrowser(); } }
 ``` 

**Why Helium over Selenium?**

  1. Get rid of finding and locating HTML elements.
  2. No need for Xpath and other locators.
  3. Reduces the complexity of code by 50%.
  4. All driver instantiation, WebDriver, WebElement references can be done by very less keywords.

   [1]: http://heliumhq.com/download ( http://heliumhq.com/download)
   [2]: https://lh3.googleusercontent.com/zCYEOXVPSxs2pj-Kjecudi22ONd5qZselTF0QrxmNM042GSLqz2YwnUtmdHbtArfnZr5OynxmK3i9Q93A_gkSWZFE30qy2Ba1DCXb8Oq93cJWjX9hqEAoxFGj3cMdv1ccA
   [3]: https://lh4.googleusercontent.com/M8BXoiEIbTxMbDmA71bC-vun4mTtnRoqGalKjpe24GbGWXJnqxw7fJdV3pf6tRnUjGv-KUe4n8FjVlYH0NExTpsO52jbXm_OcFvQX49nVF0T4cuHJnniUa6PLWuBYs7Rtg
   [4]: https://lh5.googleusercontent.com/75M9rRp1uPg1shp22ldE_TIabr92iLXCJlobOj6nuYqNLLz9D4-HhD9U071XiCod-v6fIaHrtTY5IeM7od5N8RdIZmN2ToVOpFKltBnrxLsuaB5nXuinPKOgAqQvVcHs5Q

## Comments

**[Jaya](#9496 "2014-07-01 14:51:08"):** Nice Blog. Suppose we have two links with the same name on a webpage. Then how can we tell Helium to click a specific link?

**[Khyati Sehgal](#9497 "2014-07-01 14:58:33"):** Hi Jaya, Helium has the capability to search for the most-nearest-elements . Let say, you are in a flow of doing some activities ,so Helium will pick the most nearest element from the flow you will be working on. Hence it will captures the locator which is near to the links,buttons,frames,etc you will be working on. Hope this answers your question.

