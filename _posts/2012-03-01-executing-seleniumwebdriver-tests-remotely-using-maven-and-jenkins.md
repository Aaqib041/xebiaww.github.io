---
layout: post
header-img: img/default-blog-pic.jpg
author: rnamta
description: 
post_id: 12487
created: 2012/03/01 23:36:38
created_gmt: 2012/03/01 18:36:38
comment_status: open
---

# Executing Selenium/WebDriver tests remotely using Maven and Jenkins

<p>Recently our team created a nifty selenium test suite for testing a salesforce.com app we developed. We used <a href="http://testng.org/doc/index.html">TestNG </a> framework for writing tests along with the latest and greatest selenium available out there which is <a href="http://code.google.com/p/selenium/wiki/GettingStarted">WebDriver</a>. In case you want to know why we chose web driver over RC,read <a href="http://xebee.xebia.in/2012/02/07/selenium-web-driver-or-rc-decide-yourself/">this</a>. Since we planned to execute the tests on a remote windows machine triggered from our Jenkins machine (linux), we leveraged the capabilities of <a href="http://code.google.com/p/selenium/wiki/RemoteWebDriver">RemoteWebdriver</a>. Eventually we wanted to be able to run the tests with our Continuous Integration environment which meant that the tests are build on Jenkins (linux) with maven and executed on dedicated windows machine remotely.
<!--more-->
Following are the key objectives that we identified to make it happen:</p>
<ol>
<li>
<p>Configure/trigger the tests in way so that we can put them into the Jenkins machines as build jobs.</p>
</li>
<li>
<p>Use maven targets to be able to force a browser type at build time (for example: mvn integration-test -DBrowserType=chrome). Using this we can create multiple Jenkins jobs for execution of selenium tests against each browser.</p>
</li>
<li>
<p>Make remote execution capability configurable along with the location (IP and port) of the selenium server (if Remote Execution is true) so that we can easily switch server (if required) or use multiple servers simultaneously executing test against different browsers.</p>
</li>
<li>
<p>Run the tests on multiple browsers like FF,IE and chrome.</p>
</li>
</ol>
<p>With the help of this blog I intend to share the way we set about to achieve our objectives. Here I am assuming that the reader is aware of basic concepts of selenium and maven.</p>
<p>As a first step we created a maven project for the test suite and added the required dependencies to the POM file. We prepared a remote windows virtual machine with all the required browsers and downloaded the selenium-server-standalone-{VERSION}.jar on it.</p>
<p>In our framework, we added a few system properties in our BaseTest.java (A helper class used by all tests to instantiate a WebDriver/RemoteWebDriver instance).</p>
<p><code><span style="color: #993300;">String BrowserType = System.getProperty("BrowserType");</span>
<span style="color: #993300;"> String RemoteExecution = System.getProperty("RemoteExecution");</span>
<span style="color: #993300;"> String RemoteUrl = System.getProperty("RemoteUrl");</span>
</code></p>
<p>Next, we made the invocation of a browser type and remote execution capability dependent on these property values in our BaseTest.java.</p>
<p><span style="color: #000080;"><code>if(!<strong>RemoteExecution</strong>.equals("true")) <span style="color: #333333;">// remote execution is false</span></code></span>
<span style="color: #333333;"> <code>{</code></span>
<span style="color: #000080;"> <code>if(<strong>BrowserType</strong>.equals("firefox"))</code></span>
<span style="color: #000080;"> <code>{</code></span>
<span style="color: #000080;"> <code>WebDriver driver = new FirefoxDriver(); <span style="color: #333333;">// Create a new instance of firefox driver</span></code></span>
<code><span style="color: #000080;">}</span>
<span style="color: #000080;"> else if</span>
<span style="color: #000080;"> {</span>
<span style="color: #000080;"> code for ie and chrome goes here</span>
<span style="color: #000080;"> }</span>
<span style="color: #000080;"> else                              <span style="color: #333333;"> //remote execution is true</span></span>
<span style="color: #000080;"> <code> if(<strong>BrowserType</strong>.equals("firefox"))</code></span>
<span style="color: #000080;"> {</span>
<span style="color: #000080;"> WebDriver driver = new RemoteWebDriver(newURL(<strong>RemoteUrl</strong>+"/wd/hub"),DesiredCapabilities.firefox());</span>
<span style="color: #333333;"> //Create a new instance of RemoteWebDriver with the  remote server url</span>
<span style="color: #000080;"> }</span>
<span style="color: #000080;"> else if</span>
<span style="color: #000080;"> {</span>
<span style="color: #000080;"> code for ie and chrome goes here</span>
<span style="color: #000080;"> }</span>
</code></p>
<p>Finally, we configured the maven-surefire-plugin to run the tests in integration-test phase. In the plugin execution we added the parameter 'systemPropertyVariables' to pass values for the system properties defined above at build time.</p>
<p><span style="color: #000080;">&lt;plugin&gt;</span>
<span style="color: #000080;"> &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;</span>
<span style="color: #000080;"> &lt;artifactId&gt;maven-surefire-plugin&lt;/artifactId&gt;</span>
<span style="color: #000080;"> &lt;configuration&gt;</span>
<span style="color: #000080;"> &lt;skip&gt;true&lt;/skip&gt;</span>
<span style="color: #000080;"> &lt;/configuration&gt;</span>
<span style="color: #000080;"> &lt;executions&gt;</span>
<span style="color: #000080;"> &lt;execution&gt;</span>
<span style="color: #000080;"> &lt;phase&gt;integration-test&lt;/phase&gt;</span>
<span style="color: #000080;"> &lt;goals&gt;</span>
<span style="color: #000080;"> &lt;goal&gt;test&lt;/goal&gt;</span>
<span style="color: #000080;"> &lt;/goals&gt;</span>
<span style="color: #000080;"> &lt;configuration&gt;</span>
<span style="color: #000080;"> &lt;skip&gt;false&lt;/skip&gt;</span>
<span style="color: #000080;"> <strong> &lt;systemPropertyVariables&gt;</strong></span>
<span style="color: #000080;"> <strong> &lt;BrowserType&gt;firefox&lt;/BrowserType&gt;</strong></span>
<span style="color: #000080;"> <strong> &lt;RemoteExecution&gt;false&lt;/RemoteExecution&gt;</strong></span>
<span style="color: #000080;"> <strong> &lt;RemoteUrl&gt;http://127.0.0.1:4444&lt;/RemoteUrl&gt;</strong></span>
<span style="color: #000080;"> <strong> &lt;/systemPropertyVariables&gt;</strong></span>
<span style="color: #000080;"> &lt;suiteXmlFiles&gt;</span>
<span style="color: #000080;"> &lt;suiteXmlFile&gt;${project.build.directory}/test-classes/testng.xml&lt;/suiteXmlFile&gt;</span>
<span style="color: #000080;"> &lt;/suiteXmlFiles&gt;</span>
<span style="color: #000080;"> &lt;/configuration&gt;</span>
<span style="color: #000080;"> &lt;/execution&gt;</span>
<span style="color: #000080;"> &lt;/executions&gt;</span>
<span style="color: #000080;"> &lt;/plugin&gt;</span></p>
<p>Now once we had the server running on the remote VM (using java -jar selenium-server-standalone-{VERSION}.jar) we were able to build and execute our tests using the following maven command.
<code>
<span style="color: #993300;">mvn integration-test -DBrowserType=firefox -DRemoteExecution=true -DRemoteUrl=http://xyz.com:4444</span>
</code>
Now we already achieved our objective 1,2 and 3 but not 4. We ran into issues while running the tests on chrome remotely. We encountered an exception with the message 'The path to the chrome driver executable must be set by the webdriver.chrome.driver system property'. The solution to this is already discussed on <a href="http://stackoverflow.com/questions/6921170/how-to-build-remote-webdriver-for-chrome">stackoverflow</a>. In nutshell, you need to have the chromedriver.exe on the remote machine (where the selenium server is hosted) and run the selenium server on the remote machine with the following argument
<code>
<span style="color: #993300;"> java -Dwebdriver.chrome.driver="path-to-chromedriver.exe" -jar selenium-server-standalone.jar</span>
</code>
That's it. This enabled us to run our tests on any remote machine against a specific browser with ease in continuous integration environment. It would be great to hear what alternative approaches you have used to solve something similar.</p>

## Comments

**[Priyank](#7864 "2012-03-06 01:16:00"):** Good summarization. Like it.

**[Viva](#9126 "2012-07-03 21:37:29"):** Hi Rajneesh, Could you please provide example to test through Android Webdriver instead of RemoteWebDriver? We found that through remoteWedriver test are running slow. Thanks, Viva

**[Rayan](#8306 "2012-04-05 00:22:07"):** can you put your BaseTest class here ?

**[Ramesh palloli](#9310 "2013-01-19 11:13:45"):** Good post....Thank u so much ...it is very helpful to me...i started jenkins...can u please provide me Base Test Class.....

