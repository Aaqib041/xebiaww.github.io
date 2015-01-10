---
layout: post
header-img: img/default-blog-pic.jpg
---

# Spring Integration : Configuring Mail Replier Adapter

On 17th & 18th November, 2010 in Xebia India we had an in-house training on basics of [Spring Integration](http://www.springsource.org/spring-integration) by the Spring Integration Guru - Iwein Fuld, himself. The training had an exclusive emphasis on the hands on practice of the basics of spring integration including Message Channels, Message Endpoints , Core Messaging Framework and few Integration Adapters like File Adapter, Mail Adapter and Twitter Adapter. I got to be a part of the team which was exploring the **Mail Support Adapter**. We worked on features that include reading a message through a channel and sending it within a mail, parsing the message for a url link and enriching the content with the data on that web page and finally replying via a mail in response to a mail received. The last feature although appeared as quite easier to implement but I somehow faced few problems in configuring the Mail Replier. So in this blog I will elaborate on some common problems in configuring the mail replier and possible ways to solve them as well. A **Mail Replier** consists of two basic steps: 

  1. Configuring Mail Inbound Channel Adapter to be able to access and read a received mail.
  2. Generating a Reply Message and sending it back to the original sender.
For the first step, we used **IMAP Adapter** as the Inbound Channel Adapter out of the two available options of IMAP & POP3 Mail Receiver. Within IMAP Adapter we used Polling option rather than Event-Driven because we used Gmail Server which doesn’t supports IMAP IDLE and therefore doesn’t supports Event-Driven. For the correct syntax you can refer to the [Spring Integration Manual](http://static.springsource.org/spring-integration/docs/2.0.0.RELEASE/reference/htmlsingle/#mail-inbound). This blog revolves more around the second step of the Mail Replier. So once the configuration of the IMAP Adapter is successfully completed, the received message can thereby be accessed. The received message is of the type of **MimeMessage**. Mime Message has a predefined **reply** function with a “**reply-to-all**” Boolean flag that returns a message of type **Message**. The new message will have its attributes and headers set up appropriately. The "Subject" field is filled in with the original subject prefixed with "Re:" . It will not have any content, which can then be added using various predefined functions. So finally a call is made to a function of a service-activator that accepts the received MimeMessage and in turn returns a reply of type Message. 
    
    
    public Message sendMessage(MimeMessage mimeMessage) throws MessagingException {
     	Message replyMessage = mimeMessage.reply(false);
    	replyMessage.setText("MimeMessage Reply");
    	return replyMessage;
    }

The reply message is then passed to the Mail-Outbound-Adapter which using the mail-sender (for eg. JavaMailSenderImpl ) is expected to send this message to its marked destination. [sourcecode language="xml"] <i:service-activator input-channel="imapChannel" ref="mailReplier" method="sendMessage" output-channel="mailReply"/> <i:channel id="mailReply" /> <bean id="mailReplier" class="com.xebia.nuse.mail.ImapReplyActivator" /> <mail:outbound-channel-adapter channel="mailReply" mail-sender="mailSender" /> <bean id="mailSender" class="org.springframework.mail.javamail.JavaMailSenderImpl"> <property name="host" value="${mail.host}" /> <property name="username" value="${mail.username}" /> <property name="password" value="${mail.password}" /> <property name="javaMailProperties"> <props> <prop key="mail.smtp.auth">true</prop> <prop key="mail.smtp.starttls.enable">true</prop> </props> </property> </bean> [/sourcecode] Oops….an exception occurs when we try to run the application _ SEVERE: org.springframework.integration.MessageHandlingException: Unable to create MailMessage from payload type [javax.mail.internet.MimeMessage], expected byte array or String. at org.springframework.integration.mail.MailSendingMessageHandler.convertMessageToMailMessage (MailSendingMessageHandler.java:98) org.springframework.integration.mail.MailSendingMessageHandler.handleMessageInternal (MailSendingMessageHandler.java:71) org.springframework.integration.handler.AbstractMessageHandler.handleMessage (AbstractMessageHandler.java:78) org.springframework.integration.dispatcher.UnicastingDispatcher.doDispatch (UnicastingDispatcher.java:110) org.springframework.integration.dispatcher.UnicastingDispatcher.dispatch (UnicastingDispatcher.java:97) org.springframework.integration.channel.AbstractSubscribableChannel.doSend (AbstractSubscribableChannel.java:44) org.springframework.integration.channel.AbstractMessageChannel.send (AbstractMessageChannel.java:157) org.springframework.integration.channel.AbstractMessageChannel.send (AbstractMessageChannel.java:128) org.springframework.integration.core.MessagingTemplate.doSend (MessagingTemplate.java:288) org.springframework.integration.core.MessagingTemplate.send (MessagingTemplate.java:149) … _ As you can see when mail-outbound-adapter tries to send this reply with payload of type MimeMessage, it throws MessageHandlingException. So in order to solve this problem I came across two plausible solutions: ** Solution 1: Modify the received MimeMessage ** This includes changing the subject of the received message explicitly and replacing the recipient’s address with that of sender’s address. Now if we return this modified MimeMessage to the mail-outbound-Adapter, we will get the same exception again. So in order to avoid this, we need to inject **JavaMailSender** into the corresponding service-activator which in turn uses its predefined **send** function to send the MimeMessage. This also avoids the need for the use of mail-outbound-adapter. 
    
    
    private JavaMailSender mailSender;
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
    }

** Solution 2: Create a new SimpleMailMessage** This includes creating a new reply message altogether, i.e. using a **SimpleMailMessage **which models a simple mail message, including data such as the from, to, cc, subject, and text fields and it lacks various sophisticated features like attachment, special character encodings, etc but there is very less need of such features in an auto-reply. And one more thing to note is that we need not to inject JavaMailSender into the service-activator and can continue to use the mail-outbound-adapter for sending the reply. 
    
    
    Address[] senderAddresses = mimeMessage.getFrom();
    MailMessage mailMessage = new SimpleMailMessage();
    mailMessage.setSubject("Reply: "+ mimeMessage.getSubject());
    mailMessage.setTo(senderAddresses[0].toString());
    mailMessage.setText("Thanks for sending the mail");
    return mailMessage;
    }

In this way the problem of configuring the Mail Replier was hence-forth solved. The **Integration Graph** of the xml file we created for exploring Spring Integration Mail Support is as follows: ![Spring Integration Mail Support](/wp-content/uploads/2011/01/spring-integration-mail.bmp)