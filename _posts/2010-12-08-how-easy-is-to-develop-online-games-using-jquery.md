---
layout: post
header-img: img/default-blog-pic.jpg
author: jdhingra
description: 
post_id: 6377
created: 2010/12/08 17:30:02
created_gmt: 2010/12/08 12:30:02
comment_status: open
---

# How easy is to develop online games using jQuery!

Online Games!!! Be it a bear fishery, yeti sports, shooting gallery, battlefield or urban terror only thing that comes to mind is the powerful graphics and the animations that attract and bind the player to play. Now a days most of the lightweight online games are driven by flash leveraging the benefits of flash based technologies and other heavyweights fall under the category of offline or network based.  On a smaller scale, the basic premise of the gaming effects is to obtain image tranformation, image manipulation, animations, visual and audio effects. The effects are basically driven geometrically which could be amongst rotary, linear motion, circular motion, rectilinear or polygonic motion etc. The rotation includes circular or angular etc. These kinds of effects and motions, in two dimensions or three dimensions, can also be very easily obtained by using scripting language (jQuery) and CSS. There are other stuffs too that take care of storage and state management but in this blog I am only writing about how to get the frontend effects using jQuery. 

Developing an online game 2d or 3d is not that much difficult ever since [jquery][1] launched animate plugin in the API. The plugin not only helps in creating animation effects but also provides a platform where in one can introduce the added effects with the help of CSS. Most of animations like easing, moving, rotating, looping, delay, timeout intervals can be caused by .animate() function of jQuery object. Another important thing is to make good use of z-index of CSS which actually helps positioning of elements or images. Lastly, these effects are bound to the user events (mouse or keys) for the virtual reality experience. 

To start with let me show you how to write a _jQuery plugin_. The reason for writing plugin is to make the most out of the library and abstract the most clever and useful functions out into reusable code that can save time and make development even more efficient.

The code listing below shows the sample plugin skeleton.

[sourcecode language="java"]<br /> (function( $ ){<br /> $.fn.myPlugin = function() {</p> <p> // provide some logic this plugin does.</p> <p> };<br /> })( jQuery );</p> <p>[/sourcecode]

The listing below calls the myPlugin written above on the given element.

[sourcecode language="java"]<br /> $('#element').myPlugin();<br /> [/sourcecode]

Next code listing shows how to set width to all the div elements and setting color to blue.

[sourcecode language="java"]<br /> (function( $ ){<br /> $.fn.setWidthDim = function( type ) {</p> <p> return this.each(function() {</p> <p> var $this = $(this);<br /> if ( !type || type == 'width' ) {<br /> $this.width( $this.width() );<br /> }<br /> });<br /> };<br /> })( jQuery );<br /> [/sourcecode]

Calling above plugin:

[sourcecode language="java"]<br /> $('div').setWidthDim('width').css('color', 'blue');<br /> [/sourcecode]

To understand more about the syntax and jQuery function object, refer the docs at [jQuery Plugin][2].

Here, I intend to _rotate an image at an angle and move the image linearly in that direction_ with some animation effects. This probably is fair enough start for the preparation of gaming engine as far as scripting is concerned. The animation effects could be chosen directly from .animate plugin or some open source resources which can add needed effects to it as per the requirements. In this example, the easing effect used is the default .animate easing feature, special easing functions could also be plugged-in though. The duration of the animation is the time in ms; higher the duration slower the speed and vice versa.

** **

**Image Movement with angular rotation using jQuery Plugin-AngularMove.js**

**Usage:**

$("#image").angularMove(parameters)

**Returns:**

jQueryAngularMoveElement

function returns angularMoveElement instance to help connect events with actually created 'rotation' element.

**Parameters:**

({

angleRotation: angleRotationValue,

move: moveValue,

duration: durationValue,

keepGoing: keepGoingValue,

easing: easingValue

})

jQuery(imgElement).angularMove

**Where:**

  * angleRotationValue: clockwise rotation given in degrees (between -360 and 360, angle given more than 360 leads to only rotation

but no linear motion),

  * moveValue(boolean): boolean value for linear motion,

  * durationValue: A string or number determining duration of the animation {"slow", "fast", or number like 100},

  * keepGoingValue(boolean): boolean value for same animation effects to go infinitely,

  * easingValue: A string indicating which easing function to use for the transition {"linear", "swing"}

**Example:**

[sourcecode language="java"]</p> <p> 1\. Animation effects to go infinitely<br /> $(document).ready(function()<br /> {<br /> $("#image").angularMove({<br /> angleRotation: -60,<br /> move: true,<br /> duration: 100,<br /> keepGoing: true,<br /> easing: 'linear'<br /> });<br /> });</p> <p> 2\. Animation effects to go until duration as specified in parameter<br /> $(document).ready(function()<br /> {<br /> $("#image").angularMove({<br /> angleRotation: -60,<br /> move: true,<br /> duration: 4000,<br /> keepGoing: false,<br /> easing: 'linear'<br /> });<br /> });</p> <p>[/sourcecode]

**Demo:**

The example shown below works fine on IE 8 though script is tested on IE 8 and Google Chrome.

The plugin does bind to key events to a certain extent. Controls for the manual movementare as follows:

Key- j : left, i: up and l: right

and switching from infinite to manual mode requires window refresh.

<!-- /* <[CDATA[ */ html, body { padding: 0; margin: 0; height:710px; } img { position:relative; z-index:0; width:auto; opacity: 1.00; } /* ]]

   [1]: http://jquery.com/
   [2]: http://docs.jquery.com/Plugins/Authoring