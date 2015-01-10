---
layout: post
header-img: img/default-blog-pic.jpg
author: rnamta
description: 
post_id: 3091
created: 2010/02/08 18:00:36
created_gmt: 2010/02/08 13:00:36
comment_status: open
---

# An introduction to soapUI for testing web services

<p><a href="http://www.soapui.org/index.html">soapUI</a> is a free, open-source desktop application specifically designed to inspect, develope, invoke and test WSDL or REST based web services.In this blog I have tried to introduce the tool and list some of its most useful features.In my successive blogs I will dig deeper and try to explain how it can be used to test functionality and performance of web services by anyone who is just starting with web services testing.</p>
<p>I have been using soapUI for a while now and it not only provides the capability to test services for its functionality but is also quite capable of load testing them under different scenarios.Add to it the limited but powerful capability to do compliance testing and we almost have a one stop solution for our testing needs as far as web services are concerned.</p>
<p>Its intuitive and interactive GUI stacked with useful features makes web service functional and performance testing a breeze.Since its free and open-source its power mainly lies in scripting using groovy and the user must have some experience with it as trivial tasks like loading values from files and passing property values in between test steps would require the user to write scripts.The soapUI project is hosted on <a href="http://sourceforge.net/projects/soapui/)">sourceforge</a>.
<h3>soapUI Pro</h3>
A commercial version of soapUI, soapUI Pro, which adds expert professional support and a number of nifty features to the soapUI interface is available and its been gaining popularity within the development community (programmers and testers alike).This is mainly due to its ease of usage,low cost of licensing (resulting in high ROI) and a excellent support team.SoapUI pro adds to the core functionality of soapUI some very user friendly and powerful features which makes it possible for a user having very less and even negligible scripting experience to use the tool extensively for testing services.
<h3>Some powerful features of soapUI/soapUI Pro</h3>
Some of the features of the tool which are worth mentioning are listed below :
<ul>
    <li>Data driven testing of services to verify and assert request-response messages against a large set of data.</li>
    <li>Load testing to measure performance alongwith ensuring that the functionality is not breaking by applying different load test strategies.</li>
    <li>Schema and SOAP compliance assertions which can be included as default assertions in test cases to verify that incoming and outgoing messages are schema compliant and the response is not a soap fault.</li>
    <li>Xpath,Xquery and groovy assertions to handle response validations.</li>
    <li>WSDL,XSD and XML inspectors to verify wsdl,scehma and message content.</li>
    <li>Support for groovy scripting: This enables creation of generic libraries for common functions that can be reused.</li>
    <li>Covergae analyzer at project,test Suite and test case level to determine contract covergae.</li>
    <li>Fairly interactive and intuitive UI which can act as a service client and hence we are not required to build any additional client interface.</li>
    <li>A nifty editor for xml which provides different views(form and tree views in addition to xml view) for editing the xml requests and viewing responses</li>
    <li>Customizable reporting using JUinit Style HTML reports or data exports.</li>
    <li>A maven plug-in which can help integrate the tests with the build process.</li>
    <li>Command line runner for running functional and load tests which dispenses the need for the UI for executing the tests.</li>
    <li>Web service mocking and simulation.</li>
    <li>Some recently added features like REST and AMF support,JMS and JDBC test steps.</li>
    <li>Plugin for different IDEs like eclipse,Net Beans and IntelliJ.</li>
</ul>
<h3>Summary</h3>
Overall, I found the tool quiet powerful and capable of doing pretty much everything I wanted to do with it.Although there are a couple of things which need improvements such as the reporting in free version is pretty basic,security testing capability is still in its nascent stage and load testing cannot be done for concurrent users beyond certain limit (dependent on OS of the client machine) as it still does not support distributed load testing.</p>
<p>Despite this,I would encourage anyone involved in web service developement and testing to give their services a spin using the tool as it is worth the effort.I would say that its an excellent tool to get started with web service testing and can help developers and testers equally in developing and delivering robust and high quality web services timely and in an cost effective manner.</p>