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

During the past few weeks I've been looking into a lot of javascript based UI frameworks and thats when I discovered "Backbone.js".

Backbone.js basically is a MVC (Model View Collections) based framework for client side scripting. In a nutshell a Model represents a domain object. A Collection is a set of models. And a View governs how to display a particular model or collection and also when to refresh the view as and when the models or collections change.

I developed a very simple application which shows the tweets of **@XebiaIndia** in an infinite scrolling view (as seen on facebook or twitter as we keep scrolling the page, new feeds keep on loading). _**Here is the link to the demo application : **_<http://kgujral-backbone.appspot.com/>

Lets get started and try to understand the code.

Here is the index.html file (landing page):

``` 
 <html> <head> <link type="text/css" rel="stylesheet" href="css/main.css"> <script type="text/javascript" src="js/libs/jquery.js"></script> <script type="text/javascript" src="js/libs/jquery-tmpl.js"></script> <script type="text/javascript" src="js/libs/underscore.js"></script> <script type="text/javascript" src="js/libs/backbone.js"></script> <script type="text/javascript" src="js/app.js"></script> <title>Infinite Scroll View -- BackBone.js</title> </head> <body> <h1>Infinite Scroll View -- BackBone.js</h1> <h3>Tweets from @XebiaIndia</h3> <div id="tweets"> </div> <div id="footer">Please scroll down inside the upper div to load more tweets</div>

<!-- Template definition for displaying tweets --> <script id="tweet-tmpl" type="text/x-jquery-tmpl"> <div class = "post"> <img class = "img" src = "${user.profile_image_url}" /> <span class = "text">${text}</span> </div>     </script> </body> </html> 
 ```

**Javascript libraries used and their significance**

  * **_jquery.js_** for we cannot work without it !
  * _**jquery-tmpl.js**_ is a library to create HTML Templates and render them with JSON data.
  * _**underscore.js**_ : backbone.js has a hard dependency on this. It contains a lot of utility functions.
  * _**backbone.js**_ : For the MVC support.
  * _**app.js **_: This js file contains all the view and collection definitions and also defines an entry point for the app.  (For the ease of understanding the example I wrote all the view and collection definitions in one js file; but its recommended to keep each Model, View and collection definition in different js files)

Lets understand the app.js file now one part at a time :

**Collection definition:**

``` 
 var Tweets = Backbone.Collection.extend({ url : function() { return "tweets/"+this.page; }, nextPage : function() { this.page++; }, page : 1 }); 
 ``` 

  * The _**url **_property of the Tweets collections defines which URL to hit whenever the collection needs to create, read, update, delete the data inside it. Using this URL property, Backbone automatically creates the corresponding POST, GET, PUT, DELETE request to the server.
  * The _**nextPage **_property simply defines the logic to move to the next page.
  * The _**page **_property holds the current page number to load.
**View definition:**

``` 
 var ScrollView = Backbone.View.extend({ el : $('#tweets'), events : { 'scroll' : 'scrollEvent' },
    
    
    initialize : function(options) {
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
            &amp;&amp; (this.scrollTopPrev &lt; this.el.scrollTop) &amp;&amp; ($(this.el).height() + this.el.scrollTop + offset &gt;= this.el.scrollHeight)) {
            this.tweetCollection.nextPage();
            this.getTweets();
        }
        this.scrollTopPrev = this.el.scrollTop;
    }
    

}); 
 ``` 

  * The _**el **_property defines the parent or the container tag on which all the DOM manipulations and event listening takes place.
  * The _**initialize **_property is like a constructor of the view. All the required properties like the collection, the template element  etc. are being initialized here. The initial loading of tweets is also taking place here. Another notable thing here is _this.tweetCollection.bind('reset', this.render);_ This is an in-built feature of backbone.js which triggers the 'reset' event of any collection (tweetCollection in this case) whenever we load new results into it and binds the callback to the 2nd parameter i.e. _this.render_ method contained within this view. Thus instead of getting confused between callbacks we can write very manageable and easy to understand code by just responding to events.
  * The _**render **_property just reads the jquery template and renders it with the collection data to create the actual DOM structure of tweets which is visible to the user.
  * The _**getTweets **_property fetches the next page of tweets and it internally fires the 'reset' event and hence the new data is again rendered onto the view.
  * The _**scrollEvent **_property checks if the scroll has reached to the bottom of the container and loads new tweets accordingly.

**Finally the code in app.js that acts as the entry point:**

``` 
 $(document).ready(function(){ var scrollView = new ScrollView({ template : "#tweet-tmpl" }); }); 
 ```

When the DOM is loaded, it just creates an object of the View we created above and provides it with the Template _#tweet-tmpl_ which was already created the home.html file.

**Below is the back-end code written using Spring which needs no explanation:**

``` 
 @RequestMapping(value = "/tweets/{page}", method = RequestMethod.GET) public @ResponseBody String getTweetsByPage(@PathVariable int page) { RestTemplate rt = new RestTemplate(); String tweetJson = rt.getForObject("https://api.twitter.com/1/statuses/user_timeline.json?" \+ "screen_name=XebiaIndia&count=10&page=" \+ page, String.class); return tweetJson; } 
 ```

**Conclusion**