---
layout: post
header-img: img/default-blog-pic.jpg
author: sprasadjha
description: 
post_id: 6732
created: 2010/12/30 22:25:57
created_gmt: 2010/12/30 17:25:57
comment_status: open
---

# Understanding WebSocket handshake

In this blog I will discuss in detail the process of handshake in WebSocket Protocol. The first  step in WebSocket communication is the formal handshake between the client and the server. The discussion is based on the latest [specification][1] released on 16th August 2010 by I. Hickson. I will deal in detail the request and response headers and also discuss the why aspect.

If reader is more interested in getting started with WebSocket you may read the blog -[ Web Socket Communication Using  jWebSocket][2]. 

### 

### Introduction to Web Socket

The ability to push data from server to client has been a long standing requirement for developing certain web based applications. HTTP protocol requires the client to send a request in order  to receive a response from server. To overcome this short coming few workarounds like polling, long polling and comet based solutions are commonly used. WebSocket protocol addresses this issue and provides a standard means of seamless communication between the client and server. Asynchronous requests can be sent by both client and server to each other. Clients have callback methods and servers can have listeners to listen to events from each other. This mode of communication is commonly referred to as full duplex communication and since WebSocket is based over TCP it uses the underlying TCP socket for communication. The additional advantage of using WebSocket protocol is its very high scalability due reduced network traffic and latency.

Web Socket [specification][1] along with [Web Socket API][3] forms the complete specification for using Web Sockets. 

### Why handshake?

Web Socket communication is based upon a dedicated connection between the client and server, using which both  parties can send data to each other. But before the connection gets established between the client and the server it is mandatory requirement that both the parties should agree to do so. This process is known as hand shake.  Therefore the first step in Web Socket communication is formal handshake between the client and the server which can be initiated by client alone. Browser (client) initiates the handshake by sending a HTTP GET request with an offer to upgrade to Web Socket protocol. 

### Delving deep into the handshake

Now I will start dealing with the various aspects involved in Web Socket handshake and then show how one or more of the above mentioned headers are used to facilitate the handshake. 

### Request Headers

Based on [latest Web Socket specification][1] the typical request for a handshake should look like this

[caption id="attachment_7093" align="alignleft" width="541" caption=" "]![Request Headers][4][/caption] 

  * **Connection and Upgrade** \- The browser (client) initiates the handshake by sending a HTTP GET request. But the connection is to be established using WebSocket protocol. Therefore  in the client request there should be some information which will tell the server that the client wants to upgrade the connection from HTTP protocol to  WebSocket protocol. To convey this information client includes two headers 
    * Connection : Upgrade                   - Tells the server that the connection is an upgrade type of connection.
    * Upgrade:WebSocket                      - Tells the server the protocol to which the connection is to be upgraded.
    1. The request uses GET method.
    2. The request header has a Connection header with value Upgrade and a Upgrade header with value WebSocket

> #### A brief context of Upgrade Header
> 
> To ease the deployment of incompatible future protocols  HTTP1.1 included a new request header 'Upgrade'. By sending the `Upgrade` header, a client can inform a server of the set of protocols it supports as an alternate means of communication. The server may choose to switch protocols, but this is not mandatory.

Similarly on the server side there will be checks to ensure that

Only if the above two conditions are satisfied server will further process the other headers and finally establish the connection. The server side code can be somewhat like this

[sourcecode language="java"] if (aReq.getMethod() != HttpMethod.GET) { sendHttpResponse(aCtx, aReq, new DefaultHttpResponse(HttpVersion.HTTP_1_1, HttpResponseStatus.FORBIDDEN)); return; } // Serve the WebSocket handshake request. if (HttpHeaders.Values.UPGRADE.equalsIgnoreCase(aReq.getHeader(HttpHeaders.Names.CONNECTION)) && HttpHeaders.Values.WEBSOCKET.equalsIgnoreCase(aReq.getHeader(HttpHeaders.Names.UPGRADE))) {

//start processing the upgrade request.

} [/sourcecode]
  * **Host** \- It is important to ensure that the protocol is protected against threats of DNS rebinding and it should be possible to serve multiple domains from one IP address. Keeping these requirements in mind the WebSocket protocol specification requires that the handshake request should contain the Host header. Hackers use DNS rebinding to read data from private network. To know more about DNS rebinding [watch this video][5].
  * **Origin** \- The server should be able to make an informed decision about whether it wishes to accept a handshake invitation from a client. The Origin request header enables the client to send information about the origin of each request to the server. Servers can use this information to decide if each request is valid or not. To know more about Origin header [click here][6].
  * **Sec-WebSocket-Protocol** \- It is possible that the applications develop a few application level protocol layered over the WebSocket protocol. Such protocols are considered  as sub-protocols. The client should explicitly mention the sub-protocol /s which it can use to send/receive data to/from server. Sec-WebSocket-Protocol header is the sub protocol selector. In this header the client can send space separated list of strings. These string represent the sub-protocol the (the application-level protocol  layered over the WebSocket protocol) that the client can use. For example a client may be capable of handling sub protocols which send data packets in JSON, CSV or XML format. In that case the sub protocol details can be added in this header.
  * **Sec-WebSocket-Key1 and Sec-WebSocket-Key2** \- Since the handshake request is a plain HTTP request, there should be some way for server to identify that  it received a valid Web Socket handshake request.  This is important to ensure that the server accepts connection only from valid Web Socket requests. The security against cross protocol hacking is ensured by sending the headers Sec-WebSocket-Key1 and Sec-WebSocket-Key2 from the client. We will see in detail how these keys help ensure a cross protocol security when we discuss the response headers.
  * **Cookies** \- Like Sec-WebSocket-Protocol, Cookies is also an optional header which the client can use to send cookies to the server.

   [1]: http://www.whatwg.org/specs/web-socket-protocol/
   [2]: http://xebee.xebia.in/2010/10/05/web-socket-communication-using-jwebsocket/
   [3]: http://dev.w3.org/html5/websockets/
   [4]: http://xebee.xebia.in/wp-content/uploads/2010/12/requestHeaders3.jpg (requestHeaders)
   [5]: http://vimeo.com/7907871
   [6]: http://people.mozilla.com/~bsterne/content-security-policy/origin-header-proposal.html