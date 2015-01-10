---
layout: post
header-img: img/default-blog-pic.jpg
author: kgujral
description: 
post_id: 14403
created: 2012/06/12 13:10:22
created_gmt: 2012/06/12 08:10:22
comment_status: open
---

# Rich UI interactions the easy way -- BackBone.js

<p>During the past few weeks I've been looking into a lot of javascript based UI frameworks and thats when I discovered "Backbone.js".</p>
<p>Backbone.js basically is a MVC (Model View Collections) based framework for client side scripting. In a nutshell a Model represents a domain object. A Collection is a set of models. And a View governs how to display a particular model or collection and also when to refresh the view as and when the models or collections change.</p>
<p>I developed a very simple application which shows the tweets of <strong>@XebiaIndia</strong> in an infinite scrolling view (as seen on facebook or twitter as we keep scrolling the page, new feeds keep on loading).
<em><strong>Here is the link to the demo application : </strong></em><a href="http://kgujral-backbone.appspot.com/">http://kgujral-backbone.appspot.com/</a></p>
<!--more-->

<p>Lets get started and try to understand the code.</p>
<p>Here is the index.html file (landing page):</p>
<p>[code]
&lt;html&gt;
&lt;head&gt;
    &lt;link type=&quot;text/css&quot; rel=&quot;stylesheet&quot; href=&quot;css/main.css&quot;&gt;
    &lt;script type=&quot;text/javascript&quot; src=&quot;js/libs/jquery.js&quot;&gt;&lt;/script&gt;
    &lt;script type=&quot;text/javascript&quot; src=&quot;js/libs/jquery-tmpl.js&quot;&gt;&lt;/script&gt;
    &lt;script type=&quot;text/javascript&quot; src=&quot;js/libs/underscore.js&quot;&gt;&lt;/script&gt;
    &lt;script type=&quot;text/javascript&quot; src=&quot;js/libs/backbone.js&quot;&gt;&lt;/script&gt;
    &lt;script type=&quot;text/javascript&quot; src=&quot;js/app.js&quot;&gt;&lt;/script&gt;
    &lt;title&gt;Infinite Scroll View -- BackBone.js&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1&gt;Infinite Scroll View -- BackBone.js&lt;/h1&gt;
    &lt;h3&gt;Tweets from @XebiaIndia&lt;/h3&gt;
    &lt;div id=&quot;tweets&quot;&gt;
    &lt;/div&gt;
    &lt;div id=&quot;footer&quot;&gt;Please scroll down inside the upper div to load more tweets&lt;/div&gt;</p>
<p>&lt;!-- Template definition for displaying tweets --&gt;
    &lt;script id=&quot;tweet-tmpl&quot; type=&quot;text/x-jquery-tmpl&quot;&gt;
        &lt;div class = &quot;post&quot;&gt;
            &lt;img class = &quot;img&quot; src = &quot;${user.profile_image_url}&quot; /&gt;
            &lt;span class = &quot;text&quot;&gt;${text}&lt;/span&gt;
        &lt;/div&gt;
    &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
[/code]</p>
<p><span style="font-size: small;"><strong><span style="color: #f09021;">Javascript libraries used and their significance</span></strong></span>
<ul>
    <li><strong><em>jquery.js</em></strong> for we cannot work without it !</li>
    <li><em><strong>jquery-tmpl.js</strong></em> is a library to create HTML Templates and render them with JSON data.</li>
    <li><em><strong>underscore.js</strong></em> : backbone.js has a hard dependency on this. It contains a lot of utility functions.</li>
    <li><em><strong>backbone.js</strong></em> : For the MVC support.</li>
    <li><em><strong>app.js </strong></em>: This js file contains all the view and collection definitions and also defines an entry point for the app.  (For the ease of understanding the example I wrote all the view and collection definitions in one js file; but its recommended to keep each Model, View and collection definition in different js files)</li>
</ul>
<div>Lets understand the app.js file now one part at a time :</div>
<span style="color: #f09021;"><strong><span style="font-size: small;">Collection definition:</span></strong></span></p>
<p>[code]
var Tweets = Backbone.Collection.extend({
    url : function() {
        return &quot;tweets/&quot;+this.page;
    },
    nextPage : function() {
        this.page++;
    },
    page : 1
});
[/code]
<ul>
    <li>The <em><strong>url </strong></em>property of the Tweets collections defines which URL to hit whenever the collection needs to create, read, update, delete the data inside it. Using this URL property, Backbone automatically creates the corresponding POST, GET, PUT, DELETE request to the server.</li>
    <li>The <em><strong>nextPage </strong></em>property simply defines the logic to move to the next page.</li>
    <li>The <em><strong>page </strong></em>property holds the current page number to load.</li>
</ul>
<span style="color: #f09021;"><strong><span style="font-size: small;">View definition:</span></strong></span></p>
<p>[code]
var ScrollView = Backbone.View.extend({
    el : $('#tweets'),
    events : {
        'scroll' : 'scrollEvent'
    },</p>
<pre><code>initialize : function(options) {
    this.template = options.template;
    _.bindAll(this, 'render', 'getTweets', 'scrollEvent');
    this.scrollTopPrev = this.el.scrollTop;
    this.tweetCollection = new Tweets();
    this.tweetCollection.bind('reset', this.render);
    this.isLoading = false;
    this.getTweets();
},

render : function() {
    var template = $(this.template);
    var html = template.tmpl(this.tweetCollection.toJSON());
    $(this.el).append(html);
    this.isLoading = false;
},

getTweets : function() {
    this.isLoading = true;
    this.tweetCollection.fetch();
},

scrollEvent : function() {
    var offset = 300;
    if (!this.isLoading
        &amp;amp;&amp;amp; (this.scrollTopPrev &amp;lt; this.el.scrollTop) &amp;amp;&amp;amp; ($(this.el).height() + this.el.scrollTop + offset &amp;gt;= this.el.scrollHeight)) {
        this.tweetCollection.nextPage();
        this.getTweets();
    }
    this.scrollTopPrev = this.el.scrollTop;
}
</code></pre>
<p>});
[/code]
<ul>
    <li>The <em><strong>el </strong></em>property defines the parent or the container tag on which all the DOM manipulations and event listening takes place.</li>
    <li>The <em><strong>initialize </strong></em>property is like a constructor of the view. All the required properties like the collection, the template element  etc. are being initialized here. The initial loading of tweets is also taking place here. Another notable thing here is <em>this.tweetCollection.bind('reset', this.render);</em> This is an in-built feature of backbone.js which triggers the 'reset' event of any collection (tweetCollection in this case) whenever we load new results into it and binds the callback to the 2nd parameter i.e. <em>this.render</em> method contained within this view. Thus instead of getting confused between callbacks we can write very manageable and easy to understand code by just responding to events.</li>
    <li>The <em><strong>render </strong></em>property just reads the jquery template and renders it with the collection data to create the actual DOM structure of tweets which is visible to the user.</li>
    <li>The <em><strong>getTweets </strong></em>property fetches the next page of tweets and it internally fires the 'reset'
event and hence the new data is again rendered onto the view.</li>
    <li>The <em><strong>scrollEvent </strong></em>property checks if the scroll has reached to the bottom of the container and loads new tweets accordingly.</li>
</ul>
<div></div>
<div></div>
<strong style="color: #f09021;"><span style="font-size: small;">Finally the code in app.js that acts as the entry point:</span></strong></p>
<p>[code]
$(document).ready(function(){
    var scrollView = new ScrollView({
        template  : &quot;#tweet-tmpl&quot;
    });
});
[/code]</p>
<p>When the DOM is loaded, it just creates an object of the View we created above and provides it with the Template <em>#tweet-tmpl</em> which was already created the home.html file.</p>
<p><strong style="color: #f09021;"><span style="font-size: small;">Below is the back-end code written using Spring which needs no explanation:</span></strong></p>
<p>[code]
    @RequestMapping(value = &quot;/tweets/{page}&quot;, method = RequestMethod.GET)
    public @ResponseBody String getTweetsByPage(@PathVariable int page) {
        RestTemplate rt = new RestTemplate();
        String tweetJson = rt.getForObject(&quot;https://api.twitter.com/1/statuses/user_timeline.json?&quot;
                        + &quot;screen_name=XebiaIndia&amp;count=10&amp;page=&quot; + page, String.class);
        return tweetJson;
    }
[/code]</p>
<p><strong style="color: #f09021;"><span style="font-size: small;">Conclusion</span></strong></p>