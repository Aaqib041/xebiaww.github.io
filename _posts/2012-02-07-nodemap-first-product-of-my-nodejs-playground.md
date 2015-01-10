---
layout: post
header-img: img/default-blog-pic.jpg
author: vrawat
description: 
post_id: 11297
created: 2012/02/07 14:31:19
created_gmt: 2012/02/07 09:31:19
comment_status: open
---

# Nodemap - First product of my Nodejs playground

I started learning Nodejs few weeks back and fell in love with the way it eases the development of real-time web applications. In general, when I start learning a new technology I try to make an app out of that learning. In that process, the most challenging task is "What to develop?" rather than How to develop? Finally an interesting app idea came into my mind and I was able to turn that idea into an application. So here I am explaining the app and its code in detail.

[NodeMap][1] : A realtime Nodejs application with Express, Nowjs, Ejs, Google Maps, Mongodb hosted on Joyent SmartMachines. What does this app do? It simply shows real-time messages on the world map from different users around the world in an awesome way. **[Click Here To See Working App**][1] First things first, the technology aspect : 

  * **Why Nodejs?** Because its awesome and amazingly fast server side javascript.(For more info refer [Nodejs Homepage][2]).
  * **Why Express?** Because I wanted a Web-MVC and this one is the best out there, with community support.(For more info refer [Expressjs Web MVC][3])
  * **Why Nowjs?** Because it allows me to "Call client functions from the server and server functions from client".(It internally uses socket.io, for more info refer [Nowjs][4])
  * **Why ejs?** Because this template engine is very similar to html. So it was easy to grasp.(low learning curve For more info refer [EJS][5])
  * **Why Google Maps?** Because I wanted to show some maps in my app. (For more info refer [Google Maps API v3][6])
  * **Why Mongodb?** Because I wanted a light-weight NoSQL database which can easily run on Joyent SmartMachines.(For more info refer [Mongodb][7])
Lets deep dive into the application code, here is the link to [Github Repository][8].

In a real-time web application both server side and client side code plays vital role in the proper functioning of the applicaiton. So I will switch between server-side code and client-side code while explaining the code.

Lets start with the server side snippet below :

[sourcecode lang="javascript"] //Module dependencies. var express = require('express'), nowjs = require('now'), mongojs = require('mongojs'); //starting server var app = express.createServer().listen(80); //Server configuration app.configure(function(){ app.set('views', **dirname + '/views'); app.set('view engine', 'ejs'); app.use(express.bodyParser()); app.use(express.methodOverride()); app.use(express.static(**dirname + '/public')); }); //Initialize mongojs to connect with mongodb var db = mongojs.connect('nodemapdb', ['mycollection']); //Initialize nowjs for real-time client-to-server and server-to-client calls var everyone = nowjs.initialize(app, {socketio: {transports: ['xhr-polling', 'jsonp-polling']}}); [/sourcecode]

Server side code starts with including the required dependencies( which are expressjs, nowjs and mongojs) and configuring them appropriately. Lets have a look at each one of them : **Express** Line 4 creates a HttpServer to listen on port 80. Line 6 starts with configuration of the server. Line 7 sets the views directory path. Line 8 sets the view engine to ejs (if we don't set it the default jade view engine will be used). bodyParser() parses the request body and populates req.body methodOverride() checks req.body.method for the HTTP method override(i.e override to GET/POST/PUT etc.) Line 11 sets the static resource directory which will serve the static resources to the client. Thats all with express(for more info. please refer [Express Guide][9])

 

**Mongojs** Line 14 initializes mongojs and configures it to connect with database 'nodemapdb' and collection 'mycollection'. For more configuration options refer : [Mongojs Read Me][10]

**Nowjs** Line 16 initializes nowjs to work with the current HttpServer and explicitly set the socket.io transport to xhr or json polling. The second parameter is optional and by default uses websockets as first priority. I have explicitly set it because my application host does not support websockets on free accounts.

Now lets see how the request to home page is handled :

[sourcecode lang="javascript"] app.get('/', function(req, res){ var dataArray=[]; db.mycollection.find({},{_id:0}).forEach(function(err, doc) { if (!doc) { // we visited all docs in the collection res.render('index', { title: 'NodeMap : Using Nodejs,Nowjs,Mongodb & Google Maps ' , messages : JSON.stringify(dataArray)}); return; } dataArray.push(doc); }); }); [/sourcecode]

Whenever a person hits <http://nodemap.no.de> the above app.get("/",...) method will be called. What I am doing here is, simply getting all the documents stored in the database(without the "_id" field). And for each document found, it is pushed in "dataArray" and finally a call to **res.render** which will render the **index.ejs** in the views directory, and, since I haven't set **"layout : false"** while configuring the server, the contents of [index.ejs][11] will be rendered as "body" of [layout.ejs][12](the concept of layouts in express is similar to concept of master-pages).

 

Now lets see how the initial markers are set on the map(client-side code):

[sourcecode lang="javascript" highlight="15"] <body onload="initialize();"> <div id="map_canvas" ></div> <div id="form-wrapper" style="z-index: 1000002; position: absolute; right: 0px; bottom: 0px; "> <div id="form1"> Name : <input id="owner" type="text" /><br/> Message : <input id="message" type="text" /><br/> <input id="locationCheck" type="checkbox" >Use my location</input><br/> <div id="cord" style="display:none;"> Latitude : <input id="latitude" type="text" /><br/> Longitude : <input id="longitude" type="text" /> </div> <input type="button" value="Submit" onclick="getGeoLocation();"/> </div> </div> <div id="messages" style="display:none;"><%=messages%></div> </body>

<script> function initialize() { var mapOptions = { maxZoom: 9, minZoom:2, zoom: 2, center: new google.maps.LatLng(27, 30), mapTypeControl: false, streetViewControl: false }; var mapType = new google.maps.StyledMapType([ { featureType: "all", elementType: "all", stylers: [ { visibility: "off" }, // Hide everything { lightness: 100 } // Makes the land white ] }, { featureType: "water", elementType: "geometry", stylers: [ { visibility: "on" }, // Show water, but no labels { lightness: -9 }, // Must be < 0 to compensate for the "all" lightness

   [1]: http://nodemap.no.de
   [2]: http://nodejs.org (Nodejs Homepage)
   [3]: http://expressjs.com/ (Expressjs Web MVC)
   [4]: http://nowjs.com/ (Nowjs)
   [5]: https://github.com/visionmedia/ejs (EJS)
   [6]: http://code.google.com/apis/maps/documentation/javascript/ (Google Maps API v3)
   [7]: http://www.mongodb.org/ (Mongodb)
   [8]: https://github.com/vijayrawatsan/nodemap (Nodemap)
   [9]: http://expressjs.com/guide.html
   [10]: https://github.com/gett/mongojs/blob/master/README.md
   [11]: https://github.com/vijayrawatsan/nodemap/blob/master/views/index.ejs
   [12]: https://github.com/vijayrawatsan/nodemap/blob/master/views/layout.ejs

## Comments

**[professional photographer](#8014 "2012-03-23 18:26:13"):** **Recent Blogroll Additions…...** [...]usually posts some very interesting stuff like this. If you’re new to this site[...]......

**[Vijay Rawat](#8682 "2012-05-04 08:39:49"):** Hi Rajendra, This seems to be a installation problem.. Please have a look at : http://stackoverflow.com/questions/7754799/error-cannot-find-module-ejs

**[Rajendra](#8684 "2012-05-04 10:21:25"):** Thanx for the reply.. yes there as an installation problem with ejs. can we extend this code to share the map as well (i mean share the map with zoom out & zoom in features. If one user zooms in, it should be emitted to other users as well).

**[Vijay Rawat](#8685 "2012-05-04 10:36:39"):** Well I haven't tried that. You might need to check Google Maps API for that. :)

**[Shekhar Gulati](#7454 "2012-02-07 23:12:57"):** Very cool application. Liked it a lot. Will look forward to learn node.js from you. Thanks Shekhar

**[Vijay Rawat](#7460 "2012-02-08 09:01:26"):** Hi Shekhar, I will be giving an XKE on Nodejs very soon. Thanks Vijay

**[Kamal](#8489 "2012-04-19 02:35:39"):** Qalqi@Devadatta:~/Projects/JavaScript/nodeJS/vijayrawatsan-nodemap-6a26624$ node server.js Express server listening on port 80 { [Error: listen EACCES] code: 'EACCES', errno: 'EACCES', syscall: 'listen' } Error: listen EACCES at errnoException (net.js:670:11) at Array.1 (net.js:756:28) at EventEmitter._tickCallback (node.js:192:40) While running the server code I was getting this error. Would you please have alook at it,Vijay.

**[Vijay Rawat](#8494 "2012-04-19 09:41:35"):** This seems to be an access error. Assuming you are running on you personal machine. Try running on a different port(like 8000). It should work. If same error comes try running as a root.

**[Rajendra](#8674 "2012-05-03 23:22:11"):** I getting following error. node server.js Express server listening on port 80 warn - error raised: Error: listen EACCES can you pls help me to run this code vijay.

**[Rajendra](#8675 "2012-05-03 23:28:07"):** now i am able to run it on port 8000, still getting error related to express. Express 500 Error: Cannot find module 'ejs' at Function._resolveFilename (module.js:334:11) at Function._load (module.js:279:25) at Module.require (module.js:357:17) at require (module.js:368:17) at View.templateEngine (/Users/rmudale/nodetest/node_modules/express/lib/view/view.js:134:38) at Function.compile (/Users/rmudale/nodetest/node_modules/express/lib/view.js:68:17) at ServerResponse._render (/Users/rmudale/nodetest/node_modules/express/lib/view.js:417:18) at ServerResponse.render (/Users/rmudale/nodetest/node_modules/express/lib/view.js:318:17) at /Users/rmudale/nodetest/server.js:46:8 at /Users/rmudale/nodetest/node_modules/mongojs/node_modules/common/index.js:100:4

**[Anand George](#7500 "2012-02-09 08:20:38"):** Cool. Works like a charm. Thanks.

**[Kris](#9240 "2012-07-26 18:34:01"):** I see your code on github. Can you explain how you set layout page. I am not found them working when I am using Ejs

**[Vijay Rawat](#9248 "2012-07-27 11:03:54"):** Hi Kris, In layout.ejs the tag " <%- body %> " would be populated by contents of index.ejs Thanks

