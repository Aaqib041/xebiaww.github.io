---
layout: post
header-img: img/default-blog-pic.jpg
author: sprasadjha
description: 
post_id: 7566
created: 2011/01/26 21:54:40
created_gmt: 2011/01/26 16:54:40
comment_status: open
---

# Why WebSocket is quicker

<p>There is a lot of buzz all around - 'WebSocket is quicker'. But why WebSocket is quicker? As you read this blog this question should get answered.
<h3>When we say quicker comparison is with?</h3>
When we say WebSocket is quicker the comparison is always with HTTP. So before we start discussing WebSocket protocol let us talk about HTTP.</p>
<!--more-->

<h3>HTTP is stateless</h3>

<p>In HTTP the server never remembers the interaction with a client. Every time a client sends a request to the server the client needs to introduce itself. HTTP has the request headers which helps clients send the so called introduction information to the server. Please note that this information has to be sent over the wire every time client wants to send a request to the server. Similarly when server sends the response back to the client it needs to send details about itself back to the client. HTTP protocol has response headers for this communication. But what is the reason behind this constant overhead.The reason is that the HTTP protocol is stateless.
<h3>WebSocket is a means of full duplex communication between peers (browser and server)</h3>
But the WebSocket protocol is not stateless. In WebSocket communication the client and server go through a process of handshake (to read more about handshake refer to my blog on 'Understanding WebSocket handshake'). Handshake helps the client and server get acquainted with each other. During handshake communicating peers (client and server) share information about each other, like mode(protocol) and language (sub-protocol) of communication, and all other details which are generally shared using the request and response headers of HTTP. Once the handshake is successfully executed a dedicated lane of communication gets established between the client and the server. Which means subsequent communication will not require any introduction information to be sent over the wire.
<h3>Let us understand the difference between HTTP and WebSocket communication using an example.</h3>
Suppose the server wants to send latest NAV of an asset to the client.
<h3>HTTP</h3>
In case of HTTP the client will send a request to the server requesting the NAV and the server will respond with the NAV.
<h4>HTTP Ajax Request</h4>
[sourcecode language="xml"]
Request Headersview source (this is an Ajax call).
Host    localhost:8080
User-Agent    Mozilla/5.0 (Windows; U; Windows NT 6.0; en-US; rv:1.9.2.13) Gecko/20101203 Firefox/3.6.13 (.NET CLR 3.5.30729)
Accept    <em>/</em>
Accept-Language    en,hi;q=0.7,sq;q=0.3
Accept-Encoding    gzip,deflate
Accept-Charset    ISO-8859-1,utf-8;q=0.7,*;q=0.7
Keep-Alive    115
Connection    keep-alive
Content-Type    application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With    XMLHttpRequest
Referer    http://localhost:8080/navApp/getNav.do
Content-Length    4858
Cookie    JSESSIONID=385DB76E1CC72C99ACFEE0DCCFD30576; UserInteraction7=KonaFlashBase
Pragma    no-cache
Cache-Control    no-cache&lt;/p&gt;
[/sourcecode]
<h4>HTTP Response</h4>
[sourcecode language="xml"]
Response Headersview source
Server    Apache-Coyote/1.1
Content-Type    text/html
Content-Length    2
Date    Sun, 23 Jan 2011 05:21:16 GMT
[/sourcecode]</p>
<p>The Actual Response</p>
<p>[sourcecode language="xml"]
21.22
[/sourcecode]
<h3>WebSocket</h3>
WebSocket protocol ensures two way communication(full duplex communication) which means client does not need to send request to the server to receive a response. If the server wants to send the latest NAV of an asset to the client it can do so at will, till the client and server have a connection established between them.
<h4>WebSocket Message</h4>
As compared to the request/response paradigm of HTTP, WebSocket uses a paradigm close to messaging. Any peer can send a message to other at will and the other party would keep listening to any such messages from the peer. To send a message the message needs to be wrapped in a frame. <strong>Each frame of data starts with a  frame type (0xFF byte in our case), followed by the number of bytes in the data, expressed as a big-endian 64 bit unsigned integer, followed by the UTF-8 data.</strong></p>
<p>Which means the extra information sent with the actual data are</p>
<ol>
<li>
<p>0xFF  and</p>
</li>
<li>
<p>number of bytes in the data.
<h4>WebSocket frame types</h4>
But what is 0xFF - In web socket communication the data can be sent using frames. All these frames have to start with a frame type. Frame type signifies the format of data packaged in the data frame. 0xFF frame type means the data is in UTF-8 text format. There is one more frame type 0x00, this frame type is used to close the WebSocket connection. Currently WebSocket protocol supports only two frame types 0xFF and 0x00. In future more frame types may be defined to support more data types.
<h3>Are the headers actually heavy?</h3>
If you compare the extra information in WebSocket with overhead involved in HTTP request/reponse paradigm, the amount of extra data is really less in WebSocket communication. But you may wonder why are we worried about the request and response headers so much? Does it really make any difference? If you look at the total data footprint of request and response it may come close to 1Kb. If the actual data to be communicated is of a few bytes and the rate of exchange of data is very high then the overhead of 1Kb for communicating a few bytes can cause real performance hit. That is why in all such scenarios WebSocket proves to be quicker.</p>
</li>
</ol>