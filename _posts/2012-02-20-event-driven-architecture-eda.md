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

We live in an event driven world. Companies, employees and even animals survive and thrive in this world on their ability to respond to any given opportunities and threats.

These opportunities and threats occur in more unpredictable manner then any economic theory or sixth sense can map or forecast them. So to handle them we need event based handlers that are externally determined. 

Have a look at the below picture:

[gallery columns="1"]

To give more sense to above picture:

**Event :** It can be any thing which happens in this world or even universe like in this picture a striking light/ football match/finding water on moon etc. etc.

**Event listener & generator :**This element will listen an event (as a listener) and thereafter creates and send a signal to the Event Engine (as a generator).

**Event Engine :** Upon receiving a signal this element will process the details and then send it to clients.

**Client :** They all will catch the event and interrupt in the manner they want.

Gear up your imagination with the example below:

Imagine your air flight is running late (Event for us, although not the pleasant one). Now what could be the action plan in perfect world:

Your booked cab can be notified for the same so that they can adjust their bookings by assigning a new cab to you and may be book your cab for someone else.

In the same manner hotel reservation can also be handled when notified about your air flight delay.

Even your travel agent can look for other alternative routes or any other flights which can be taken if you have tight schedule. He can even notify the people at seminar you suppose to attend for your delayed plan.

The most important thing to catch here is  _"when an air flight gets delayed, the airline doesn't know any of these above cases."_

Airline will just announce the delay or cancellation of a flight (which is a Event). The event will be announced to everything which says it's interested in that flight, which may be no one, a passenger or may be several agents for each passenger on the flight. The event listener & generator (cab, hotel and travel agent) decides how to react to the notification. Thus processing the notification by re assigning in case of cab or re booking in case of hotel (Event Engine). After that they will send the confirmation notification to clients.

**Magento** (CMS in PHP) uses EDA as its core of existence. Let's look how can we define an event and its affect in Magento.

Within Magento you can dispatch an event as simple as by calling a method like :

_BaseController_::dispatchEvent('custom_event', array('data1'=>$data));

This method accepts two parameters - event unique identifier and associative array of data passed to listener.

In Listener custom class, all the data dispatched by event will be captured and a listener would be added :

class Listener {     /__     _ function to take out the 'object' key from $data passed to Listener     _/     public static function addtoCart($data)     {         $object = $data->getEvent()->getObject();         $object->callPayPalEvent();     } }

In some cases, we need to process the request as per the event received.

public static function cashPaymentEvent($object) {     foreach($object->getEvent() as $event)     {         updateCart($event);     } }

The above Airline example is a simple case of Event Driven Architecture (EDA), it has more complex parts named as **Complex event processing (CEP)**

Complex event processing (CEP) in above cases could be like delay in flight can affect the airport management, crew allocation, opening of gates, passengers intimation, subsequent delay in on going journey of that flight.

If you notice that event (Air flight delay) which is fired resulted in some services. Thus a service is triggered by incoming events. Hence, making its way to **Service oriented architecture** (SOA).

So in today's IT world where we talk more about events and services, we can easily state that  **SOA 2.0** evolves with the implications of both SOA and EDA architectures.

Hopefully, in next blog we will discuss more about SOA and EDA as winning combination.

Reference : https://www.ibm.com/developerworks/wikis/display/woolf/Event-Driven+Architecture+Examples