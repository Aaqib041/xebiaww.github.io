---
layout: post
header-img: img/default-blog-pic.jpg
---

# Liferay 5.2x | Mobile Cookbook 

Recently, I was researching upon how to support mobile versions of the websites on Liferay 5.2x. This post would mainly talk about the changes need to be done in Liferay 5.2x to accomplish the same. Liferay 5.2x has  in-built support for providing different themes and layouts for the WAP enabled devices. Primary mark-up language in WAP enabled devices is XHTML Mobile Profile whereas current smart phones browsers are HTML recommended. WAP has become an old school and thus it calls for extending Liferay 5.2x to support Smart phones. To support mobileVersion of the website we need to find answer to following 3 questions. 

  1. How to detect it is a mobile device ?
  2. How to get the theme for the mobile device and ask it to use that theme ?
  3. How to get the page layout for the mobile device and ask it to use that layout ?

**Mobile Device Detection**

Before picking up any strategy, we need to ramp up the device detection capabilities of  Liferay 5.2x, so that it can detect our new smartPhones. This can be done by overriding _isMobile() _in _BrowserSnifferUtil.java_. _isMobile() _method would read user-agent from request and with the help of [WURFL ](http://wurfl.sourceforge.net/)device repository, it can determine if it is mobile.

**Themes and Layouts**

Path to Question 2 and 3 can be found through the following strategies:

Strategy 1 -  **Single Site Strategy **: This mainly is used when  content across the devices is almost same with a variations in the look and feel.  It can be implemented in 2 ways:

  1. **Responsive Web design:** It states managing look and feel (CSS) based on screen size of the device. It can be implemented using CSS media queries and/or JavaScript. Liferay 6, provides same support through AUI().use('aui-viewport').
  2. **Seperate Themes: **It involves defining seperate themes for different devices. A mechanism would detect (to be coded) the device from which the request is coming and based on that it will apply the  theme defined for the device.

Strategy 2 -  **Multiple Site Strategy**: Mutli-site involves developing mobile specific sites and it finds it place when the content across the devices differ. For instance: Facebook mobile version is m.facebook.com. This can be implemented on liferay as separate communities .It can also easily provide support of different sites for different devices (android/ iphone) by having device specific community.

In multiple site strategy, after the device is been detected as mobile it will be redirected to mobile specific site. Since it is a separate community  in itself, it will have its own theme and its own page layout which would be specific to mobile.

  


Now lets see, how the questions 2 and 3 can be resolved for the **single site strategy**. It can be done with the following 2 approaches:

**Approach 1 : **It is based upon leveraging the existing WAP support on Liferay 5.2x.

  * It uses WAP theme as a mobile theme which we can specify for a page through _mobile devices_ in_ look and feel _tab of a page.  To make a HTML theme as a wap theme, enable property <wap-theme> for the theme in liferay-look-and-feel.xml
  * It uses the WAP version of the layout applied on each page.
  * It supports single theme and layout for all mobile devices, CSS can be made device dimension specific by CSS media queries.

How Liferay5.2x supports WAP ?:

  * it picks up WAP theme/HTML theme in _ServicePreAction.java_ depending upon _isWap()_.
  * it picks up WAP/HTML path from _includeLayoutContent() _in _LayoutAction_ depending upon _isWap()_. If _isWap()_ then it calls _LayoutLocalServiceUtil.getWapContent() _in wap/../_layout/view/portlet.jsp _else  it calls_ _LayoutLocalServiceUtil.getContent()__ in html_/../_layout/view/portlet.jsp. __
  * it picks up jsps from WAP/HTML path depending upon _isWap()_.__ __
  * Response type is set to ContentTypes.XHTML_MP if _isWap()_.

Now the question arises how to make it mobile specific.  It can be done in 2 ways.

Solution1 :

  * Override  _ServicePre() _ in_ ServicePreAction.java_ to incorporate check for the _isMobile() _and if found true use Wap theme.
  * Override html_/../_layout/view/portlet.jsp __to include check for _isMobile() and if found true use _LayoutLocalServiceUtil.getWapContent(). __

Solution 2:

  * Override _isWap() _to return true if _isMobile()_ is true.
  * Change contentTypes in pages/classes used by wap implemention from ContentTypes.XHTML_MP to ContentTypes.TEXT_HTML.

**Approach 2: **Build a separate path for mobile on the same lines as HTML and WAP works. It would involve overriding theme/ layout to have support for mobile same as it has for WAP. UI support in manage pages through which theme can be defined for mobile. Besides that_ServicePre() _ in_ ServicePreAction.java _ to use mobile theme when isMobile. It would involve a lot of more work and can be made all together a seperate feature as we have in suppport in Liferay 6.1 provided by [Milen Dyankov](http://milen.commsen.com/2011/03/liferay-multidevice-extension.html).

  


I hope the blog helps you in providing direction for mobile support for sites in Liferay 5.2x. Please refer to the following links for further reference:

<http://www.liferay.com/web/nathan.cavanaugh/blog/-/blogs/8907527>

<http://milen.commsen.com/2011/03/liferay-multidevice-extension.html>

<http://www.liferay.com/community/forums/-/message_boards/message/7864566>