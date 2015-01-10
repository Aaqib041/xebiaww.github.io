---
layout: post
header-img: img/default-blog-pic.jpg
---

#  Flex: The wmode nightmare!! - Some workarounds

It is highly likely that if you have at any point of your development cycle fiddled around with the _feature __called _\- _wmode_, then you have landed into some trouble or the other.  _**Brief introduction to wmode:**_ _'wmode'_ stands for Windowless Mode. The wmode parameter is available in the object/embed tags used to place the Flash swf (compiled application) on stage: [java] <div id="mainContainer" align="center"> <object classid="......" id="Main" width="969" height="100%" codebase="......"> <param name="movie" value="#request....URL#" /> <param name="quality" value="high" /> <param name="bgcolor" value="##FFFFFF" /> <param name="allowScriptAccess" value="sameDomain" /> <param name="wmode" value="opaque" /> <embed src="#request....URL#" quality="high" bgcolor="##FFFFFF" width="969" height="100%" name="Main" align="middle" play="true" loop="false" quality="low" allowScriptAccess="sameDomain" type="application/x-shockwave-flash" pluginspage="http://www.adobe.com/go/getflashplayer" wmode="opaque"> </embed> </object> </div> [/java] There are three wmode's available with the Flash player across most browsers and platforms. 

  * **Window**: Use the Window value to play a Flash Player movie in its own rectangular window on a web page. This is the default value for wmode and it works the way the classic Flash Player works. This normally provides the fastest animation performance.(If you do not notice a wmode specified in your SWF wrapper, then the wmode value is Window)
  * **Opaque**: By using the Opaque value you can use JavaScript to move or resize movies that don't need a transparent background. Opaque mode makes the movie hide everything behind it on the page. Additionally, opaque mode moves elements behind Flash movies (for example, with dynamic HTML) to prevent them from showing through.
  * **Transparent**: Transparent mode allows the background of the HTML page, or the DHTML layer underneath the Flash movie or layer, to show through all the transparent portions of the movie. This allows you to overlap the movie with other elements of the HTML page. Animation performance might be slower when you use this value.
_**wmode Issues:**_ _wmode_ bestows inconsistencies while using different browsers. Here are a few issues:

  * When using a transition between states the resize effect flashes the final resize state before resizing (more apparent with wmode set to opaque or transparent)
https://bugs.adobe.com/jira/browse/SDK-12421

  * When using wmode=opaque or transparent text input fields do not allow special characters 
https://bugs.adobe.com/jira/browse/SDK-12420

  * Wheel mouse does not work in Firefox when wmode is set to opaque or transparent (Firefox)
https://bugs.adobe.com/jira/browse/SDK-12415

  * Flex TextInput does not gain focus when cursor is in text field of iframe
https://bugs.adobe.com/jira/browse/SDK-12377

  * Multiple SWF embeds, wmode enabled, fullscreen requested (FP 9.0.115 >), on exit of fullscreen mode, out of focus flash movies will not be redrawn.(IE)
  * SetInterval will be constrained to enterframe count with wmode enabled. Will cause redraw and performance issues.(IE)
  * Fullscreen not available with FP 9.0.23 – FP 9.0.100 when wmode enabled.
  * Animation sequences / Video will stutter and show empty frames (effect more prevalent in FF). Animated sequences of bitmaps show blank frames (memory issues), video will stutter or skip frames unnecessarily. Generally the issue is related to memory management – with wmode enabled all aspects of the scripting engine is throttled and swapping of memory is handled poorly.
  * Text fields may be offset from their natural location during text entry if page is scrolled.
  * Text field tabbing will not function if wmode is enabled.
_**A major wmode bug and a workaround to avoid it:**_ It turns out to be a nightmare if while merging your flex application into a coldfusion(or scripting) base application you need to switch the wmode to opaque or transparent to enable scripting pop up be visible above the flex layer.  I encountered a bug that switching wmode to opaque or transparent made my complete flex application go weird. Everytime I scrolled down the application, the complete screen used to distort. Every componenent rendered on the stage used to break and nothing was legible. So, in a way the complete effort of building the application was ruined by just switching from wmode = window to wmode = transparent. The UI seemed like a scrambled puzzle. With lilliputian help from Adobe, I had resort to the following : _Workaround for avoiding screen distortion on flex application scrolling when wmode set to opaque or transparent:_ On carefully analysing the issue, I figured out that the application distorts everytime flex appplication level scrollbar is used. So I thought of a workaround wherein there would be no flex application level scrollbar. Thus, if there is no scrolling on flex application – the screen would not break. Since the flex swf is wrapped in an html(or cfm) wrapper, I thought of adjusting the height of the browser everytime the height of the my flex swf changes. So this way, everytime there is a change in the height of the swf (stage), I would be sending the height to the underlying wrapper which would then set the height of the browser equivalent to the height of the swf. Sounds confusing? Heres the gist of the logic:

>> Disable vertical scrolling of the flex application by overriding the measure method at the application level and thereon adjusting the browser height:Write this code in the flex application class: [java] /**

## Comments

**[Charanjit Singh](#441 "2010-09-02 13:40:07"):** Really nice and helpful article for flex user.

**[Sunil](#3122 "2010-11-03 02:00:04"):** I am trying to solve a problem with IE that you may find with this approach. Product uses wmode="transparent", I assume it is the same with "opaque". In either of these modes, keystrokes (like UP/DOWN arrows) are processed both by Flash and IE. So if the browser has scroll bars, and you are using keyboard navigation in the Flash app, pressing the arrow keys causes the browser to scroll the page. It's really annoying, and so far no way to stop it. wmode is evil and full of bugs and problems as you point out.

