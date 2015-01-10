---
layout: post
header-img: img/default-blog-pic.jpg
author: rgarg
description: 
post_id: 5353
created: 2010/11/01 15:13:30
created_gmt: 2010/11/01 10:13:30
comment_status: open
---

<!--Simple and basic steps and options that will help you to implement Network based messaging using Spring integration framework.-->

# Data transmission over the network with Spring Integration

<p>Spring Integration is a quite useful framework which provides asynchronous messaging capabilities to a system. Unlike traditional frameworks like JMS, this framework can be plugged into any application. I will not be going into the details of messaging and '<strong><em>How messaging works!</em></strong>' but an overview should be a good start.</p>
<!--more-->

<p>Messaging is a mechanism to invoke some actions asynchronously. Message passing helps to de-couple the logic from infrastructure and can be applied to almost every system. Sending across a communication to a system or more appropriately a <em>message handler</em> is called messaging.</p>
<p>There are two types of message handling implementations in Spring Integration framework - Queue channel based and Direct channel. In case of Queue based message handling, a pollable queue is placed after each message generator and one or more pollers are attached to that queue for handling and processing messages whereas in case of direct message handling, our message handler is attached to the direct channel which gets called as soon as the message is published to the channel.</p>
<p>Spring Integration framework can also be used to send data across the network. Think of a scenario where a file needs to be sent to a receiver and there is no limit to the size of that file. The only possible solution to reliably and quickly transmit that file over to the other end could be splitting it into number of messages and sending them across. Spring integration framework facilitates message transmission functionality by providing the necessary API to transmit Messages over the network in a reliable manner,</p>
<p>There are two base classes provided in spring integration framework to achieve this task - <strong>AbstractInternetProtocolSendingMessageHandler</strong> and <strong>AbstractInternetProtocolReceivingChannelAdapter</strong>. For the details of API, one must refer to the <a href="http://static.springsource.org/spring-integration/docs/2.0.0.M5/javadoc-api/org/springframework/integration/ip/tcp/package-summary.html">package summary</a> provided.</p>
<p><em><strong>How to use it??</strong></em></p>
<p>To add a simple TCP message handler/ TCP message receiver, the following snippets can be added to the <em>applicationContext.xml</em> file:</p>
<p>[code language="xml"]
&lt;ip:inbound-channel-adapter protocol=&quot;tcp/udp&quot;
     channel=&quot;outputChannel&quot;
     close=&quot;false&quot;
     custom-socket-reader-class-name=&quot;CustomClass&quot;
     id=&quot;inbound&quot;
     local-address=&quot;localIpAddressForMultiHomeServers&quot;
     message-format=&quot;length-header&quot;
     pool-size=&quot;noOfThreads&quot;
     port=&quot;portNumber&quot;
     receive-buffer-size=&quot;bufferSize&quot;
     using-direct-buffers=&quot;true/false&quot;
     using-nio=&quot;true/false&quot; /&gt;[/code]</p>
<p>[code language="xml"]
&lt;ip:outbound-channel-adapter protocol=&quot;tcp/udp&quot;
     channel=&quot;inputChannel&quot;
     custom-socket-writer-class-name=&quot;CustomClass&quot;
     host=&quot;hostname&quot;
     id=&quot;outbound&quot;
     local-address=&quot;localIpAddressForMultiHomeServers&quot;
     message-format=&quot;length-header&quot;
     port=&quot;portnumber&quot;
     using-direct-buffers=&quot;true/false&quot;
     using-nio=&quot;true/false&quot;/&gt;[/code]</p>
<p><em><strong>Description of tags</strong></em></p>
<p>An inbound channel adapter is something that is used to receive the data from some sender and convert it to a <em>Message</em> object which would then be sent to the OutputChannel for further processing. It is an instance of <strong>AbstractInternetProtocolReceivingChannelAdapter's</strong> implementation depending upon the protocol specified.</p>
<p>On the contrary, an outbound channel adapter is supposed to send whatever is present on the InputChannel to some listener identified by hostname:portNumber. This is an instance of <strong>AbstractInternetProtocolSendingMessageHandler's</strong> implementation.</p>
<p>Besides, each handler-adapter pair implementation also uses an underlying <strong>SocketWriter-SocketReader</strong> pair as well.</p>
<p><a rel="attachment wp-att-5404" href="http://xebee.xebia.in/2010/11/01/data-transmission-over-the-network-with-spring-integration/springnetworkmessaging-2/"><img class="alignnone size-full wp-image-5404" title="SpringNetworkMessaging" src="http://xebee.xebia.in/wp-content/uploads/2010/10/SpringNetworkMessaging1.jpg" alt="" width="679" height="307" /></a></p>
<p>There are different <strong>message_formats</strong> via which, one can convert the Message object to byte data, namely :-
<ul>
    <li> FORMAT_LENGTH_HEADER - Sends message length in header first and then the byte message.</li>
    <li> FORMAT_STX_ETX - Sends a special bit pattern STX, then data followed by ETX bit pattern.</li>
    <li> FORMAT_CRLF - Starts sending a message and when its complete, sends a CR_LF bit pattern.</li>
    <li> FORMAT_JAVA_SERIALIZED - Sends data after serializing the object using ObjectOutputStream.</li>
    <li> FORMAT_CUSTOM - Left as an exercise for the reader ;).</li>
</ul>
Hint: You can add your own implementation of NetSocketWriter/NetSocketReader class in case you want to use FORMAT_CUSTOM.</p>
<p>In case someone doesn't know the destination beforehand, or maybe the port to listen to on the local machine, objects of above mentioned API can be created and parameters can be set dynamically. Finally, these objects can be registered to a ConfigurableApplicationContext for making them running.</p>
<p>These classes have four implementation classes each, two for TCP and two for UDP protocols. For TCP the two classes are segregated as:</p>
<ol>
<li>
<p><strong>TcpNetSendingMessageHandler</strong> and <strong>TcpNetReceivingChannelAdapter</strong> pair - This pair works over TCP simple Socket based implementation and does the usual socket stuff like blocking reads over streams, handling EOF when stream is closed. Both the classes use a SocketFactory class to create an instance of <strong>NetSocketWriter</strong> and a <strong>NetSocketReader</strong> respectively which follow the same protocol. You can choose to implement custom logic for sending data by adding a custom implementation to each of the <strong>NetSocketWriter</strong> and <strong>NetSocketReader</strong> classes and implement the methods <em>writeCustomFormat</em> and <em>assembleDataCustomFormat</em> respectively.</p>
</li>
<li>
<p><strong>TcpNioSendingMessageHandler</strong> and <strong>TcpNioReceivingChannelAdapter</strong> pair - This pair works almost the same way as the above mentioned apart from the fact that this pair uses NioSocketWriter and NioSocketReader pair. It does have an additional capability to use direct buffers for channels which enhances the speed many folds. Although the NIO based IO is a little slow, its never blocking. So there might be some performance concerns on the system as well as it will always be executing code to read from the streams even though there is nothing present. Please note that FORMAT_JAVA_SERIALIZED is not supported by this kind of pair. Rest all formats work fine. Also, the hint in this implementation changes to <strong>NioSocketWriter/NioSocketReader</strong> pair.</p>
</li>
</ol>
<p>For UDP, the pairs are not used much because most of the network firewalls have security issues using UDP protocol. Moreover its not a reliable protocol anyways. The pairs for sending messages over UDP are <strong>UnicastSendingMessageHandler-UnicastReceivingChannelAdapter</strong> and <strong>MulticastSendingMessageHandler-MulticastReceivingChannelAdapter</strong>. This system is an ACK driven setup and relies on the successful reception of ACK message from the receiver. Reliability can be added through code by enforcing a check for successful reception of ACK message else the message goes to error handler in case the TTL(Time To Live) is expired.</p>

## Comments

**[Rohit Garg](#3144 "2010-11-05 13:07:06"):** Hey Everyone, Thanks for the comments. I just wanted to mention that we have used v2.0.0.M3 in our project and found it pretty ok when it comes to implementation. Thanks for mentioning the release Gary. Will try that as well in the upcoming projects. @Iwein : I have created a sample project which is more like a loopback chat. The URL to the same is http://code.google.com/p/si-chat/ EDIT: As requested by Shrikant, I have also posted the code on github.com. The URL to the repository is https://github.com/xebia/SIChat/

**[Iwein Fuld](#3118 "2010-11-02 19:28:58"):** Nice and detailed howto, good stuff! Do you have working source code you could share on a public repository?

**[anehra63](#3204 "2010-11-11 16:21:54"):** Nice work thanks for poting it

**[Gary Russell](#3119 "2010-11-02 20:05:39"):** Nice post - please note, though, that the configuration changed a little since the 2.0 milestone version used here. It is now possible to share connections between inbound and outbound adapters. Also, it is now simpler to provide custom wire protocols by implementing the serializer strategy interfaces available in Spring 3.0.5. Please see the latest documentation here... http://static.springsource.org/spring-integration/docs/2.0.x/reference/html/ip.html Spring Integration 2.0.0 Release Candidate 1 was released 10/29/10, but these changes have been available in milestones since late summer.

**[Gary Russell](#3121 "2010-11-02 23:52:02"):** For an example of the new configuration, please see the tcp-client-server sample The samples are no longer bundled with the release; see this article for information about how to get the samples... http://blog.springsource.com/2010/09/29/new-spring-integration-samples/

**[Priya Dandekar](#5464 "2011-04-14 01:57:47"):** Thanks for this short but good information on data transmission. I am just starting to digg deep into latest release of spring integration.

**[Haibo Hu](#5677 "2011-07-07 00:11:41"):** Good stuff! Just wondering how Spring Integration could reduce the resource consumptions during large data (tens or hundreds of mb) transmition, and perform/scale well. Thanks in advance!

**[Rohit Garg](#5681 "2011-07-07 13:52:11"):** Spring integration adapters don't fetch the entire data into the memory before transmitting, moreover it uses thread pool to perform almost every task. Once you increase the workers upto the performant limit, you would obviously get better results. Unlike microsoft's direct copy which uses only a single thread, chunked data transfer with multiple threads and that too unordered does the trick!

