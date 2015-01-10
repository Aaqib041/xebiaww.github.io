---
layout: post
header-img: img/default-blog-pic.jpg
author: prachinagpal
description: 
post_id: 15924
created: 2013/02/28 10:09:53
created_gmt: 2013/02/28 05:09:53
comment_status: open
---

# Webdriver: No HTTP Response Headers

<p>Recently, in my project we came across a situation where headers were to be passed in the application via URL. We tried passing it through many inbuilt functions which are present in Webdriver but failed to do that.
After struggling for one day, we realized that Webdriver misses this functionality of passing HTTP headers and showing it in the reponse headers.
Selenium developers have taken this decision of not adding this feature to Webdriver since it will suffer its overall quality. It is logged as <a href="https://code.google.com/p/selenium/issues/detail?id=141">issue #141</a>   and is marked as "won't fix" after all the discussions.</p>
<p>So the solution is,  if we want to test HTTP request/response headers, then we have to write an appropriately customized tests. All we need is a simple HTTP client library (<a href="https://code.google.com/p/selenium/wiki/HtmlUnit">HtmlUnit </a>already offers some of these capabilities but it runs in a headless java browser with limited support so it might not run all your test cases). If we really want to test the response headers then, we need to setup a proxy for the browsers so that we can capture net traffic and modify headers there.</p>
<!--more-->

<p>WebDriver is concerned with driving the browser. In the case of inspecting HTTP traffic, the only way to do this in a cross-browser way is via a proxy. WebDriver exposes APIs to make it easy to set the proxy to use for a browser instance, so you can hook in whichever is the most suitable tool for your project.
We also used Fiddler, a commercial tool to capture the headers.Selenium is a browser automation framework, not an HTTP recording framework, and as such HTTP recording and manipulation is out of scope for the project.
In the common case, where http headers are not required, we can still run without an additional server. But in the case where that data is important to us, we will need a separate server. A proxy solution, will work for every browser, and provide us with status codes, HTTP headers, and much more information.To mention, Proxy Server is a small library named Browser Mob Proxy which is open source and availlable on <a href="https://github.com/webmetrics/browsermob-proxy">github</a>:</p>
<p>The simple workaround is to write the response code .
So, we created the following piece of code using ProxyServer in my selenium framework to pass headers in the URL :-</p>
<p><em><strong>Steps to intercept Web Requests and modify Headers:</strong></em>
<strong>1.</strong> In your Selenium test suite, initialize proxy server as a global variable and start it in before running any test in the suite</p>
<p>[sourcecode lang="text"]
 ProxyServer server = new ProxyServer(4444);
 server.start();</p>
<p>[/sourcecode]</p>
<p><strong>2.</strong> Create a Generic method which takes a list of headers and then adds a requestInterceptor on the proxy server. Inside the interceptor, we can pass the headers and write them onto the incoming request.</p>
<p>[sourcecode lang="text"]</p>
<p>if ( headersList!= null &amp;&amp; !headersList.isEmpty())
 {
 for (Entry&lt;String,String&gt; header : headersList.entrySet())
 {
 server.addHeader(header.getKey(), header.getValue());
 }
 }</p>
<p>[/sourcecode]</p>
<p><strong>3.</strong> Now call this generic method in every test case where you want to modify some request headers. IMPORTANT: Make sure that you explicitly clear out any interceptors that might have added in previous test cases since this generic method would be called from all such test cases.</p>
<p>[sourcecode lang="text"]</p>
<p>// get the Selenium proxy object
 Proxy proxy = server.seleniumProxy();</p>
<p>if(browserName.equalsIgnoreCase(&quot;firefox&quot;)){
 FirefoxProfile ffprofile = new FirefoxProfile();
 ffprofile.setPreference(&quot;network.proxy.http&quot;, &quot;localhost&quot;);
 ffprofile.setPreference(&quot;network.proxy.http_port&quot;, 4444);
 ffprofile.setPreference(&quot;network.proxy.ssl&quot;, &quot;localhost&quot;);
 ffprofile.setPreference(&quot;network.proxy.ssl_port&quot;, 4444);
 ffprofile.setPreference(&quot;network.proxy.type&quot;, 1);
 ffprofile.setPreference(&quot;network.proxy.no_proxies_on&quot;, &quot;&quot;);
 ffprofile.setAcceptUntrustedCertificates(true);
 ffprofile.setAssumeUntrustedCertificateIssuer(true);
 ffprofile.setPreference(
 &quot;webdriver_assume_untrusted_issuer&quot;, &quot;false&quot;);</p>
<p>[/sourcecode]</p>
<p><strong>4.</strong> Now add the proxy server instance while setting up the capabilities of the browser desired for testing.</p>
<p>[sourcecode lang="text"]
 DesiredCapabilities capabilities = new DesiredCapabilities();
 capabilities.setCapability(FirefoxDriver.PROFILE,ffprofile);
 capabilities.setCapability(CapabilityType.PROXY, proxy);
 driver = new FirefoxDriver(capabilities);
 }
 else if(browserName.equalsIgnoreCase(&quot;internetexplorer&quot;)){
 DesiredCapabilities iecapabilities = DesiredCapabilities
 .internetExplorer();
 iecapabilities.setCapability(&quot;ignoreProtectedModeSettings&quot;,
 &quot;true&quot;);
 iecapabilities
 .setCapability(
 InternetExplorerDriver.INTRODUCE_FLAKINESS_BY_IGNORING_SECURITY_DOMAINS,
 &quot;true&quot;);
 iecapabilities.setCapability(CapabilityType.PROXY, proxy);
 driver = new InternetExplorerDriver(iecapabilities);
 }
 else if(browserName.equalsIgnoreCase(&quot;chrome&quot;)){</p>
<p>DesiredCapabilities chromeCapabilities = DesiredCapabilities.chrome();
 chromeCapabilities.setCapability(&quot;chrome.switches&quot;, Arrays.asList(&quot;--start-maximized&quot;,&quot;--ignore-certificate-errors&quot;));
 // added capability for proxy server for adding custom headers
 chromeCapabilities.setCapability(CapabilityType.PROXY, proxy);</p>
<p>//chromeCapabilities.setCapability(&quot;chrome.switches&quot;, Arrays.asList(&quot;--ignore-certificate-errors&quot;));
 ChromeDriverService service = new ChromeDriverService.Builder().usingAnyFreePort().usingDriverExecutable(new File(&quot;chromedriver.exe&quot;)).build();
 service.start();
 driver = new ChromeDriver(service,chromeCapabilities);</p>
<p>}</p>
<p>[/sourcecode]</p>
<p><strong>5.</strong> Now run the test and intercept all the requests using fiddler.(<a href="http://www.fiddler2.com/fiddler2/">Fiddler</a> was also not able to capture all the headers. So,a tool called <a href="http://www.wireshark.org/download.html">Wireshark </a> can be used to do the same).
<strong>6. </strong>Verify if the customized headers have been added/updated in your web request.</p>
<p>So, the above piece of code helped me in adding HTTP headers with the request.</p>