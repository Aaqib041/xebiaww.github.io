---
layout: post
header-img: img/default-blog-pic.jpg
author: jgupta
description: 
post_id: 18879
created: 2014/08/30 16:17:04
created_gmt: 2014/08/30 11:17:04
comment_status: open
---

# Extending Selenium Grid capabilities

<p>Selenium 2.xx has built-in Grid Functionality which provides support for distributed test execution i.e. multiple test automation scripts can be run at the same time on different machines running on different OS and browsers. Grid uses a DefaultCapabilityMatcher to match the machine(Node) requested by test script in the grid setup. Currently it uses only the Platform, Browser and its version to find the requested node. This can be extended to match the RAM or the System Type or any other Machine configuration to decide the node.
<!--more-->
<strong>Why?</strong></p>
<p>Suppose you have a grid with 100 nodes and you want to run your tests on 64-bit machines to check for specific errors or their performance on this system type. You have 100 proxies to deal with. You will have to go and check the system type of each, and unregister the nodes that have 32-bit or any other system type. This becomes very tedious job. To make this simple, we can extend the existing DefaultCapabilitMatcher class to add a capability to match the system type. Once this is done, grid will make sure to execute tests on the machines with the right capabilities.</p>
<p><strong>Setting up</strong></p>
<p>Open the <a title="DefaultCapabilityMatcher" href="https://code.google.com/p/selenium/source/browse/java/server/src/org/openqa/grid/internal/utils/DefaultCapabilityMatcher.java?spec=svn95dd5912a5c741eacf1c0ea9a37abd494deb79d8&amp;name=selenium-2.23.0&amp;r=95dd5912a5c741eacf1c0ea9a37abd494deb79d8" target="_blank">DefaultCapabilityMatcher</a> code and go through the logic of “matches” method. This is responsible for checking if the capability (OS, Browser and Version) requested by Test script is available with the grid. We can also write this method from scratch by directly Implementing the interface <a title="CapabilityMatcher" href="https://code.google.com/p/selenium/source/browse/java/server/src/org/openqa/grid/internal/utils/CapabilityMatcher.java" target="_blank">CapabilityMatcher</a>.</p>
<p>For now, we want to keep the existing code to match OS, browser and version as it is, and just add a feature to match the system type also. Let us see how we can get this going.</p>
<p>1.Add a new Java Project in Eclipse. Name it as “ExtendingGrid”.
2.Add a new Class “MyCapabilityMatcher”.
3.Add the latest selenium-server-standalone jar to the project class path.
4.Write the code to include capabilities for OS Type or installed RAM.</p>
<p><strong>Writing code to extend DefaultCapabilityMatcher</strong></p>
<p>[code language="java"]
import org.openqa.grid.internal.utils.DefaultCapabilityMatcher;
import java.util.Map;</p>
<p>public class MyCapabilityMatcher extends DefaultCapabilityMatcher {
    private final String nodeOS = &quot;nodeOS&quot;;
    boolean allCapabilitiesAvailability = false;
    @Override
    public boolean matches(Map&lt;String, Object&gt; nodeCapability, Map&lt;String, Object&gt; requestedCapability) {
        boolean defaultCapabilitiesAvailability = super.matches(nodeCapability, requestedCapability);
        if (! requestedCapability.containsKey(nodeOS)){
            return defaultCapabilitiesAvailability;
        }
        else{
            boolean nodeOSAvailability = nodeCapability.get(nodeOS).equals(requestedCapability.get(nodeOS));
            allCapabilitiesAvailability = (defaultCapabilitiesAvailability &amp;&amp; nodeOSAvailability);
            return allCapabilitiesAvailability;
        }
    }
}
[/code]</p>
<p><strong>How it works:</strong></p>
<p>This class will first get the result of matching the original default capabilities and save it in defaultCapabilitiesAvailability. It will then check if the user has requested for the System Type also. If the test script has not asked for any specific system type, then this program will return the result of default capabilities availability. In case, the test script has asked for a system type Eg: 64-bit, then the code will reach the else block. Here it will check if the requested 64-bit OS is there in the available nodes, and return the result. To test the above code, we can write a test script requesting for 64-bit OS as given below.</p>
<p><strong>Writing Selenium Webdriver Test requesting 64-bit OS</strong></p>
<p>[code language="java"]
import java.net.MalformedURLException;
import java.net.URL;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.openqa.selenium.remote.RemoteWebDriver;</p>
<p>public class sampleTest {
    public static void main ( String [] args){
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setBrowserName(&quot;firefox&quot;);
        capabilities.setCapability(&quot;nodeOS&quot;, &quot;64-bit&quot;);
        RemoteWebDriver remoteWD = null;
        try{
            remoteWD = new RemoteWebDriver(
                    new URL(&quot;http://127.0.0.1:4444/wd/hub&quot;),
                    capabilities);
        }catch(MalformedURLException e){
            e.printStackTrace();
        }
        remoteWD.get(&quot;http://www.xebia.com&quot;);
        //Add your code to test different things
        remoteWD.quit();
    }
}
[/code]</p>
<p><strong>Steps to run the test</strong></p>
<p>To use this in our Grid setup, we need to create a jar for the above project. Place it along with selenium standalone jar and add both of them to the classpath. Here is the sample command to start the hub:</p>
<p><strong>&gt;java -cp *;. org.openqa.grid.selenium.GridLauncher -role hub -capabilityMatcher MyCapabilityMatcher</strong></p>
<p>This will start Hub at the default port 4444. We need to specify the capability while starting the node:</p>
<p><strong>&gt;java -jar selenium-server-standalone-2.42.2.jar -role node -hub http://127.0.0.1:4444/grid/register -browser browserName=firefox,maxInstances=3,nodeOS=64-bit</strong></p>
<p>This will start the Node at the default port 5555. All the tests that request for 64-bit OS and firefox browser or just firefox browser will be routed to this node. To confirm the configuration, launch browser and try to access the grid at http://localhost:4444/grid/console#.</p>
<p>You should get something like this:</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/08/Configuration.png"><img class="alignnone size-full wp-image-18881" alt="Configuration" src="http://xebee.xebia.in/wp-content/uploads/2014/08/Configuration.png" width="1011" height="680" /></a>In the above page, you can see that the node is using MyCapabilityMatcher and has a 64-bit OS. Now you can run the tests and it will find the machine with 64-bit OS and run the test. If we modify the test script to ask for a 32-bit OS and if it is not available in the grid, then it will give an error saying the new session cannot find the Capabilities.</p>
<p><strong>Conlusion:</strong></p>
<p>Extending Grid capabilities will not only help to manage the nodes better, it will also give a better idea about what is going on during test script execution and help in debugging test failures.</p>