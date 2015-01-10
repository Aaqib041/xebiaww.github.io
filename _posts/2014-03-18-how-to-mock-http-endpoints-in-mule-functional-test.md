---
layout: post
header-img: img/default-blog-pic.jpg
author: anirudh.xebia
description: 
post_id: 18160
created: 2014/03/18 14:21:40
created_gmt: 2014/03/18 09:21:40
comment_status: open
---

# How to mock HTTP endpoints in Mule Functional Test

<p><a href="http://www.mulesoft.org/what-mule-esb">Mule</a> is an enterprise service bus (ESB) and integration framework. In Mule, we define <a href="http://www.mulesoft.org/documentation/display/current/Flows+and+Subflows">flows and sub-flows</a> in order to integrate applications and orchestrate services.These flows contain endpoints to integrate different applications or services. These endpoints can be HTTP, VM, JMS, etc. More details about development in mule can be found <a href="http://www.mulesoft.org/documentation/display/current/Mule+User+Guide">here</a>. Below is a sample flow.
<a href="http://xebee.xebia.in/wp-content/uploads/2014/03/Screen-Shot-2014-03-18-at-1.12.54-PM.png"><img src="http://xebee.xebia.in/wp-content/uploads/2014/03/Screen-Shot-2014-03-18-at-1.12.54-PM-300x160.png" alt="Screen Shot 2014-03-18 at 1.12.54 PM" width="300" height="160" class="aligncenter size-medium wp-image-18167" /></a></p>
<p>In order to write unit test cases for mule flows, mule provides an abstract JUnit test case called <a href="http://www.mulesoft.org/docs/site/current3/apidocs/org/mule/tck/junit4/FunctionalTestCase.html">org.mule.tck.junit4.FunctionalTestCase</a> that runs Mule inside a test case and manages the lifecycle of the server.More details about can be found <a href="http://www.mulesoft.org/documentation/display/current/Functional+Testing">here</a>.
<!--more-->
Now, while writing these test cases, we face an issue when one application makes a call to another application using HTTP endpoint. For our unit test case to run independently, we need to mock this HTTP endpoint. There are few solutions available in the market like <a href="http://confluex.com">Confluex</a>, or <a href="http://wiremock.org">wireMock</a> but I did not want to increase the technology footprint of my application and thought of a simpler in-house solution.</p>
<p>To solve this problem, I used the embedded Jetty Server in my Functional Test Case.
[sourcecode]
public abstract class BaseTest extends FunctionalTestCase {</p>
<pre><code>static Server mockServer;
protected abstract int getPortNumber();

@Before
public void createServer() throws Exception {
    mockServer = new Server(getPortNumber());
    mockServer.setHandler(new MockHttpRequestHandler());
    mockServer.start();
}

@After
public void stopServer() throws Exception {
    mockServer.stop();
}

protected String get(String endpointURL, String contentType)
        throws URISyntaxException, ClientProtocolException, IOException {
    HttpClient hc = new DefaultHttpClient();
    HttpGet get = new HttpGet();
    get.setURI(new URI(endpointURL));
    get.setHeader(&amp;quot;Content-Type&amp;quot;, contentType);
    HttpResponse resp = hc.execute(get);
    return EntityUtils.toString(resp.getEntity());
}

protected String post(String payload, String endpointURL, String contentType)
        throws URISyntaxException, ClientProtocolException, IOException {
    HttpClient hc = new DefaultHttpClient();
    HttpPost post = new HttpPost();
    post.setURI(new URI(endpointURL));
    post.setHeader(&amp;quot;Content-Type&amp;quot;, contentType);
    post.setEntity(new StringEntity(payload));
    HttpResponse resp = hc.execute(post);
    return EntityUtils.toString(resp.getEntity());
}
</code></pre>
<p>}
[/sourcecode]
Let's understand this step by step :
[sourcecode]
public abstract class BaseTest extends FunctionalTestCase {
[/sourcecode]
To write a functional test case in mule, we need to extend FunctionalTestCase, this will start a mule instance inside the test case. As we have not overridden the method "getConfigFiles()" in this abstract base class, the config xml file required to start mule instance will be used from the class extending this baseTest class.</p>
<p>[sourcecode]
        static org.eclipse.jetty.server.Server mockServer;
[/sourcecode]
We make a reference to org.eclipse.jetty.server.Server, which would act as the mock server.
[sourcecode]
    protected abstract int getPortNumber();
[/sourcecode]
This method needs to be implemented by the class extending this baseTest class, this is done to externalise the port number, so that multiple tests can be configured using this base class.
[sourcecode]
    @Before
    public void createServer() throws Exception {
        mockServer = new Server(getPortNumber());
        mockServer.setHandler(new MockHttpRequestHandler());
        mockServer.start();
    }
[/sourcecode]
We use this method to create a new instance of the server and define the port number on which it will run.
The most important part of the implementation here is declaring handler for the server.
Lets discuss about handlers first.</p>
<p><strong>Jetty Handlers :</strong> Handlers in jetty can be used to filter requests and manipulate response and content. Using this api, we can write custom handlers and set them to the server instance.
<a href="http://www.eclipse.org/jetty/documentation/current/writing-custom-handlers.html">Here</a> is a wonderful tutorial which talks about creating custom handlers.</p>
<p>In our implementation we have made a class MockHttpRequestHandler extending abstractHandler.
[sourcecode]
public class MockHttpRequestHandler extends AbstractHandler {
@Override
    public void handle(String targetUri, Request baseRequest,
            HttpServletRequest request, HttpServletResponse response)
            throws IOException, ServletException {</p>
<pre><code>    String contentType = request.getHeader(&amp;quot;Content-Type&amp;quot;);
    if(targetUri.contains(&amp;quot;/mockTest&amp;quot;)){
        handleMockTest(response, contentType);
        baseRequest.setHandled(true);
    }
}

private void handleMockTest(HttpServletResponse response,String contentType)throws IOException  {

    InputStream in = this.getClass().getClassLoader()
            .getResourceAsStream(&amp;quot;/responseMockTest/file/path&amp;quot;);
    response.setContentType(contentType);
    String responseString = IOUtils.toString(in);
    response.setStatus(HttpServletResponse.SC_OK);
    response.getWriter().print(responseString);

}
</code></pre>
<p>}</p>
<p>[/sourcecode]
As we can see above, AbstractHandler declares a method handle(), which has request, response and uri. These can be used to filter requests ,check the uri and mock the content based on specific request or uri.
In the above we test the path and manipulate response accordingly in method handleMockTest.
We can see here, we are reading the response from a file in the class path. We are also setting the status and content Type of the response.
So, we can mock any response we want for a particular request here.</p>
<p>Also, in the end, one thing to note here is :
[sourcecode]
baseRequest.setHandled(true);
[/sourcecode]
Setting it true means, we are telling the server that we have mapped a response for the base Request, setting it to false will return in 404.</p>
<p>We can play around this, generalise this and make a framework for better and easier usage.</p>
<p>Finally, lets see a basic test case, which will use these 2 classes to mock the HTTP endpoint in a mule flow.
[sourcecode]
public class TestMockHttpEndPoint extends BaseTest {</p>
<pre><code>private static final String JSON_PAYLOAD = &amp;quot;{\&amp;quot;Authentication\&amp;quot;{\&amp;quot;client\&amp;quot;:\&amp;quot;10000003\&amp;quot;,\&amp;quot;password\&amp;quot;:\&amp;quot;qbgNqpzQBwB3DfAP\&amp;quot;}}&amp;quot;;
private static final String JSON_RESPONSE = &amp;quot;{\&amp;quot;status&amp;quot;:\&amp;quot;success\&amp;quot;}&amp;quot;;

@Override
protected String[] getConfigFiles() {
    String[] arr = {&amp;quot;mule-unit-test-mock-framework.xml&amp;quot;};
    return arr;
}

@Override
protected int getPortNumber() {
    return 10002;
}

@Test
public void shouldTestPOSTMockFramework() throws ClientProtocolException,
        URISyntaxException, IOException {
    Assert.assertEquals(
            JSON_RESPONSE,
            post(JSON_PAYLOAD, &amp;quot;http://localhost:10001/mockTest&amp;quot;,
                    &amp;quot;application/json&amp;quot;));
}

@Test
public void shouldTestGETMock() throws ClientProtocolException,
        URISyntaxException, IOException {
    get(&amp;quot;http://localhost:10001/mockTest&amp;quot;, &amp;quot;application/json&amp;quot;);
}
</code></pre>
<p>}
[/sourcecode]
Here, the flow "mule-unit-test-mock-framework.xml" is being tested, this mule app has inbound HTTP endpoint running on port 10001 and path /mockTest.
We are running our mock Jetty server on path 10002, as seen in the overridden method "getPortNumber()" .
This is what happens in the test: </p>

## Comments

**[jvanerp](#9470 "2014-03-18 15:11:56"):** Can't you use something like Mike Kotsur's Restito (https://github.com/mkotsur/restito) for this? It's a rest/http mocking framework in use at XebiaLabs.

**[Anirudh Bhatnagar](#9471 "2014-03-18 15:15:23"):** Of Course I can use this framework, I have explored this as well, but there was some strange jar conflict issue with Mule dependencies concerning hamcrest. I could have done some tweaking like excluding them from Mule, but was not sure if that was a great idea.

