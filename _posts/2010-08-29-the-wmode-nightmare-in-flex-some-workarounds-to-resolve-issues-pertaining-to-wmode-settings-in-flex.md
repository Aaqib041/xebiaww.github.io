---
layout: post
header-img: img/default-blog-pic.jpg
author: KaranNangru
description: 
post_id: 4620
created: 2010/08/29 23:14:30
created_gmt: 2010/08/29 18:14:30
comment_status: open
---

#  Flex: The wmode nightmare!! - Some workarounds

<!--        @page { margin: 2cm }       P { margin-bottom: 0.21cm } -->

<p>It is highly likely that if you have at any point of your development cycle fiddled around with the <em>feature </em><em>called </em>- <em>wmode</em>, then you have landed into some trouble or the other.</p>
<!--        @page { margin: 2cm }       P { margin-bottom: 0.21cm } -->

<p><!--        @page { margin: 2cm }       P { margin-bottom: 0.21cm } --> <!--        @page { margin: 2cm }       P { margin-bottom: 0.21cm } --><span style="font-size: medium;"><em><strong>Brief introduction to wmode:</strong></em></span></p>
<!--        @page { margin: 2cm }       P { margin-bottom: 0.21cm } -->

<p><em>'wmode'</em> stands for Windowless Mode. The wmode parameter is available in the object/embed tags used to place the Flash swf (compiled application) on stage:<!--more--></p>
<p>[java]
&lt;div id=&quot;mainContainer&quot; align=&quot;center&quot;&gt;
&lt;object classid=&quot;......&quot; id=&quot;Main&quot; width=&quot;969&quot; height=&quot;100%&quot; codebase=&quot;......&quot;&gt;
&lt;param name=&quot;movie&quot; value=&quot;#request....URL#&quot; /&gt;
&lt;param name=&quot;quality&quot; value=&quot;high&quot; /&gt;
&lt;param name=&quot;bgcolor&quot; value=&quot;##FFFFFF&quot; /&gt;
&lt;param name=&quot;allowScriptAccess&quot; value=&quot;sameDomain&quot; /&gt;
&lt;param name=&quot;wmode&quot; value=&quot;opaque&quot; /&gt;
&lt;embed src=&quot;#request....URL#&quot; quality=&quot;high&quot; bgcolor=&quot;##FFFFFF&quot; width=&quot;969&quot; height=&quot;100%&quot; name=&quot;Main&quot; align=&quot;middle&quot; play=&quot;true&quot; loop=&quot;false&quot; quality=&quot;low&quot;
allowScriptAccess=&quot;sameDomain&quot; type=&quot;application/x-shockwave-flash&quot; pluginspage=&quot;http://www.adobe.com/go/getflashplayer&quot; wmode=&quot;opaque&quot;&gt;
&lt;/embed&gt;
&lt;/object&gt;
&lt;/div&gt;
[/java]</p>
<p>There are three wmode's available with the Flash player across most browsers and platforms.
<ul>
    <li><strong>Window</strong>: Use the    Window value to play a Flash Player movie in its own rectangular    window on a web page. This is the default value for wmode and it    works the way the classic Flash Player works. This normally provides    the fastest animation performance.(If you do not notice a wmode     specified in your SWF wrapper, then the wmode value is Window)</li>
    <li><strong>Opaque</strong>: By using   the Opaque value you can use JavaScript to move or resize movies    that don't need a transparent background. Opaque mode makes the     movie hide everything behind it on the page. Additionally, opaque   mode moves elements behind Flash movies (for example, with dynamic  HTML) to prevent them from showing through.</li>
    <li><strong>Transparent</strong>: Transparent mode allows the   background of the HTML page, or the DHTML layer underneath the Flash    movie or layer, to show through all the transparent portions of the     movie. This allows you to overlap the movie with other elements of  the HTML page. Animation performance might be slower when you use   this value.</li>
</ul>
<!--        @page { margin: 2cm }       P { margin-bottom: 0.21cm } --><span style="font-size: medium;"><em><strong>wmode Issues:</strong></em></span></p>
<p><span style="font-size: small;"><em>wmode</em> bestows inconsistencies while using different browsers.</span></p>
<p><span style="font-size: small;">Here are a few issues:</span>
<ul>
    <li><span style="font-size: small;">When using a transition between states the resize effect flashes the final resize state before resizing (more apparent with wmode set to opaque or transparent)</span></li>
</ul>
<span style="font-size: small;">https://bugs.adobe.com/jira/browse/SDK-12421</span>
<ul>
    <li><span style="font-size: small;">When using wmode=opaque or transparent text input fields do not allow special characters </span></li>
</ul>
<span style="font-size: small;">https://bugs.adobe.com/jira/browse/SDK-12420</span>
<ul>
    <li><span style="font-size: small;"> Wheel mouse does not work in Firefox when wmode is set to opaque or transparent (Firefox)</span></li>
</ul>
<span style="font-size: small;">https://bugs.adobe.com/jira/browse/SDK-12415</span>
<ul>
    <li><span style="font-size: small;">Flex TextInput does not gain focus when cursor is in text field of iframe</span></li>
</ul>
<span style="font-size: small;">https://bugs.adobe.com/jira/browse/SDK-12377</span>
<ul>
    <li><span style="font-size: small;">Multiple SWF embeds, wmode enabled, fullscreen requested (FP 9.0.115 &gt;), on exit of fullscreen mode, out of focus flash movies will not be redrawn.(IE)</span></li>
</ul>
<ul>
    <li><span style="font-size: small;">SetInterval will be constrained to enterframe count with wmode enabled. Will cause redraw and performance issues.(IE)</span></li>
</ul>
<ul>
    <li><span style="font-size: small;">Fullscreen not available with FP 9.0.23 – FP 9.0.100 when wmode enabled.</span></li>
</ul>
<ul>
    <li><span style="font-size: small;">Animation sequences / Video will stutter and show empty frames (effect more prevalent in FF). Animated sequences of bitmaps show blank frames (memory issues), video will stutter or skip frames unnecessarily. Generally the issue is related to memory management – with wmode enabled all aspects of the scripting engine is throttled and swapping of memory is handled poorly.</span></li>
</ul>
<ul>
    <li><span style="font-size: small;">Text fields may be offset from their natural location during text entry if page is scrolled.</span></li>
</ul>
<ul>
    <li><span style="font-size: small;"> Text field tabbing will not function if wmode is enabled.</span></li>
</ul>
<!--        @page { margin: 2cm }       P { margin-bottom: 0.21cm } --><span style="font-size: small;"><span style="font-size: medium;"><em><strong>A major wmode bug and a workaround to avoid it:</strong></em></span> </span></p>
<p><span style="font-size: small;">It turns out to be a nightmare if while merging  your flex application into a coldfusion(or scripting) base application you need to switch the wmode to opaque or transparent to enable scripting pop up be visible above the flex layer. </span></p>
<p><span style="font-size: small;">I encountered a bug that switching wmode to opaque or transparent made my complete flex application go </span>weird<span style="font-size: small;">. Everytime I scrolled down the application, the complete screen used to distort. Every componenent rendered on the stage used to break and nothing was legible. So, in a way the complete effort of building the application was ruined by just </span>switching <span style="font-size: small;">from wmode = window to wmode = transparent. The UI seemed like a scrambled puzzle.</span></p>
<p><span style="font-size: small;">With </span>lilliputian<span style="font-size: small;"> help from Adobe, I had resort to the following :</span></p>
<!--        @page { margin: 2cm }       P { margin-bottom: 0.21cm } -->

<p><span style="text-decoration: underline;"><span style="font-size: small;"><em>Workaround for avoiding screen distortion on flex application scrolling when wmode set to opaque or transparent:</em></span></span></p>
<p><span style="font-size: small;">On carefully analysing the issue, I figured out that the application distorts everytime flex appplication level scrollbar is used. So I thought of a workaround wherein there would be no flex application level scrollbar. Thus, if there is no scrolling on flex application – the screen would not break.</span></p>
<p><span style="font-size: small;">Since the flex swf is wrapped in an html(or cfm) wrapper, I thought of adjusting the height of the browser everytime the height of the my flex swf changes. So this way, everytime there is a change in the height of the swf (stage), I would be sending the height to the underlying wrapper which would then set the height of the browser equivalent to the height of the swf.  Sounds confusing? Heres the gist of the logic:</span>
<ul><span style="font-size: small;">&gt;&gt;    Disable vertical scrolling of the flex application by overriding the    measure method at the application level and thereon adjusting the   browser height:</span><span style="font-size: small;">Write this code in the flex application class:</span><span style="font-size: small;">
</span></ul>
[java]</p>
<p>/**</p>

## Comments

**[Charanjit Singh](#441 "2010-09-02 13:40:07"):** Really nice and helpful article for flex user.

**[Sunil](#3122 "2010-11-03 02:00:04"):** I am trying to solve a problem with IE that you may find with this approach. Product uses wmode="transparent", I assume it is the same with "opaque". In either of these modes, keystrokes (like UP/DOWN arrows) are processed both by Flash and IE. So if the browser has scroll bars, and you are using keyboard navigation in the Flash app, pressing the arrow keys causes the browser to scroll the page. It's really annoying, and so far no way to stop it. wmode is evil and full of bugs and problems as you point out.

