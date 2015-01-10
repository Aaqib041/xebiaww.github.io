---
layout: post
header-img: img/default-blog-pic.jpg
author: anantsingh
description: 
post_id: 11515
created: 2012/02/20 12:22:14
created_gmt: 2012/02/20 07:22:14
comment_status: open
---

# Event Driven Architecture - EDA

<p>We live in an event driven world. Companies, employees and even animals survive and thrive in this world on their ability to respond to any given opportunities and threats.</p>
<p>These opportunities and threats occur in more unpredictable manner then any economic theory or sixth sense can map or forecast them. So to handle them we need event based handlers that are externally determined. <!--more--></p>
<p>Have a look at the below picture:</p>
<p>[gallery columns="1"]</p>
<p>To give more sense to above picture:</p>
<p><strong>Event :</strong> It can be any thing which happens in this world or even universe like in this picture a striking light/ football match/finding water on moon etc. etc.</p>
<p><strong>Event listener &amp; generator :</strong>This element will listen an event (as a listener) and thereafter creates and send a signal to the Event Engine (as a generator).</p>
<p><strong>Event Engine :</strong> Upon receiving a signal this element will process the details and then send it to clients.</p>
<p><strong>Client :</strong> They all will catch the event and interrupt in the manner they want.</p>
<p>Gear up your imagination with the example below:</p>
<p>Imagine your air flight is running late (Event for us, although not the pleasant one). Now what could be the action plan in perfect world:</p>
<p>Your booked cab can be notified for the same so that they can adjust their bookings by assigning a new cab to you and may be book your cab for someone else.</p>
<p>In the same manner hotel reservation can also be handled when notified about your air flight delay.</p>
<p>Even your travel agent can look for other alternative routes or any other flights which can be taken if you have tight schedule. He can even notify the people at seminar you suppose to attend for your delayed plan.</p>
<p>The most important thing to catch here is  <em>"when an air flight gets delayed, the airline doesn't know any of these above cases."</em></p>
<p>Airline will just announce the delay or cancellation of a flight (which is a Event). The event will be announced to everything which says it's interested in that flight, which may be no one, a passenger or may be several agents for each passenger on the flight. The event listener &amp; generator (cab, hotel and travel agent) decides how to react to the notification. Thus processing the notification by re assigning in case of cab or re booking in case of hotel (Event Engine). After that they will send the confirmation notification to clients.</p>
<p><strong>Magento</strong> (CMS in PHP) uses EDA as its core of existence. Let's look how can we define an event and its affect in Magento.</p>
<p>Within Magento you can dispatch an event as simple as by calling a method like :</p>
<p><em>BaseController</em>::dispatchEvent('custom_event', array('data1'=&gt;$data));</p>
<p>This method accepts two parameters - event unique identifier and associative array of data passed to listener.</p>
<p>In Listener custom class, all the data dispatched by event will be captured and a listener would be added :</p>
<p>class Listener
{
&nbsp;&nbsp;&nbsp;&nbsp;/<em><em>
&nbsp;&nbsp;&nbsp;&nbsp;</em> function to take out the 'object' key from $data passed to Listener
&nbsp;&nbsp;&nbsp;&nbsp;</em>/
&nbsp;&nbsp;&nbsp;&nbsp;public static function addtoCart($data)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$object = $data-&gt;getEvent()-&gt;getObject();
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$object-&gt;callPayPalEvent();
&nbsp;&nbsp;&nbsp;&nbsp;}
}</p>
<p>In some cases, we need to process the request as per the event received.</p>
<p>public static function cashPaymentEvent($object)
{
&nbsp;&nbsp;&nbsp;&nbsp;foreach($object-&gt;getEvent() as $event)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;updateCart($event);
&nbsp;&nbsp;&nbsp;&nbsp;}
}</p>
<p>The above Airline example is a simple case of Event Driven Architecture (EDA), it has more complex parts named as <strong>Complex event processing (CEP)</strong></p>
<p>Complex event processing (CEP) in above cases could be like delay in flight can affect the airport management, crew allocation, opening of gates, passengers intimation, subsequent delay in on going journey of that flight.</p>
<p>If you notice that event (Air flight delay) which is fired resulted in some services. Thus a service is triggered by incoming events. Hence, making its way to <strong>Service oriented architecture</strong> (SOA).</p>
<p>So in today's IT world where we talk more about events and services, we can easily state that  <strong>SOA 2.0</strong> evolves with the implications of both SOA and EDA architectures.</p>
<p>Hopefully, in next blog we will discuss more about SOA and EDA as winning combination.</p>
<p>Reference : https://www.ibm.com/developerworks/wikis/display/woolf/Event-Driven+Architecture+Examples</p>