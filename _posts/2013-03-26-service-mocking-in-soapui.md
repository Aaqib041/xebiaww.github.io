---
layout: post
header-img: img/default-blog-pic.jpg
author: prachinagpal
description: 
post_id: 16335
created: 2013/03/26 16:09:51
created_gmt: 2013/03/26 11:09:51
comment_status: open
---

# Service Mocking in SoapUI

<p>Few days back I encountered a situation in my project, where we had to test a web service to which we were not having access to since it was at client's side.</p>
<p>So, we decided to create a simulation or approximation of the Web Service before the actual Web Service by using the SOAP Service Mocking functionality in soapUI which is called a <strong>"MockService"</strong>.</p>
<p>So in this post, I will showcase how Web Service Mocking can be a useful tool in the testing arsenal.</p>
<p>One of the critical usage of this service is Client testing or development; create mock implementations of desired operations and set up a number of alternative responses (including scripts, attachments and custom http headers). Clients can be developed against the MockService and tested without access to the live services. We should use mocks when we can't use the real thing.</p>
<p>MockService, contains MockOperations which further contains MockResponses.
Taking the example of the MockService that we created in our project :-
<!--more-->
<p style="text-align: center"><a href="http://xebee.xebia.in/wp-content/uploads/2013/03/image1.png" target="_blank"><img class="size-full wp-image-16340 aligncenter" style="float: none" alt="image1" src="http://xebee.xebia.in/wp-content/uploads/2013/03/image1.png" width="296" height="101" /></a></p>
Here the MockService contains two MockOperations which each contain a different MockResponse messages.</p>
<p>Each <strong>MockOperation</strong> corresponds to the WSDL Operation of a WSDL Service in your testing project, and the MockResponse messages are generated from the schema associated with the operation.</p>
<p>After creating a MockService, we can generate it from WDSL by right clicking on Generate MockService action. This allows us to select the operations to be mocked and the path or port to be mounted on MockService.</p>
<p>After setting the desired configuration, a MockService is added to the project like this,
<p style="text-align: center"><a href="http://xebee.xebia.in/wp-content/uploads/2013/03/img1.png" target="_blank"><img class="size-medium wp-image-16341" style="float: none" alt="img1" src="http://xebee.xebia.in/wp-content/uploads/2013/03/img1-300x146.png" width="300" height="146" /></a></p>
These will be dispatched to the corresponding MockOperation based on their content.</p>
<p>Double Click on the MockOperation to see the MockResponses that we have in the MockService.The <strong>MockResponse</strong> editor is very similar to the standard SoapUI Request editor , but instead of requests, we can also edit responses.
<p style="text-align: center"><a href="http://xebee.xebia.in/wp-content/uploads/2013/03/img11.png" target="_blank"><img class="size-medium wp-image-16342 aligncenter" style="float: none" alt="img1" src="http://xebee.xebia.in/wp-content/uploads/2013/03/img11-300x167.png" width="300" height="167" /></a></p>
So, the above response was created for the mock service , which is a message to be returned by the MockService to the calling client.
Also, following window configuration reveals the response message to be returned by the second response.
<p style="text-align: center"><a href="http://xebee.xebia.in/wp-content/uploads/2013/03/img12.png" target="_blank"><img class="size-medium wp-image-16343 aligncenter" style="float: none" alt="img1" src="http://xebee.xebia.in/wp-content/uploads/2013/03/img12-300x158.png" width="300" height="158" /></a></p>
Also, following was the actual request for which the above mock response was created. Here you can give the endpoint of your local machine where you have actually created the simulation of the real environment.
<p style="text-align: center"><a href="http://xebee.xebia.in/wp-content/uploads/2013/03/img13.png" target="_blank"><img class="size-medium wp-image-16344 aligncenter" style="float: none" alt="img1" src="http://xebee.xebia.in/wp-content/uploads/2013/03/img13-300x197.png" width="300" height="197" /></a></p>
Now the MockService is ready to handle incoming SOAP requests on the configured path and port.</p>
<p>When clicking the Run button, the MockService is started immediately inside soapUI, you can see this if you click the jetty log tab at the bottom of the soapUI window.</p>
<p>You can also <strong><em>add dynamic behavior</em></strong> to your MockService by customizing it allowing you to mock more complex service functionality. For example, following can be the situations:-
<ul>
    <li>Transferring  values from the request to the response.</li>
    <li>Reading some data from a request and use its value to select which response to return</li>
    <li>Reading the response from a database instead of having it statically in soapUI</li>
    <li>Manually create a custom HTTP response etc.</li>
</ul>
Some of the <strong><em>benefits of Mocking</em></strong> that you can easily get on SoapUI's website too are as follows:-
<ul>
    <li>We can create tests in advance hereby giving us Test Driven Development.</li>
    <li>Teams can work in parallel since QA can create tests for which the code  doesn't exists by mocking.</li>
    <li>We can create proof of concepts or demos for actual design.</li>
    <li>We can write test for resource not accessible. We can just create a MockService on our local machine and can test a service which actually exists at client side.</li>
    <li>We can test the Live Environments before you have the service in it. We can mock the actual environment and can reveal the problems that can occur in the real environment.</li>
</ul>
Everything is of course not perfect! So,<strong><em> few lags</em></strong> that mocking comes with are :-
<ul>
    <li>Since mock is for short span of period so it may be a case that one puts too much effort in MockService or it may result to do rework for the actual service.</li>
    <li>Sometimes there is a chance that mock representation is not accurate. For instance, there can be a difference between you and your photo. Just like that, A MockService can misrepresent the actual service.</li>
</ul>
Lags above aside, Mocking is extremely powerful if used correctly.</p>
<p><strong><em>Happy Mocking! Good Luck :)</em></strong></p>