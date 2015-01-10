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

<p><em>Web Workers</em> defines an API that can be used for running scripts in background threads. In traditional browsers javascript was capable of running in a single thread, due to which it was difficult to run background tasks. Also the capabilities and performance of client side applications were limited. However, there are some asynchronous functions like "setTimeout" and "setInterval" which provide nice work around for running background tasks.
<!--more-->
I have divided this blog into two parts. First one explains the single threaded model of traditional browsers and the second one explains how to use <em>Web Workers</em>. You can skip the first section, if you have good hold on javascript.</p>
<p><strong>Single Threaded Model</strong>
The way javascript handles various asynchronous ajax calls or various timer functions, it makes us feel that javascript runs in multiple threads. Now, how does browser handle the UI events when it is executing a particular block of code, or how does browser handle the asynchronous timer and ajax calls when it just has a single thread for execution. The answer to all these questions lies in the way browser queues up various functions for execution.</p>
<p>Let us take an example of the following script</p>
<p>[sourcecode language="javascript"]
function init(){
    takes 5 ms to be executed
    mouseClickEvent occurs
    takes 5 ms
    setInterval(timerTask,&quot;10&quot;);
    takes 5 ms
}</p>
<p>function handleMouseClick(){
      takes 8 ms to be executed
}</p>
<p>function timerTask(){
      takes 2 ms to be executed
}
[/sourcecode]</p>
<p>Figure below shows the execution of the above script. Please pay extra attention to this figure as it contains a lot of information
<a href="http://xebee.xebia.in/wp-content/uploads/2010/10/web-workers1.png"><img class="alignleft size-full wp-image-5909" title="web workers" src="http://xebee.xebia.in/wp-content/uploads/2010/10/web-workers1.png" alt="" width="536" height="370" /></a></p>
<p><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
Let us say that at 0 ms <em>init()</em> function starts executing and during the execution of <em>init()</em> function, a mouse click event occurs at 5ms. Now, as the main thread is already executing the <em>init()</em> function and is capable of executing just a single block of code at a time, it queues up <em>handleMouseClick()</em> function next in the execution queue. At  10 ms <em>init()</em> function sets a timer which will execute every 10 ms. First execution of the timer task is scheduled at 20 ms, second at 30 ms and so on. The execution of <em>init()</em> function takes 15 ms and after its completion, <em>handleMouseClick()</em> being next in the queue starts executing i.e. starts executing at 15ms.  Execution of <em>handleMouseClick()</em> function takes 8 ms i.e. it completes at 23 ms and as <em>timerTask()</em> function was scheduled to run at 20ms, <em>timerTask()</em> function gets queued up in execution queue and is executed after the <em>handleMouseClick()</em> completes. Since there is no other function present in the execution queue that need to be run, <em>timerTask()</em> function runs at its scheduled time i.e. 30 ms , 40 ms and so on.</p>
<p><strong>Web Workers</strong>
<em>Web Workers</em> allows you to load your script dynamically and run it in background thread. Creating a worker is simple. All you have to do is,  call the constructor and pass the URI of the script you want to execute as an argument.  Workers are relatively heavy weight and are not intended to be used in large number.</p>
<p>[sourcecode language="javascript"]
// Creates a Web Worker
 var worker = new Worker(&quot;worker.js&quot;);
[/sourcecode]</p>
<p>Workers don't have access to the DOM i.e. you don't have direct access to the 'parent' page. However, Web Workers API do provide you with methods like "onmessage" and "postmessage"  which let you communicate with the main thread.
<ol>
    <li> <strong>onmessage</strong> :  method used for receiving messages sent from the worker, also can be used in worker for  receiving messages sent from main thread.</li>
    <li><strong>postmessage</strong> : method used for sending messages from worker to the main thread, also can be used in main thread for sending messages to worker.</li>
</ol>
Lets take an example and see how exactly these methods can be used for communicating through messages</p>
<p>Here is the main script that created a new worker. In worker's <em>onmessage</em> event handler, we log all the messages received from the worker, i.e. this event handler is called when <em>postmessage()</em> function is called from the worker. This script also sets a timer, which calls the <em>sendMsgToWorker()</em> function every 5 sec.</p>
<p>[sourcecode language="javascript"]</p>
<p>var worker = new Worker('worker.js');
worker.onmessage = function(e){
    console.log(e.data);
}
function sendMsgToWorker(){
    worker.postMessage(&quot;Sent  Message  From main &quot;);
}
setInterval(sendMsgToWorker,&quot;5000&quot;);</p>
<p>&amp;lt;/script&amp;gt;
[/sourcecode]</p>
<p>Lets have a look at  "worker.js" script.</p>
<p>[sourcecode language="javascript"]
onmessage = function(event) {
     var message = &quot;In worker on message method  :&quot; + event.data;
     postMessage(message);
};
function sendMsgToMain(){
     postMessage(&quot;Sent  Message From worker&quot;);
}
// Call sendMsgToMain method again after 5seconds
setInterval(sendMsgToMain,&quot;5000&quot;);
[/sourcecode]</p>
<p>When we load the page containing the main script, the logs shown on the browser console are as follows</p>
<p>[sourcecode language="text"]</p>
<p>In worker on message method : Sent Message From main
Sent Message From worker
In worker on message method : Sent Message From main
Sent Message From worker
In worker on message method : Sent Message From main
Sent Message From worker
............</p>
<p>[/sourcecode]</p>
<p>In this example, the main thread starts a new thread and instantiates a timer which calls <em>sendMsgToWorker()</em> function every 5 seconds. <em>sendMsgToWorker()</em> function sends a message i.e. <em>"Sent Message From main"</em> to worker by calling <em>postmessage</em> function on worker object. The worker receives this message in the <em>onmessage</em> callback function(in "worker.js" script) which prefixes the message with <em>"In worker on message method :"</em> and sends it back to the main thread using <em>postMessage()</em> function of worker API. Also, worker thread instantiates a timer which calls <em>sendMsgToMain()</em> function every 5 seconds. <em>sendMsgToMain()</em> sends a message i.e. <em>"Sent Message From worker"</em> to the main thread using <em>postmessage()</em> function. Messages sent by calling <em>postmessage</em> in 'worker.js' are received by <em>worker.onmessage</em> callback function(in main script) and are then logged to the console.</p>
<p>So, now there are two threads that run forever. First one is the main thread that receives messages sent from worker and logs them to the console and also sends a message every 5 seconds to the worker. Second thread, is the worker thread which sends the received messages from the main thread, back to the main thread and also sends a message every 5 seconds.</p>
<p>At last I would like to mention the objects and functions that can be used inside worker</p>
<p>Functions that can be called from Web Workers are
<ul>
    <li><strong>importScripts()</strong>: used to load external Javascript for use in the worker</li>
    <li><strong>setTimeout()</strong> and <strong>setInterval()</strong>: used for setting timer tasks</li>
    <li><strong>close()</strong>: stops the worker immediately</li>
</ul>
Objects that are available in Web Workers are
<ul>
    <li>navigator</li>
    <li>location</li>
    <li>All javascript objects, like Array, Date, etc</li>
    <li>XMLHttpRequest</li>
</ul></p>

## Comments

**[anehra63](#3205 "2010-11-11 16:35:43"):** nice post bro again thanks

**[Ivan Zuzak](#3126 "2010-11-03 13:06:28"):** Nice post, Robin! You should check out Metaworker – “A javascript work parallelizer/distributor library for both HTML5 web workers and server-side nodejs” – http://goo.gl/Sd8fh. I’ve also played with workers and created the pmrpc library – a library for RPC-style communication with web workers (and iframes/windows) – http://goo.gl/R9pa.

