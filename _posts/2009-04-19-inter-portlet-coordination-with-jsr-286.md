---
layout: post
header-img: img/default-blog-pic.jpg
author: vgupta
description: 
post_id: 1087
created: 2009/04/19 16:06:28
created_gmt: 2009/04/19 15:06:28
comment_status: open
---

# Inter Portlet Coordination with JSR 286

<p style="text-align: left;">Coordination between portlets is a very common requirement. An example of information sharing between portlets can be a weather portlet displaying the weather information of a city and a map portlet displaying the location of the city. Since, both the portlets would be using the same zip code for a user, there should be mechanism provided by the portlal containers to allow portlets to share the zip code.</p>

<p>Prior to JSR 286, the support for inter portlet communication was rather minimal and information sharing between different portlets was accompalished primarily using application scoped session objects or vendor specific APIs. Both of above methods were rather problematic as in the former maintaining the uniqueness of the session attribute over a complex aaplication was a concern and in the later portability of the portlet was hampered.  In order to provide coordination between portlets the Java Portlet Specification v2.0 (JSR 286) introduces the following mechanisms:
<ol>
    <li>public render parameters in order to share render state between portlets.</li>
    <li>portlet events that a portlet can receive and send. </li>
</ol>
Let's have a look how to use the above features.
<!--more-->
<strong>Public Render Parameters</strong></p>
<p>In JSR 168, the render parameters propagated by one portlet were only available in the render method of the same portlet. This is explained in the following figure.
 <img class="alignnone size-full wp-image-1140" title="jsr-168-render" src="http://xebee.xebia.in/wp-content/uploads/2009/04/dia1.png" alt="jsr-168-render" width="604" height="399" /></p>
<p>In JSR 286, the render parameters propagated by one portlet can be made available to the render methods of other portlets. This is explained in the following figure.
<img class="alignnone size-full wp-image-1141" title="jsr-286-render" src="http://xebee.xebia.in/wp-content/uploads/2009/04/dia2.png" alt="jsr-286-render" width="604" height="398" /></p>
<p>In order to allow coordination of render parameters with other portlets, within the same portlet application or across portlet applications, in JSR 286 portlets, the portlet can declare public render parameters in its deployment descriptor using the public-render-parameter element in the portlet application section.  In the portlet section each portlet can specify the public render parameters it would like to share via the supported-public-render-parameter element. The supported public-ender-parameter element must reference the identifier of a public render parameter defined in the portlet application section in a public-render-parameter element. The portlet should use the defined public render parameter identifier in its code in order to access the public render parameter.</p>
<p>Let's see how it works..
Consider the following example..</p>
<ol>
<li>Set the public render parameters at the portlet application level.
<pre lang="xml"> <public-render-parameter>
    <identifier>id1</identifier>
    <qname xmlns:x="http://sun.com/params">x:param1</qname>
</public-render-parameter>
<public-render-parameter>
    <identifier>id2</identifier>
    <qname xmlns:x="http://sun.com/params">x:param2</qname>
 </public-render-parameter></pre></li>
<li>Specify the render parameter the portlet would like to share in the portlet section.
<pre lang="xml"> <portlet>
  <portlet-name>PortletA</portlet-name>
  <supported-public-render-parameter>id1</supported-public-render-parameter>
  <supported-public-render-parameter>id2</supported-public-render-parameter>
<portlet>
<portlet>
  <portlet-name>PortletB</portlet-name>
  <supported-public-render-parameter>id1</supported-public-render-parameter>
</portlet>
<portlet>
<portlet-name>PortletC</portlet-name>
  <supported-public-render-parameter>id2</supported-public-render-parameter>
<portlet></pre>
The public render paramters declared above are processed as explained in the figure below.
<img class="alignnone size-full wp-image-1146" title="parameter-sending" src="http://xebee.xebia.in/wp-content/uploads/2009/04/dia4.png" alt="parameter-sending" width="606" height="310" />
Now, since the public render parameters are encoded in the URL, the values that can be shared between portlets are restricted to String and String arrays. Since, public render parameters are available only in the render method, the information shared by the portlets should be used for rendering the view rather than for processing the shared information. </li>
</ol>
<p><strong>Portlet Events</strong>
Portlet events could be generated as a result of a user interaction with other portlets. The portlet event model is a loosely coupled, brokered model that allows creating portlets as stand-alone portlets that can be wired together with other portlets at runtime. Portlet programmers should therefore not make any specific assumptions about the environment of portlets they are running together with. The means of wiring different portlets together is portal implementation specific. An example where a portlet may want to offer receiving events is for state changes triggered by simple user interactions, e.g. adding an item to a shopping cart. By offering this as an event to other portlets these can trigger adding items to the shopping cart based on the user interactions happing inside these portlets.</p>
<p><em>EventPortlet Interface</em>
In order to receive events the portlet must implement the <em>EventPortlet</em> interface in he <em>javax.portlet</em> package. The portlet container will call the <em>processEvent</em> method for each event targeted to the portlet with an <em>EventRequest</em> and <em>EventResponse</em> object.</p>
<p>Events are targeted by the portal / portlet container to a specific portlet window in the current client request.  Events are a lifecycle operation that occurs before the rendering phase. The portlet may issue events via the setEvent method during the action processing which will be processed by the portlet container after the action processing has finished. As a result of issuing an event the portlet may optionally receive events from other portlets or container events. A portlet that is not target of a user action may optionally receive container events, e.g. a portlet mode changed event, or events from other portlets, e.g. an item was added to the shopping cart event. </p>
<p>The JSR 286 event processing is explained in the following figure.
<img class="alignnone size-full wp-image-1142" title="jsr-286-event-processing" src="http://xebee.xebia.in/wp-content/uploads/2009/04/dia32.png" alt="jsr-286-event-processing" width="659" height="399" /></p>
<p>To create portlets that use the eventing feature, follow these steps
1. Declare the events in the portlet.xml      
(i) Set the event definition at the portlet application level. This specifies the event name and the object type.
<div class="code panel">
<div class="codeContent panelContent">
<pre lang="xml"><portlet-app ...>
 <portlet>
 . . .
 . . .
 </portlet></p>
<p><event-definition>
   <qname xmlns:x="http:xebia.com/address">x:Address</qname>
   <value-type>com.xebia.Address</value-type>
 </event-definition>
</portlet-app></pre>
<pre lang="java">  @XmlRootElement
  public class Address implements Serializable {
      public Address() {
      }
      private String street;
      private String city;
      private String country;</p>
<pre><code>  //getters and setters
</code></pre>
<p>}</pre>
Note: The object must be serializable and must be instrumented with valid JAXB annotation. This might be required to ensure that portlets can send events to and receive events from remote portlets. However, in case of local communication, portal containers, for optimization purposes, might not serialize event payload.</div>
</div>
(ii) In the portlet section, specify the event name defined above for those portlets that want to publish this event.
<pre lang="xml"><portlet>
  <description>ContinentPortlet</description>
  <portlet-name>ContinentPortlet</portlet-name>
    .................
   <supported-publishing-event>
     <qname xmlns:x="http:xebia.com/address">x:Address</qname>
   </supported-publishing-event>
</portlet></pre></p>

## Comments

**[Sukdev](#8269 "2012-04-03 13:37:51"):** Its a fine example

