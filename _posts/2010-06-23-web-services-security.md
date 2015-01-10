---
layout: post
header-img: img/default-blog-pic.jpg
author: pranjan
description: 
post_id: 3863
created: 2010/06/23 01:44:06
created_gmt: 2010/06/22 20:44:06
comment_status: open
---

# Web Services Security

<p style="text-align: justify;">Web services is all about interoperability among different technology platforms. So, while applying security for web services, the key thing is to have an interoperable security platform. The organizations behind web services standards ( <strong><a href="http://www.oasis-open.org/home/index.php">OASIS</a></strong> ) have kept this in mind while creating standards for web services security. And I will say that they have been able to achieve it.</p>

<p>Like there is a language for web services description. The same way there is a <strong><a href="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/ws-securitypolicy-1.2-spec-os.html">WS-Security Policy</a></strong> Language which describes the security requirements of a web service in a standard, interoperable manner.
<!--more-->
One of the basic works of the policy standards is shown below.</p>
<p>[caption id="attachment_3866" align="aligncenter" width="200" caption="Security Assertion"]<img class="size-full wp-image-3866" title="Security Assertion" src="http://xebee.xebia.in/wp-content/uploads/2010/06/Security-Assertion.jpg" alt="Security Assertion" width="200" height="120" />[/caption]</p>
<p>The security assertion above defines that te body of SOAP messages must be signed.</p>
<p>Security policy language defines 5 main policy assertion types. They are,
<ul> <span style="color: #ff0000;"><strong>Security binding assertions</strong></span> - defines properties like, the keys being used, algorithms, etc.</ul>
<ul> <span style="color: #ff0000;"><strong>Protection assertions</strong></span> - Part of the body to be signed/encrypted</ul>
<ul> <span style="color: #ff0000;"><strong>Token assertions</strong></span> - Appearance of security tokens, like once, never, always etc. Some of the security tokens are Username Tokens, X509 Tokens, Issued Tokens, Secure Conversation Tokens, SAML Tokens.</p>
<p>An example of token is :</p>
<p>[caption id="attachment_3867" align="aligncenter" width="546" caption="UserNameToken"]<img class="size-full wp-image-3867" title="UserNameToken" src="http://xebee.xebia.in/wp-content/uploads/2010/06/UserNameToken.png" alt="UserNameToken" width="546" height="162" />[/caption]</ul>
<ul><span style="color: #ff0000;"><strong>Supporting token assertions</strong></span> - Mostly Related to Encryption and Signature related info of the SOAP Header and Body elements.</ul>
<ul><span style="color: #ff0000;"><strong>Protocol assertions</strong> </span>- Related to the signature of the body, signature type etc.</ul>
<span style="text-decoration: underline;"><strong>Policy Subjects</strong></span></p>
<p>Policy subjects are the attachment points for policies.
<ul> <span style="color: #0000ff;"><strong>Message Policy Subject</strong></span></ul>
<ul><span style="color: #0000ff;"><strong>Operation Policy Subject</strong></span></ul>
<ul><span style="color: #0000ff;"><strong>Endpoint Policy Subject</strong></span></ul>
Security policies can be attached to each of these policy subjects, so we have a policy that is common to the service at the end point level, and additionally define different policies for each operation and message level.</p>
<p>An example of policy in WSDL is shown here.</p>
<p>[caption id="attachment_3868" align="aligncenter" width="620" caption="WS-Security Policy"]<img class="size-full wp-image-3868" title="WS-Security Policy" src="http://xebee.xebia.in/wp-content/uploads/2010/06/Policy.png" alt="WS-Security Policy" width="620" height="307" />[/caption]</p>
<p>These were some of the standards of the WS-Policy language. It can be considered same as standards defined for SOAP, WSDL etc.</p>
<p>However, it would be not wise and easy to code the policy by hand in the <span style="color: #00ff00;"><strong>WSDL</strong></span>. Like we have tools to create WSDL. Soap API with Attachment for Java(<span style="color: #00ff00;"><strong>SAAJ</strong></span>) for SOAP related functions, there is an open source project <span style="color: #00ff00;"><strong><a href="http://ws.apache.org/commons/neethi">Apache Neethi</a></strong></span> which enables programmers to use WS-Policy by exposing API's for WS-Policy creation manipulation.<span style="color: #00ff00;"><strong><a href="http://ws.apache.org/commons/neethi">Apache Neethi</a></strong></span> also provides a mechanism to extend domain specific types as assertions.</p>
<p>A soap message which needs to call a secured web service need to adhere with the security policy defined for the subject (Message/Operation/Endpoint). The security information is sent in the SOAP header. A SOAP message with security information looks like this.</p>
<p>[caption id="attachment_3869" align="aligncenter" width="803" caption="WS-Security Headers"]<img class="size-full wp-image-3869" title="WS-Security Headers" src="http://xebee.xebia.in/wp-content/uploads/2010/06/WS-Security-Headers.png" alt="WS-Security Headers" width="803" height="347" />[/caption]</p>
<p>Well, even to insert these security headers yourself will be cumbersome. So, there is <strong><a href="http://ws.apache.org/rampart">Apache Rampart</a></strong> to help you. Its also an open source project from the house of Apache. Its developed as an Axis2 module.</p>
<p><strong><a href="http://ws.apache.org/rampart">Apache Rampart</a></strong> provides WS-Security functionality to Axis2 Web services and their clients. With Axis2/Rampart, either the Web service or the client can be secured using some Web service stack other than Axis2/Java, such as  .NET , C or PHP.</p>
<p><strong><a href="http://ws.apache.org/rampart">Apache Rampart</a></strong> module needs to be engaged in axis2 modules list. Then the WSDL2Java tool automatically takes care of the security policy. It extracts the relevant policy from the WSDL and apply it correctly to the generatd stub.</p>
<p><strong><a href="https://tcpmon.dev.java.net">TcpMonitor</a></strong> or <strong>HttpMonitor </strong>can be used to see the SOAP messages with security headers generated.</p>