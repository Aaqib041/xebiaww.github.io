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

<p dir="ltr" style="text-align: left"><span style="font-family: verdana, geneva">There are many tools in the market widespread which tells how to automate web, web pages, web sites and all web activities which a user can do and an automation engineer wishes to automate.</span></p>

<p dir="ltr" style="text-align: left"><span style="text-decoration: underline;font-family: verdana, geneva"><strong>What automation testers desires ?</strong></span></p>

<p dir="ltr" style="text-align: left"><span style="font-family: verdana, geneva">An Automation tester wishes to make type of framework which mimics all the events which he/she wants to test. So the leading tool for this is Selenium, it is widely adopted by testers for functional GUI testing.</span></p>

<p dir="ltr" style="text-align: left"><span style="font-family: verdana, geneva">Another thing could be if he can make framework with less coding and he can do the same work in equivalent or even less time, as the other testers are taking.</span></p>

<p style="text-align: left"><span style="font-family: verdana, geneva"><!--more--></span></p>

<p dir="ltr" style="text-align: left"><span style="text-decoration: underline"><span style="font-family: verdana, geneva"><strong>Why Helium?</strong></span></span></p>

<p dir="ltr" style="text-align: left"><span style="font-family: verdana, geneva">Helium is a tool which has the similar functioning as Selenium but in a more advanced and easier way. What Helium does is, it uses the API which Selenium offers in such a way that it lessen the code complexities to less than half.</span></p>

<ul style="text-align: left">
    <li><span style="line-height: 1.5em;font-family: verdana, geneva">It offers commands which are as good as we are writing simple English.</span></li>
    <li><span style="line-height: 1.5em;font-family: verdana, geneva">It is lightweight and is very simple to use.</span></li>
    <li><span style="line-height: 1.5em;font-family: verdana, geneva">It supports Java and Python, so you can apply OOPS concepts in your framework.</span></li>
    <li><span style="line-height: 1.5em;font-family: verdana, geneva">It supports tools like Eclipse for making projects so that you can manage your test scripts in IDE.</span></li>
    <li><span style="line-height: 1.5em;font-family: verdana, geneva">And last but not the least, it gives full support to Sauce Labs and Selenium, hence it is worth-working-with.</span></li>
</ul>

<p dir="ltr" style="text-align: left"><span style="text-decoration: underline;font-family: verdana, geneva"><strong>How Helium does the magic?</strong></span></p>

<p dir="ltr" style="text-align: left"><span style="color: #000080;font-family: 'arial black', 'avant garde'">Basic set-up:</span></p>

<p dir="ltr" style="text-align: left"><span style="font-family: verdana, geneva">For setting up Helium, all you need to do is to download Helium jar from<a title=" http://heliumhq.com/download" href="http://heliumhq.com/download"> http://heliumhq.com/download</a>. Then unzip the zip file and extract it to a folder. And that's it, you are ready to make your test scripts in testing framework.</span></p>

<p dir="ltr" style="text-align: left"><span style="font-family: verdana, geneva">This is the folder structure which you will get when you extract the zipped file :</span></p>

<ol style="text-align: left">
    <li><span style="line-height: 1.5em;font-family: verdana, geneva"><strong>Examples</strong> folder has a sample example of Google and you can run it with the <strong>‘Demo -run me</strong>’ batch file.</span></li>
    <li><span style="line-height: 1.5em;font-family: verdana, geneva"><strong>heliumlib</strong> folder has all the libraries you need to make your own project via Helium.</span></li>
    <li><span style="line-height: 1.5em;font-family: verdana, geneva"><strong>runtime</strong> will have all the logs, you will get after each execution.</span></li>
    <li><span style="line-height: 1.5em;font-family: verdana, geneva"><strong>webdrivers</strong> has chrome and IE driver you require to run your scripts on these browsers.</span></li>
</ol>

<p dir="ltr" style="text-align: left"><span style="font-family: verdana, geneva"><img alt="Helium.PNG" src="https://lh3.googleusercontent.com/zCYEOXVPSxs2pj-Kjecudi22ONd5qZselTF0QrxmNM042GSLqz2YwnUtmdHbtArfnZr5OynxmK3i9Q93A_gkSWZFE30qy2Ba1DCXb8Oq93cJWjX9hqEAoxFGj3cMdv1ccA" width="624px;" height="271px;" /></span></p>

<p dir="ltr" style="text-align: left"><span style="font-family: verdana, geneva"><span style="text-decoration: underline"><strong>Setting up eclipse for the project creation</strong></span></span></p>

<p dir="ltr" style="text-align: left"><span style="font-family: verdana, geneva">For making your own project you need to make a ‘<strong><em>new project</em></strong>’ in eclipse. Then you have to add all the libraries placed in <strong>heliumlib</strong> folder to the <strong>CLASSPATH</strong> of project.</span></p>

<p dir="ltr" style="text-align: left"><span style="font-family: verdana, geneva"><img alt="ClassPAth.PNG" src="https://lh4.googleusercontent.com/M8BXoiEIbTxMbDmA71bC-vun4mTtnRoqGalKjpe24GbGWXJnqxw7fJdV3pf6tRnUjGv-KUe4n8FjVlYH0NExTpsO52jbXm_OcFvQX49nVF0T4cuHJnniUa6PLWuBYs7Rtg" width="624px;" height="357px;" /></span></p>

<p dir="ltr" style="text-align: left"><span style="font-family: verdana, geneva">And start making your own test scripts :)</span></p>

<p dir="ltr" style="text-align: left"><span style="font-family: verdana, geneva"><span style="text-decoration: underline"><strong>Helium API</strong></span></span></p>

<p dir="ltr" style="text-align: left"><span style="font-family: verdana, geneva">1.  For every test case class you need to import helium API.</span></p>

<p dir="ltr" style="text-align: left"><strong><span style="font-family: verdana, geneva"><em>import static com.heliumhq.API.*;</em></span></strong></p>

<p dir="ltr" style="text-align: left"><span style="font-family: verdana, geneva">2. <span style="text-decoration: underline"><span style="color: #000080;text-decoration: underline">Start driver</span></span></span></p>

<p dir="ltr" style="text-align: left"><span style="font-family: verdana, geneva">To start an instance of a browser, you need to write start along with the name of browser you want to run your test scripts on.</span></p>

<p dir="ltr" style="text-align: left"><span style="font-family: verdana, geneva"><img alt="Start.png" src="https://lh5.googleusercontent.com/75M9rRp1uPg1shp22ldE_TIabr92iLXCJlobOj6nuYqNLLz9D4-HhD9U071XiCod-v6fIaHrtTY5IeM7od5N8RdIZmN2ToVOpFKltBnrxLsuaB5nXuinPKOgAqQvVcHs5Q" width="562px;" height="456px;" /></span></p>

<p dir="ltr" style="text-align: left"><span style="font-family: verdana, geneva">And then write the test steps you want to follow. For example, I have automated Xebia India first with Helium, here is the code for the same:</span></p>

<p>[code language="java"]
package com.xebia.web.pages;
import static com.heliumhq.API.*;</p>
<p>public class XebiaIndia {
   public static void main(String args[]) {
      startChrome(&quot;http://xebia.in/&quot;);
      click(&quot;About Us&quot;);
      click(&quot;Services&quot;);
      click(&quot;Products&quot;);
      click(&quot;Trainings&quot;);
      click(&quot;Knowledge&quot;);
      click(&quot;Careers&quot;);
      click(&quot;Events&quot;);
      killBrowser();
   }
}[/code]
<p dir="ltr" style="text-align: left"><span style="font-family: verdana, geneva"><span style="text-decoration: underline"><strong>Why Helium over Selenium?</strong></span></span></p></p>
<ol style="text-align: left">
    <li><span style="line-height: 1.5em;font-family: verdana, geneva">Get rid of finding and locating HTML elements.</span></li>
    <li><span style="line-height: 1.5em;font-family: verdana, geneva">No need for Xpath and other locators.</span></li>
    <li><span style="line-height: 1.5em;font-family: verdana, geneva">Reduces the complexity of code by 50%.</span></li>
    <li><span style="line-height: 1.5em;font-family: verdana, geneva">All driver instantiation, WebDriver, WebElement references can be done by very less keywords.</span></li>

## Comments

**[Jaya](#9496 "2014-07-01 14:51:08"):** Nice Blog. Suppose we have two links with the same name on a webpage. Then how can we tell Helium to click a specific link?

**[Khyati Sehgal](#9497 "2014-07-01 14:58:33"):** Hi Jaya, Helium has the capability to search for the most-nearest-elements . Let say, you are in a flow of doing some activities ,so Helium will pick the most nearest element from the flow you will be working on. Hence it will captures the locator which is near to the links,buttons,frames,etc you will be working on. Hope this answers your question.

