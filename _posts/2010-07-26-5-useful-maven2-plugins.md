---
layout: post
header-img: img/default-blog-pic.jpg
author: ankitk
description: 
post_id: 4238
created: 2010/07/26 20:38:09
created_gmt: 2010/07/26 15:38:09
comment_status: open
---

# 5 Useful Maven2 Plugins

In this blog we will talk about some common problems faced while building, deploying and releasing java based projects in a maven build environment and also explain solutions for them using different maven plugin(s). This post will cover usage of the following maven plugin(s): - 

> > > > > > >   * [Maven WAR Plugin][1]
>>>>>>>   * [Maven Cobertura Plugin][2]
>>>>>>>   * [Maven Java2WSDL Plugin][3] ([Axis2][4])
>>>>>>>   * [Maven WSDL2Code Plugin][5] ([Axis2][4])
>>>>>>>   * [Maven Release Plugin][6]

![][7]

### 

### **Problem: Overlay ZIP or WAR file on another WAR source.**

**Description:** We have a maven project with some artifact dependencies, classes and resources which need to be bundled together into a WAR file. The artifact dependencies to be bundled together are: - axis2 AAR file (Axis Archive), a zipped folder and another WAR file.

**Solution:** Use maven WAR plugin with overlays. Add below plugin configuration to the project [POM][8] file.

[sourcecode language="xml"] <plugin> <groupId>org.apache.maven.plugins</groupId> <artifactId>maven-war-plugin</artifactId> <version>2.1-beta-1</version> <configuration> <warName>FinalProject</warName> <overlays> <!--Overlay an AAR file inside the generated WAR file --> <overlay> <groupId>com.test.services</groupId> <artifactId>test-services</artifactId> <type>aar</type> <targetPath>WEB-INF/services</targetPath> </overlay> <!--Overlay a ZIP file and place extracted content under WEB-INF/knowledge --> <overlay> <groupId>com.test.knowledge</groupId> <artifactId>test-knowledge</artifactId> <type>zip</type> <targetPath>WEB-INF/knowledge</targetPath> </overlay> <!--Overlay a WAR file and exclude the .jpg in final war --> <overlay> <groupId>com.example.project</groupId> <artifactId>projectdependency</artifactId> <excludes> <exclude>images/sampleimage-dependency.jpg</exclude> </excludes> </overlay> </overlays> </configuration> </plugin> [/sourcecode]

Overlays are applied in the order in which they are defined in the overlays configuration. By default, the source of the project (a.k.a the current build) is added first (e.g. before any overlay is applied). The current build is defined as a special overlay with no groupId, artifactId. If overlays need to be applied first, simply configure the current build after those overlays. 

### **Problem: Ensure build breaks if code coverage < threshold.**

**Description:** Ensure that during each build code coverage is above the required threshold value, if not then the maven build fails.

**Solution:** Use [Maven cobertura][2] plugin.

[sourcecode language="xml"] <plugin> <groupId>org.codehaus.mojo</groupId> <artifactId>cobertura-maven-plugin</artifactId> <version>2.4</version> <configuration> <instrumentation> <excludes> <exclude>com/project/test/TestMain.class</exclude> </excludes> </instrumentation> <check> <totalLineRate>85</totalLineRate> </check> <formats> <format>html</format> <format>xml</format> </formats> </configuration> <executions> <execution> <id>clean</id> <phase>pre-site</phase> <goals> <goal>clean</goal> </goals> </execution> <execution> <id>instrument</id> <phase>install</phase> <goals> <goal>check</goal> </goals> </execution> </executions> </plugin> [/sourcecode] 

### **Problem: Generate [WSDL][9] from a Java class at build time.**

**Solution:** Use [Maven Java2WSDL][3] plugin. Below configuration creates a TestProject.wsdl from the class _com.test.service.TestWebService_.

[sourcecode language="xml"] <plugin> <groupId>org.apache.axis2</groupId> <artifactId>axis2-java2wsdl-maven-plugin</artifactId> <version>1.5</version> <executions> <execution> <id>generate web service wsdl</id> <goals> <goal>java2wsdl</goal> </goals> <configuration> <serviceName>TestService</serviceName> <className>com.test.service.TestWebService</className> <targetNamespace>http://www.test.com/TestProject</targetNamespace> <schemaTargetNamespace>http://www.test.com/TestProject</schemaTargetNamespace> <elementFormDefault>qualified</elementFormDefault> <package2Namespace> <property> <name>com.test.domain</name> <value>http://www.test.com/TestProject</value> </property> </package2Namespace> <outputFileName>target/generated-resources/TestProject.wsdl</outputFileName> </configuration> </execution> </executions> </plugin> [/sourcecode]

NOTE: The plugin will be invoked automatically in the generate-sources phase. You can also invoke it directly from the command line by running the command.

[sourcecode language="xml"] mvn java2wsdl:java2wsdl [/sourcecode] 

### **Problem: Generate web service stub from the wsdl file.**

**Solution:** Use [Maven WSDL2Code][5] plugin. This plugin takes as input a WSDL and generates client and server stubs for calling or implementing a Web service matching the WSDL.

[sourcecode language="xml"] <!-- Generate Java code for the Admin web service (target/wsdl/test.wsdl). Places result in target/generated-sources/axis2/wsdl2code/src \--> <plugin> <groupId>org.apache.axis2</groupId> <artifactId>axis2-wsdl2code-maven-plugin</artifactId> <version>1.5</version> <executions> <execution> <goals> <goal>wsdl2code</goal> </goals> <configuration> <packageName>com.test.wsclient</packageName> <wsdlFile>${project.build.directory}/wsdl/test.wsdl</wsdlFile> <syncMode>sync</syncMode>

   [1]: http://maven.apache.org/plugins/maven-war-plugin/
   [2]: http://mojo.codehaus.org/cobertura-maven-plugin/
   [3]: http://ws.apache.org/axis2/tools/1_2/maven-plugins/maven-java2wsdl-plugin.html
   [4]: http://ws.apache.org/axis2/
   [5]: http://ws.apache.org/axis2/tools/1_2/maven-plugins/maven-wsdl2code-plugin.html
   [6]: http://maven.apache.org/plugins/maven-release-plugin/
   [7]: http://ankitk.wordpress.com/wp-includes/js/tinymce/plugins/wordpress/img/trans.gif (More...)
   [8]: http://maven.apache.org/pom.html
   [9]: http://www.w3.org/TR/wsdl