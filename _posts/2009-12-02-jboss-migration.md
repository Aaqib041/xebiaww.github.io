---
layout: post
header-img: img/default-blog-pic.jpg
author: rahulagrawal
description: 
post_id: 2239
created: 2009/12/02 11:48:13
created_gmt: 2009/12/02 06:48:13
comment_status: open
---

# JBoss Migration

<p>Recently, at Xebia, Rocky Jaiswal, Pallavi Jain , Megha Jain and myself  did the migration of an application, JMSConsole, running on WebLogic 8.1.4 to JBoss 5.1. In this article we have shared some of the findings on the same.</p>
<p>The details of the JMSConsole application and the migration exercise  have been captured in the clipping.</p>
<p><object classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" width="425" height="344" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,40,0"><param name="allowFullScreen" value="true" /><param name="allowscriptaccess" value="always" /><param name="src" value="http://www.youtube.com/v/gIybgjCnGZg&amp;hl=en_US&amp;fs=1&amp;" /><param name="allowfullscreen" value="true" /><embed type="application/x-shockwave-flash" width="425" height="344" src="http://www.youtube.com/v/gIybgjCnGZg&amp;hl=en_US&amp;fs=1&amp;" allowscriptaccess="always" allowfullscreen="true"></embed></object></p>
<p><span style="text-decoration: underline;">About JMSConsole application</span>
<h5>Source environment  :  JDK1.4 , WebLogic's 8.1.4 and Oracle 10g.</h5>
<h5>Target environment  :  JDK 1.5 , JBoss 5.1 , Oracle 10g.</h5>
The JMSConsole application is a simple application that picks messages from a jms queue, Q_1, and pushes those messages to an Oracle database.</p>
<p>The application displays these messages on a UI and gives the user a choice to either delete the messages from the database or push the message to another  jms queue, Q_2.  Q_1 is configured to be the error destination for Q_2.</p>
<!--more-->

<p>Following are the major changes that we had to do to make the application work on JBoss.
<h4>JNDI changes</h4>
<span style="text-decoration: underline;">For JBoss the the following JNDI properties are to be used.</span>
<span style="color: #000080;">
java.naming.factory.initial=org.jnp.interfaces.NamingContextFactory
java.naming.factory.url.pkgs=org.jboss.naming:org.jnp.interfaces
java.naming.provider.url=jnp://myhost:1099
</span></p>
<p><span style="text-decoration: underline;">For WebLogic the following JNDI properties are to be used.</span>
<span style="color: #000080;">
java.naming.factory.initial=weblogic.jndi.WLInitialContextFactory
java.naming.provider.url=t3://myhost:7001
</span></p>
<!--nextpage-->

<h4>Database configuration changes:</h4>

<p>The standard approach would be to configure a Datasource for Oracle in JBoss and use jndi to lookup to access the datasource.</p>
<p>Things looked fine and simple until we ran into the code that uses  <em>oracle.sql.CLOB.createTemporary(....)</em> function call.</p>
<p>Accessing Oracle 10g via jndi datasource in JBoss 5.1 using the oracle standard jdbc driver for 10g  gives the following exception:</p>
<p><span style="color: #000080;">
<code>
java.lang.ClassCastException: org.jboss.resource.adapter.jdbc.jdk5.WrappedConnectionJDK5
at oracle.sql.CLOB.createTemporary(CLOB.java:754)
at oracle.sql.CLOB.createTemporary(CLOB.java:716)
at com.xebia.tools.jmsconsole.common.dao.impl.ClobHandlerImpl.createClob(ClobHandlerImpl.java:35)
</code>
</span></p>
<p><span style="color: #000080;"><span style="color: #000000;">The code used to access the database is as follows: </span></span></p>
<p><span style="color: #000080;"><span style="color: #000000;">Spring bean to access the jdbc datasource via jndi.</span>
</span></p>
<p>[xml]
&lt;bean id=&quot;auditDataSource&quot; class=&quot;org.springframework.jndi.JndiObjectFactoryBean&quot;&gt;
        &lt;property name=&quot;jndiName&quot;&gt;
            &lt;value&gt;jmsConsoleDataSourceOra&lt;/value&gt;
        &lt;/property&gt;
 &lt;/bean&gt;
[/xml]</p>
<p>The source code that caused  the exception.</p>
<p>[java]</p>
<p>import oracle.sql.CLOB;
....
....</p>
<p>public Clob createClob(DataSource dataSource, String text) throws SQLException {
 CLOB tempClob = null;
 Writer tempClobWriter = null;
 final Connection con = DataSourceUtils.getConnection(dataSource);</p>
<p>try {
 // If the temporary CLOB has not yet been created, create new
 if (con != null) {
 tempClob = CLOB.createTemporary(con, true, CLOB.DURATION_SESSION);
 } else {
 log.warn(&quot;Unable to retrieve connection&quot;);
 }</p>
<p>[/java]</p>
<p>One of the work arounds to get past this error is to avoid the container manage the jdbc datasource.</p>
<p>Instead use org.springframework.jdbc.datasource.DriverManagerDataSource to get connections.Yes we do loose the connection pooling capabilities provided by the container in the approach. The connection pooling has to be taken care by the developer.</p>
<p>[xml]
&lt;bean id=&quot;auditDataSource&quot;
          class=&quot;org.springframework.jdbc.datasource.DriverManagerDataSource&quot;&gt;
        &lt;property name=&quot;driverClassName&quot;&gt;
            &lt;value&gt;oracle.jdbc.driver.OracleDriver&lt;/value&gt;
        &lt;/property&gt;
        &lt;property name=&quot;url&quot;&gt;
            &lt;value&gt;jdbc:oracle:thin:@localhost:1521:jmsdb&lt;/value&gt;
        &lt;/property&gt;
        &lt;property name=&quot;username&quot;&gt;
            &lt;value&gt;jmsconsoletest&lt;/value&gt;
        &lt;/property&gt;
        &lt;property name=&quot;password&quot;&gt;
            &lt;value&gt;jmsconsoletest&lt;/value&gt;
        &lt;/property&gt;
 &lt;/bean&gt;
[/xml]</p>
<!--nextpage-->

<h4>Security changes</h4>

<p>JBoss application security is configured via JAAS login configuration. The XML based file is located at ~myserver/conf/login-config.xml</p>
<p>Add an application-policy element for each security domain. For this exercise we added the following entry:</p>
<p>[xml]
&lt;application-policy name=&quot;jmsconsole&quot;&gt;
      &lt;authentication&gt;
        &lt;login-module code=&quot;org.jboss.security.auth.spi.UsersRolesLoginModule&quot; flag=&quot;required&quot;&gt;
          &lt;module-option name=&quot;usersProperties&quot;&gt;props/jmsconsole-users.properties&lt;/module-option&gt;
          &lt;module-option name=&quot;rolesProperties&quot;&gt;props/jmsconsole-roles.properties&lt;/module-option&gt;
        &lt;/login-module&gt;
      &lt;/authentication&gt;
  &lt;/application-policy&gt;
[/xml]</p>
<p>The users and the role definition and the mapping between users and roles are configured in the usersProperties and rolesProperties.</p>
<p>The policy name jmsconsole would be referred in the jboss-web.xml file.</p>
<p>[xml]
&lt;jboss-web&gt;
    &lt;security-domain&gt;
     java:/jaas/jmsconsole
    &lt;/security-domain&gt;
&lt;/jboss-web&gt;
[/xml]
<h4>JMS changes</h4>
<h5>WebLogic  Error Destination and  JBoss DLQ.</h5>
Once WebLogic JMS fails to redeliver a message to a destination for a specific number of times, the message can be redirected to an error destination that is associated to the message destination. <a href="http://xebee.xebia.in/wp-content/uploads/2009/12/beaErrorDestinations1.JPG">How to configure Error Destinations in WebLogic</a></p>
<p>In JBoss the same concept is achieved by configuring the Dead Letter Queue.
<a href="http://xebee.xebia.in/wp-content/uploads/2009/12/Jboss_DLQ1.PNG">How to configure DLQ in JBoss</a></p>
<!--nextpage-->

<h4>Writing Container Independent J2EE Applications.</h4>

<p>Although the main idea behind Java and J2EE is portability and "Write Once, Run Anywhere" (WORA), this is not always the case, since the unique way in which each vendor implements the J2EE specification often leads to problems when migrating a J2EE application. Vendors also implement features that are not included in the J2EE specification. Sun provides the J2EE Compatibility Test Suite (CTS). The CTS test suite is used by Sun to verify that a specific J2EE application server complies with the specification. This ensures that applications deployed on one J2EE application server will also run on another vendor's application server.</p>
<p>A few of the common observations that are applicable for any J2EE migration from one container to another container are listed below.
<h5>Using Vendor Specific features:</h5>
Sometimes some of the vendor specific features causes the application to become dependent on the application server runtime environment. Applications using vendor specific features get locked into the container.
<h5><strong>Deployment Descriptors:</strong></h5></p>

## Comments

**[Harpreet](#5311 "2011-02-18 22:12:33"):** This is a nice article sir, Excellent Thank you so much great help for newbee's

**[Ankitdhar](#6419 "2011-12-19 23:41:05"):** Hi Rahul. I am facing an issue with on of our app which has been migrated from Weblogic to JBOSS. This is the issue: The application has two user-roles: 1\. Admin 2\. Normal User Firstly,an admin logs in,he is able to work fine with his privileges. Secondly,a normal user logs in and he is able to work fine with his respective privileges. Thirldy,when an admin logs in,his privileges have been set as of a normal-user.The buttons which were generally enabled for him have been disabled. Rest of the application is working fine. This problem persists until we restart the servers.And if a normal user logs in,the issue persists. As of now,as a work-around,we have requested the normal users not to login into the application. Note: 1.The JDK has been upgraded from 1.4 to 1.6 2.Oracle has been upgraded from 9i to 11G. We do not know whether the issue is with code or the configuration settings of JBOSS server. The log files are not showing any errors. If you can help me,please contact me on my mai: ainkollu@gmail.com I could not send a mail to your mail ragrawal@xebia.com

