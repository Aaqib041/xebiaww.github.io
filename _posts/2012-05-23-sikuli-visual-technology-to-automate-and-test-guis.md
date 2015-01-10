---
layout: post
header-img: img/default-blog-pic.jpg
author: jgupta
description: 
post_id: 14019
created: 2012/05/23 16:32:04
created_gmt: 2012/05/23 11:32:04
comment_status: open
---

# SIKULI - Visual Technology to automate and test GUIs

<p>This blog is about an interesting technology named Sikuli for GUI test automation, where it can be used, how to develop test automation scripts using Sikuli, and some of current limitations with this tool.</p>
<p><span style="font-size: small"><b>INTRODUCTION</b></span></p>
<p>Sikuli is an open source tool to automate and test graphical user interfaces (GUI) using image recognition technology. It identifies existence of particular areas of user interface like a button based on the fact that it looks like a certain image and then performs the user defined set of actions like clicking on it. It not only allows user to control actions which take place in a browser like Selenium/Webdriver, but can also be used to control other desktop applications - or even an iPhone or Android application running in a simulator.
<!--more--></p>
<p>Sikuli Script is a visual scripting API using Jython used to create automation scripts. Sikuli includes an integrated development environment (IDE) for writing visual Sikuli scripts with screen-shots.</p>
<p>Why would you need another automation tool when you have so many open source and commercial tools available in the market. Let’s ponder over the following scenarios:
<ul>
    <li>A popular test automation tool, Selenium works great for all normal stuff like HTML, AJAX but its no go when we try to test third party ActiveX, Java Applets and Flash Components. Sikuli comes to the rescue!! It can be integrated with other automation tools and can be used to overcome the limitations in other tools.</li>
    <li>You want to specifically test the application for UI stuff and it should stay reasonably similar in appearance. Normal automation testing doesn't care what the button looks like, and won't catch if the image used got exchanged or deleted with an old button image.</li>
    <li>Suppose if we have almost accurate screen shots of how the application would look like in the requirement specs, then UI tests could be created before the application is actually developed. This could be a great way to ensure specs.</li>
</ul>
There are various other scenarios where this powerful tool can be used.</p>
<p><span style="font-size: small"><b>POWERFUL FEATURES AND ADVANTAGES</b></span></p>
<p>Sikuli in itself makes the development of user interface automation quite simple. The entry bar to learn and create working scripts is quite low. The scripts also offer reliability across various platforms. (Windows, Mac OS X and Linux). Below you can see a simple Sikuli script that performs Yahoo search:</p>
<p><a href="http://xebee.xebia.in/2012/05/23/sikuli-%e2%80%93-visual-technology-to-automate-and-test-guis/sikuli/" rel="attachment wp-att-14021"><img class="size-full wp-image-14021" alt="Sikuli script that performs Yahoo Search" src="http://xebee.xebia.in/wp-content/uploads/2012/05/Sikuli.png" width="664" height="634" /></a></p>
<p>Another thing that makes Sikuli powerful is that it provides an API for writing the automation in Java. So the above 'Yahoo Search' code can be written and executed through Java as follows:</p>
<p>[code language="java"]
//Sikuli code to perform Yahoo Search written and executed in Java.</p>
<p>public class TestSikuli {</p>
<p>public static void main(String[] args) {</p>
<pre><code>//Declare a new screen object - Obtain the desktop screen.
Screen s = new Screen();

//Perform Search on yahoo.com for keyword &amp;amp;quot;Xebia India&amp;amp;quot;
try{
    //Launch Browser and wait for address bar.
    s.click(&amp;amp;quot;imgs/firefox-icon.png&amp;amp;quot;, 0);
    s.wait(&amp;amp;quot;imgs/address-bar.png&amp;amp;quot;);

    //Navigate to the website.
    s.click(&amp;amp;quot;imgs/address-bar.png&amp;amp;quot;, 0);
    s.type(null, &amp;amp;quot;www.yahoo.com&amp;amp;quot;, 0);
    s.click(&amp;amp;quot;imgs/go.png&amp;amp;quot;, 0);

    //Wait for search page to load, Type the search keyword and search.
    s.wait(&amp;amp;quot;imgs/search-button.png&amp;amp;quot;);
    s.type(null, &amp;amp;quot;Xebia India&amp;amp;quot;, 0);
    s.click(&amp;amp;quot;imgs/search-button.png&amp;amp;quot;, 0);
}
catch(FindFailed e){
    e.printStackTrace();
}
</code></pre>
<p>}
}
[/code]</p>
<p><span style="font-size: small"><b>INTEGRATION WITH OTHER AUTOMATION TOOLS
</b></span></p>
<p>Sikuli can seamlessly integrate with our existing automation framework as we can include sikuli jar in our framework which is same as including any other library. For e.g. Sikuli can be called from within the Selenium/Webdriver script, have it get through the part where we are stuck (by scripting out that part in Sikuli), and then continue scripting in Selenium.</p>
<p>As another example, we know that presence of Adobe Flash content would make some scenarios (use cases) impossible to automate with Selenium/Webdriver. A simple way to overcome these difficulties could be the use of Sikuli.</p>
<p>Let’s see an example of how Selenium/Webdriver can be seamlessly integrated with Sikuli code in java:</p>
<p>[code language="java"]
// Webdriver code to navigate to a video on youtube and perform Pause and Play actions</p>
<p>//Launch Firefox and navigate to the desired URL on YouTube using Webdriver.
driver = new FirefoxDriver();
driver.get(&amp;quot;http://www.youtube.com/watch?v=LuuUyblJsAo&amp;quot;);</p>
<p>//Verify the title of the video.
assertEquals(driver.findElement(By.id(&amp;quot;eow-title&amp;quot;)).getText(),&amp;quot;Life at Xebia India&amp;quot;);</p>
<p>//Declare a new screen object – Obtain the desktop screen.
Screen s = new Screen();</p>
<p>//Perform operations on Flash Components
   try{
       //Pause you tube video, switch to full screen mode and Play again.
       s.click(&amp;quot;imgs/Pause.png&amp;quot;, 0);
       s.click(&amp;quot;imgs/Full-Screen.png&amp;quot;, 0);
       s.click(&amp;quot;imgs/Play.png&amp;quot;, 0);
   }
   catch(FindFailed e){
       e.printStackTrace();
   }
}</p>
<p>//Back to Webdriver code
driver.quit();
[/code]</p>
<p>In the above example, we see that there are some elements on web pages which are not identified by Webdriver. E.g. Adobe Flash content. These are now easily handled by Sikuli.</p>
<p><strong>Example 2:</strong> Selenium does NOT support JavaScript confirmations that are generated in a page's onload() event handler. In this case a visible dialog WILL be generated and Selenium will hang until you manually click OK or CANCEL. Here, Sikuli could be used to click on Ok or Cancel and you can proceed with your test.</p>
<p><strong>Example 3:</strong> Sikuli can be used to deal with File open dialogues which have always been a real pain to automate using Webdriver. Let us see an example of “File download” from a website:</p>
<p>[code language="java"]
//Code to deal with file download using Sikuli.</p>
<p>//Launch Firefox and navigate to the desired URL on Teambeachbody.com using Webdriver.
driver = new FirefoxDriver();
driver.get(&amp;quot;http://www.teambeachbody.com/get-fit/fitness-tools/workout-sheets&amp;quot;);</p>
<p>//Declare a new screen object - Obtain the desktop screen.
Screen s = new Screen();</p>
<p>// Perform operations on Adobe PDF and File Open Dialog
  try{
      //Click on the image to download PDF
      s.click(&amp;quot;imgs/LESMILLSPUMP.png&amp;quot;, 0);</p>
<pre><code>  //Click on Save icon for the Adobe PDF
  s.wait(&amp;amp;quot;imgs/Save_PDF_Icon.png&amp;amp;quot;,10);

  //Perform operations on File Open Dialog
  s.click(&amp;amp;quot;imgs/File_Open_Dialog_Save_Button.png&amp;amp;quot;, 0);
</code></pre>
<p>}
   catch(FindFailed e){
      e.printStackTrace();
   }</p>
<p>//Back to Webdriver code
driver.quit();
[/code]</p>
<p><strong>Example 4:</strong> Sikuli can also be used to handle HTTPS authentication and self-signed certificate exceptions, thus eliminating the need for using specific browser profile each time the browser is started in test automation execution.</p>
<p><strong>Example 5: </strong>Sikuli manages to do Drag &amp; Drop properly. Another solution to Selenium limitations:</p>
<p>[code language="java"]
//Code to deal with Drag and drop using Sikuli</p>
<p>//Navigate to the desired url on Myntra.com using Selenim.
selenium.open(&amp;quot;http://www.myntra.com/sales&amp;quot;);</p>
<p>//Declare a new screen object - Obtain the desktop screen.
Screen s = new Screen();</p>
<p>//Perform Drag and drop operation on price slider
  try{</p>

## Comments

**[Aman Singhal](#8864 "2012-05-25 13:33:39"):** Hi jaya, I had to automate the installer part of my project and I needed an automation tool for the same. Now, I have an idea how to implement those functionalities using same tool. Thanks a lot for providing such a nice explanation and hope to see some more blogs on different technologies..

**[Gaurav Bansal](#8856 "2012-05-24 21:28:07"):** Nice write up. I really liked its integration with Java.

**[Rahul Madan](#8861 "2012-05-25 11:49:24"):** was looking for something like this....thanks to the author.... my AIR application worked just fine..exactly what i wanted..

**[Suman](#8895 "2012-05-29 19:59:59"):** This blog was very useful for me specially the integration with other tool part. Thanks Jaya

**[Shanaka](#9105 "2012-06-28 10:16:56"):** Nicely written and thanks a lot for the code examples.

**[Priyanka](#8858 "2012-05-25 10:17:03"):** Hi Jaya, Really nice write up. Your blog solved my problem of downloading file issue.

**[Nitesh](#8865 "2012-05-25 13:42:49"):** Good one. Very well written.

