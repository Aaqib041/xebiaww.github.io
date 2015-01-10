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

<div>

Online Games!!!

Be it a bear fishery, yeti sports, shooting gallery, battlefield or urban terror only thing that comes to mind is the powerful graphics and the animations that attract and bind the player to play. Now a days most of the lightweight online games are driven by flash leveraging the benefits of flash based technologies and other heavyweights fall under the category of offline or network based. <!--more-->

On a smaller scale, the basic premise of the gaming effects is to obtain image tranformation, image manipulation, animations, visual and audio effects. The effects are basically driven geometrically which could be amongst rotary, linear motion, circular motion, rectilinear or polygonic motion etc. The rotation includes circular or angular etc. These kinds of effects and motions, in two dimensions or three dimensions, can also be very easily obtained by using scripting language (jQuery) and CSS. There are other stuffs too that take care of storage and state management but in this blog I am only writing about how to get the frontend effects using jQuery.

</div>

<p>Developing an online game 2d or 3d is not that much difficult ever since <a href="http://jquery.com/">jquery</a> launched animate plugin in the API. The plugin not only helps in creating animation effects but also provides a platform where in one can introduce the added effects with the help of CSS. Most of animations like easing, moving, rotating, looping, delay, timeout intervals can be caused by .animate() function of jQuery object. Another important thing is to make good use of z-index of CSS which actually helps positioning of elements or images. Lastly, these effects are bound to the user events (mouse or keys) for the virtual reality experience.
<div></p>
<p>To start with let me show you how to write a <em>jQuery plugin</em>. The reason for writing plugin is to make the most out of the library and abstract the most clever and useful functions out into reusable code that can save time and make development even more efficient.</p>
<p>The code listing below shows the sample plugin skeleton.</p>
<p>[sourcecode language="java"]&lt;br /&gt;
(function( $ ){&lt;br /&gt;
  $.fn.myPlugin = function() {&lt;/p&gt;
&lt;p&gt;    // provide some logic this plugin does.&lt;/p&gt;
&lt;p&gt;  };&lt;br /&gt;
})( jQuery );&lt;/p&gt;
&lt;p&gt;[/sourcecode]</p>
<p>The listing below calls the myPlugin written above on the given element.</p>
<p>[sourcecode language="java"]&lt;br /&gt;
$('#element').myPlugin();&lt;br /&gt;
[/sourcecode]</p>
<p>Next code listing shows how to set width to all the div elements and setting color to blue.</p>
<p>[sourcecode language="java"]&lt;br /&gt;
(function( $ ){&lt;br /&gt;
  $.fn.setWidthDim = function( type ) {&lt;/p&gt;
&lt;p&gt;   return this.each(function() {&lt;/p&gt;
&lt;p&gt;       var $this = $(this);&lt;br /&gt;
      if ( !type || type == 'width' ) {&lt;br /&gt;
        $this.width( $this.width() );&lt;br /&gt;
      }&lt;br /&gt;
    });&lt;br /&gt;
  };&lt;br /&gt;
})( jQuery );&lt;br /&gt;
[/sourcecode]</p>
<p>Calling above plugin:</p>
<p>[sourcecode language="java"]&lt;br /&gt;
$('div').setWidthDim('width').css('color', 'blue');&lt;br /&gt;
[/sourcecode]</p>
<p>To understand more about the syntax and jQuery function object, refer the docs at <a href="http://docs.jquery.com/Plugins/Authoring">jQuery Plugin</a>.</p>
<p>Here, I intend to <em>rotate an image at an angle and move the image linearly in that direction</em> with some animation effects. This probably is fair enough start for the preparation of gaming engine as far as scripting is concerned. The animation effects could be chosen directly from .animate plugin or some open source resources which can add needed effects to it as per the requirements. In this example, the easing effect used is the default .animate easing feature, special easing functions could also be plugged-in though. The duration of the animation is the time in ms; higher the duration slower the speed and vice versa.</p>
<p><strong> </strong>
<div style="text-align: left; width: 700px; margin-left: 0px;"><strong>Image Movement with angular rotation using jQuery Plugin-AngularMove.js</strong></div>
<strong>Usage:</strong></p>
<p>$("#image").angularMove(parameters)</p>
<p><strong>Returns:</strong></p>
<p>jQueryAngularMoveElement</p>
<p>function returns angularMoveElement instance to help connect events with actually created 'rotation' element.</p>
<p><strong>Parameters:</strong></p>
<p>({</p>
<p>angleRotation: angleRotationValue,</p>
<p>move: moveValue,</p>
<p>duration: durationValue,</p>
<p>keepGoing: keepGoingValue,</p>
<p>easing: easingValue</p>
<p>})</p>
<p>jQuery(imgElement).angularMove</p>
<p><strong>Where:</strong></p>
<ul>
<li>angleRotationValue: clockwise rotation given in degrees (between -360 and 360, angle given more than 360 leads to only rotation</li>
</ul>
<p>but no linear motion),</p>
<ul>
<li>
<p>moveValue(boolean): boolean value for linear motion,</p>
</li>
<li>
<p>durationValue: A string or number determining duration of the animation {"slow", "fast", or number like 100},</p>
</li>
<li>
<p>keepGoingValue(boolean): boolean value for same animation effects to go infinitely,</p>
</li>
<li>
<p>easingValue: A string indicating which easing function to use for the transition {"linear", "swing"}</p>
</li>
</ul>
<p><strong>Example:</strong></p>
<p>[sourcecode language="java"]&lt;/p&gt;
&lt;p&gt;   1. Animation effects to go infinitely&lt;br /&gt;
        $(document).ready(function()&lt;br /&gt;
        {&lt;br /&gt;
            $(&quot;#image&quot;).angularMove({&lt;br /&gt;
                    angleRotation: -60,&lt;br /&gt;
                    move: true,&lt;br /&gt;
                    duration: 100,&lt;br /&gt;
                    keepGoing: true,&lt;br /&gt;
                    easing: 'linear'&lt;br /&gt;
            });&lt;br /&gt;
        });&lt;/p&gt;
&lt;p&gt;   2. Animation effects to go until duration as specified in parameter&lt;br /&gt;
        $(document).ready(function()&lt;br /&gt;
        {&lt;br /&gt;
            $(&quot;#image&quot;).angularMove({&lt;br /&gt;
                    angleRotation: -60,&lt;br /&gt;
                    move: true,&lt;br /&gt;
                    duration: 4000,&lt;br /&gt;
                    keepGoing: false,&lt;br /&gt;
                    easing: 'linear'&lt;br /&gt;
            });&lt;br /&gt;
        });&lt;/p&gt;
&lt;p&gt;[/sourcecode]</p>
<p><strong>Demo:</strong></p>
<p>The example shown below works fine on IE 8 though script is tested on IE 8 and Google Chrome.</p>
<p>The plugin does bind to key events to a certain extent. Controls for the manual movementare as follows:</p>
<p>Key- j : left, i: up and l: right</p>
<p>and switching from infinite to manual mode requires window refresh.</p>
<!--

/* <[CDATA[ */          html, body {                padding: 0;                 margin: 0;              height:710px;           }           img {               position:relative;              z-index:0;              width:auto;                 opacity: 1.00;          }           /* ]]