---
layout: post
header-img: img/default-blog-pic.jpg
author: prachinagpal
description: 
post_id: 16818
created: 2013/06/21 14:32:43
created_gmt: 2013/06/21 09:32:43
comment_status: open
---

# Window Popups via Webdriver

<p>Recently I came across a situation where a browser window opens a new pop up window on clicking a button or a link on the page. So, this blog is regarding the handling of these scenario via webdriver. Usually, if we work on web application. It may have some browser popup windows on which we want to perform some action and come back to the main window using selenium webdriver.
The trick is to find the handle. It then uses the new window handle and returns the original window handle to the caller, so the caller knows which handle to switch back to.
There are many websites or html pages where we can find the popup windows but rather than searching for all those I created a <a href="http://www.w3.org/TR/REC-html40/">HTML</a> page.So, for testing the popup window I created a HTML page which has a button which opens a new pop up window when clicked. The html code for the same is as follows :-</p>
<!--more-->

<p>[sourcecode lang="text"]</p>
<p>&lt;input type=&quot;button&quot;
value=&quot;Open a Popup Window&quot;
onclick=&quot;window.open
('http://www.quackit.com/common/link_builder.cfm',
'popUpWindow','height=500,width=400,left=100,top=100,
resizable=yes,scrollbars=yes,toolbar=yes,menubar=no,
location=no,directories=no, status=yes');&quot;&gt;</p>
<p>[/sourcecode]</p>
<p>So, for the popup handling, lets take a scenario as follows :-
- Open the above html page.
- Click on the link "Open a Popup window".
- A new window appears.
- Perform some action on the new window.
- Move the control back to the main window.
There are many solutions for the above scenario available online. One of the solution that I thought for the above can be as follows :-</p>
<p><strong>Step 1 :-</strong></p>
<p>Create a class (lets say PopUpWindow). Initialize the driver as :-</p>
<p>[sourcecode lang="text"]</p>
<p>public class PopUpWindow {
public static void main(String[] args) {
WebDriver driver=new FirefoxDriver(); //Initialize the Driver
WebDriver popup=null;
[/sourcecode]</p>
<p><strong>Step 2 :-</strong>
Open the page by hitting the URL and click on the link which will open up the new window as follows :-</p>
<p>[sourcecode lang="text"]</p>
<p>driver.get(&quot;file:///C:/Users/Prachi/Desktop/PopUpWindow.html&quot;);
WebElement element= driver.findElement(By.xpath(&quot;html/body/input&quot;));
element.click();</p>
<p>[/sourcecode]</p>
<p><strong>Step 3 :-</strong>
Now, the task is to get the window handles and save the original window handle.Switch to the new popup window and get back to the original one. So , to do this we will use getWindowHandles , which is a webdriver method used to return a set of window handles which can be used to iterate over all open windows of this webdriver instance by passing them to #switchTo().window(String).
The above is done by the following :</p>
<p>[sourcecode lang="text"]
 //Getting the window handles.
Set&lt;String&gt; windowIterator= driver.getWindowHandles();
&lt;em id=&quot;__mceDel&quot;&gt;
[/sourcecode]</p>
<p><strong>Step 4 :-</strong></p>
<p>Once we get the windowhandles, we will now convert it into array by :-</p>
<p>[sourcecode lang="text"]
 //Converting it into Array
Object[] handles = windowIterator.toArray();</p>
<p>[/sourcecode]</p>
<p><strong>Step 5 :-</strong></p>
<p>Now switching it to the latest window by the following logic :-</p>
<p>[sourcecode lang="text"]
//Switch to latest window
driver.switchTo().window(handles[handles.length-1].toString());</p>
<p>[/sourcecode]</p>
<p><strong>Step 6 :-</strong></p>
<p>Also storing the current title of the window by using getCurrentUrl() which returns the URL of the page currently loaded in the browser. Lets print the URL of the current window so as to be sure on which window we are. It is done as follows :-</p>
<p>[sourcecode lang="text"]</p>
<p>String Title = driver.getCurrentUrl();
 //Print its url.
 System.out.println(&quot;Popup Title: &quot; + Title);
 // do some stuff
 driver.close();</p>
<p>[/sourcecode]</p>
<p><strong>Step 7 :-</strong></p>
<p>Do some stuff here in the current window that the user intends to do for example, asserting, verifying a text present etc.
After that we want to switch back to the parent window by following :-</p>
<p>[sourcecode lang="text"]
//Switch to main window.
driver.switchTo().window(handles[0].toString());</p>
<p>[/sourcecode]</p>
<p>Also, we can print its URL to make sure that we have switched back to the parent window as follows :-</p>
<p>[sourcecode lang="text"]</p>
<p>System.out.println(&quot;Main Title&quot; + driver.getCurrentUrl());
 driver.close();
[/sourcecode]</p>
<p>So, in this way pop up windows can be handled in webdriver. Hope the post is useful :)</p>
<p>Happy Testing!</p>