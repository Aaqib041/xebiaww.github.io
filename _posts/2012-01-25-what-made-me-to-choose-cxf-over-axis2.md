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

<p>My last assignment required me to expose soap based web service. While exploring I came to know that there are many types of web services, for example  JBossWS, GlassFish Metro, Apache CXF, Axis1, Axis2 and likewise. All of them are widely used and were also promising to fulfill most of my requirements. Since I had to deploy my web service on tomcat web container, I shortlisted Axis2 and Apache CXF for comparison. While working on POC for their comparison, factors mentioned below helped me to choose the appropriate.</p>
<p><strong>Factors in favor of CXF</strong>
<!--more-->
<ol>
    <li>I do not have to download any soap engine as we do when we use Axis2.</li>
    <li>Deployment of axis2 is not as easy as CXF because there is no well known plugin available for axis2 which can directly deploy aar file on soap engine.</li>
    <li>CXF provides nice and simple integration with service component architecture(SCA) container like Tuscany.</li>
    <li>CXF also gives web service standards support, Frontend programming APIs support, Tools support(like xsd to wsdl, wsdl validator, wsdl to soap, wsdl to xml, wsdl to javascript, wsdl to java, java to web service  etc.) and RESTful services support in very simple fashion.</li>
    <li>The last but not least is that CXF provides tools to generate java client as well as JavaScript client.</li></p>
<p></ol>
Now lets have a glance on the steps which I followed during CXF and Axis2 POCs.</p>
<p><strong>Axis2 POC</strong>
Steps followed during POC that describes how to get started with Axis2:-</p>
<p><strong>Step-1:</strong> Download Axis2, which is required as soap engine to process soap based request.
<strong>Step-2:</strong> Create a maven based web project with services. Service class and pom configurations are given below.</p>
<p><strong>service class:</strong></p>
<p>[code]
public class StringWebService {
    public String getMessage(String s) {
        return s;
    }
}
[/code]</p>
<p><strong>pom.xml</strong></p>
<p>[code]
&lt;dependencies&gt;
        &lt;!--Axis2 Jars --&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;org.apache.axis2&lt;/groupId&gt;
            &lt;artifactId&gt;axis2&lt;/artifactId&gt;
            &lt;version&gt;1.4&lt;/version&gt;
        &lt;/dependency&gt;
    &lt;/dependencies&gt;
    &lt;build&gt;
        &lt;plugins&gt;
            &lt;plugin&gt;
                &lt;groupId&gt;org.apache.axis2&lt;/groupId&gt;
                &lt;artifactId&gt;axis2-aar-maven-plugin&lt;/artifactId&gt;
                &lt;version&gt;1.4.1&lt;/version&gt;
                &lt;configuration&gt;
                    &lt;aarName&gt;ws_axis2&lt;/aarName&gt;
                &lt;/configuration&gt;
            &lt;/plugin&gt;
        &lt;/plugins&gt;
    &lt;/build&gt;
[/code]</p>
<p>You can use other dependencies as per your requirement. I have used axis2-aar-maven-plugin to build Apache archive (aar) file, to be deployed on soap engine.</p>
<p><strong>Step-3:</strong> Create a services.xml file to expose service name and its operations in src/main/resources/META-INF directory. Also add web.xml in WEB-INF directory as we do in normal web project to define AxisServlet and its mapping.</p>
<p><strong>services.xml</strong></p>
<p>[code]
&lt;service name=&quot;WebService&quot;&gt;
    &lt;parameter name=&quot;ServiceClass&quot; locked=&quot;false&quot;&gt;com.test.ws.StringWebService&lt;/parameter&gt;
    &lt;operation name=&quot;getMessage&quot;&gt;
        &lt;messageReceiver class=&quot;org.apache.axis2.rpc.receivers.RPCMessageReceiver&quot;/&gt;
    &lt;/operation&gt;
&lt;/service&gt;
[/code]</p>
<p><strong>web.xml</strong></p>
<p>[code]
&lt;web-app&gt;
    &lt;display-name&gt;Apache-Axis2&lt;/display-name&gt;
    &lt;servlet&gt;
        &lt;servlet-name&gt;AxisServlet&lt;/servlet-name&gt;
        &lt;display-name&gt;Apache-Axis Servlet&lt;/display-name&gt;
        &lt;servlet-class&gt;org.apache.axis2.transport.http.AxisServlet&lt;/servlet-class&gt;
        &lt;load-on-startup&gt;1&lt;/load-on-startup&gt;
    &lt;/servlet&gt;
    &lt;servlet-mapping&gt;
        &lt;servlet-name&gt;AxisServlet&lt;/servlet-name&gt;
        &lt;url-pattern&gt;/servlet/AxisServlet&lt;/url-pattern&gt;
    &lt;/servlet-mapping&gt;
    &lt;servlet-mapping&gt;
        &lt;servlet-name&gt;AxisServlet&lt;/servlet-name&gt;
        &lt;url-pattern&gt;/services/*&lt;/url-pattern&gt;
    &lt;/servlet-mapping&gt;
&lt;/web-app&gt;
[/code]</p>
<p><strong>Step-4:</strong> Execute below maven command to build web service.
<strong>mvn clean install axis2-aar:aar</strong>
(axis2-aar:aar basically generates the .aar file)</p>
<p><strong>Step-5:</strong> The final step is to deploy .aar file in $CATALINA_HOME/webapps/axis2/WEB-INF/services directory. Now our web service is ready to use.
After deployment you can access wsdl by hitting this url:- http://localhost:8080/axis2/services/WebService?wsdl</p>
<p><strong>CXF POC</strong>
Steps followed during POC which describes how to get started with CXF:-</p>
<p><strong>Step-1:</strong> Create maven based web project with services</p>
<p><strong>service class:</strong></p>
<p>[code]
@WebService
public class SampleCXFService {
    public String getMessage(String s) {
        return s;
    }
}</p>
<p>[/code]</p>
<p><strong>pom.xml</strong></p>
<p>&nbsp;</p>
<p>[code]
&lt;build&gt;
        &lt;plugins&gt;
            &lt;plugin&gt;
                &lt;!-- Plugin for compiling Java code --&gt;
                &lt;artifactId&gt;maven-compiler-plugin&lt;/artifactId&gt;
                &lt;configuration&gt;
                    &lt;source&gt;1.5&lt;/source&gt;
                    &lt;target&gt;1.5&lt;/target&gt;
                &lt;/configuration&gt;
            &lt;/plugin&gt;
            &lt;plugin&gt;
                &lt;artifactId&gt;maven-war-plugin&lt;/artifactId&gt;
                &lt;version&gt;2.1.1&lt;/version&gt;
                &lt;configuration&gt;
                    &lt;warName&gt;ws_cxf&lt;/warName&gt;
                &lt;/configuration&gt;
            &lt;/plugin&gt;
            &lt;plugin&gt;
                &lt;groupId&gt;org.codehaus.mojo&lt;/groupId&gt;
                &lt;artifactId&gt;tomcat-maven-plugin&lt;/artifactId&gt;
                &lt;configuration&gt;
                    &lt;url&gt;http://127.0.0.1:8080/manager&lt;/url&gt;
                    &lt;server&gt;TomcatServer&lt;/server&gt;
                    &lt;path&gt;/ws_cxf&lt;/path&gt;
                    &lt;warFile&gt;target/ws_cxf.war&lt;/warFile&gt;
                &lt;/configuration&gt;
            &lt;/plugin&gt;
        &lt;/plugins&gt;
    &lt;/build&gt;
    &lt;dependencies&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;org.apache.cxf&lt;/groupId&gt;
            &lt;artifactId&gt;cxf-rt-frontend-jaxws&lt;/artifactId&gt;
            &lt;version&gt;2.2.3&lt;/version&gt;
        &lt;/dependency&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;org.apache.cxf&lt;/groupId&gt;
            &lt;artifactId&gt;cxf-rt-transports-http&lt;/artifactId&gt;
            &lt;version&gt;2.2.3&lt;/version&gt;
        &lt;/dependency&gt;
    &lt;/dependencies&gt;
[/code]</p>
<p>You can use other dependencies as per your requirement and to use tomcat-maven-plugin effectively you will have to add below configuration snippet in settings.xml of maven repository. Make sure that username and password exist in your $CATALINA_HOME/conf/tomcat-users.xml file.</p>
<p>[code]
&lt;server&gt;
    &lt;id&gt;TomcatServer&lt;/id&gt;
    &lt;username&gt;admin&lt;/username&gt;
    &lt;password&gt;password&lt;/password&gt;
&lt;/server&gt;
[/code]</p>
<p><strong>Step-2:</strong> Create beans.xml configuration file which is basically used to define service endpoint and few configuration file i.e. cxf.xml, cxf-extension-soap.xml and cxf-servlet.xml. Also add web.xml in WEB-INF directory as we do in normal web project to define CXFServlet and its mapping.</p>
<p><strong>beans.xml</strong></p>
<p>[code]
&lt;beans xmlns=&quot;http://www.springframework.org/schema/beans&quot;
    xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot;
    xmlns:jaxws=&quot;http://cxf.apache.org/jaxws&quot;
    xsi:schemaLocation=&quot;
http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
http://cxf.apache.org/jaxws http://cxf.apache.org/schemas/jaxws.xsd&quot;&gt;
    &lt;import resource=&quot;classpath:META-INF/cxf/cxf.xml&quot; /&gt;
    &lt;import resource=&quot;classpath:META-INF/cxf/cxf-extension-soap.xml&quot; /&gt;
    &lt;import resource=&quot;classpath:META-INF/cxf/cxf-servlet.xml&quot; /&gt;
    &lt;jaxws:endpoint id=&quot;sampleService&quot;
      implementor=&quot;com.test.SampleCXFService&quot;
      address=&quot;/sampleService&quot; /&gt;
&lt;/beans&gt;
[/code]</p>
<p><strong>
web.xml</strong></p>
<p>&nbsp;</p>
<p>[code]
&lt;web-app&gt;
    &lt;context-param&gt;
        &lt;param-name&gt;contextConfigLocation&lt;/param-name&gt;
        &lt;param-value&gt;WEB-INF/beans.xml&lt;/param-value&gt;</p>

## Comments

**[Shekhar Gulati](#7176 "2012-01-28 20:58:23"):** Hello Mukesh, From the looks of the post it looks like you have just bothered about ease of deployment. Do you need the two other two features that you talked integration with Apache Tuscany and JavaScript client. In my opinion if you are not going to use them it should not matter to you. I think ease of deployment should not be the only criteria to choose one technology over other. Probably if you have talked about which project is more active or has more community, or which supports ws standards, or other features which might impact you like web service security should be given due consideration. Thanks Shekhar

**[Mukesh Shah](#7222 "2012-01-30 10:38:12"):** Hi Shekhar, Thanks for your comment. Writing JavaScript client was required to display some information on UI which I would be explaining in my next blog. Integration with Apache Tuscany is an additional feature which may or may not be used as per your project requirements.

**[Vivek Kondur](#7463 "2012-02-08 11:03:54"):** Here is an interesting thread on Apache CXF vs Acis2 - http://stackoverflow.com/questions/1243247/difference-between-apache-cxf-and-axis

