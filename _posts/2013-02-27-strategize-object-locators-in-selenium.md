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

<p>Often, Recording and playing back a recorded script using Selenium IDE and Webdriver is just a stepping stone to write a more meaningful test script(see my last blog). Almost, in all the cases, it serves as a template to add more code and to do more.</p>
<p>At times there are many facts which we used to overlook which seems very minute but turns to be very weird when comes to actual writing. While it may be a straight thing to use a locator strategy suggested by the recorded test, this is seldom useful while dealing with dynamic resources. One of them is finding locators.
<p style="text-align: left;">There are many ways to detect locators in selenium. One way is via <a title="Selenium IDE" href="http://docs.seleniumhq.org/docs/02_selenium_ide.jsp">Selenium IDE</a>,which records the user action and in return gives the locator. The other way is to find the locator via HTML from web page.<!--more--></p>
<p style="text-align: left;">Locators can be written in several ways via xpath ,css, dom,etc.Most of the web sites have dynamic id’s ,classes, etc so which makes difficult to locate element on web page.For any element to locate in xpath you can use various methods like contains, siblings etc</p>
<p style="text-align: left;">Lets take an example of<a title=" http://xebee.xebia.in/" href="http://xebee.xebia.in/"> http://xebee.xebia.in/</a> website.</p>
<p style="text-align: left;">I opened <a title="http://xebee.xebia.in/" href="http://xebee.xebia.in/">http://xebee.xebia.in/</a> and try locating link=Design Thinking and Software Development there could be multiple ways to locate it. Lets try all one by one:</p>
<p style="text-align: left;"><em>1.By clicking on link via name of the link</em></p>
<p style="text-align: left;"><strong>link=Design Thinking and Software Development</strong></p></p>
<p>[code]selenium.click(&quot;link=Design Thinking and Software Development&quot;);[/code]
<p style="text-align: left;"><em>2.By css ,to use this type you need to write css= in front of the locator</em></p>
<p style="text-align: left;"><strong>css=a..firepath-matching-node</strong></p></p>
<p>[code]selenium.click(&quot;css=a..firepath-matching-node&quot;)[/code]
<p style="text-align: left;"><em>3.By using xpath,in this particular case I have taken the link directly by using contains</em></p>
<p style="text-align: left;"><strong>//a[contains(text(),'Design Thinking and Software Development')]</strong></p></p>
<p>[code]selenium.click(&quot;//a[contains(text(),'Design Thinking and Software Development')]&quot;);[/code]
<p style="text-align: left;"><em>4.By using xpath,this is called Xpath:relative i.e by giving the relative path of the HTML page.</em></p>
<p style="text-align: left;"><strong>//div[@id='post-15910']/h2/a</strong></p></p>
<p>[code]selenium.click(&quot;//div[@id='post-15910']/h2/a&quot;);[/code]
<p style="text-align: left;"><em>5.By using xpath and giving the link(href) in reference.</em></p>
<p style="text-align: left;"><strong>//a[@href='http://xebee.xebia.in/2013/03/01/design-thinking-and-software-development/']</strong></p></p>
<p>[code]selenium.click(&quot;//a[@href='http://xebee.xebia.in/2013/03/01/design-thinking-and-software-development/']&quot;);[/code]
<p style="text-align: left;"><em>6.By giving xpath:postion</em></p>
<p style="text-align: left;"><strong>//h2/a</strong></p></p>
<p>[code]selenium.click(&quot;//h2/a&quot;);[/code]
<p style="text-align: left;"><strong>How to handle dynamic classes?</strong>
You can try with the contains() function.For example:If you have:</p>
<p style="text-align: left;">(Where "table" is static in all the dynamics ids)</p>
<p style="text-align: left;">You can try with:
//table[contains(@id,"table")]</p>
<p style="text-align: left;">If you have the same static text in the ID for different elements, you can add other attribute to the xpath.
Basic principal with dynamic IDs is to find the pattern if any in the HTML.For example if your application has a table with dynamically generated rows, columns, classes and ids are generated based on some content or sequence, You can easily program-me that through test code.
If you want to click on a link which contains xebia,then one ways it to use contains.</p>
<p style="text-align: left;">with css:</p></p>
<p>[code]css=li[id*=&quot;text&quot;]:contains(&quot;xebia&quot;)[/code]
<p style="text-align: left;">With XPath:</p></p>
<p>[code]//li[contains(@id,&quot;text&quot;) and contains (text(),&quot;xebia&quot;)][/code]
<p style="text-align: left;"><strong>What kind of browser and tools one can use in your strategy for different kinds of locators?</strong></p>
<p style="text-align: left;">You can always use Firefox to locate any element on web.But chrome can also be use to locate elements via HTML code and tree structuring.
Tool which can help you in locating is :<a title="Firebug" href="http://getfirebug.com/whatisfirebug">Firebug </a> and Selenium IDE
<strong>Fireubg</strong>: Firebug integrates with Firefox to put a wealth of web development tools at your fingertips while you browse. You can edit, debug, and monitor CSS, HTML, and JavaScript live in any web page.You can download firebug according to the firefox version you have in your machine.</p>
<p style="text-align: center;"></p>
For more info ,please visit : <a title="Firebug" href="http://getfirebug.com/whatisfirebug">http://getfirebug.com/whatisfirebug</a></p>
<p><strong>Selenium IDE</strong>: It is also an add-on to firefox .It record/playback tool for authoring tests without learning a test scripting language.
<p style="text-align: left;"><strong>Which locator to use ,css or xpath?</strong></p>
<p style="text-align: left;">I tested on multi browser environment in which I used: IE, FIREFOX, CHROME.</p>
<p style="text-align: left;">On IE xpath works very slow that it can't be managed. So I used CSS where ever we can.
1. Css are faster
2. They’re more readable
3. CSS is jQuery’s locating strategy
4. No one else uses XPATH anyways!</p>
<p style="text-align: left;">Refernce:<a title="http://sauceio.com/index.php/2011/05/why-css-locators-are-the-way-to-go-vs-xpath/" href="//sauceio.com/index.php/2011/05/why-css-locators-are-the-way-to-go-vs-xpath/">http://sauceio.com/index.php/2011/05/why-css-locators-are-the-way-to-go-vs-xpath/</a></p>
<p style="text-align: left;">Happy coding!</p></p>
<table class="alignleft" id="table_[a0df2984-dedf-4d3e-a479-c70cbce5ae19]"></table>