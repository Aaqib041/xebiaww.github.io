---
layout: post
header-img: img/default-blog-pic.jpg
author: srentala
description: 
post_id: 13933
created: 2012/05/29 11:06:04
created_gmt: 2012/05/29 06:06:04
comment_status: open
---

# Creating Automation Framework using Selenium and Test-NG

<p>How to automate a website project?
What are the tools one can use while automating?
Why is Automation required over Manual Testing?
When is the appropriate time to use the Automation Scripts?
These are some questions which I am going to answer in this blog. I will cover and let you know the basic effective steps one can follow in creating a ‘Data Driven Automation Framework using Selenium and Test-NG’. I have created this Framework for my existing projects and let us see how we can proceed step by step.
<!--more--></p>
<p><strong>1) INSTALL JAVA AND SET ENVIRONMENT VARIABLES</strong>
Install Java (JDK 1.7) form the location: <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html/">http://www.oracle.com/technetwork/java/javase/downloads/index.html/</a>
After Successful installation of Java, it will install JDK and JRE in your system.  Now change the environment variables.</p>
<p>a) Go to Environment variables -&gt; User variables- &gt; New-&gt; Create a variable “JAVA_HOME” and gives its value the JDK path “C:\Program Files\Java\jdk1.7.0_03” (in our case).
b) Then in System variables (under User variables) -&gt;New -&gt; in the ‘Path’ variable append a new value “C:\Program Files\Java\jdk1.7.0_03\bin” (bin path of the JDK).</p>
<p>Open the command prompt and type “java –version” to check whether java is installed properly.</p>
<p><strong>2) INSTALL ECLIPSE</strong>
Install Eclipse IDE from the location: <a href="http://www.eclipse.org/downloads/">http://www.eclipse.org/downloads/</a>
Download the “Eclipse IDE for JAVA EE Developers -212 Mb” version for 32 / 64 bit Windows PC.</p>
<p><strong>3) INSTALL Test-NG PLUGIN</strong>
In Eclipse IDE -&gt; Help -&gt; Install New Software-&gt; Add Button -&gt; Another dialog Box is opened -&gt; Put “Test-NG” as name and its location as <a href="http://beust.com/eclipse">http://beust.com/eclipse</a> .</p>
<p><strong>4) INSTALL SELENIUM IDE AND FIREFOX</strong>
a) Install Firefox (3.x or above)  <a href="http://www.mozilla.org/en-US/firefox/all-older.html">http://www.mozilla.org/en-US/firefox/all-older.html</a>
b) Install Selenium ( IDEVersion 1.7.2 –Latest)  <a href="http://seleniumhq.org/download/">http://seleniumhq.org/download/</a></p>
<p><strong>5) INSTALL  JARS</strong>
a) Download Selenium JARS (2.20.0 – Latest) from the location: <a href="http://code.google.com/p/selenium/downloads/list">http://code.google.com/p/selenium/downloads/list</a></p>
<p>Download following three jars:
1) selenium-java-2.20.0.zip  - Java Bindings for Selenium 2 including Web driver and RC
2) selenium-server-2.20.0.zip  - All variants of Selenium Server
3) Selenium-server-standalone-2.20.0.jar - Selenium RC, Remote Web driver or GRID.
b) Download POI API JAR files from the location:http://poi.apache.org/download.html  (download the bin distribution and not the source distribution) .Make sure it has dom4j.jar and xml-beans.jar included in it.
POI APIs are created by Apache for reading /writing excels sheets.
c) Download Log4j JARS from the following location: <a href="http://logging.apache.org/log4j/1.2/download.html">http://logging.apache.org/log4j/1.2/download.html</a>
Log4j JARS are required for logging the logs when the scripts are running.
Create a log4j.properties file or download it from the following location: <a href="http://www.vaannila.com/log4j/log4j-example-multiple-appender.html">http://www.vaannila.com/log4j/log4j-example-multiple-appender.html</a>
and put it in Project’s ‘src’ folder and give the path of the file to the ‘path’ where you want to generate logs inside the file properties.
Import the “import org.apache.log4j.Logger” package and not any other package or else it will not recognize few logger APIs.</p>
<p><strong>6) CREATE A PROJECT IN ECLIPSE</strong>
Right Click on Project Explorer -&gt; New -&gt;Project -&gt; Create a Project.
By default a ‘src’ folder a ‘JRE system Library’ (Basic Java JARS required) will be created.</p>
<p><strong>7) IMPORT JARS IN YOUR PROJECT</strong>
a) Test-NG: Go to Project Properties -&gt; Java Build Path -&gt; Libraries -&gt; Add Library -&gt;Test-NG.
b) Selenium JARS:  Go to Project Properties -&gt; Java Build Path -&gt; Libraries -&gt; Add Library -&gt;Select ‘User Library’ -&gt; Next-&gt; User Libraries -&gt; New-&gt; Give a name (say: JAR Libraries) -&gt;Ok -&gt; Add JARS of selenium -&gt; OK.</p>
<p>A new System library will be created with the name “JAR Libraries”. We can add any other Jars in this specific Library.
c) POI API /Log4J JARS: Under the ‘User Library’ -&gt;import the POI API and Log4j Jars similarly.</p>
<p><strong>8) CREATE CONFIG PROPERTIES FILE:</strong>
Config.properties file is useful in storing the configurable parameters in your project. It consists key and values pairs.</p>
<p>a) Create a package under /src folder (say: com.wapautomation.Config)
b) Under this package create a config properties file . A sample Config.properties file may look like(Fig-1):</p>
<p><strong>Site = htt://wap.abccomp.com/wap</strong>
<strong>Browser = firefox</strong>
<strong>Username = <a href="mailto:srentala@gmail.com">srentala@gmail.com</a></strong>
<strong>Password = <a href="mailto:pwd@1234">pwd@1234</a></strong>
<strong>Environment = production</strong>
<strong>Server = 10.254.236.214</strong>
<strong>Client = 10.252.26.12</strong></p>
<p>c) Create a ReadPropertiesFile.class file in your project , and use the following code snippet in it. The following code is to retrieve the values of the keys in config.properties file. Refer below:</p>
<p><strong>public class ReadPropertiesFile</strong>
<strong>{</strong>
<strong>public static void main(String[] args) throws IOException</strong>
<strong>{</strong>
<strong>Properties prop = new Properties();</strong>
<strong>FileInputStream f = new FileInputStream(System.getProperty("user.dir")+\src\com\config.properties);</strong>
<strong>prop.load(f);</strong>
<strong>System.out.println(prop.getProperty("browser");</strong>
<strong>System.out.println(prop.getProperty("username");</strong>
<strong>}</strong>
<strong>}</strong></p>
<p>This is how we can extract the values of the keys from config properties file.</p>
<p><strong>9) CREATE LOG4j XML FILE</strong>
Create and place log4j.xml file in your project. A sample Log4j.xml file may look like:</p>
<p><strong>&lt;appender name="FA"&gt;</strong>
<strong> &lt;param name="File" value="src/org/reference/extras/loggers.log"/&gt;</strong>
<strong> &lt;param name="Append" value="false" /&gt;</strong>
<strong> &lt;param name="Threshold" value="ALL"/&gt;</strong>
<strong> &lt;layout&gt;</strong>
<strong> &lt;param name="ConversionPattern" value="%-4r [%t] %-5p %c %x - %m%n"/&gt;</strong>
<strong> &lt;/layout&gt;</strong>
<strong> &lt;/appender&gt;</strong></p>
<p><strong> </strong></p>
<p>‘File’ value will contain the path where user wants the logs to be generated. Once the file is created, use the following code snippet to be used in each class where logs need to be generated. Refer below:</p>
<p><strong>public class Logging {</strong></p>
<p><strong> Logger logger = Logger.getLogger(Logging.class);</strong>
<strong> static {</strong>
<strong> DOMConfigurator.configure("src/org/reference/extras/log4j.xml");</strong>
<strong> }</strong></p>
<p><strong> @Test </strong>
<strong> public void abc()</strong>
<strong> {</strong>
<strong> logger.debug("Hello debug");</strong>
<strong> logger.info("Hello Info");</strong>
<strong> logger.warn("Hello Warn");</strong>
<strong> logger.error("Hello Error");</strong>
<strong> logger.fatal("Hello Fatal");</strong></p>
<p><strong> }</strong>
<strong>}<a rel="attachment wp-att-13941" href="http://xebee.xebia.in/2012/05/29/creating-automation-framework-using-selenium-and-test-ng-2/4-2/"></a></strong></p>
<p>a) Logger is an in built function of Log4j, and ‘getLogger’ function takes the ‘current class name’ as its parameter.
b) ‘DOMConfigurator’ is responsible for fetching the log4j.xml file.
c) Once the configuration is done we can create logs in the log file by using logger. debug /logger.info/logger. error/logger. fatal/logger. warn.
d) On running the above code it will generate a ‘Logger.log’ file which will look like:</p>
<p><strong>0    [main</p>