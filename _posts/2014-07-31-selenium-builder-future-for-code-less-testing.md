---
layout: post
header-img: img/default-blog-pic.jpg
author: RSingh
description: 
post_id: 18731
created: 2014/07/31 21:46:47
created_gmt: 2014/07/31 16:46:47
comment_status: open
---

# Selenium Builder – Future for Code-less Testing

<h1 align="CENTER"></h1>

<p align="LEFT">Selenium Builder is an extension to Firefox which <span style="color: #313a3f"><span style="font-family: 'Times New Roman', serif"><span style="font-size: medium">helps you easily build and export Selenium tests using record and playback capabilities, and then export those scripts to any programming language.</span></span></span></p>

<p align="LEFT"><span style="color: #313a3f"><span style="font-family: 'Times New Roman', serif"><span style="font-size: medium">The most important feature to come along in Selenium Builder is built-in support for Selenium 2/Webdriver. You can record, edit, and play back test scripts based on the new Selenium version, which is particularly handy for folks needing help making the switch from Selenium RC to WebDriver. Plus, since Builder uses the same interface for this as it does for Selenium 1, you can use a mixture of both technologies to build your scripts. Selenium Builder can also translate between the two versions with reasonable accuracy.</span></span></span></p>

<p align="LEFT"><span style="color: #313a3f"><span style="font-family: 'Times New Roman', serif"><span style="font-size: medium">Additionally, you can export Selenium 2 scripts into a wide variety of languages: Java, Ruby, Python, PHP, Node.js, and C# . Se Builder 2 also defines a clean JSON-based format for storing scripts for later editing. These JSON-based scripts can be played back by a standalone </span></span></span><span style="color: #000080"><a href="https://github.com/sebuilder/se-builder/downloads"><span style="color: #1a83d4"><span style="font-size: medium">interpreter</span></span></a></span><span style="color: #313a3f"><span style="font-family: 'Times New Roman', serif"><span style="font-size: medium">, so you don’t have to commit to a particular programming language for playback.</span></span></span></p>

<h2 align="LEFT"><span style="color: #333333"><span style="font-family: 'Times New Roman', serif"><span style="font-size: medium">Record the test</span></span></span></h2>

<p><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>You are now ready to record a test against xebia.in. The test will check that the site does not allow logging in with non-valid credentials.</span></span></span>
<ul>
    <li><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>In Firefox: </span></span></span><strong><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>Tools &gt;Web Developer &gt; Launch Selenium Builder</span></span></span></strong></li>
</ul>
<a href="http://xebee.xebia.in/wp-content/uploads/2014/07/11.png"><img class="alignnone size-medium wp-image-18732" alt="1" src="http://xebee.xebia.in/wp-content/uploads/2014/07/11-300x212.png" width="300" height="212" /></a></p>
<p><strong></strong><strong><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>In </span></span></span></strong><strong><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>Start recording at</span></span></span></strong><strong><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>, enter </span></span></span></strong><strong><span style="color: #c21e3b"><span>http://xebia.in/</span></span></strong>
<ul>
    <li><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>Hit the </span></span></span><strong><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>Selenium 2</span></span></span></strong><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span> button</span></span></span></li>
    <li><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>This will open the URL in a browser tab, and that tab will be highlighted, signalling that this is where you are recording the test. The SeleniumBuilder window will now look like this:</span></span></span></li>
</ul>
<a href="http://xebee.xebia.in/wp-content/uploads/2014/07/2.png"><img class="alignnone size-medium wp-image-18733" alt="2" src="http://xebee.xebia.in/wp-content/uploads/2014/07/2-300x61.png" width="300" height="61" /></a></p>
<p><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span> As you can see, there is already an instruction in the list, which says "get </span></span></span><strong><span style="color: #c21e3b"><span>http://xebia.in/</span></span></strong><span style="color: #000080"><a href="http://doc.tiki.org/Documentation" target="_blank"><span style="color: #c21e3b"><span>".</span></span></a></span>
<ul>
    <li><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>Next, you want to carry out the steps in the test</span></span></span>
<ul>
    <li><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>Click on link text “ News “ on xebia.in.</span></span></span></li>
    <li><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>Click on link text “ Continuous Delivery “ on xebia.in</span></span></span></li>
    <li><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>Click on link text “ Advanced Scrum “ on xebia.in</span></span></span></li>
</ul>
</li>
</ul>
<span style="color: #333333"><span style="font-family: Lato, sans-serif"><span> At that point, your SeleniumBuilder window looks something like this:</span></span></span></p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/07/3.png"><img class="alignnone size-medium wp-image-18734" alt="3" src="http://xebee.xebia.in/wp-content/uploads/2014/07/3-300x114.png" width="300" height="114" /></a></p>
<p><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span> The next step is to add an assertion. </span></span></span><span style="color: #333333"> </span><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>we will add the verification manually...</span></span></span></p>
<p><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span> Put the mouse over Step 2</span></span></span></p>
<p><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span> You will see a menu appear on the left.</span></span></span></p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/07/4.png"><img class="alignnone size-medium wp-image-18735" alt="4" src="http://xebee.xebia.in/wp-content/uploads/2014/07/4-300x197.png" width="300" height="197" /></a></p>
<p><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span> Click on </span></span></span><strong><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>new step below</span></span></span></strong></p>
<p><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span> You will see a new step that says </span></span></span><em><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>"clickElement id:"</span></span></span></em><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>. This is the default type of step. You will have to change it to be an assertion.</span></span></span>
<ul>
    <li><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>Put the mouse over that new step, and click on </span></span></span><strong><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>edit type</span></span></span></strong><span style="color: #333333"><span style="font-family: Lato, sans-serif"><span>.</span></span></span></li></p>