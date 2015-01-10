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

<p class="clean" style="text-align: left">HermesJMS is a console for JMS used for sending messages over client server contract. The main motive is to send a message to multiple interested consumers - think of it as a process of sending an email to a single person or a mailing list having a group of people, where in the one who is sending the messages does not need to know who is subscribed in that mailing list. He has to just take the request and do the needful to make that request a success.</p>

<p class="clean" style="text-align: left">The body of the message can cater multiple request at one time and can have one request at a time per message.Via this blog I will show how to configure Hermes with SOAP-UI . Following are the steps which one need to take.</p>

<p class="clean"><!--more--></p>

<p class="clean" style="text-align: left"><strong>1. Installation of ActiveMQ.</strong></p>

<p class="clean" style="text-align: left"> For which you require Java JDK 1.6 or higher.</p>

<p class="clean" style="text-align: left">Download a copy of ActiveMQ from the Active MQ web site ( http://activemq.apache.org/) and unzip the file.</p>

<p class="clean" style="text-align: left">Now make a placeholder wherein you will place the extracted directory.</p>

<p class="clean" style="text-align: left">And run “activemq start”.</p>

<p class="clean" style="text-align: left"><strong>2.Installation of HermesJMS</strong></p>

<p class="clean" style="text-align: left">Next , you need to get a client from where you will be sending your messages.</p>

<p class="clean" style="text-align: left">For this we will be using Soap-UI. Soap-Ui has Hermes JMS asone of the goodies , which you can use directly without any installation.</p>

<p class="clean" style="text-align: left"><strong>Getting started:</strong></p>

<p class="clean" style="text-align: left">To start hermes all you need to do is to follow these steps:</p>

<p class="clean" style="text-align: left">1. Download Soap Ui from http://sourceforge.net/projects/soapui/files/. Unzip soap ui and open it.</p>

<p class="clean" style="text-align: left">2 . Now click on HermesJMS from within soapUI tools menu and configure as specified below.</p>

<p class="clean" style="text-align: left"><a href="http://xebee.xebia.in/wp-content/uploads/2014/03/Hermes.png1.jpg"><img class="aligncenter size-medium wp-image-18154" title="HermesJMS" alt="Hermes.png" src="http://xebee.xebia.in/wp-content/uploads/2014/03/Hermes.png1-223x300.jpg" width="223" height="300" /></a></p>

<p class="clean" style="text-align: left">Now go toFile --&gt; preferences --&gt; tools . And change the path of HermesJms to the valid path( instead of  c:\.hermes which comes as default )where JMS is placed in your local like this<em> C:\Program Files\SmartBear\soapUI-Pro-4.6.0\hermesJMS . </em>Now save preferences and restart Soap UI.</p>

<p class="clean" style="text-align: left"><a href="http://xebee.xebia.in/wp-content/uploads/2014/03/Tools.jpg"><img class="aligncenter size-medium wp-image-18221" alt="Tools" src="http://xebee.xebia.in/wp-content/uploads/2014/03/Tools-300x233.jpg" width="300" height="233" /></a></p>

<p class="clean" style="text-align: left">i)  <strong><em>Create a new session</em></strong>:</p>

<ul style="text-align: left">
    <li>
<p class="clean"><span style="line-height: 1.5em">Now open Soap-UI --&gt; Hermes JMS.</span></p>
</li>
    <li>
<p class="clean"><span style="line-height: 1.5em">Right click on sessions -&gt; New - &gt; New Session -&gt; give session a name.</span></p>
</li>
    <li>
<p class="clean"><span style="line-height: 1.5em">Go to providers section by clicking on Providers tab.</span></p>
</li>
    <li>
<p class="clean"><span style="line-height: 1.5em">Right click on the Classpath Groups and add a group and provide a name.</span></p>
</li>
    <li>
<p class="clean"><span style="line-height: 1.5em">Right click on the Library and select Add JAR(s) and add activemq-core-5.7.0.jar, geronimo-j2ee-management_1.1_spec-1.0.1.jar andactivemq-all-5.9-20130711.131848-74,jar</span></p>
</li>
    <li>
<p class="clean"><span style="line-height: 1.5em">Click Apply and OK and Save the settings using File-&gt;Save Settings.</span></p>
</li>
</ul>

<p class="clean">Active JMS is controlled by some config files , like hermes-config.xml , ems-hermes-config.xml , vas-hermes-config.xml.</p>

<p class="clean">These files drive whole work flow of hermes. Lets talk about these files in a bit more detail.</p>

<p class="clean"><span style="text-decoration: underline"><strong>Hermes-config.xml:</strong></span></p>

<p class="clean">This is an xml file which has all the details of the jms session in tags form. you can see tags like 'renderer' , 'extension' , 'provider' ,etc. This file grows as your number of sessions will grow. The more sessions you will add , this file will get an connection session entry.</p>

<blockquote>
<p class="clean"><em>&lt;renderer className="hermes.renderers.EBCDICMessageRenderer"&gt;</em>
<em> &lt;properties&gt;</em>
<em> &lt;property name="rowLength" value="16"/&gt;</em>
<em> &lt;property name="undisplayableChar" value="."/&gt;</em>
<em> &lt;property name="active" value="true"/&gt;</em>
<em> &lt;property name="maxMessageSize" value="5242880"/&gt;</em>
<em> &lt;/properties&gt;</em>
<em> &lt;/renderer&gt;</em></p>
</blockquote>

<blockquote>
<p class="clean"><em>&lt;classpathGroup id="ActiveMQ"&gt;</em>
<em> &lt;library jar="<strong>F:\software\ActiveMQ\activemq-core-5.7.0.jar</strong>" noFactories="true"/&gt;</em>
<em> &lt;library jar="<strong>F:\software\ActiveMQ\geronimo-j2ee-management_1.1_spec-1.0.1.jar</strong>" noFactories="true"/&gt;</em>
<em> &lt;library jar="<strong>F:\software\ActiveMQ\activemq-client-5.8-20130206.030759-64.jar</strong>" noFactories="false" factories="org.apache.activemq.ActiveMQXAConnectionFactory,org.apache.activemq.ActiveMQSslConnectionFactory,org.apache.activemq.ActiveMQConnectionFactory"/&gt;</em>
<em> &lt;/classpathGroup&gt;</em></p>
</blockquote>

<blockquote>
<p class="clean"><em>&lt;factory classpathId="ActiveMQ"&gt;</em>
<em> &lt;provider className="org.apache.activemq.ActiveMQConnectionFactory"&gt;</em>
<em> &lt;properties&gt;</em>
<em> &lt;property name="<strong>brokerURL</strong>" value="<strong>tcp://localhost:61616</strong>"/&gt;</em>
<em> &lt;/properties&gt;</em>
<em> &lt;/provider&gt;</em>
<em> &lt;connection clientID="" connectionPerThread="false"&gt;</em>
<em> &lt;<strong>session id</strong>="<strong>TEST</strong>" reconnects="0" transacted="true" audit="false" useConsumerForQueueBrowse="false"/&gt;</em>
<em> &lt;/connection&gt;</em>
<em> &lt;<strong>destination name</strong>="<strong>MDMMessageQueue</strong>" domain="1" durable="false"/&gt;</em>
<em> &lt;extension className="hermes.ext.activemq.ActiveMQAdminFactory"&gt;</em>
<em> &lt;properties&gt;</em>
<em> &lt;property name="<strong>brokerName</strong>" value="<strong>Test</strong>"/&gt;</em>
<em> &lt;property name="<strong>serviceURL</strong>" value="<strong>service:jmx:rmi://jndi/rmi://localhost:1099/jmxrmi</strong>"/&gt;</em>
<em> &lt;/properties&gt;</em>
<em> &lt;/extension&gt;</em>
<em> &lt;/factory&gt;</em></p>
</blockquote>

<p class="clean" style="text-align: left"><span style="text-decoration: underline"><strong>ems-hermes-config.xml</strong></span></p>

<p class="clean" style="text-align: left">This configuration file is placed at location : <em>\SmartBear\soapUI-Pro-4.6.0\hermesJMS\cfg</em></p>

<p class="clean" style="text-align: left">This contains all the jars added while making session/s in hermes.</p>

<blockquote>
<p class="clean">&lt;classpathGroup id="EMS 4.2"&gt;
&lt;library jar="C:\local\tibco\ems\clients\java\crimson.jar" noFactories="true"/&gt;
&lt;library jar="C:\local\tibco\ems\clients\java\jaxp.jar" noFactories="true"/&gt;
&lt;library jar="C:\local\tibco\ems\clients\java\jcert.jar" noFactories="true"/&gt;
&lt;library jar="C:\local\tibco\ems\clients\java\jms.jar" noFactories="true"/&gt;
&lt;library jar="C:\local\tibco\ems\clients\java\jndi.jar" noFactories="true"/&gt;
&lt;library jar="C:\local\tibco\ems\clients\java\jnet.jar" noFactories="true"/&gt;