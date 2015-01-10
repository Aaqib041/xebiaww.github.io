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

Recently, at Xebia, Rocky Jaiswal, Pallavi Jain , Megha Jain and myself  did the migration of an application, JMSConsole, running on WebLogic 8.1.4 to JBoss 5.1. In this article we have shared some of the findings on the same.

The details of the JMSConsole application and the migration exercise have been captured in the clipping.

About JMSConsole application

##### Source environment :  JDK1.4 , WebLogic's 8.1.4 and Oracle 10g.

##### Target environment :  JDK 1.5 , JBoss 5.1 , Oracle 10g.

The JMSConsole application is a simple application that picks messages from a jms queue, Q_1, and pushes those messages to an Oracle database.

The application displays these messages on a UI and gives the user a choice to either delete the messages from the database or push the message to another  jms queue, Q_2.  Q_1 is configured to be the error destination for Q_2.

Following are the major changes that we had to do to make the application work on JBoss. 

#### JNDI changes

For JBoss the the following JNDI properties are to be used. java.naming.factory.initial=org.jnp.interfaces.NamingContextFactory java.naming.factory.url.pkgs=org.jboss.naming:org.jnp.interfaces java.naming.provider.url=jnp://myhost:1099 

For WebLogic the following JNDI properties are to be used. java.naming.factory.initial=weblogic.jndi.WLInitialContextFactory java.naming.provider.url=t3://myhost:7001 

#### Database configuration changes:

The standard approach would be to configure a Datasource for Oracle in JBoss and use jndi to lookup to access the datasource.

Things looked fine and simple until we ran into the code that uses  _oracle.sql.CLOB.createTemporary(....)_ function call.

Accessing Oracle 10g via jndi datasource in JBoss 5.1 using the oracle standard jdbc driver for 10g  gives the following exception:

` java.lang.ClassCastException: org.jboss.resource.adapter.jdbc.jdk5.WrappedConnectionJDK5 at oracle.sql.CLOB.createTemporary(CLOB.java:754) at oracle.sql.CLOB.createTemporary(CLOB.java:716) at com.xebia.tools.jmsconsole.common.dao.impl.ClobHandlerImpl.createClob(ClobHandlerImpl.java:35) `

The code used to access the database is as follows: 

Spring bean to access the jdbc datasource via jndi.

[xml] <bean id="auditDataSource" class="org.springframework.jndi.JndiObjectFactoryBean"> <property name="jndiName"> <value>jmsConsoleDataSourceOra</value> </property> </bean> [/xml]

The source code that caused  the exception.

[java]

import oracle.sql.CLOB; .... ....

public Clob createClob(DataSource dataSource, String text) throws SQLException { CLOB tempClob = null; Writer tempClobWriter = null; final Connection con = DataSourceUtils.getConnection(dataSource);

try { // If the temporary CLOB has not yet been created, create new if (con != null) { tempClob = CLOB.createTemporary(con, true, CLOB.DURATION_SESSION); } else { log.warn("Unable to retrieve connection"); }

[/java]

One of the work arounds to get past this error is to avoid the container manage the jdbc datasource.

Instead use org.springframework.jdbc.datasource.DriverManagerDataSource to get connections.Yes we do loose the connection pooling capabilities provided by the container in the approach. The connection pooling has to be taken care by the developer.

[xml] <bean id="auditDataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource"> <property name="driverClassName"> <value>oracle.jdbc.driver.OracleDriver</value> </property> <property name="url"> <value>jdbc:oracle:thin:@localhost:1521:jmsdb</value> </property> <property name="username"> <value>jmsconsoletest</value> </property> <property name="password"> <value>jmsconsoletest</value> </property> </bean> [/xml]

#### Security changes

JBoss application security is configured via JAAS login configuration. The XML based file is located at ~myserver/conf/login-config.xml

Add an application-policy element for each security domain. For this exercise we added the following entry:

[xml] <application-policy name="jmsconsole"> <authentication> <login-module code="org.jboss.security.auth.spi.UsersRolesLoginModule" flag="required"> <module-option name="usersProperties">props/jmsconsole-users.properties</module-option> <module-option name="rolesProperties">props/jmsconsole-roles.properties</module-option> </login-module> </authentication> </application-policy> [/xml]

The users and the role definition and the mapping between users and roles are configured in the usersProperties and rolesProperties.

The policy name jmsconsole would be referred in the jboss-web.xml file.

[xml] <jboss-web> <security-domain> java:/jaas/jmsconsole </security-domain> </jboss-web> [/xml] 

#### JMS changes

##### WebLogic  Error Destination and  JBoss DLQ.

Once WebLogic JMS fails to redeliver a message to a destination for a specific number of times, the message can be redirected to an error destination that is associated to the message destination. [How to configure Error Destinations in WebLogic][1]

In JBoss the same concept is achieved by configuring the Dead Letter Queue. [How to configure DLQ in JBoss][2]

#### Writing Container Independent J2EE Applications.

Although the main idea behind Java and J2EE is portability and "Write Once, Run Anywhere" (WORA), this is not always the case, since the unique way in which each vendor implements the J2EE specification often leads to problems when migrating a J2EE application. Vendors also implement features that are not included in the J2EE specification. Sun provides the J2EE Compatibility Test Suite (CTS). The CTS test suite is used by Sun to verify that a specific J2EE application server complies with the specification. This ensures that applications deployed on one J2EE application server will also run on another vendor's application server.

A few of the common observations that are applicable for any J2EE migration from one container to another container are listed below. 

##### Using Vendor Specific features:

Sometimes some of the vendor specific features causes the application to become dependent on the application server runtime environment. Applications using vendor specific features get locked into the container. 

##### **Deployment Descriptors:**

   [1]: http://xebee.xebia.in/wp-content/uploads/2009/12/beaErrorDestinations1.JPG
   [2]: http://xebee.xebia.in/wp-content/uploads/2009/12/Jboss_DLQ1.PNG

## Comments

**[Harpreet](#5311 "2011-02-18 22:12:33"):** This is a nice article sir, Excellent Thank you so much great help for newbee's

**[Ankitdhar](#6419 "2011-12-19 23:41:05"):** Hi Rahul. I am facing an issue with on of our app which has been migrated from Weblogic to JBOSS. This is the issue: The application has two user-roles: 1\. Admin 2\. Normal User Firstly,an admin logs in,he is able to work fine with his privileges. Secondly,a normal user logs in and he is able to work fine with his respective privileges. Thirldy,when an admin logs in,his privileges have been set as of a normal-user.The buttons which were generally enabled for him have been disabled. Rest of the application is working fine. This problem persists until we restart the servers.And if a normal user logs in,the issue persists. As of now,as a work-around,we have requested the normal users not to login into the application. Note: 1.The JDK has been upgraded from 1.4 to 1.6 2.Oracle has been upgraded from 9i to 11G. We do not know whether the issue is with code or the configuration settings of JBOSS server. The log files are not showing any errors. If you can help me,please contact me on my mai: ainkollu@gmail.com I could not send a mail to your mail ragrawal@xebia.com

