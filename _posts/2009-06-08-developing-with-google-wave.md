---
layout: post
header-img: img/default-blog-pic.jpg
author: sonnygill
description: 
post_id: 1949
created: 2009/06/08 11:58:02
created_gmt: 2009/06/08 10:58:02
comment_status: open
---

# Developing with Google Wave

<p>The last blog post, <a href="http://blog.xebia.com/2009/06/08/understanding-google-wave/">Understanding Google Wave</a>, discussed the architecture and technical underpinnings of <strong>Google Wave</strong>. In this post, we will look at different ways of developing with Google Wave.
<!--more-->
There are three ways you can extend or use Google Wave in your applications.</p>
<p><strong>Embedding Wave</strong></p>
<p>You can embed a Wave into a web page by adding some simple JavaScript code.
The Wave Embed API provides the <a href="http://code.google.com/apis/wave/embed/guide.html#WavePanel">WavePanel</a> object which can hold a wave. You ask the WavePanel to use an HTML element on your web page to show a wave. The conversations on the wave will be visible in the WaveClient.</p>
<p>The steps to embed a wave on a web page are -</p>
<ol>
<li>
Load the Embed API JavaScript -
<pre lang="JavaScript">
<script src="http://wave-api.appspot.com/public/embed.js" type="text/javascript"></script>
</pre>
</li>
<li>
Create an HTML element that will contain the embedded wave -
<pre lang="html">
<div id="waveframe" style="width: 500px; height: 100%"></div>
</pre>
</li>
<li>
Initialize the WavePanel object -
<pre lang="JavaScript">
    function initialize() {
           var wavePanel = new WavePanel('http://wave.google.com/a/wavesandbox.com/');
            wavePanel.loadWave('wavesandbox.com!w' + waveID);
            wavePanel.init(document.getElementById('waveframe'));
        }
    ...
      <body onload="initialize()">
    ...
</pre>
</li>
</ol>

<p>The wave is embedded in an iframe created inside the supplied HTML element.
The argument passed to the WavePanel is the <em>Wave server instance</em>. This value is used to set up the URLs used in the iframe created. For the early developer access, it must be http://wave.google.com/a/wavesandbox.com/ (including the trailing slash).</p>
<p><strong>Extending Wave - Robots</strong></p>
<p>Robots are server side programs that can act as participants in a Wave. They can edit content, add users, extract content and post it to an external service thus acting as gateways between a Wave and an external service such as Twitter.
A robot can respond to wave events such as wavelet_blip_created or wavelet_participants_changed. You can also specify a Cron schedule to ask the Wave to contact the robot at regular intervals.</p>
<p>You use a capabilities.xml file to specify what events a robot is interested in. An example from one of the samples is -</p>
<pre lang="xml">
<?xml version="1.0"?>
<w:robot xmlns:w="http://wave.google.com/extensions/robots/1.0">
     <w:capabilities>
          <w:capability name="wavelet_participants_changed"/>
      </w:capabilities>  
     <w:crons>
          <w:cron path="/_wave/robot/fetchupdate" timerinseconds="10" />
     </w:crons>
     <w:profile name="stocky" imageurl="/images/dj.jpg" profileurl="/_wave/profile.xml" />
</w:robot>
</pre>

<p>There are Python and Java libraries to help you write Robots.</p>
<p>In Java, you can extend <a href="http://wave-robot-java-client.googlecode.com/svn/trunk/doc/com/google/wave/api/AbstractRobotServlet.html">AbstractRobotServlet</a> class and implement <a href="http://wave-robot-java-client.googlecode.com/svn/trunk/doc/com/google/wave/api/AbstractRobotServlet.html#processEvents(com.google.wave.api.RobotMessageBundle)">processEvents(RobotMessageBundle events)</a> to create a wave robot. This class translates the incoming Wave events in the form of HTTP requests into Java method calls, and communicates the changes made back to the wave.</p>
<p>You interact with the wave by using objects provided by the <a href="http://wave-robot-java-client.googlecode.com/svn/trunk/doc/index.html">Wave Robot API</a>.
The following lines of code add a new "blip" to a wave -
<pre lang="Java">
        Wavelet wavelet = events.getWavelet();
                Blip blip = wavelet.appendBlip();
                TextView textView = blip.getDocument();
                textView.append("Hello World!");
</pre></p>
<p>It seems that the only way to deploy a robot at the moment is to deploy it to Google App Engine. Then you can add the robot to a wave using its Wave ID, which is its App Engine application ID followed by @appspot.com. This should change in near future and allow you to deploy the robot to any URL.</p>
<p>See the <a href="http://code.google.com/apis/wave/extensions/robots/guide.html">Robots Tutorial</a> for a step by step guide.</p>
<p><strong>Extending Wave - Gadgets</strong></p>
<p>Gadgets are small programs that add functionality to the Google Wave client. They are written in JavaScript and HTML.
Gadgets use the wave object defined in <a href="http://wave-api.appspot.com/public/wave.js">http://wave-api.appspot.com/public/wave.js</a> file to interact with the wave they are part of. Each gadget has a state object, which is a a map of key-value pairs. You can implement callbacks that are called by the wave whenever the gadget state or the list of participant changes.
The HTML and JavaScript making up the gadget are packaged as an XML file, and can be hosted anywhere on the internet.
See the Wave <a href="http://code.google.com/apis/wave/extensions/gadgets/guide.html">Gadgets tutorial</a> for example code for a gadget that allows you to run an auction in a wave.</p>
<p>
<strong>Resources</strong> -

<a href="http://code.google.com/apis/wave/extensions/robots/guide.html">Google Wave Robots: Creating a Robot</a>
<a href="http://code.google.com/apis/wave/samples/index.html">Google Wave API samples</a>
<a href="http://code.google.com/apis/wave/extensions/gadgets/guide.html">Wave Gadgets Tutorial</a>
<a href="http://wave-robot-java-client.googlecode.com/svn/trunk/doc/index.html">Wave Robot API</a>
<a href="http://groups.google.com/group/google-wave-api">Google Wave API forum</a>
</p>