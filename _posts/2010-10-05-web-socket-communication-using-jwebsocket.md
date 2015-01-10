---
layout: post
header-img: img/default-blog-pic.jpg
author: sprasadjha
description: 
post_id: 5130
created: 2010/10/05 09:27:21
created_gmt: 2010/10/05 04:27:21
comment_status: open
---

# Web Socket Communication using jWebSocket

Web Socket has been introduced in HTML5 to enable  full duplex communication with minimum network traffic and latency. I was working on a PoC on HTMl5 Web Socket. In this blog I am sharing my experience about working with jWebSocket to get the basic WebSocket communication running. jWebSocket is pure Java/JavaScript based implementation for implementing Web Socket communication.  After reading this blog the readers should be able to send asynchronous request from client through Web Socket and get response from the server. This blog assumes that readers have basic knowledge of HTML5 Web Sockets

HTML 5 Web Socket specification defines an API that enables full duplex communication over one (TCP) socket. Before web sockets the server could respond only on receiving a request from a web client. To overcome this limitation the common workarounds are polling, long polling and comet based solutions. HTML 5 Web Sockets comes as an inbuilt support for bi-directional communication which promises to be highly scalable due to reduced network traffic and latency. 

In this blog I have used  jWebSocket the Web Socket implementation for Java/JS. The first step is to download the required distribution files from [jWebSocket.org ][1] and follow the steps for configuration.

To get the Web Socket communication running both client and server side coding efforts are involved. I will deal with both of them.  Let us start with the client side code

The jWebSocket distribution comes with jWebSocket.js which needs to be included in your view file. Basically the jWebSocket.js exposes the JS implementation for WebSocket. The following steps are to be followed to make the client interact with the server through Web Sockets. 

  1. Create an instance of jWebSocketJSONClient.
  2. Create a WebSocket connection.
  3. Send/Receive tokens.
Step 1: Create an instance of jWebSocketJSONClient: jWebSocket exposes clients like jWebSocketJSONClient, jWebSocketCSVClient, jWebSocketXMLClient. For our example we will use jWebSocketJSONClient. As a first step I have written initWebSocket function which checks if  the browser supports HTML5's WebSocket and only then creates an instance of the jWebSocketJsonClient.

[sourcecode language="xml"] var jWebSocketClient = null; function initWebSocket(){ if( jws.browserSupportsWebSockets() ) { jWebSocketClient = new jws.jWebSocketJSONClient(); } else { var lMsg = jws.MSG_WS_NOT_SUPPORTED; } } [/sourcecode]

Step 2: Create a WebSocket connection - Once it is ascertained that the browser supports Web Sockets and the jWebSocketClient is created then logon() function of the client can be invoked to create a physical WebSocket connection between the client and the server. The function takes the url, username, password and the callback functions as the argument. 

  1. url -  represents the end-point to which you wish to connect. Please note that the url starts with ws:// and not http:// as the Web Socket connection is established using the Web Socket protocol. Web Socket protocol is an upgrade over HTTP protocol using the same underlying TCP/IP connection. ws:// and wss:// prefix are proposed to indicate a WebSocket and a secure WebSocket connection, respectively.
  2. username / password - The user details can be configured in jWebSocket.xml which is packages in conf folder of the jWebSocket server deployment directory. All the authentication details by default will be validated against this file.
  3. callback functions -  [WebSocket interface][2] has attribute functions onOpen, onClose, onError and onMessage. Whenever we create a Web Socket connection we can provide callbacks for the Web Socket connection open and close events, an error event and a message receive event. When ever any of these events occur the corresponding call backs will get called allowing you to handle the situation.
[sourcecode language="xml"] function logon(){ var lRes = jWebSocketClient.logon( "ws://hostname|localhost:8787", "username", "password", { // OnOpen callback OnOpen: function( aEvent ) { log( "jWebSocket connection established." ); }, // OnMessage callback OnMessage: function( aEvent, aToken ) { log( "jWebSocket '" \+ aToken.type + "' token received, full message: '" \+ aEvent.data); }, // OnClose callback OnClose: function( aEvent ) { log( "jWebSocket connection closed." ); } }); } [/sourcecode]

Step 3: Send/Receive tokens - Once the connection request is authenticated and the socket connection is established both the client and server can send information to each other whenever they want.  To send token from the client  just create a token with namespace and type. Remember that the client being used  to send data is JSON client and therefore the token should follow the JSON format. Once the token is ready, invoke the send method of  the JSON client which takes token and the callback function (which will be invoked if the server sends some response) as arguments.

[sourcecode language="xml"] function sampleListener() { if( jWebSocketClient.isConnected() ) { log("sending message to jwebsocket"); var lToken = { ns: "my.namespace", type: "getInfo" }; jWebSocketClient.sendToken( lToken, { OnResponse: function( aToken ) { log("Server responded: " \+ "vendor: " \+ aToken.vendor \+ ", version: " \+ aToken.version ); } }); } else { log( "Not connected." ); } } [/sourcecode]

Let us look into the server side code, all the requests related to Web Sockets are handled by Web Socket server. The following steps are required to be followed to make the server Web Socket enabled 

  1. Start the jWebSocket server and register a JWebSocketTokenServerListener with the jWebSocketServer
  2. Create a Listener implementation of JWebSocketTokenServerListener and provide the implementation (this is the listener which was registered with the server int the previous step)
Step 1 : Start the jWebSocket server and register a JWebSocketTokenServerListener with the jWebSocketServer :  When the application starts up in the application server it has to be ensured that the jWebSocket server also starts up. To ensure this a listener of type ServletContextListener can be configured in web.xml. I have created a JWSContextListener and configured the listener in web.xml as below.

[sourcecode language="xml"] <listener> <description>ServletContextListener</description> <listener-class>com.checkList.web.contextUtils.JWSContextListener</listener-class> </listener> [/sourcecode]

jWebSocketServer is started in the _contextInitialized_ method of JWSContextListener. To be able to receive tokens and resopond to certain events in the jWebSocketServer a listener_ JWebSocketListener_ is registered to the token server. The listener registered to the server will listen to the events occurring in the sever and the corresponding methods will get called. In jWebSocket.xml in conf folder of the jWebSocket server deployment you can specify the server types to be instantiated for jWebSocket. Each server type has a unique id. By default _ token server_ and_ custom server_ are defined with ids as ts0 and cs0 respectively. That is why there is reference to ts0 in "_TokenServer tokenServer = (TokenServer)JWebSocketFactory.getServer("ts0")_;."

[sourcecode language="java"] <pre>public class JWSContextListener implements ServletContextListener { @Override public void contextInitialized(ServletContextEvent sce) { // start the jWebSocket server sub system JWebSocketFactory.start(""); TokenServer tokenServer = (TokenServer)JWebSocketFactory.getServer("ts0"); if(tokenServer !=null){ tokenServer.addListener(new JWebSocketListener()); } } @Override

   [1]: http://jwebsocket.org/ (http://jwebsocket.org/)
   [2]: http://dev.w3.org/html5/websockets/#websocket (http://dev.w3.org/html5/websockets/#websocket)

## Comments

**[Happy](#6995 "2012-01-17 16:04:46"):** Can you post .zip file of example.

**[Duy Nguyen](#8724 "2012-05-07 09:49:25"):** Hello, nice tutorial indeed. Appreciate if you upload whole sample project for more reference. Thanks in advance.

**[Vic Hargrave](#8350 "2012-04-06 04:01:59"):** Nice article and very helpful. Unfortunately I'm stuck with jWebSocket at a very basic stage. I can't get the sample clients to work when I load from the same system on which I'm running my jWebSocket server The server keesp dropping the connections indicating that there is a problem with the connection handshaking. When I inquired about this with the jWebSocket dev team I was told it most likely had to do with the domain not be set up properly, yet I see statements in my jWebSocket.xml config file for 127.0.0.1. There is no documentation that I could find on the jWebSocket website showing how to configure the jWebsocket.xml file properly. Do you have any suggestions? Thanks in advance. \-- vic

**[Manish](#9274 "2012-07-30 14:56:04"):** Hi, Is there any API in jWebSocket that will help to connect the tomcat websocket from java stand alone client.

