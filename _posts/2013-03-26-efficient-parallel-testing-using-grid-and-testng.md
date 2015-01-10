---
layout: post
header-img: img/default-blog-pic.jpg
author: srentala
description: 
post_id: 16435
created: 2013/03/26 12:06:36
created_gmt: 2013/03/26 07:06:36
comment_status: closed
---

# Efficient Parallel Testing using GRID and TestNG

<p><span style="font-family: Constantia,serif;"><span style="font-size: small;">There is no point explaining, how efficient ‘Automation’ is for a Product Testing. Now imagine ‘icing on the cake’, when parallel testing is introduced along with it. I will showcase how Parallel testing could reduce time and effort, and the same time run the test cases parallel on multiple machines ,operating systems and browsers in this particular blog.</span></span>
<span style="font-family: Constantia,serif;"><span style="font-size: small;">Traditional Execution of test cases occurred serially, where automation scripts would execute one at a time (even though the CPU resources or other machines are free). Imagine why would one does not want to utilize these idle resources? Somebody imagined out of the box and came up with parallel testing.</span></span>
<span style="font-family: Constantia,serif;"><span style="font-size: small;">Many Frameworks and utilities are provided and came into existence through which one can implement the same. I will explain one such functionality provided by a framework, which I found very interesting and extremely useful while applying parallel testing using it.</span></span></p>
<p><span style="font-family: Constantia,serif;"><span style="font-size: medium;"><b>GRID</b></span></span>
<span style="font-family: Constantia,serif;"><span style="font-size: small;">Selenium GRID provides the functionality of distributing the tests on different machines (on different operating systems/browsers).</span></span>
<span style="font-family: Constantia,serif;"><span style="font-size: small;">Let us see the steps to run tests using Selenium GRID.</span></span></p>
<!--more-->

<p><span style="font-family: Constantia,serif;"><span style="font-size: medium;"><b>1) Download the Selenium-Server-Standalone JAR:</b></span></span>
<span style="font-family: Constantia,serif;"><span style="font-size: small;">GRID contains a central HUB to which various NODES (machines) are connected. More the number of machines, more the scalability of parallelism. One has to download the selenium-server-standalone jar from <a title="Selenium Server Standalone Jar" href="http://docs.seleniumhq.org/download/" target="_blank">http://docs.seleniumhq.org/download/</a>.
On each node, a port is used where selenium test cases runs. Each node on itself can have multiples nodes running on different ports.</span></span></p>
<p><span style="font-family: Constantia,serif;"><span style="font-size: medium;"><b>2) Make a Machine HUB:</b></span></span>
<span style="font-family: Constantia,serif;"><span style="font-size: small;">In order to make one of the machines as hub, go to the folder where the jar is downloaded and run the following command from the command prompt.</span></span>
<code>java –jar selenium-server-standalone.jar –role hub</code>
<span style="font-family: Constantia,serif;"><span style="font-size: small;">This will start the hub at 4444 port (default).</span></span></p>
<p><span style="font-family: Constantia,serif;"><span style="font-size: medium;"><b>3) Start the NODE/Client:</b></span></span>
<span style="font-family: Constantia,serif;"><span style="font-size: small;">In order to make a machine as a node, so that test can be executed at them, we need to register each node with the HUB running. So that, the HUB can see how many nodes are registered to it. After which it will divide the number of test cases (T) by the number of nodes (N). So, now each node will run (T/N) test cases.
In order to start a node, we will use the following command:</span></span>
<code>java - jar selenium-server-standalone-2.30.0.jar -role webdriver -hub http://localhost:4444/grid/register</code>
<span style="font-family: Constantia,serif;"><span style="font-size: small;">a)Role: It tells whether the role to be given is of hub/node.
b)Hub: The value for this parameter is the path where the hub is running. It could be either the localhost or any machine’s IP.</span></span></p>
<p><span style="font-family: Constantia,serif;"><span style="font-size: medium;"><b>4) Check the Status:</b></span></span>
<span style="font-family: Constantia,serif;"><span style="font-size: small;">To see if the HUB has started and number of nodes registered with it, we will check the GRID console running at the HUB’s machine by going to the url: <a href="localhost:4444/grid/console" target="_blank">localhost:4444/grid/console</a>. It will show the following page:</span></span></p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2013/03/grid_console.jpg"><img class="aligncenter size-full wp-image-16280" alt="pic1" src="http://xebee.xebia.in/wp-content/uploads/2013/03/grid_console.jpg" width="680" height="386" /></a></p>
<p><span style="font-family: Constantia,serif;"><span style="font-size: small;">
a) It shows GRID HUB selenium-server-standalone jar version.
b) The node IP and the port at which it is listening the GRID.
c) Timeout for tests is 5 minutes.
d) This node can support maximum of 5 Firefox, 5 Chrome and 1 IE instance.</span></span></p>
<p><span style="font-family: Constantia,serif;"><span style="font-size: medium;"><b>5) Set MaxInstances and maxSessions:</b></span></span>
<span style="font-family: Constantia,serif;"><span style="font-size: small;">Sometimes people get confused between maxInstances and maxSessions. Let us see what the difference is:
a. maxInstances = Number of maximum browser instances that can be opened.
b. maxSessions = Number of browsers that can be opened in a single time.
In order to set the following we can use the complete command as: </span></span></p>
<p><code>java -jar selenium-server-standalone-2.30.0.jar -role webdriver -hub http://localhost:4444/grid/register -port 5555 -browser browserName=chrome,maxInstances=5, -browser browserName=firefox,maxInstances=3, -browser browserName=iexplore,maxInstances=8 –maxSession 3</code>.</p>
<p><span style="font-family: Constantia,serif;"><span style="font-size: medium;"><b>6) IE and Chrome Driver Path:</b></span></span>
<span style="font-family: Constantia,serif;"><span style="font-size: small;">As we know that to run chrome and IE we need their external servers(chromedriver.exe and iedriver.exe). to include them in the command while running tests on IE and chrome use the following command where the paths to drivers are provided.</span></span></p>
<p><code>java - Dwebdriver.chrome.driver=C:\RVS\Automation\JarFiles\chromedriver\chromedriver.exe -Dwebdriver.ie.driver=C:\RVS\Automation\JarFiles\internetdriver\IEDriverServer.exe -jar selenium-server-standalone-2.30.0.jar -role webdriver -hub http://localhost:4444/grid/register -port 5555 -browser browserName=chrome,maxInstances=3, -browser browserName=firefox,maxInstances=3, -browser browserName=iexplore,maxInstances=3 –maxSession 3</code>.</p>
<p><span style="font-family: Constantia,serif;"><span style="font-size: medium;"><b>7) Configuring JSON to configure NODE and HUB:</b></span></span>
<span style="font-family: Constantia,serif;"><span style="font-size: small;">We can use a JSON file to initialize the key pair values of the parameters used above rather than specifying them in the command prompt every time to make a messy and long command.
This is how we can create our JSON file:</span></span></p>
<p><code>{
"capabilities":
[
{
"browserName": "firefox",
"maxInstances": 2
},</code></p>
<p><code>{
"browserName": "chrome",
"maxInstances": 2
},</code>
<code>
{
"browserName": "iexplore",
"maxInstances": 2
},</code>
<code>],</code></p>
<p><code>"configuration":
{
"port":5555,
"host": IP</code></p>
<p><code> "hubPort":4444
"hubHost": IP
"maxSession":6,
}
}</code></p>
<p><span style="font-family: Constantia,serif;"><span style="font-size: small;">Now we can use this json file(hubjson.json and nodejson.json) as a config file to start a HUB or a NODE by giving the following command:
a) For HUB:
<code> java –jar selenium-server-standalone-2.30.0.jar -role hub -hubConfig hubjson.json</code>
b) For NODE:
<code>java –jar selenium-server-standalone-2.30.0.jar -role webdriver -nodeConfig nodejson.json</code>.
</span></span></p>
<p><span style="font-family: Constantia,serif;"><span style="font-size: medium;"><b>8) Create Test Script:</b></span></span></p>