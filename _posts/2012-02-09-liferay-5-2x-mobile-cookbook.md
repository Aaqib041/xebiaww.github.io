---
layout: post
header-img: img/default-blog-pic.jpg
author: mjain
description: 
post_id: 11371
created: 2012/02/09 16:52:00
created_gmt: 2012/02/09 11:52:00
comment_status: open
---

# Liferay 5.2x | Mobile Cookbook 

<p>Recently, I was researching upon how to support mobile versions of the websites on Liferay 5.2x. This post would mainly talk about the changes need to be done in Liferay 5.2x to accomplish the same.</p>
<p>Liferay 5.2x has  in-built support for providing different themes and layouts for the WAP enabled devices. Primary mark-up language in WAP enabled devices is XHTML Mobile Profile whereas current smart phones browsers are HTML recommended. WAP has become an old school and thus it calls for extending Liferay 5.2x to support Smart phones.</p>
<p>To support mobileVersion of the website we need to find answer to following 3 questions.
<ol>
    <li>How to detect it is a mobile device ?</li>
    <li>How to get the theme for the mobile device and ask it to use that theme ?</li>
    <li>How to get the page layout for the mobile device and ask it to use that layout ?</li>
</ol>
<div></p>
<!--more-->

<div><strong>Mobile Device Detection</strong></div>

<div>Before picking up any strategy, we need to ramp up the device detection capabilities of  Liferay 5.2x, so that it can detect our new smartPhones. This can be done by overriding <em>isMobile() </em>in <em>BrowserSnifferUtil.java</em>. <em>isMobile() </em>method would read user-agent from request and with the help of <a href="http://wurfl.sourceforge.net/">WURFL </a>device repository, it can determine if it is mobile.</div>

<div><strong>Themes and Layouts</strong></div>

<p></div>
<div>Path to Question 2 and 3 can be found through the following strategies:</div>
<div>Strategy 1 -  <strong>Single Site Strategy </strong>: This mainly is used when  content across the devices is almost same with a variations in the look and feel.  It can be implemented in 2 ways:</div>
<div>
<ol>
    <li><b>Responsive Web design:</b> It states managing look and feel (CSS) based on screen size of the device. It can be implemented using CSS media queries and/or JavaScript. Liferay 6, provides same support through AUI().use('aui-viewport').</li>
    <li><b>Seperate Themes: </b>It involves defining seperate themes for different devices. A mechanism would detect (to be coded) the device from which the request is coming and based on that it will apply the  theme defined for the device.</li>
</ol>
</div>
<div>Strategy 2 -  <strong>Multiple Site Strategy</strong>: Mutli-site involves developing mobile specific sites and it finds it place when the content across the devices differ. For instance: Facebook mobile version is m.facebook.com. This can be implemented on liferay as separate communities .It can also easily provide support of different sites for different devices (android/ iphone) by having device specific community.</div>
<div></div>
<div></div>
<div>In multiple site strategy, after the device is been detected as mobile it will be redirected to mobile specific site. Since it is a separate community  in itself, it will have its own theme and its own page layout which would be specific to mobile.</div>
<div></div>
<br/>
<div>Now lets see, how the questions 2 and 3 can be resolved for the <strong>single site strategy</strong>. It can be done with the following 2 approaches:</div>
<div><strong>Approach 1 : </strong>It is based upon leveraging the existing WAP support on Liferay 5.2x.</div>
<div>
<ul>
    <li>It uses WAP theme as a mobile theme which we can specify for a page through <em>mobile devices</em> in<em> look and feel </em>tab of a page.  To make a HTML theme as a wap theme, enable property &lt;wap-theme&gt; for the theme in liferay-look-and-feel.xml</li>
    <li>It uses the WAP version of the layout applied on each page.</li>
    <li>It supports single theme and layout for all mobile devices, CSS can be made device dimension specific by CSS media queries.</li>
</ul>
<div>How Liferay5.2x supports WAP ?:</div>
<div>
<ul>
    <li> it picks up WAP theme/HTML theme in <em>ServicePreAction.java</em> depending upon <em>isWap()</em>.</li>
    <li>it picks up WAP/HTML path from <em>includeLayoutContent() </em>in <em>LayoutAction</em> depending upon <em>isWap()</em>. If <em>isWap()</em> then it calls <em>LayoutLocalServiceUtil.getWapContent() </em>in wap/../<em>layout/view/portlet.jsp </em>else  it calls<em> <em>LayoutLocalServiceUtil.getContent()</em></em> in html<em>/../<em>layout/view/portlet.jsp. </em></em></li>
    <li>it picks up jsps from WAP/HTML path depending upon <em>isWap()</em>.<em><em>
</em></em></li>
    <li>Response type is set to ContentTypes.XHTML_MP if <em>isWap()</em>.</li>
</ul>
</div>
<div></div>
<div>Now the question arises how to make it mobile specific.  It can be done in 2 ways.</div>
<div>Solution1 :</div>
<div>
<ul>
    <li>Override  <em>ServicePre() </em> in<em> ServicePreAction.java</em> to incorporate check for the <em>isMobile() </em>and if found true use Wap theme.</li>
    <li>Override html<em>/../<em>layout/view/portlet.jsp </em></em>to include check for <em>isMobile() and if found true use <em>LayoutLocalServiceUtil.getWapContent(). </em></em></li>
</ul>
<div>Solution 2:</div>
<div>
<ul>
    <li>Override <i>isWap() </i>to return true if <em>isMobile()</em> is true.</li>
    <li>Change contentTypes in pages/classes used by wap implemention from ContentTypes.XHTML_MP to ContentTypes.TEXT_HTML.</li>
</ul>
<div><strong>Approach 2: </strong>Build a separate path for mobile on the same lines as HTML and WAP works. It would involve overriding theme/ layout to have support for mobile same as it has for WAP. UI support in manage pages through which theme can be defined for mobile. Besides that<em>ServicePre() </em> in<em> ServicePreAction.java </em> to use mobile theme when isMobile. It would involve a lot of more work and can be made all together a seperate feature as we have in suppport in Liferay 6.1 provided by <a href="http://milen.commsen.com/2011/03/liferay-multidevice-extension.html">Milen Dyankov</a>.</div>
<div></div>
</div>
<div></div>
<br/>
<div>I hope the blog helps you in providing direction for mobile support for sites in Liferay 5.2x. Please refer to the following links for further reference:</div>
<div><a href="http://www.liferay.com/web/nathan.cavanaugh/blog/-/blogs/8907527">http://www.liferay.com/web/nathan.cavanaugh/blog/-/blogs/8907527</a></div>
<div><a href="http://milen.commsen.com/2011/03/liferay-multidevice-extension.html">http://milen.commsen.com/2011/03/liferay-multidevice-extension.html</a></div>
<div><a href="http://www.liferay.com/community/forums/-/message_boards/message/7864566">http://www.liferay.com/community/forums/-/message_boards/message/7864566</a></div>
<div></div>
</div>
<div></div>
<div></div>
<div></div>
</div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div></p>