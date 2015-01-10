---
layout: post
header-img: img/default-blog-pic.jpg
author: nkhattar
description: 
post_id: 7300
created: 2010/12/31 23:29:17
created_gmt: 2010/12/31 18:29:17
comment_status: open
---

# Spring Integration : Configuring Mail Replier Adapter

<p>On 17th &amp; 18th November, 2010 in Xebia India we had an in-house training on basics of <a href="http://www.springsource.org/spring-integration" target="_blank">Spring Integration</a> by the Spring Integration Guru - Iwein Fuld, himself. The training had an exclusive emphasis on the hands on practice of the basics of spring integration including Message Channels, Message Endpoints , Core Messaging Framework and few Integration Adapters like File Adapter, Mail Adapter and Twitter Adapter.</p>
<p>I got to be a part of the team which was exploring the <strong>Mail Support Adapter</strong>. We worked on features that include reading a message through a channel and sending it within a mail, parsing the message for a url link and enriching the content with the data on that web page and finally replying via a mail in response to a mail received.</p>
<p>The last feature although appeared as quite easier to implement but I somehow faced few problems in configuring the Mail Replier. So in this blog I will elaborate on some common problems in configuring the mail replier and possible ways to solve them as well.</p>
<p>A <strong>Mail Replier</strong> consists of two basic steps:
<ol>
    <li>Configuring Mail Inbound      Channel Adapter to be able to access and read a received mail.</li>
    <li>Generating a Reply Message      and sending it back to the original sender.</li>
</ol>
<!--more-->For the first step, we used <strong>IMAP Adapter</strong> as the Inbound Channel Adapter out of the two available options of IMAP &amp; POP3 Mail Receiver. Within IMAP Adapter we used Polling option rather than Event-Driven because we used Gmail Server which doesn’t supports IMAP IDLE and therefore doesn’t supports Event-Driven. For the correct syntax you can refer to the <a href="http://static.springsource.org/spring-integration/docs/2.0.0.RELEASE/reference/htmlsingle/#mail-inbound" target="_blank">Spring Integration Manual</a>.</p>
<p>This blog revolves more around the <span style="text-decoration: underline;">second step</span> of the Mail Replier. So once the configuration of the IMAP Adapter is successfully completed, the received message can thereby be accessed. The received message is of the type of <strong>MimeMessage</strong>. Mime Message has a predefined <strong>reply</strong> function with a “<strong>reply-to-all</strong>” Boolean flag that returns a message of type <strong>Message</strong>. The new message will have its attributes and headers set up appropriately. The "Subject" field is filled in with the original subject prefixed with "Re:" . It will not have any content, which can then be added using various predefined functions.</p>
<p>So finally a call is made to a function of a service-activator that accepts the received MimeMessage and in turn returns a reply of type Message.
<pre lang="java">public Message sendMessage(MimeMessage mimeMessage) throws MessagingException {
    Message replyMessage = mimeMessage.reply(false);
    replyMessage.setText("MimeMessage Reply");
    return replyMessage;
}</pre>
The reply message is then passed to the Mail-Outbound-Adapter which using the mail-sender (for eg. JavaMailSenderImpl ) is expected to send this message to its marked destination.</p>
<p>[sourcecode language="xml"]
&lt;i:service-activator input-channel=&quot;imapChannel&quot;
    ref=&quot;mailReplier&quot; method=&quot;sendMessage&quot; output-channel=&quot;mailReply&quot;/&gt;
&lt;i:channel id=&quot;mailReply&quot; /&gt;
&lt;bean id=&quot;mailReplier&quot; class=&quot;com.xebia.nuse.mail.ImapReplyActivator&quot; /&gt;
&lt;mail:outbound-channel-adapter channel=&quot;mailReply&quot;
mail-sender=&quot;mailSender&quot; /&gt;
&lt;bean id=&quot;mailSender&quot; class=&quot;org.springframework.mail.javamail.JavaMailSenderImpl&quot;&gt;
    &lt;property name=&quot;host&quot; value=&quot;${mail.host}&quot; /&gt;
    &lt;property name=&quot;username&quot; value=&quot;${mail.username}&quot; /&gt;
    &lt;property name=&quot;password&quot; value=&quot;${mail.password}&quot; /&gt;
    &lt;property name=&quot;javaMailProperties&quot;&gt;
        &lt;props&gt;
            &lt;prop key=&quot;mail.smtp.auth&quot;&gt;true&lt;/prop&gt;
            &lt;prop key=&quot;mail.smtp.starttls.enable&quot;&gt;true&lt;/prop&gt;
        &lt;/props&gt;
    &lt;/property&gt;
&lt;/bean&gt;
[/sourcecode]</p>
<p>Oops….an exception occurs when we try to run the application</p>
<p><em><span style="color: #ff0000;"> SEVERE: org.springframework.integration.MessageHandlingException: Unable to create MailMessage from payload type [javax.mail.internet.MimeMessage], expected byte array or String.
at org.springframework.integration.mail.MailSendingMessageHandler.convertMessageToMailMessage (MailSendingMessageHandler.java:98)
org.springframework.integration.mail.MailSendingMessageHandler.handleMessageInternal (MailSendingMessageHandler.java:71)
org.springframework.integration.handler.AbstractMessageHandler.handleMessage (AbstractMessageHandler.java:78)
org.springframework.integration.dispatcher.UnicastingDispatcher.doDispatch (UnicastingDispatcher.java:110)
org.springframework.integration.dispatcher.UnicastingDispatcher.dispatch (UnicastingDispatcher.java:97) org.springframework.integration.channel.AbstractSubscribableChannel.doSend (AbstractSubscribableChannel.java:44)
org.springframework.integration.channel.AbstractMessageChannel.send (AbstractMessageChannel.java:157) org.springframework.integration.channel.AbstractMessageChannel.send (AbstractMessageChannel.java:128)
org.springframework.integration.core.MessagingTemplate.doSend (MessagingTemplate.java:288)
org.springframework.integration.core.MessagingTemplate.send (MessagingTemplate.java:149)
…
</span></em></p>
<p>As you can see when mail-outbound-adapter tries to send this reply with payload of type MimeMessage, it throws MessageHandlingException.</p>
<p>So in order to solve this problem I came across two plausible solutions:</p>
<p><strong> Solution 1: Modify the received MimeMessage </strong>
This includes changing the subject of the received message explicitly and replacing the recipient’s address with that of sender’s address. Now if we return this modified MimeMessage to the mail-outbound-Adapter, we will get the same exception again. So in order to avoid this, we need to inject <strong>JavaMailSender</strong> into the corresponding service-activator which in turn uses its predefined <strong>send</strong> function to send the MimeMessage. This also avoids the need for the use of mail-outbound-adapter.
<pre lang="java">private JavaMailSender mailSender;
public void setMailSender(JavaMailSender mailSender) {
    this.mailSender = mailSender;
}
.
.
.
final Address[] senderAddresses = mimeMessage.getFrom();
mimeMessage.setSubject("Reply::"+mimeMessage.getSubject());
mimeMessage.setRecipient(RecipientType.TO, senderAddresses[0]);
mailSender.send(mimeMessage);
}</pre>
<strong> Solution 2: Create a new SimpleMailMessage</strong>
This includes creating a new reply message altogether, i.e. using a <strong>SimpleMailMessage </strong>which models a simple mail message, including data such as the from, to, cc, subject, and text fields and it lacks various sophisticated features like attachment, special character encodings, etc but there is very less need of such features in an auto-reply. And one more thing to note is that we need not to inject JavaMailSender into the service-activator and can continue to use the mail-outbound-adapter for sending the reply.
<pre lang="java">Address[] senderAddresses = mimeMessage.getFrom();
MailMessage mailMessage = new SimpleMailMessage();
mailMessage.setSubject("Reply: "+ mimeMessage.getSubject());
mailMessage.setTo(senderAddresses[0].toString());
mailMessage.setText("Thanks for sending the mail");
return mailMessage;
}</pre>
In this way the problem of configuring the Mail Replier was hence-forth solved.
The <strong>Integration Graph</strong> of the xml file we created for exploring Spring Integration Mail Support is as follows:
<a href="http://xebee.xebia.in/wp-content/uploads/2011/01/spring-integration-mail.bmp"><img class="aligncenter" src="http://xebee.xebia.in/wp-content/uploads/2011/01/spring-integration-mail.bmp" alt="Spring Integration Mail Support" width="690" height="319" /></a></p>