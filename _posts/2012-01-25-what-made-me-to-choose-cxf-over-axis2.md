---
layout: post
header-img: img/default-blog-pic.jpg
author: mshah
description: 
post_id: 11063
created: 2012/01/25 17:11:40
created_gmt: 2012/01/25 12:11:40
comment_status: open
---

# What made me to choose CXF over Axis2

My last assignment required me to expose soap based web service. While exploring I came to know that there are many types of web services, for example JBossWS, GlassFish Metro, Apache CXF, Axis1, Axis2 and likewise. All of them are widely used and were also promising to fulfill most of my requirements. Since I had to deploy my web service on tomcat web container, I shortlisted Axis2 and Apache CXF for comparison. While working on POC for their comparison, factors mentioned below helped me to choose the appropriate.

**Factors in favor of CXF**

  1. I do not have to download any soap engine as we do when we use Axis2.
  2. Deployment of axis2 is not as easy as CXF because there is no well known plugin available for axis2 which can directly deploy aar file on soap engine.
  3. CXF provides nice and simple integration with service component architecture(SCA) container like Tuscany.
  4. CXF also gives web service standards support, Frontend programming APIs support, Tools support(like xsd to wsdl, wsdl validator, wsdl to soap, wsdl to xml, wsdl to javascript, wsdl to java, java to web service etc.) and RESTful services support in very simple fashion.
  5. The last but not least is that CXF provides tools to generate java client as well as JavaScript client.

Now lets have a glance on the steps which I followed during CXF and Axis2 POCs.

**Axis2 POC** Steps followed during POC that describes how to get started with Axis2:-

**Step-1:** Download Axis2, which is required as soap engine to process soap based request. **Step-2:** Create a maven based web project with services. Service class and pom configurations are given below.

**service class:**

``` 
 public class StringWebService { public String getMessage(String s) { return s; } } 
 ```

**pom.xml**

``` 
 <dependencies> <!--Axis2 Jars --> <dependency> <groupId>org.apache.axis2</groupId> <artifactId>axis2</artifactId> <version>1.4</version> </dependency> </dependencies> <build> <plugins> <plugin> <groupId>org.apache.axis2</groupId> <artifactId>axis2-aar-maven-plugin</artifactId> <version>1.4.1</version> <configuration> <aarName>ws_axis2</aarName> </configuration> </plugin> </plugins> </build> 
 ```

You can use other dependencies as per your requirement. I have used axis2-aar-maven-plugin to build Apache archive (aar) file, to be deployed on soap engine.

**Step-3:** Create a services.xml file to expose service name and its operations in src/main/resources/META-INF directory. Also add web.xml in WEB-INF directory as we do in normal web project to define AxisServlet and its mapping.

**services.xml**

``` 
 <service name="WebService"> <parameter name="ServiceClass" locked="false">com.test.ws.StringWebService</parameter> <operation name="getMessage"> <messageReceiver class="org.apache.axis2.rpc.receivers.RPCMessageReceiver"/> </operation> </service> 
 ```

**web.xml**

``` 
 <web-app> <display-name>Apache-Axis2</display-name> <servlet> <servlet-name>AxisServlet</servlet-name> <display-name>Apache-Axis Servlet</display-name> <servlet-class>org.apache.axis2.transport.http.AxisServlet</servlet-class> <load-on-startup>1</load-on-startup> </servlet> <servlet-mapping> <servlet-name>AxisServlet</servlet-name> <url-pattern>/servlet/AxisServlet</url-pattern> </servlet-mapping> <servlet-mapping> <servlet-name>AxisServlet</servlet-name> <url-pattern>/services/*</url-pattern> </servlet-mapping> </web-app> 
 ```

**Step-4:** Execute below maven command to build web service. **mvn clean install axis2-aar:aar** (axis2-aar:aar basically generates the .aar file)

**Step-5:** The final step is to deploy .aar file in $CATALINA_HOME/webapps/axis2/WEB-INF/services directory. Now our web service is ready to use. After deployment you can access wsdl by hitting this url:- http://localhost:8080/axis2/services/WebService?wsdl

**CXF POC** Steps followed during POC which describes how to get started with CXF:-

**Step-1:** Create maven based web project with services

**service class:**

``` 
 @WebService public class SampleCXFService { public String getMessage(String s) { return s; } }


 ```

**pom.xml**

 

``` 
 <build> <plugins> <plugin> <!-- Plugin for compiling Java code --> <artifactId>maven-compiler-plugin</artifactId> <configuration> <source>1.5</source> <target>1.5</target> </configuration> </plugin> <plugin> <artifactId>maven-war-plugin</artifactId> <version>2.1.1</version> <configuration> <warName>ws_cxf</warName> </configuration> </plugin> <plugin> <groupId>org.codehaus.mojo</groupId> <artifactId>tomcat-maven-plugin</artifactId> <configuration> <url>http://127.0.0.1:8080/manager</url> <server>TomcatServer</server> <path>/ws_cxf</path> <warFile>target/ws_cxf.war</warFile> </configuration> </plugin> </plugins> </build> <dependencies> <dependency> <groupId>org.apache.cxf</groupId> <artifactId>cxf-rt-frontend-jaxws</artifactId> <version>2.2.3</version> </dependency> <dependency> <groupId>org.apache.cxf</groupId> <artifactId>cxf-rt-transports-http</artifactId> <version>2.2.3</version> </dependency> </dependencies> 
 ```

You can use other dependencies as per your requirement and to use tomcat-maven-plugin effectively you will have to add below configuration snippet in settings.xml of maven repository. Make sure that username and password exist in your $CATALINA_HOME/conf/tomcat-users.xml file.

``` 
 <server> <id>TomcatServer</id> <username>admin</username> <password>password</password> </server> 
 ```

**Step-2:** Create beans.xml configuration file which is basically used to define service endpoint and few configuration file i.e. cxf.xml, cxf-extension-soap.xml and cxf-servlet.xml. Also add web.xml in WEB-INF directory as we do in normal web project to define CXFServlet and its mapping.

**beans.xml**

``` 
 <beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:jaxws="http://cxf.apache.org/jaxws" xsi:schemaLocation=" http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://cxf.apache.org/jaxws http://cxf.apache.org/schemas/jaxws.xsd"> <import resource="classpath:META-INF/cxf/cxf.xml" /> <import resource="classpath:META-INF/cxf/cxf-extension-soap.xml" /> <import resource="classpath:META-INF/cxf/cxf-servlet.xml" /> <jaxws:endpoint id="sampleService" implementor="com.test.SampleCXFService" address="/sampleService" /> </beans> 
 ```

** web.xml**

 

``` 
 <web-app> <context-param> <param-name>contextConfigLocation</param-name> <param-value>WEB-INF/beans.xml</param-value>

## Comments

**[Shekhar Gulati](#7176 "2012-01-28 20:58:23"):** Hello Mukesh, From the looks of the post it looks like you have just bothered about ease of deployment. Do you need the two other two features that you talked integration with Apache Tuscany and JavaScript client. In my opinion if you are not going to use them it should not matter to you. I think ease of deployment should not be the only criteria to choose one technology over other. Probably if you have talked about which project is more active or has more community, or which supports ws standards, or other features which might impact you like web service security should be given due consideration. Thanks Shekhar

**[Mukesh Shah](#7222 "2012-01-30 10:38:12"):** Hi Shekhar, Thanks for your comment. Writing JavaScript client was required to display some information on UI which I would be explaining in my next blog. Integration with Apache Tuscany is an additional feature which may or may not be used as per your project requirements.

**[Vivek Kondur](#7463 "2012-02-08 11:03:54"):** Here is an interesting thread on Apache CXF vs Acis2 - http://stackoverflow.com/questions/1243247/difference-between-apache-cxf-and-axis

