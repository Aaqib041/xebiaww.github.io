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

Recently, in my project we came across a situation where headers were to be passed in the application via URL. We tried passing it through many inbuilt functions which are present in Webdriver but failed to do that. After struggling for one day, we realized that Webdriver misses this functionality of passing HTTP headers and showing it in the reponse headers. Selenium developers have taken this decision of not adding this feature to Webdriver since it will suffer its overall quality. It is logged as [issue #141][1]   and is marked as "won't fix" after all the discussions.

So the solution is,  if we want to test HTTP request/response headers, then we have to write an appropriately customized tests. All we need is a simple HTTP client library ([HtmlUnit ][2]already offers some of these capabilities but it runs in a headless java browser with limited support so it might not run all your test cases). If we really want to test the response headers then, we need to setup a proxy for the browsers so that we can capture net traffic and modify headers there.

WebDriver is concerned with driving the browser. In the case of inspecting HTTP traffic, the only way to do this in a cross-browser way is via a proxy. WebDriver exposes APIs to make it easy to set the proxy to use for a browser instance, so you can hook in whichever is the most suitable tool for your project. We also used Fiddler, a commercial tool to capture the headers.Selenium is a browser automation framework, not an HTTP recording framework, and as such HTTP recording and manipulation is out of scope for the project. In the common case, where http headers are not required, we can still run without an additional server. But in the case where that data is important to us, we will need a separate server. A proxy solution, will work for every browser, and provide us with status codes, HTTP headers, and much more information.To mention, Proxy Server is a small library named Browser Mob Proxy which is open source and availlable on [github][3]:

The simple workaround is to write the response code . So, we created the following piece of code using ProxyServer in my selenium framework to pass headers in the URL :-

_**Steps to intercept Web Requests and modify Headers:**_ **1.** In your Selenium test suite, initialize proxy server as a global variable and start it in before running any test in the suite

[sourcecode lang="text"] ProxyServer server = new ProxyServer(4444); server.start();

[/sourcecode]

**2.** Create a Generic method which takes a list of headers and then adds a requestInterceptor on the proxy server. Inside the interceptor, we can pass the headers and write them onto the incoming request.

[sourcecode lang="text"]

if ( headersList!= null && !headersList.isEmpty()) { for (Entry<String,String> header : headersList.entrySet()) { server.addHeader(header.getKey(), header.getValue()); } }

[/sourcecode]

**3.** Now call this generic method in every test case where you want to modify some request headers. IMPORTANT: Make sure that you explicitly clear out any interceptors that might have added in previous test cases since this generic method would be called from all such test cases.

[sourcecode lang="text"]

// get the Selenium proxy object Proxy proxy = server.seleniumProxy();

if(browserName.equalsIgnoreCase("firefox")){ FirefoxProfile ffprofile = new FirefoxProfile(); ffprofile.setPreference("network.proxy.http", "localhost"); ffprofile.setPreference("network.proxy.http_port", 4444); ffprofile.setPreference("network.proxy.ssl", "localhost"); ffprofile.setPreference("network.proxy.ssl_port", 4444); ffprofile.setPreference("network.proxy.type", 1); ffprofile.setPreference("network.proxy.no_proxies_on", ""); ffprofile.setAcceptUntrustedCertificates(true); ffprofile.setAssumeUntrustedCertificateIssuer(true); ffprofile.setPreference( "webdriver_assume_untrusted_issuer", "false");

[/sourcecode]

**4.** Now add the proxy server instance while setting up the capabilities of the browser desired for testing.

[sourcecode lang="text"] DesiredCapabilities capabilities = new DesiredCapabilities(); capabilities.setCapability(FirefoxDriver.PROFILE,ffprofile); capabilities.setCapability(CapabilityType.PROXY, proxy); driver = new FirefoxDriver(capabilities); } else if(browserName.equalsIgnoreCase("internetexplorer")){ DesiredCapabilities iecapabilities = DesiredCapabilities .internetExplorer(); iecapabilities.setCapability("ignoreProtectedModeSettings", "true"); iecapabilities .setCapability( InternetExplorerDriver.INTRODUCE_FLAKINESS_BY_IGNORING_SECURITY_DOMAINS, "true"); iecapabilities.setCapability(CapabilityType.PROXY, proxy); driver = new InternetExplorerDriver(iecapabilities); } else if(browserName.equalsIgnoreCase("chrome")){

DesiredCapabilities chromeCapabilities = DesiredCapabilities.chrome(); chromeCapabilities.setCapability("chrome.switches", Arrays.asList("\--start-maximized","\--ignore-certificate-errors")); // added capability for proxy server for adding custom headers chromeCapabilities.setCapability(CapabilityType.PROXY, proxy);

//chromeCapabilities.setCapability("chrome.switches", Arrays.asList("\--ignore-certificate-errors")); ChromeDriverService service = new ChromeDriverService.Builder().usingAnyFreePort().usingDriverExecutable(new File("chromedriver.exe")).build(); service.start(); driver = new ChromeDriver(service,chromeCapabilities);

}

[/sourcecode]

**5.** Now run the test and intercept all the requests using fiddler.([Fiddler][4] was also not able to capture all the headers. So,a tool called [Wireshark ][5] can be used to do the same). **6\. **Verify if the customized headers have been added/updated in your web request.

So, the above piece of code helped me in adding HTTP headers with the request.

   [1]: https://code.google.com/p/selenium/issues/detail?id=141
   [2]: https://code.google.com/p/selenium/wiki/HtmlUnit
   [3]: https://github.com/webmetrics/browsermob-proxy
   [4]: http://www.fiddler2.com/fiddler2/
   [5]: http://www.wireshark.org/download.html