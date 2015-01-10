---
layout: post
header-img: img/default-blog-pic.jpg
author: Khyati
description: 
post_id: 15918
created: 2013/02/27 16:58:53
created_gmt: 2013/02/27 11:58:53
comment_status: open
---

# Strategize object locators in selenium

Often, Recording and playing back a recorded script using Selenium IDE and Webdriver is just a stepping stone to write a more meaningful test script(see my last blog). Almost, in all the cases, it serves as a template to add more code and to do more.

At times there are many facts which we used to overlook which seems very minute but turns to be very weird when comes to actual writing. While it may be a straight thing to use a locator strategy suggested by the recorded test, this is seldom useful while dealing with dynamic resources. One of them is finding locators. 

There are many ways to detect locators in selenium. One way is via [Selenium IDE][1],which records the user action and in return gives the locator. The other way is to find the locator via HTML from web page.

Locators can be written in several ways via xpath ,css, dom,etc.Most of the web sites have dynamic id’s ,classes, etc so which makes difficult to locate element on web page.For any element to locate in xpath you can use various methods like contains, siblings etc

Lets take an example of[ http://xebee.xebia.in/][2] website.

I opened <http://xebee.xebia.in/> and try locating link=Design Thinking and Software Development there could be multiple ways to locate it. Lets try all one by one:

_1.By clicking on link via name of the link_

**link=Design Thinking and Software Development**

[code]selenium.click("link=Design Thinking and Software Development");[/code] 

_2.By css ,to use this type you need to write css= in front of the locator_

**css=a..firepath-matching-node**

[code]selenium.click("css=a..firepath-matching-node")[/code] 

_3.By using xpath,in this particular case I have taken the link directly by using contains_

**//a[contains(text(),'Design Thinking and Software Development')]**

[code]selenium.click("//a[contains(text(),'Design Thinking and Software Development')]");[/code] 

_4.By using xpath,this is called Xpath:relative i.e by giving the relative path of the HTML page._

**//div[@id='post-15910']/h2/a**

[code]selenium.click("//div[@id='post-15910']/h2/a");[/code] 

_5.By using xpath and giving the link(href) in reference._

**//a[@href='http://xebee.xebia.in/2013/03/01/design-thinking-and-software-development/']**

[code]selenium.click("//a[@href='http://xebee.xebia.in/2013/03/01/design-thinking-and-software-development/']");[/code] 

_6.By giving xpath:postion_

**//h2/a**

[code]selenium.click("//h2/a");[/code] 

**How to handle dynamic classes?** You can try with the contains() function.For example:If you have:

(Where "table" is static in all the dynamics ids)

You can try with: //table[contains(@id,"table")]

If you have the same static text in the ID for different elements, you can add other attribute to the xpath. Basic principal with dynamic IDs is to find the pattern if any in the HTML.For example if your application has a table with dynamically generated rows, columns, classes and ids are generated based on some content or sequence, You can easily program-me that through test code. If you want to click on a link which contains xebia,then one ways it to use contains.

with css:

[code]css=li[id*="text"]:contains("xebia")[/code] 

With XPath:

[code]//li[contains(@id,"text") and contains (text(),"xebia")][/code] 

**What kind of browser and tools one can use in your strategy for different kinds of locators?**

You can always use Firefox to locate any element on web.But chrome can also be use to locate elements via HTML code and tree structuring. Tool which can help you in locating is :[Firebug ][3] and Selenium IDE **Fireubg**: Firebug integrates with Firefox to put a wealth of web development tools at your fingertips while you browse. You can edit, debug, and monitor CSS, HTML, and JavaScript live in any web page.You can download firebug according to the firefox version you have in your machine.

For more info ,please visit : <http://getfirebug.com/whatisfirebug>

**Selenium IDE**: It is also an add-on to firefox .It record/playback tool for authoring tests without learning a test scripting language. 

**Which locator to use ,css or xpath?**

I tested on multi browser environment in which I used: IE, FIREFOX, CHROME.

On IE xpath works very slow that it can't be managed. So I used CSS where ever we can. 1\. Css are faster 2\. They’re more readable 3\. CSS is jQuery’s locating strategy 4\. No one else uses XPATH anyways!

Refernce:[http://sauceio.com/index.php/2011/05/why-css-locators-are-the-way-to-go-vs-xpath/][4]

Happy coding!

   [1]: http://docs.seleniumhq.org/docs/02_selenium_ide.jsp (Selenium IDE)
   [2]: http://xebee.xebia.in/ ( http://xebee.xebia.in/)
   [3]: http://getfirebug.com/whatisfirebug (Firebug)
   [4]: //sauceio.com/index.php/2011/05/why-css-locators-are-the-way-to-go-vs-xpath/ (http://sauceio.com/index.php/2011/05/why-css-locators-are-the-way-to-go-vs-xpath/)