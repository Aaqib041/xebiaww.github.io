---
layout: post
header-img: img/default-blog-pic.jpg
author: bsingh
description: 
post_id: 15120
created: 2013/01/07 17:44:40
created_gmt: 2013/01/07 12:44:40
comment_status: closed
---

# Arquillian for Spring Injection Integration Test cases

While writing integration tests for J2ee application, we face some challenges like

1\. To bootstrap a container environment  
2\. To Run a build to create/deploy application archive  
3\. Mocking dependent components  
4\. Configuring application to use test data source(s)  
5\. Resolving classpath isolation 

Due to these challenges, integration testing is not as easy unit testing. So we need a framework that can handle all these tasks and we can focus on test logic only. Arquillian is one such kind of platform that can bring the test to the runtime.

In this blog, I will explore [Arquillian][1] platform for creating integration tests for spring services. Arquillian is capable of executing test cases on deployed components in some container.This platform is useful for writing integration tests in real scenario as it manages the container, builds the archive of all the dependent classes, deploys the archive in the container and then executes the test cases inside the container.

So arquillian brings the test to the runtime environment.

In this example, i will be using a repository and service for student management module and will create arquillian tests of spring injection module for jboss container.

Classes for this example are as following -

StudentRepositoryImpl – This is the implementation of StudentRepository interface used to save, update and get the student.

StudentServiceImpl – This is implementation of StudentService interface to perform save, update and get operation.

Arquillian tests for the service and repository are StudentRepositoryTestCase and StudentServiceTestCase.

Below are the steps to setup arquillian in any maven based project to create arquillian test case -

**Step -1** – Add arquillian dependencies – Append arquillian bom dependency in pom.xml as Listing-1. Add dependency for Jboss arquillian junit container and jboss service deployer for Spring 3 as Listing-2.

Listing-1

[code language="text"]<br /> &lt;dependencyManagement&gt;<br /> &lt;dependencies&gt;<br /> &lt;dependency&gt;<br /> &lt;groupId&gt;org.jboss.arquillian&lt;/groupId&gt;<br /> &lt;artifactId&gt;arquillian-bom&lt;/artifactId&gt;<br /> &lt;version&gt;1.0.2.Final&lt;/version&gt;<br /> &lt;scope&gt;import&lt;/scope&gt;<br /> &lt;type&gt;pom&lt;/type&gt;<br /> &lt;/dependency&gt;<br /> &lt;/dependencies&gt;<br /> &lt;/dependencyManagement&gt;<br /> 
 ```

Listing-2

[code language="text"]<br /> &lt;dependency&gt;<br /> &lt;groupId&gt;org.jboss.arquillian.junit&lt;/groupId&gt;<br /> &lt;artifactId&gt;arquillian-junit-container&lt;/artifactId&gt;<br /> &lt;scope&gt;test&lt;/scope&gt;<br /> &lt;/dependency&gt;</p> <p>&lt;dependency&gt;<br /> &lt;groupId&gt;org.jboss.arquillian.extension&lt;/groupId&gt;<br /> &lt;artifactId&gt;arquillian-service-deployer-spring-3&lt;/artifactId&gt;<br /> &lt;version&gt;1.0.0.Beta1&lt;/version&gt;<br /> &lt;/dependency&gt;<br /> &lt;dependency&gt;<br /> &lt;groupId&gt;org.jboss.arquillian.extension&lt;/groupId&gt;<br /> &lt;artifactId&gt;arquillian-service-integration-spring-inject&lt;/artifactId&gt;<br /> &lt;version&gt;1.0.0.Beta1&lt;/version&gt;<br /> &lt;scope&gt;test&lt;/scope&gt;<br /> &lt;/dependency&gt;<br /> 
 ```

**Step -2** – Setup container for deployment – I will be using Jboss as container so create a profile for jboss as arquillian container according to listing-3. We can create multiple profiles for other container as well and can execute the tests in one of those containers.

Listing-3

[code language="text"]<br /> &lt;profiles&gt;</p> <p>&lt;profile&gt;<br /> &lt;id&gt;arquillian-jbossas-managed&lt;/id&gt;<br /> &lt;activation&gt;<br /> &lt;activeByDefault&gt;true&lt;/activeByDefault&gt;<br /> &lt;/activation&gt;</p> <p>&lt;dependencies&gt;</p> <p>&lt;dependency&gt;<br /> &lt;groupId&gt;org.jboss.spec&lt;/groupId&gt;<br /> &lt;artifactId&gt;jboss-javaee-6.0&lt;/artifactId&gt;<br /> &lt;version&gt;1.0.0.Final&lt;/version&gt;<br /> &lt;type&gt;pom&lt;/type&gt;<br /> &lt;scope&gt;provided&lt;/scope&gt;<br /> &lt;/dependency&gt;</p> <p>&lt;dependency&gt;<br /> &lt;groupId&gt;org.jboss.as&lt;/groupId&gt;<br /> &lt;artifactId&gt;jboss-as-arquillian-container-managed&lt;/artifactId&gt;<br /> &lt;version&gt;7.1.1.Final&lt;/version&gt;<br /> &lt;scope&gt;test&lt;/scope&gt;<br /> &lt;/dependency&gt;<br /> &lt;dependency&gt;<br /> &lt;groupId&gt;org.jboss.arquillian.protocol&lt;/groupId&gt;<br /> &lt;artifactId&gt;arquillian-protocol-servlet&lt;/artifactId&gt;<br /> &lt;scope&gt;test&lt;/scope&gt;<br /> &lt;/dependency&gt;<br /> &lt;/dependencies&gt;<br /> &lt;/profile&gt;<br /> &lt;/profiles&gt;<br /> 
 ```

Create a arquillian.xml file in test resources package. This file contains the Jboss configuration to be used by arquillian teat cases. In this file jboss home directory is defined as Listing-4.

Listing-4

[code language="xml"]<br /> &lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;<br /> &lt;arquillian xmlns=&quot;http://jboss.org/schema/arquillian&quot; xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot;<br /> xsi:schemaLocation=&quot;http://jboss.org/schema/arquillian http://jboss.org/schema/arquillian/arquillian_1_0.xsd&quot;&gt;<br /> &lt;defaultProtocol type=&quot;Servlet 3.0&quot;/&gt;<br /> &lt;!-- Managed container settings --&gt;<br /> &lt;container qualifier=&quot;jbossas-managed&quot; default=&quot;true&quot;&gt;<br /> &lt;configuration&gt;<br /> &lt;property name=&quot;jbossHome&quot;&gt;G:\Projects\jboss-as-7.1.1.Final&lt;/property&gt;<br /> &lt;property name=&quot;outputToConsole&quot;&gt;true&lt;/property&gt;<br /> &lt;/configuration&gt;<br /> &lt;/container&gt;<br /> &lt;/arquillian&gt;</p> <p>[/code

   [1]: http://www.jboss.org/arquillian.html