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

How to automate a website project? What are the tools one can use while automating? Why is Automation required over Manual Testing? When is the appropriate time to use the Automation Scripts? These are some questions which I am going to answer in this blog. I will cover and let you know the basic effective steps one can follow in creating a ‘Data Driven Automation Framework using Selenium and Test-NG’. I have created this Framework for my existing projects and let us see how we can proceed step by step. 

**1) INSTALL JAVA AND SET ENVIRONMENT VARIABLES** Install Java (JDK 1.7) form the location: <http://www.oracle.com/technetwork/java/javase/downloads/index.html/> After Successful installation of Java, it will install JDK and JRE in your system.  Now change the environment variables.

a) Go to Environment variables -> User variables- > New-> Create a variable “JAVA_HOME” and gives its value the JDK path “C:\Program Files\Java\jdk1.7.0_03” (in our case). b) Then in System variables (under User variables) ->New -> in the ‘Path’ variable append a new value “C:\Program Files\Java\jdk1.7.0_03\bin” (bin path of the JDK).

Open the command prompt and type “java –version” to check whether java is installed properly.

**2) INSTALL ECLIPSE** Install Eclipse IDE from the location: <http://www.eclipse.org/downloads/> Download the “Eclipse IDE for JAVA EE Developers -212 Mb” version for 32 / 64 bit Windows PC.

**3) INSTALL Test-NG PLUGIN** In Eclipse IDE -> Help -> Install New Software-> Add Button -> Another dialog Box is opened -> Put “Test-NG” as name and its location as <http://beust.com/eclipse> .

**4) INSTALL SELENIUM IDE AND FIREFOX** a) Install Firefox (3.x or above)  <http://www.mozilla.org/en-US/firefox/all-older.html> b) Install Selenium ( IDEVersion 1.7.2 –Latest)  <http://seleniumhq.org/download/>

**5) INSTALL  JARS** a) Download Selenium JARS (2.20.0 – Latest) from the location: <http://code.google.com/p/selenium/downloads/list>

Download following three jars: 1) selenium-java-2.20.0.zip  - Java Bindings for Selenium 2 including Web driver and RC 2) selenium-server-2.20.0.zip  - All variants of Selenium Server 3) Selenium-server-standalone-2.20.0.jar - Selenium RC, Remote Web driver or GRID. b) Download POI API JAR files from the location:http://poi.apache.org/download.html  (download the bin distribution and not the source distribution) .Make sure it has dom4j.jar and xml-beans.jar included in it. POI APIs are created by Apache for reading /writing excels sheets. c) Download Log4j JARS from the following location: <http://logging.apache.org/log4j/1.2/download.html> Log4j JARS are required for logging the logs when the scripts are running. Create a log4j.properties file or download it from the following location: <http://www.vaannila.com/log4j/log4j-example-multiple-appender.html> and put it in Project’s ‘src’ folder and give the path of the file to the ‘path’ where you want to generate logs inside the file properties. Import the “import org.apache.log4j.Logger” package and not any other package or else it will not recognize few logger APIs.

**6) CREATE A PROJECT IN ECLIPSE** Right Click on Project Explorer -> New ->Project -> Create a Project. By default a ‘src’ folder a ‘JRE system Library’ (Basic Java JARS required) will be created.

**7) IMPORT JARS IN YOUR PROJECT** a) Test-NG: Go to Project Properties -> Java Build Path -> Libraries -> Add Library ->Test-NG. b) Selenium JARS:  Go to Project Properties -> Java Build Path -> Libraries -> Add Library ->Select ‘User Library’ -> Next-> User Libraries -> New-> Give a name (say: JAR Libraries) ->Ok -> Add JARS of selenium -> OK.

A new System library will be created with the name “JAR Libraries”. We can add any other Jars in this specific Library. c) POI API /Log4J JARS: Under the ‘User Library’ ->import the POI API and Log4j Jars similarly.

**8) CREATE CONFIG PROPERTIES FILE:** Config.properties file is useful in storing the configurable parameters in your project. It consists key and values pairs.

a) Create a package under /src folder (say: com.wapautomation.Config) b) Under this package create a config properties file . A sample Config.properties file may look like(Fig-1):

**Site = htt://wap.abccomp.com/wap** **Browser = firefox** **Username = [srentala@gmail.com][1]** **Password = [pwd@1234][2]** **Environment = production** **Server = 10.254.236.214** **Client = 10.252.26.12**

c) Create a ReadPropertiesFile.class file in your project , and use the following code snippet in it. The following code is to retrieve the values of the keys in config.properties file. Refer below:

**public class ReadPropertiesFile** **{** **public static void main(String[] args) throws IOException** **{** **Properties prop = new Properties();** **FileInputStream f = new FileInputStream(System.getProperty("user.dir")+\src\com\config.properties);** **prop.load(f);** **System.out.println(prop.getProperty("browser");** **System.out.println(prop.getProperty("username");** **}** **}**

This is how we can extract the values of the keys from config properties file.

**9) CREATE LOG4j XML FILE** Create and place log4j.xml file in your project. A sample Log4j.xml file may look like:

**<appender name="FA">** ** <param name="File" value="src/org/reference/extras/loggers.log"/>** ** <param name="Append" value="false" />** ** <param name="Threshold" value="ALL"/>** ** <layout>** ** <param name="ConversionPattern" value="%-4r [%t] %-5p %c %x - %m%n"/>** ** </layout>** ** </appender>**

** **

‘File’ value will contain the path where user wants the logs to be generated. Once the file is created, use the following code snippet to be used in each class where logs need to be generated. Refer below:

**public class Logging {**

** Logger logger = Logger.getLogger(Logging.class);** ** static {** ** DOMConfigurator.configure("src/org/reference/extras/log4j.xml");** ** }**

** @Test ** ** public void abc()** ** {** ** logger.debug("Hello debug");** ** logger.info("Hello Info");** ** logger.warn("Hello Warn");** ** logger.error("Hello Error");** ** logger.fatal("Hello Fatal");**

** }** **}**

a) Logger is an in built function of Log4j, and ‘getLogger’ function takes the ‘current class name’ as its parameter. b) ‘DOMConfigurator’ is responsible for fetching the log4j.xml file. c) Once the configuration is done we can create logs in the log file by using logger. debug /logger.info/logger. error/logger. fatal/logger. warn. d) On running the above code it will generate a ‘Logger.log’ file which will look like:

**0    [main

   [1]: mailto:srentala@gmail.com
   [2]: mailto:pwd@1234