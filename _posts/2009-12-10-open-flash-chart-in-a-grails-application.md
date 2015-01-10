---
layout: post
header-img: img/default-blog-pic.jpg
author: sunil.inteti
description: 
post_id: 2800
created: 2009/12/10 12:42:56
created_gmt: 2009/12/10 07:42:56
comment_status: open
---

# Open Flash Chart in a Grails Application

<p>In my Grails project we had a requirement that we needed to show graphs. Our client wanted to go for a opensource solution. We decided on the option <a href="http://teethgrinder.co.uk/open-flash-chart/" target="_blank">OpenFlashChart</a>. Grails comes up with its <a href="http://www.grails.org/Open+Flash+Chart+Plugin" target="_blank">plugin for OpenFlashChart</a>. With Open Flash Chart we can easily show barchart, line chart, piechart, Area charts etc. Let me explain very briefly how Open Flash Chart works. Basically in a web page we need to add this open flash chart which is the swf object. We should provide swf object with a data-file which contains the data it displays. This data could come from backend as well.  When the web page is rendered the swf object tries to get the data and render the graph.  Lets look at how we can use this in a Grails application.</p>
<!--more-->

<p>For example purpose <strong>I choose to show a barchart here which shows goals scored by Striker in a EPL season by name</strong>.  We need to add the chart to our view page. Taglibrary of grails is of a great help here especially if we are using the chart at various places in our application.So first let us write a Taglibrary <strong>OFCTaglibrary.groovy</strong> and add a closure chart. Ofcourse this is put under the Taglibrary folder for grails for convention purposes.</p>
<p>[sourcecode language="java"]
public class OFCTaglibrary {
  static namespace = &quot;grailsOfc&quot;
  def chart = { attrs -&gt;
  if (!attrs.name)
     throwTagError(&quot;Tag [chart] is missing required attribute [name]&quot;)
    String name = attrs.remove('name')
    String width = attrs.width ? attrs.remove(&quot;width&quot;) : &quot;350&quot;
    String height = attrs.height ? attrs.remove(&quot;height&quot;) : &quot;200&quot;
    String url = attrs.url? attrs.remove(&quot;url&quot;) : &quot;&quot;
    String flashFileName = &quot;open-flash-chart.swf&quot;
    out &lt;&lt; &quot;&lt;div id='$name' name='$name'&gt;&lt;/div&gt;\n&quot;
    out &lt;&lt; &quot;&quot;&quot;
    &lt;script type=&quot;text/javascript&quot;&gt;
    var params = {};
    params.wmode = &quot;transparent&quot;;
    swfobject.embedSWF(&quot;${request.contextPath}/${flashFileName}&quot;, &quot;$name&quot;, &quot;$width&quot;,&quot;$height&quot;,&quot;9.0.0&quot;, &quot;expressInstall.swf&quot;, {&quot;data-file&quot;:&quot;$url&quot;},params);&lt;/script&gt;
    &quot;&quot;&quot;
  }
}
[/sourcecode]</p>
<p>Now we have created a tag library which we can use in view  pages to insert the OFC chart .  I will expain the attributes  in the closure a little later.(rather i feel it is self explainatory ;) )</p>
<p>This is the piece of code that we should put in the view to show a OFC chart.</p>
<p>[sourcecode language="html"]
&lt;grailsOfc:chart name=&quot;barChart&quot; url=&quot;${createLink(controller:'stats',action: 'bar_graph')}&quot; width=&quot;${chartWidth}&quot; height=&quot;174&quot; /&gt;
[/sourcecode]</p>
<p>So when this page is rendered grails looks for the Taglibrary with namespace <strong>grailsOfc</strong>. In this Taglibrary it looks for the chart closure, and puts the code for the swf object in the view. The  <strong>open-flash-chart.swf </strong> looks for the data-file in the url</p>
<p>[sourcecode language="java"]${createLink(controller:'stats',action: 'bar_graph')}[/sourcecode]</p>
<p><strong><a href="http://www.grails.org/Tag+-+createLink" target="_blank">createLink</a></strong> is a GrailsTag which we can use to call an action on Grails Controller. The idea here is, we can put the logic to get the data from the server.
We shall now look at the StatsController code.</p>
<p>[sourcecode language="java"]
 def bar_graph = {
 def players = Player.findAll() //could be a more complex logic. This is just to get a filtered list of players</p>
<p>def x_axis_labels = []
 List values = []
 players.each {
   x_axis_labels.add( it.playerName)
   Map bar = [:]
   bar.put(&quot;top&quot;, it.goals())
   bar.put(&quot;colour&quot;, &quot;#0080F7&quot;)
   values.add(bar)
 }</p>
<p>String backgroundColour = &quot;#FFFFFF&quot;
 List elements = [];</p>
<p>def barChart = [ type: &quot;bar&quot;, values: values, tip: &quot;#val# Goals&quot;, colour: &quot;#E3E2DD&quot;, alpha:1 ]
 elements.add(barChart);</p>
<p>//    x Axis
 Map xAxis = ['tick-height': 0, colour: &quot;#74665D&quot;, stroke: 1, 'grid-colour': &quot;#FFFFFF&quot;,
               labels: [labels: x_axis_labels, visible:true]]</p>
<p>//y Axis
 Map yAxis = [ 'tick-height': 0, colour: &quot;#74665D&quot;, stroke: 1,'grid-colour': &quot;#E0DDD8&quot;, min: 0, max: 50, steps: 15,
                labels:[text: &quot;#val#&quot;]]</p>
<p>//assemble the chart
 HashMap goalChart = [:]</p>
<p>if (values.size() == 0) {
    goalChart['title'] = [text:&quot;noDataExists&quot;]
 }</p>
<p>goalChart = [ x_axis:xAxis, y_axis:yAxis, elements:elements, bg_colour:&quot;#FFFFFF&quot; ]</p>
<p>render goalChart as JSON; //grails provides converters to convert map in to JSON format.</p>
<p>}
[/sourcecode]</p>
<p>To summarize here we are building x-axis, y-axis and its labels, adding them to the chart which can be anything (bar or pie or any) in a map and convert this to JSON format.</p>
<p><strong>The graph would look like this :</strong>
<img src="http://xebee.xebia.in/wp-content/uploads/2009/12/graph.jpg" width="85%" height="85%"" alt="bar graph" />
<strong>Problem :</strong>
By this way we can show the Bar graph easily. How ever I faced a problem here.I couldn't get a scroll on x-axis inside the chart.The chart width was increasing. I couldn't find an option for the scroll for OFC chart. So I did a workaround. I put this OFC chart inside a &lt;div&gt; and and in the div i gave the style properties as : overflow-x:scroll; overflow-y:hidden; width : 550px;
This put the chart inside the div with a scroll if the chart widht is increased to more than 550px. I dont know if this option is there in OFC by now. But i used this workaround and client was OK with that. :)</p>
<p><strong>Conclusion :</strong>
Finally i feel if your client is not ready to pay for the charts, OFC is a great option. With this we can achieve all sorts of graphs.For more examples you can visit the <a href="http://teethgrinder.co.uk/open-flash-chart-2/" target="_blank">OFC site</a></p>

## Comments

**[RAAM](#5405 "2011-04-06 15:07:19"):** could u provide a sample example for line chart. that sample example shoild have labels in y axis and values like 0 to 100 should be in x axis.It will be very thankful if you provide the same asap.

