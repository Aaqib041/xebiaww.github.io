---
layout: post
header-img: img/default-blog-pic.jpg
author: jpuri
description: 
post_id: 17101
created: 2013/09/04 22:38:33
created_gmt: 2013/09/04 17:38:33
comment_status: open
---

# jQuery Data API

<p><span style="line-height: 1.5em;">How many times has it happened with you that on your view page you need to associate some data values with a UI component like Dropdown or Checkbox, a possible solution in such cases is to keep data in hidden variables in the HTML or in variable in JavaScript.</span></p>
<p>Well there was a similar scenario in our project, our project is Spring/ Hibernate project where we are using JSP for views and jQuery for writing java script. A couple of pages heavily used Java Script and we were required to maintain data on the page, we came across jQuery Data API for storing data.</p>
<p>jQuery Data API allows you to associate any data value with a specified element on  the UI and then later retrieve it later.</p>
<p><span style="line-height: 1.5em;">For adding data it simply: <em>jQuery.data( element, key, value )</em></span>
<span style="line-height: 1.5em;">And then to fetch it back: <em>jQuery.data(element, key)</em></span></p>
<p><span style="line-height: 1.5em;">For example these are couple of line from our js file where we associate tape data with radio button:</span>
<span style="line-height: 1.5em;">                             <em>$.data(radioObject, 'tapeData', tape);</em></span>
<span style="line-height: 1.5em;">and later fetch the data when we need:</span>
<span style="line-height: 1.5em;">                             <em>setTapeInfo($.data(<em>radioObject</em>, 'tapeData'));</em></span></p>
<p>Here <em>radioObject </em>is jQuery wrapper object over the UI element.</p>
<p>jQuery data api also ensures efficiet memory management provided by jQuery. For Details refer: <a title="jQuery Date" href="http://api.jquery.com/jQuery.data/">jQuery Data</a></p>