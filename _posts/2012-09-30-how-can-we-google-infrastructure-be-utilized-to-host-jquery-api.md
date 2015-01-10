---
layout: post
header-img: img/default-blog-pic.jpg
author: Shruti Khattar
description: 
post_id: 15001
created: 2012/09/30 21:19:00
created_gmt: 2012/09/30 16:19:00
comment_status: open
---

# How can Google infrastructure be utilized to host JQuery API

<p><strong>How can Google infrastructure be utilized to host JQuery API</strong></p>
<p>The content that you are going to come across in this blog can help you speed up your AJAX applications without having to worry about issues like caching the shared JavaScript API's, network and bandwidth of your system and many other problems, frivolous and major. Now, Google's infrastructure – the fast servers and vast content delivery network have made it possible to host the works of most popular frameworks in JavaScript, including prototype.js, Dojo, script.aculo.us, MooTools and jQuery. CDN helps serving the content which is not only easily available and accessible but also enhances performance benefits by decreasing the page load times.<!--more-->
The greatest benefit is from caching. The theory is that if a visitor visited a site that was loading their JavaScript libraries, say jQuery for example from the Google CDN, then when they visit your website, the library is already in that user’s browser cache and will not have to be downloaded again.
Google AJAX libraries API has stood up beyond the expectation level of techno geeks in terms of performance and efficiency. The team at Google, keep the API up-to-date with the subsequent stable release of AJAX libraries and the minified version of it is served by default. If you specifically want the raw JavaScript itself, you can add the "uncompressed" parameter.
google.load("jquery","1.2",{uncompressed:true});</p>
<p><strong>How to access Google API's</strong>
The libraries can be accessed in two ways:
1. Method 1: Static reference to the desired jquery file/version)-
put</p>
<p>[code]&lt;script&gt;&lt;!--mce:0--&gt;&lt;/script&gt;[/code]</p>
<ol>
<li>Method 2: Dynamic reference using Google AJAX api</li>
</ol>
<p>[code]&lt;script type=&quot;text/javascript&quot;&gt;&lt;!--mce:1--&gt;&lt;/script&gt;
&lt;script type=&quot;text/javascript&quot;&gt;&lt;!--mce:2--&gt;&lt;/script&gt;[/code]</p>
<p><strong>To set value to any element-</strong></p>
<p>[code]&lt;!--</p>
<p>&lt;head&gt;
&lt;mce:script _mce_src=&quot;//ajax.googleapis.com/ajax/libs/jquery/1.8.0/jquery.min.js&quot;&gt;&lt;/mce:script&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;div&gt;
 values are
&lt;span&gt;&lt;/span&gt;
and
&lt;span&gt;&lt;/span&gt;&lt;/div&gt;
&lt;mce:script type=&quot;text/javascript&quot;&gt;&lt;!
$(&quot;div&quot;).data(&quot;value&quot;, { first: &quot;first&quot;, second: &quot;second&quot; });
$(&quot;span:first&quot;).text($(&quot;div&quot;).data(&quot;value&quot;).first);
$(&quot;span:second&quot;).text($(&quot;div&quot;).data(&quot;value&quot;).second);
// --&gt;</p>
<p>--!&gt;[/code]</p>
<p>To handle any event (like keydown)-</p>
<p>[code]
&lt;form&gt;
&lt;input value=&quot;test value&quot; type=&quot;text&quot; id=&quot;target&quot; /&gt;
&lt;/form&gt;
$('#target').keydown(function() {
 // handle key press
}); [/code]</p>
<p>For either method you choose, the things that irked, like hosting the libraries, correctly setting up the cache headers, finding tweaks to make gzip work, preventing cookies and other verbose data that is not required, saving bandwidth, staying up to date with the latest release and so on.
<ol style="list-style-type: lower-alpha;">
    <li> When you are building an intranet application where the web server is hosted on the same network as the clients.</li>
    <li><span style="font-family: arial, helvetica, sans-serif; font-size: small;">If you use Google's CDN jQuery, you will be making a call to the internet rather than a web erver on the local network. This increases bandwidth, and is slower. </span><span style="font-family: arial, helvetica, sans-serif; font-size: small;">When you are server pages over SSL that require jQuery. You should server the javascript over SSL as well as your page to avoid security problems and warnings.</span></li>
</ol></p>