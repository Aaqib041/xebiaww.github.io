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

The last blog post, [Understanding Google Wave][1], discussed the architecture and technical underpinnings of **Google Wave**. In this post, we will look at different ways of developing with Google Wave.  There are three ways you can extend or use Google Wave in your applications.

**Embedding Wave**

You can embed a Wave into a web page by adding some simple JavaScript code. The Wave Embed API provides the [WavePanel][2] object which can hold a wave. You ask the WavePanel to use an HTML element on your web page to show a wave. The conversations on the wave will be visible in the WaveClient.

The steps to embed a wave on a web page are -

  1. Load the Embed API JavaScript - 
    
        
    

  2. Create an HTML element that will contain the embedded wave - 
    
        
    
    
    

  3. Initialize the WavePanel object - 
    
            function initialize() {
               var wavePanel = new WavePanel('http://wave.google.com/a/wavesandbox.com/');
                wavePanel.loadWave('wavesandbox.com!w' + waveID);
                wavePanel.init(document.getElementById('waveframe'));
            }
        ...
          
        ...
    

The wave is embedded in an iframe created inside the supplied HTML element. The argument passed to the WavePanel is the _Wave server instance_. This value is used to set up the URLs used in the iframe created. For the early developer access, it must be http://wave.google.com/a/wavesandbox.com/ (including the trailing slash).

**Extending Wave - Robots**

Robots are server side programs that can act as participants in a Wave. They can edit content, add users, extract content and post it to an external service thus acting as gateways between a Wave and an external service such as Twitter. A robot can respond to wave events such as wavelet_blip_created or wavelet_participants_changed. You can also specify a Cron schedule to ask the Wave to contact the robot at regular intervals.

You use a capabilities.xml file to specify what events a robot is interested in. An example from one of the samples is -
    
    
    
    
         
              
            
         
              
         
         
    
    

There are Python and Java libraries to help you write Robots.

In Java, you can extend [AbstractRobotServlet][3] class and implement [processEvents(RobotMessageBundle events)][4] to create a wave robot. This class translates the incoming Wave events in the form of HTTP requests into Java method calls, and communicates the changes made back to the wave.

You interact with the wave by using objects provided by the [Wave Robot API][5]. The following lines of code add a new "blip" to a wave - 
    
    
            Wavelet wavelet = events.getWavelet();
                    Blip blip = wavelet.appendBlip();
                    TextView textView = blip.getDocument();
                    textView.append("Hello World!");
    

It seems that the only way to deploy a robot at the moment is to deploy it to Google App Engine. Then you can add the robot to a wave using its Wave ID, which is its App Engine application ID followed by @appspot.com. This should change in near future and allow you to deploy the robot to any URL.

See the [Robots Tutorial][6] for a step by step guide.

**Extending Wave - Gadgets**

Gadgets are small programs that add functionality to the Google Wave client. They are written in JavaScript and HTML. Gadgets use the wave object defined in <http://wave-api.appspot.com/public/wave.js> file to interact with the wave they are part of. Each gadget has a state object, which is a a map of key-value pairs. You can implement callbacks that are called by the wave whenever the gadget state or the list of participant changes. The HTML and JavaScript making up the gadget are packaged as an XML file, and can be hosted anywhere on the internet. See the Wave [Gadgets tutorial][7] for example code for a gadget that allows you to run an auction in a wave.

**Resources** \- [Google Wave Robots: Creating a Robot][6] [Google Wave API samples][8] [Wave Gadgets Tutorial][7] [Wave Robot API][5] [Google Wave API forum][9]

   [1]: http://blog.xebia.com/2009/06/08/understanding-google-wave/
   [2]: http://code.google.com/apis/wave/embed/guide.html#WavePanel
   [3]: http://wave-robot-java-client.googlecode.com/svn/trunk/doc/com/google/wave/api/AbstractRobotServlet.html
   [4]: http://wave-robot-java-client.googlecode.com/svn/trunk/doc/com/google/wave/api/AbstractRobotServlet.html#processEvents(com.google.wave.api.RobotMessageBundle)
   [5]: http://wave-robot-java-client.googlecode.com/svn/trunk/doc/index.html
   [6]: http://code.google.com/apis/wave/extensions/robots/guide.html
   [7]: http://code.google.com/apis/wave/extensions/gadgets/guide.html
   [8]: http://code.google.com/apis/wave/samples/index.html
   [9]: http://groups.google.com/group/google-wave-api