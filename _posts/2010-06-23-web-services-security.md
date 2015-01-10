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

Web services is all about interoperability among different technology platforms. So, while applying security for web services, the key thing is to have an interoperable security platform. The organizations behind web services standards ( **[OASIS][1]** ) have kept this in mind while creating standards for web services security. And I will say that they have been able to achieve it.

Like there is a language for web services description. The same way there is a **[WS-Security Policy][2]** Language which describes the security requirements of a web service in a standard, interoperable manner.  One of the basic works of the policy standards is shown below.

[caption id="attachment_3866" align="aligncenter" width="200" caption="Security Assertion"]![Security Assertion][3][/caption]

The security assertion above defines that te body of SOAP messages must be signed.

Security policy language defines 5 main policy assertion types. They are, 

**Security binding assertions** \- defines properties like, the keys being used, algorithms, etc. **Protection assertions** \- Part of the body to be signed/encrypted **Token assertions** \- Appearance of security tokens, like once, never, always etc. Some of the security tokens are Username Tokens, X509 Tokens, Issued Tokens, Secure Conversation Tokens, SAML Tokens.

An example of token is :

[caption id="attachment_3867" align="aligncenter" width="546" caption="UserNameToken"]![UserNameToken][4][/caption] **Supporting token assertions** \- Mostly Related to Encryption and Signature related info of the SOAP Header and Body elements. **Protocol assertions** \- Related to the signature of the body, signature type etc. **Policy Subjects**

Policy subjects are the attachment points for policies. 

**Message Policy Subject** **Operation Policy Subject** **Endpoint Policy Subject** Security policies can be attached to each of these policy subjects, so we have a policy that is common to the service at the end point level, and additionally define different policies for each operation and message level.

An example of policy in WSDL is shown here.

[caption id="attachment_3868" align="aligncenter" width="620" caption="WS-Security Policy"]![WS-Security Policy][5][/caption]

These were some of the standards of the WS-Policy language. It can be considered same as standards defined for SOAP, WSDL etc.

However, it would be not wise and easy to code the policy by hand in the **WSDL**. Like we have tools to create WSDL. Soap API with Attachment for Java(**SAAJ**) for SOAP related functions, there is an open source project **[Apache Neethi][6]** which enables programmers to use WS-Policy by exposing API's for WS-Policy creation manipulation.**[Apache Neethi][6]** also provides a mechanism to extend domain specific types as assertions.

A soap message which needs to call a secured web service need to adhere with the security policy defined for the subject (Message/Operation/Endpoint). The security information is sent in the SOAP header. A SOAP message with security information looks like this.

[caption id="attachment_3869" align="aligncenter" width="803" caption="WS-Security Headers"]![WS-Security Headers][7][/caption]

Well, even to insert these security headers yourself will be cumbersome. So, there is **[Apache Rampart][8]** to help you. Its also an open source project from the house of Apache. Its developed as an Axis2 module.

**[Apache Rampart][8]** provides WS-Security functionality to Axis2 Web services and their clients. With Axis2/Rampart, either the Web service or the client can be secured using some Web service stack other than Axis2/Java, such as  .NET , C or PHP.

**[Apache Rampart][8]** module needs to be engaged in axis2 modules list. Then the WSDL2Java tool automatically takes care of the security policy. It extracts the relevant policy from the WSDL and apply it correctly to the generatd stub.

**[TcpMonitor][9]** or **HttpMonitor **can be used to see the SOAP messages with security headers generated.

   [1]: http://www.oasis-open.org/home/index.php
   [2]: http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/ws-securitypolicy-1.2-spec-os.html
   [3]: http://xebee.xebia.in/wp-content/uploads/2010/06/Security-Assertion.jpg (Security Assertion)
   [4]: http://xebee.xebia.in/wp-content/uploads/2010/06/UserNameToken.png (UserNameToken)
   [5]: http://xebee.xebia.in/wp-content/uploads/2010/06/Policy.png (WS-Security Policy)
   [6]: http://ws.apache.org/commons/neethi
   [7]: http://xebee.xebia.in/wp-content/uploads/2010/06/WS-Security-Headers.png (WS-Security Headers)
   [8]: http://ws.apache.org/rampart
   [9]: https://tcpmon.dev.java.net