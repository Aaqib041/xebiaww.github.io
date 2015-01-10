---
layout: post
header-img: img/default-blog-pic.jpg
author: ykapoor
description: 
post_id: 3464
created: 2010/05/10 16:37:09
created_gmt: 2010/05/10 11:37:09
comment_status: open
---

# Maven Based Automated Execution of FitNium Acceptance Tests 

<h3><strong>Intent</strong></h3>

<p><strong> </strong>To automate execution of the FitNium acceptance  tests and generate reports using maven. This is particularly useful in  test driven and continuous integration environment.
<h3><strong>Motivation</strong></h3>
In one<strong> </strong>of the recent projects we wanted to integrate FitNium acceptance tests with maven based build system.We were using FitNium as it allows us to<strong> </strong>write and execute Selenium tests  using the  FitNesse framework but  without the need to write a single  line of   code.</p>
<p>We had created the FitNium test script to test our application. FitNium test script is based on FitNesse doFixture and provides a English language interpretation of  the Selenium API.</p>
<p>As part of the maven build system we wanted</p>
<ul>
<li>
<p>Automatic start up of the servers (Application server, Selenium and FitNesse)</p>
</li>
<li>
<p>Execution of the acceptance tests along with the report generation</p>
</li>
<li>
<p>Stopping the servers</p>
</li>
</ul>
<p>How we accomplished this forms the basis of this post.</p>
<!--more-->

<p><strong>Implementation Details
</strong></p>
<p>The tools we are going to use are:</p>
<p><strong>Selenium</strong>: Used to automate web application testing
<strong>FitNesse</strong>: Collaborative tool to write acceptance tests
<strong>FitNium</strong>: FitNesse + Selenium
<strong>Cargo</strong>: For deploying applications to containers from ant or maven builds
<strong>Maven</strong>: Build tool</p>
<p>Assumption here is that the FitNium tests have been created, so we can concentrate on automating the execution of the FitNium tests.</p>
<p>Let’s start with a very simple FitNium test script to serve as an example</p>
<p>[sourcecode language="css"]
!path fitnesse.jar
!path fitlibrary.jar
!path fitnium.jar
!path selenium-java-client-driver.jar</p>
<p>!|com.magneticreason.fitnium.BaseFitniumFixture|</p>
<p>Set up the browser and connect to selenium</p>
<p>|The server is located at | localhost |
|The server is on port | 9090 |
|Using the browser | *firefox | start at | http://localhost:8080/testApp/pages/login.seam |
|check | is selenium initialised | true |
|set speed to |50| milliseconds |
|set timeout to |50000| milliseconds |
|write to debug | starting test |</p>
<p>Lets try to login</p>
<p>|starting at URL| http://localhost:8080/testApp/pages/login.seam|
| enter | admin | in input field | name=home:username_input|
| enter | admin | in input field | name=home:password_input|
| check | does button With Id | !-home:LoginBtn-! | exist | true |
|click button | !-home:LoginBtn-! |
|wait For Page To Load For | 5 | seconds |
|write to debug | test complete |
|close the browser|
[/sourcecode]</p>
<p>Some things worth noting in the FitNium test script above are
<ol>
    <li><strong>!path</strong> &lt;jar&gt; notation refers to the FitNesse class path</li>
    <li><strong><em>The server is on port</em></strong> should have the port configured for the selenium server. For me it’s 9090</li>
    <li><strong><em>Starting at URL</em></strong> should point to the URL your application test should start at</li>
</ol>
Now with the knowledge of the required tools and the test script we will get onto the steps for automating the execution of FitNium tests.
There are mainly five things involved in the whole process
<ol>
    <li>Installing the application to be tested</li>
    <li>Starting the application server</li>
    <li>Starting the Selenium and FitNesse server</li>
    <li>Running the tests</li>
    <li>Stopping the servers started in step 1, 2 and 3</li>
</ol>
<h4>Step 1: Installing the application:</h4>
<strong> </strong></p>
<p>This can be accomplished using <strong><a href="http://cargo.codehaus.org/Downloads">cargo-maven2-plugin</a>.</strong></p>
<p>The pom entry for the plug-in will look like below</p>
<p>[sourcecode lang="xml"]
&lt;plugin&gt;
 &lt;groupId&gt;org.codehaus.cargo&lt;/groupId&gt;
 &lt;artifactId&gt;cargo-maven2-plugin&lt;/artifactId&gt;
 &lt;version&gt;1.0&lt;/version&gt;
 &lt;configuration&gt;
 &lt;wait&gt;false&lt;/wait&gt;
 &lt;container&gt;
 &lt;containerId&gt;jboss5x&lt;/containerId&gt;
 &lt;home&gt;${JBOSS_HOME}&lt;/home&gt;
 &lt;timeout&gt;300000&lt;/timeout&gt;
 &lt;systemProperties&gt;
 &lt;!-- This is used in jboss-log4j.xml --&gt;
 &lt;jboss.server.log.threshold&gt;INFO&lt;/jboss.server.log.threshold&gt;
 &lt;/systemProperties&gt;
 &lt;/container&gt;
 &lt;configuration&gt;
 &lt;type&gt;existing&lt;/type&gt;
 &lt;home&gt;${JBOSS_HOME}\server\default&lt;/home&gt;
 &lt;properties&gt;
 &lt;cargo.rmi.port&gt;1099&lt;/cargo.rmi.port&gt;
 &lt;cargo.jvmargs&gt;-XX:PermSize=256m -XX:MaxPermSize=512m
 &lt;/cargo.jvmargs&gt;
 &lt;/properties&gt;
 &lt;/configuration&gt;
 &lt;/configuration&gt;
&lt;/plugin&gt;
[/sourcecode]</p>
<p>I am using JBoss-5.1 application server to which the application is deployed.</p>
<p>The <em><strong>containerId </strong></em>is set to the appropriate application server.
The <em><strong>home </strong></em>refers to the application server directory. This property is picked from the environment property in the snippet above.
The configuration here is set to <em><strong>existing </strong></em>i.e. the application is deployed to a running server
<em><strong>home </strong></em>element within the configuration refers to server profile to which the application is installed.
You can find more information on cargo plug-in configuration <a href="http://cargo.codehaus.org/Maven2+plugin">here</a>
<h4>Step 2: Starting the application server</h4>
We would want the application server to be started in the pre-integration-test phase and stopped in the post-integration-test phase. This can be directly done by giving execution details for the cargo plug-in as below</p>
<p>[sourcecode language="xml"]
&lt;plugin&gt;
 &lt;groupId&gt;org.codehaus.cargo&lt;/groupId&gt;
 &lt;artifactId&gt;cargo-maven2-plugin&lt;/artifactId&gt;
 &lt;executions&gt;
 &lt;execution&gt;
 &lt;id&gt;start-container&lt;/id&gt;
 &lt;phase&gt;pre-integration-test&lt;/phase&gt;
 &lt;goals&gt;
 &lt;goal&gt;start&lt;/goal&gt;
 &lt;/goals&gt;
 &lt;/execution&gt;
 &lt;execution&gt;
 &lt;id&gt;stop-container&lt;/id&gt;
 &lt;phase&gt;post-integration-test&lt;/phase&gt;
 &lt;goals&gt;
 &lt;goal&gt;stop&lt;/goal&gt;
 &lt;/goals&gt;
 &lt;/execution&gt;
 &lt;/executions&gt;
&lt;/plugin&gt;
[/sourcecode]
<h4>Step 3:  Starting the Selenium and FitNesse servers</h4>
The next step is to automatically start Selenium and FitNesse servers. This can easily be accomplished using maven-antrun-plugin and defining execution to start the two in pre-integration-test phase</p>
<p>[sourcecode language="xml"]
&lt;profile&gt;
 &lt;id&gt;fitnium&lt;/id&gt;
 &lt;properties&gt;
 &lt;fitnium.project&gt;${project.parent.basedir}/testApp-fitnium&lt;/fitnium.project&gt;
 &lt;/properties&gt;
 &lt;build&gt;
 &lt;plugins&gt;
 &lt;plugin&gt;
 &lt;artifactId&gt;maven-antrun-plugin&lt;/artifactId&gt;
 &lt;execution&gt;
 &lt;id&gt;start-selenium-and-fitnesse&lt;/id&gt;
 &lt;phase&gt;pre-integration-test&lt;/phase&gt;
 &lt;configuration&gt;
 &lt;tasks&gt;
 &lt;echo taskname=&quot;selenium&quot; message=&quot;Starting…&quot; /&gt;
 &lt;java jar=&quot;${fitnium.project}/selenium-server-1.0/selenium-server.jar&quot;
 fork=&quot;yes&quot; spawn=&quot;true&quot;&gt;
 &lt;arg value=&quot;-port&quot; /&gt;
 &lt;arg value=&quot;9090&quot; /&gt;
 &lt;/java&gt;
 &lt;echo taskname=&quot;fitnesse&quot; message=&quot;Starting…&quot; /&gt;
 &lt;java classname=&quot;fitnesseMain.FitNesseMain&quot;
 classpath=&quot;${fitnium.project}/fitnesse.jar;${fitnium.project}/lib/json.jar&quot;
 fork=&quot;yes&quot; spawn=&quot;true&quot;&gt;
 &lt;arg value=&quot;-p&quot; /&gt;
 &lt;arg value=&quot;7000&quot; /&gt;
 &lt;arg value=&quot;-d&quot; /&gt;
 &lt;arg value=&quot;${fitnium.project}&quot; /&gt;
 &lt;/java&gt;
 &lt;/tasks&gt;
 &lt;/configuration&gt;
 &lt;goals&gt;
 &lt;goal&gt;run&lt;/goal&gt;
 &lt;/goals&gt;
 &lt;/execution&gt;
 &lt;/plugins&gt;
 &lt;/build&gt;
&lt;/profile&gt;
[/sourcecode]</p>
<p>Make sure that the path to the jars is set correctly as per your environment for the java tasks. The attributes for the selenium tasks are</p>
<p><strong>Port</strong>: The port at which the selenium server should listen at. Make sure this matches the port mentioned in the FitNium test script described earlier.</p>
<p>And for the FitNesse are:</p>

## Comments

**[abdurrahman sahin](#5305 "2011-02-17 13:48:38"):** I really liked your path to implement a complete test automation. great work.

