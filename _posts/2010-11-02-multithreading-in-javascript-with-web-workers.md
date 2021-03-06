---
layout: post
header-img: img/default-blog-pic.jpg
author: rnagpal
description: 
post_id: 5529
created: 2010/11/02 11:32:42
created_gmt: 2010/11/02 06:32:42
comment_status: open
---

# HTML5 - Multithreading in JavaScript with Web Workers

_Web Workers_ defines an API that can be used for running scripts in background threads. In traditional browsers javascript was capable of running in a single thread, due to which it was difficult to run background tasks. Also the capabilities and performance of client side applications were limited. However, there are some asynchronous functions like "setTimeout" and "setInterval" which provide nice work around for running background tasks.  I have divided this blog into two parts. First one explains the single threaded model of traditional browsers and the second one explains how to use _Web Workers_. You can skip the first section, if you have good hold on javascript.

**Single Threaded Model** The way javascript handles various asynchronous ajax calls or various timer functions, it makes us feel that javascript runs in multiple threads. Now, how does browser handle the UI events when it is executing a particular block of code, or how does browser handle the asynchronous timer and ajax calls when it just has a single thread for execution. The answer to all these questions lies in the way browser queues up various functions for execution.

Let us take an example of the following script

[sourcecode language="javascript"] function init(){ takes 5 ms to be executed mouseClickEvent occurs takes 5 ms setInterval(timerTask,"10"); takes 5 ms }

function handleMouseClick(){ takes 8 ms to be executed }

function timerTask(){ takes 2 ms to be executed } [/sourcecode]

Figure below shows the execution of the above script. Please pay extra attention to this figure as it contains a lot of information ![][1]

  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
Let us say that at 0 ms _init()_ function starts executing and during the execution of _init()_ function, a mouse click event occurs at 5ms. Now, as the main thread is already executing the _init()_ function and is capable of executing just a single block of code at a time, it queues up _handleMouseClick()_ function next in the execution queue. At 10 ms _init()_ function sets a timer which will execute every 10 ms. First execution of the timer task is scheduled at 20 ms, second at 30 ms and so on. The execution of _init()_ function takes 15 ms and after its completion, _handleMouseClick()_ being next in the queue starts executing i.e. starts executing at 15ms. Execution of _handleMouseClick()_ function takes 8 ms i.e. it completes at 23 ms and as _timerTask()_ function was scheduled to run at 20ms, _timerTask()_ function gets queued up in execution queue and is executed after the _handleMouseClick()_ completes. Since there is no other function present in the execution queue that need to be run, _timerTask()_ function runs at its scheduled time i.e. 30 ms , 40 ms and so on.

**Web Workers** _Web Workers_ allows you to load your script dynamically and run it in background thread. Creating a worker is simple. All you have to do is, call the constructor and pass the URI of the script you want to execute as an argument. Workers are relatively heavy weight and are not intended to be used in large number.

[sourcecode language="javascript"] // Creates a Web Worker var worker = new Worker("worker.js"); [/sourcecode]

Workers don't have access to the DOM i.e. you don't have direct access to the 'parent' page. However, Web Workers API do provide you with methods like "onmessage" and "postmessage" which let you communicate with the main thread. 

  1. **onmessage** : method used for receiving messages sent from the worker, also can be used in worker for receiving messages sent from main thread.
  2. **postmessage** : method used for sending messages from worker to the main thread, also can be used in main thread for sending messages to worker.
Lets take an example and see how exactly these methods can be used for communicating through messages

Here is the main script that created a new worker. In worker's _onmessage_ event handler, we log all the messages received from the worker, i.e. this event handler is called when _postmessage()_ function is called from the worker. This script also sets a timer, which calls the _sendMsgToWorker()_ function every 5 sec.

[sourcecode language="javascript"]

var worker = new Worker('worker.js'); worker.onmessage = function(e){ console.log(e.data); } function sendMsgToWorker(){ worker.postMessage("Sent Message From main "); } setInterval(sendMsgToWorker,"5000");

&lt;/script&gt; [/sourcecode]

Lets have a look at "worker.js" script.

[sourcecode language="javascript"] onmessage = function(event) { var message = "In worker on message method :" \+ event.data; postMessage(message); }; function sendMsgToMain(){ postMessage("Sent Message From worker"); } // Call sendMsgToMain method again after 5seconds setInterval(sendMsgToMain,"5000"); [/sourcecode]

When we load the page containing the main script, the logs shown on the browser console are as follows

[sourcecode language="text"]

In worker on message method : Sent Message From main Sent Message From worker In worker on message method : Sent Message From main Sent Message From worker In worker on message method : Sent Message From main Sent Message From worker ............

[/sourcecode]

In this example, the main thread starts a new thread and instantiates a timer which calls _sendMsgToWorker()_ function every 5 seconds. _sendMsgToWorker()_ function sends a message i.e. _"Sent Message From main"_ to worker by calling _postmessage_ function on worker object. The worker receives this message in the _onmessage_ callback function(in "worker.js" script) which prefixes the message with _"In worker on message method :"_ and sends it back to the main thread using _postMessage()_ function of worker API. Also, worker thread instantiates a timer which calls _sendMsgToMain()_ function every 5 seconds. _sendMsgToMain()_ sends a message i.e. _"Sent Message From worker"_ to the main thread using _postmessage()_ function. Messages sent by calling _postmessage_ in 'worker.js' are received by _worker.onmessage_ callback function(in main script) and are then logged to the console.

So, now there are two threads that run forever. First one is the main thread that receives messages sent from worker and logs them to the console and also sends a message every 5 seconds to the worker. Second thread, is the worker thread which sends the received messages from the main thread, back to the main thread and also sends a message every 5 seconds.

At last I would like to mention the objects and functions that can be used inside worker

Functions that can be called from Web Workers are 

  * **importScripts()**: used to load external Javascript for use in the worker
  * **setTimeout()** and **setInterval()**: used for setting timer tasks
  * **close()**: stops the worker immediately
Objects that are available in Web Workers are 
  * navigator
  * location
  * All javascript objects, like Array, Date, etc
  * XMLHttpRequest

   [1]: http://xebee.xebia.in/wp-content/uploads/2010/10/web-workers1.png (web workers)

## Comments

**[anehra63](#3205 "2010-11-11 16:35:43"):** nice post bro again thanks

**[Ivan Zuzak](#3126 "2010-11-03 13:06:28"):** Nice post, Robin! You should check out Metaworker – “A javascript work parallelizer/distributor library for both HTML5 web workers and server-side nodejs” – http://goo.gl/Sd8fh. I’ve also played with workers and created the pmrpc library – a library for RPC-style communication with web workers (and iframes/windows) – http://goo.gl/R9pa.

