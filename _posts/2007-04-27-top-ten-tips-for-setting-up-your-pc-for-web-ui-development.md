---
layout: post
header-img: img/default-blog-pic.jpg
author: balajidl
description: 
post_id: 206
created: 2007/04/27 13:55:15
created_gmt: 2007/04/27 11:55:15
comment_status: open
---

# Top ten tips for setting up your PC for Web UI development

If your job role involves activities like "web page scripting, AJAX, interactive user interface development, HTML content rendering, rich content website development and whatever that you do to get the webpage up and showing", then you might find this blog entry handy while setting up your PC for web application/UI developments. _This blog entry is not about web layout or image design & development._

  1. Test with [Firefox][1]

> No matter what, this is a must tool for you. While I personally I surfing with IE, Firefox stays ahead of many browsers when its comes to testing your web page. Lots of [plugins][2] developed around this make Firefox useful everyday. Continue reading this blog, you will see yourself why it is so.

  2. Add [Web Developer][3]

> Imagine a tool that can help you view/edit/list css of a webpage and that too dynamically ?. Imagine a tool that can help you to trace & inspect attributes of a html object just by placing the mouse over it ?. Imagine a tool that can show your webpage on various screen resolution.. Yes its all there in Web Developer. Want to see exactly it looks like, click this [ link][4] which shows the screenshot of a webpage in firefox with web developer activated.

  3. Add [Venkman / SeaMonkey ][5]

> At one point, you might require a tool where you need to debug your javascript line by line and/or method by method. Add watch to a variable, simulate a script and so on. Yes this kind of debugging tool is available for javascript and the best one that works quite well with Firefox2.0 is Venkman.

  4. Add [Microsoft Script Editor][6] \- Javascript Debugger for MS IE 

> Getting it working is little difficult, but atleast the tutorial at http://www.jonathanboutelle.com/mt/archives/2006/01/howto_debug_jav.html helps us to do so. This helps if you want to fix bugs that works in Firefox but not in IE.

  5. Add [FireBug][7]

> This is as powerful as Web developer. The unique features of this is the ability to sniff the messages that was sent using XMLHttprequest/AJAX. This helps you to see what exactly your request/response carries. Particulary useful for tracing JSON datas sent while using AJAX

  6. Add [Scriptaculous][8]

> If you do lots of javascripting for your page (and or AJAX), then it is advisable to use Scriptaculous - Prototype capability. Dont waste your time by writing codes that do the DHTML magics. Everything that your rich web UI expects to have is defined in it. Includes funtionality like Array manipulations, JSON, Drag & drop and so on. Dont reinvent the wheel ?

  7. Use a prebuilt Javascript library for AJAX operations 

> Dont write your code unless you want to make less than 2 ajax calls for your application. Javascript libraries like Dojo, DWR, Ajax tags works very will leading web apploication frameworks. For instance DWR with Spring makes AJAXing extremely easy.

  8. Use [Xeno Link sleuth][9]

> Dont publish your website without checking for the broken links. Even if you happen to remove a webapge from your directory, do provide a re-redirect option for that page. This will help your customer stay tuned to new changes in your site. Xenu link sleuth does a great job on finding and listing broken links.

  9. Use [Selenium][10]

> The best testing breeed that can test and tell things like Browser compatibility checking and System functionality testing. A nice tool for writing test cases on javascript as well.

  10. Add [HTML Validator][11]

> A tool that will help to confirm your html code to W3C XHTML specifications. It is important to get your web page to xhtml complaint as you will never know what user-agent will be? It can be a mobile or pda or webtv and so on. XHTML atleast helps to keep the content layout stable.

**These are just top 10 tips and if you found some more tools useful at everyday of your web development, please kindly add that as a comment. **

   [1]: http://www.mozilla.com/en-US/firefox/all.html
   [2]: https://addons.mozilla.org/en-US/firefox/addon/2207
   [3]: http://chrispederick.com/work/webdeveloper/
   [4]: http://xebee.xebia.in/wp-content/uploads/2007/04/webdeveoper_snap.gif (web developer screen shot)
   [5]: http://www.mozilla.org/projects/venkman/
   [6]: http://www.jonathanboutelle.com/mt/archives/2006/01/howto_debug_jav.html
   [7]: https://addons.mozilla.org/en-US/firefox/addon/1843
   [8]: http://wiki.script.aculo.us/scriptaculous/show/Usage
   [9]: http://home.snafu.de/tilman/xenulink.html
   [10]: http://www.openqa.org/selenium/
   [11]: https://addons.mozilla.org/en-US/firefox/addon/887