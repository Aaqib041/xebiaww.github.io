---
layout: post
header-img: img/default-blog-pic.jpg
author: Khyati
description: 
post_id: 18144
created: 2014/03/16 19:34:49
created_gmt: 2014/03/16 14:34:49
comment_status: open
---

# HermesJMS and ActiveMQ integration with SoapUI

HermesJMS is a console for JMS used for sending messages over client server contract. The main motive is to send a message to multiple interested consumers - think of it as a process of sending an email to a single person or a mailing list having a group of people, where in the one who is sending the messages does not need to know who is subscribed in that mailing list. He has to just take the request and do the needful to make that request a success.

The body of the message can cater multiple request at one time and can have one request at a time per message.Via this blog I will show how to configure Hermes with SOAP-UI . Following are the steps which one need to take.

**1\. Installation of ActiveMQ.**

 For which you require Java JDK 1.6 or higher.

Download a copy of ActiveMQ from the Active MQ web site ( http://activemq.apache.org/) and unzip the file.

Now make a placeholder wherein you will place the extracted directory.

And run “activemq start”.

**2.Installation of HermesJMS**

Next , you need to get a client from where you will be sending your messages.

For this we will be using Soap-UI. Soap-Ui has Hermes JMS asone of the goodies , which you can use directly without any installation.

**Getting started:**

To start hermes all you need to do is to follow these steps:

1\. Download Soap Ui from http://sourceforge.net/projects/soapui/files/. Unzip soap ui and open it.

2 . Now click on HermesJMS from within soapUI tools menu and configure as specified below.

![Hermes.png][1]

Now go toFile --> preferences --> tools . And change the path of HermesJms to the valid path( instead of  c:\\.hermes which comes as default )where JMS is placed in your local like this_ C:\Program Files\SmartBear\soapUI-Pro-4.6.0\hermesJMS . _Now save preferences and restart Soap UI.

![Tools][2]

i)  **_Create a new session_**:

  * Now open Soap-UI --> Hermes JMS.

  * Right click on sessions -> New - > New Session -> give session a name.

  * Go to providers section by clicking on Providers tab.

  * Right click on the Classpath Groups and add a group and provide a name.

  * Right click on the Library and select Add JAR(s) and add activemq-core-5.7.0.jar, geronimo-j2ee-management_1.1_spec-1.0.1.jar andactivemq-all-5.9-20130711.131848-74,jar

  * Click Apply and OK and Save the settings using File->Save Settings.

Active JMS is controlled by some config files , like hermes-config.xml , ems-hermes-config.xml , vas-hermes-config.xml.

These files drive whole work flow of hermes. Lets talk about these files in a bit more detail.

**Hermes-config.xml:**

This is an xml file which has all the details of the jms session in tags form. you can see tags like 'renderer' , 'extension' , 'provider' ,etc. This file grows as your number of sessions will grow. The more sessions you will add , this file will get an connection session entry.

> _<renderer className="hermes.renderers.EBCDICMessageRenderer">_ _ <properties>_ _ <property name="rowLength" value="16"/>_ _ <property name="undisplayableChar" value="."/>_ _ <property name="active" value="true"/>_ _ <property name="maxMessageSize" value="5242880"/>_ _ </properties>_ _ </renderer>_

> _<classpathGroup id="ActiveMQ">_ _ <library jar="**F:\software\ActiveMQ\activemq-core-5.7.0.jar**" noFactories="true"/>_ _ <library jar="**F:\software\ActiveMQ\geronimo-j2ee-management_1.1_spec-1.0.1.jar**" noFactories="true"/>_ _ <library jar="**F:\software\ActiveMQ\activemq-client-5.8-20130206.030759-64.jar**" noFactories="false" factories="org.apache.activemq.ActiveMQXAConnectionFactory,org.apache.activemq.ActiveMQSslConnectionFactory,org.apache.activemq.ActiveMQConnectionFactory"/>_ _ </classpathGroup>_

> _<factory classpathId="ActiveMQ">_ _ <provider className="org.apache.activemq.ActiveMQConnectionFactory">_ _ <properties>_ _ <property name="**brokerURL**" value="**tcp://localhost:61616**"/>_ _ </properties>_ _ </provider>_ _ <connection clientID="" connectionPerThread="false">_ _ <**session id**="**TEST**" reconnects="0" transacted="true" audit="false" useConsumerForQueueBrowse="false"/>_ _ </connection>_ _ <**destination name**="**MDMMessageQueue**" domain="1" durable="false"/>_ _ <extension className="hermes.ext.activemq.ActiveMQAdminFactory">_ _ <properties>_ _ <property name="**brokerName**" value="**Test**"/>_ _ <property name="**serviceURL**" value="**service:jmx:rmi://jndi/rmi://localhost:1099/jmxrmi**"/>_ _ </properties>_ _ </extension>_ _ </factory>_

**ems-hermes-config.xml**

This configuration file is placed at location : _\SmartBear\soapUI-Pro-4.6.0\hermesJMS\cfg_

This contains all the jars added while making session/s in hermes.

> <classpathGroup id="EMS 4.2"> <library jar="C:\local\tibco\ems\clients\java\crimson.jar" noFactories="true"/> <library jar="C:\local\tibco\ems\clients\java\jaxp.jar" noFactories="true"/> <library jar="C:\local\tibco\ems\clients\java\jcert.jar" noFactories="true"/> <library jar="C:\local\tibco\ems\clients\java\jms.jar" noFactories="true"/> <library jar="C:\local\tibco\ems\clients\java\jndi.jar" noFactories="true"/> <library jar="C:\local\tibco\ems\clients\java\jnet.jar" noFactories="true"/>

   [1]: http://xebee.xebia.in/wp-content/uploads/2014/03/Hermes.png1-223x300.jpg (HermesJMS)
   [2]: http://xebee.xebia.in/wp-content/uploads/2014/03/Tools-300x233.jpg