---
layout: post
header-img: img/default-blog-pic.jpg
---

# jQuery Data API

How many times has it happened with you that on your view page you need to associate some data values with a UI component like Dropdown or Checkbox, a possible solution in such cases is to keep data in hidden variables in the HTML or in variable in JavaScript. Well there was a similar scenario in our project, our project is Spring/ Hibernate project where we are using JSP for views and jQuery for writing java script. A couple of pages heavily used Java Script and we were required to maintain data on the page, we came across jQuery Data API for storing data. jQuery Data API allows you to associate any data value with a specified element on  the UI and then later retrieve it later. For adding data it simply: _jQuery.data( element, key, value )_ And then to fetch it back: _jQuery.data(element, key)_ For example these are couple of line from our js file where we associate tape data with radio button:                              _$.data(radioObject, 'tapeData', tape);_ and later fetch the data when we need:                              _setTapeInfo($.data(_radioObject_, 'tapeData'));_ Here _radioObject _is jQuery wrapper object over the UI element. jQuery data api also ensures efficiet memory management provided by jQuery. For Details refer: [jQuery Data](http://api.jquery.com/jQuery.data/)