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

<p>In this blog we will talk about some common problems faced while  building, deploying and releasing java based projects in a maven build  environment and also explain solutions for them using different maven  plugin(s). This post will cover usage of the following maven plugin(s): -
<blockquote>
<blockquote>
<blockquote>
<blockquote>
<blockquote>
<blockquote>
<blockquote>
<ul>
    <li><a href="http://maven.apache.org/plugins/maven-war-plugin/">Maven WAR Plugin</a></li>
    <li><a href="http://mojo.codehaus.org/cobertura-maven-plugin/">Maven Cobertura Plugin</a></li>
    <li><a href="http://ws.apache.org/axis2/tools/1_2/maven-plugins/maven-java2wsdl-plugin.html">Maven Java2WSDL Plugin</a> (<a href="http://ws.apache.org/axis2/">Axis2</a>)</li>
    <li><a href="http://ws.apache.org/axis2/tools/1_2/maven-plugins/maven-wsdl2code-plugin.html">Maven WSDL2Code Plugin</a> (<a href="http://ws.apache.org/axis2/">Axis2</a>)</li>
    <li><a href="http://maven.apache.org/plugins/maven-release-plugin/">Maven Release Plugin</a></li>
</ul>
</blockquote>
</blockquote>
</blockquote>
</blockquote>
</blockquote>
</blockquote>
</blockquote>
<img title="More..." src="http://ankitk.wordpress.com/wp-includes/js/tinymce/plugins/wordpress/img/trans.gif" alt="" />
<h3><!--more--></h3>
<h3><strong>Problem: Overlay ZIP or WAR file on another WAR source.</strong></h3>
<strong>Description:</strong> We have a maven project with some artifact  dependencies, classes and resources which need to be bundled together  into a WAR file. The artifact dependencies to be bundled together are: -  axis2 AAR file (Axis Archive), a zipped folder and another WAR file.</p>
<p><strong>Solution:</strong> Use maven WAR plugin with overlays. Add below plugin configuration to the project <a href="http://maven.apache.org/pom.html">POM</a> file.</p>
<p>[sourcecode language="xml"]
&lt;plugin&gt;
  &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
  &lt;artifactId&gt;maven-war-plugin&lt;/artifactId&gt;
  &lt;version&gt;2.1-beta-1&lt;/version&gt;
  &lt;configuration&gt;
    &lt;warName&gt;FinalProject&lt;/warName&gt;
    &lt;overlays&gt;
      &lt;!--Overlay an AAR file inside the generated WAR file --&gt;
      &lt;overlay&gt;
        &lt;groupId&gt;com.test.services&lt;/groupId&gt;
        &lt;artifactId&gt;test-services&lt;/artifactId&gt;
        &lt;type&gt;aar&lt;/type&gt;
        &lt;targetPath&gt;WEB-INF/services&lt;/targetPath&gt;
      &lt;/overlay&gt;
      &lt;!--Overlay a ZIP file and place extracted content under WEB-INF/knowledge --&gt;
      &lt;overlay&gt;
        &lt;groupId&gt;com.test.knowledge&lt;/groupId&gt;
        &lt;artifactId&gt;test-knowledge&lt;/artifactId&gt;
        &lt;type&gt;zip&lt;/type&gt;
        &lt;targetPath&gt;WEB-INF/knowledge&lt;/targetPath&gt;
      &lt;/overlay&gt;
      &lt;!--Overlay a WAR file and exclude the .jpg in final war --&gt;
      &lt;overlay&gt;
        &lt;groupId&gt;com.example.project&lt;/groupId&gt;
        &lt;artifactId&gt;projectdependency&lt;/artifactId&gt;
        &lt;excludes&gt;
          &lt;exclude&gt;images/sampleimage-dependency.jpg&lt;/exclude&gt;
        &lt;/excludes&gt;
      &lt;/overlay&gt;
    &lt;/overlays&gt;
  &lt;/configuration&gt;
&lt;/plugin&gt;
[/sourcecode]</p>
<p>Overlays are applied in the order in which they are defined in the  overlays configuration. By default, the source of the project (a.k.a the  current build) is added first (e.g. before any overlay is applied). The  current build is defined as a special overlay with no groupId,  artifactId. If overlays need to be applied first, simply configure the  current build after those overlays.
<h3><strong>Problem: Ensure build breaks if code coverage &lt; threshold.</strong></h3>
<strong>Description:</strong> Ensure that during each build code coverage is above the required threshold value, if not then the maven build fails.</p>
<p><strong>Solution:</strong> Use <a href="http://mojo.codehaus.org/cobertura-maven-plugin/">Maven cobertura</a> plugin.</p>
<p>[sourcecode language="xml"]
      &lt;plugin&gt;
        &lt;groupId&gt;org.codehaus.mojo&lt;/groupId&gt;
        &lt;artifactId&gt;cobertura-maven-plugin&lt;/artifactId&gt;
        &lt;version&gt;2.4&lt;/version&gt;
        &lt;configuration&gt;
          &lt;instrumentation&gt;
            &lt;excludes&gt;
              &lt;exclude&gt;com/project/test/TestMain.class&lt;/exclude&gt;
            &lt;/excludes&gt;
          &lt;/instrumentation&gt;
          &lt;check&gt;
            &lt;totalLineRate&gt;85&lt;/totalLineRate&gt;
          &lt;/check&gt;
          &lt;formats&gt;
            &lt;format&gt;html&lt;/format&gt;
            &lt;format&gt;xml&lt;/format&gt;
          &lt;/formats&gt;
        &lt;/configuration&gt;
        &lt;executions&gt;
          &lt;execution&gt;
            &lt;id&gt;clean&lt;/id&gt;
            &lt;phase&gt;pre-site&lt;/phase&gt;
            &lt;goals&gt;
              &lt;goal&gt;clean&lt;/goal&gt;
            &lt;/goals&gt;
          &lt;/execution&gt;
          &lt;execution&gt;
            &lt;id&gt;instrument&lt;/id&gt;
            &lt;phase&gt;install&lt;/phase&gt;
            &lt;goals&gt;
              &lt;goal&gt;check&lt;/goal&gt;
            &lt;/goals&gt;
          &lt;/execution&gt;
        &lt;/executions&gt;
      &lt;/plugin&gt;
[/sourcecode]
<h3><strong>Problem: Generate <a href="http://www.w3.org/TR/wsdl">WSDL</a> from a Java class at build time.</strong></h3>
<strong>Solution:</strong> Use <a href="http://ws.apache.org/axis2/tools/1_2/maven-plugins/maven-java2wsdl-plugin.html">Maven Java2WSDL</a> plugin. Below configuration creates a TestProject.wsdl from the class <em>com.test.service.TestWebService</em>.</p>
<p>[sourcecode language="xml"]
&lt;plugin&gt;
  &lt;groupId&gt;org.apache.axis2&lt;/groupId&gt;
  &lt;artifactId&gt;axis2-java2wsdl-maven-plugin&lt;/artifactId&gt;
  &lt;version&gt;1.5&lt;/version&gt;
  &lt;executions&gt;
    &lt;execution&gt;
      &lt;id&gt;generate web service wsdl&lt;/id&gt;
      &lt;goals&gt;
        &lt;goal&gt;java2wsdl&lt;/goal&gt;
      &lt;/goals&gt;
      &lt;configuration&gt;
        &lt;serviceName&gt;TestService&lt;/serviceName&gt;
        &lt;className&gt;com.test.service.TestWebService&lt;/className&gt;
        &lt;targetNamespace&gt;http://www.test.com/TestProject&lt;/targetNamespace&gt;
        &lt;schemaTargetNamespace&gt;http://www.test.com/TestProject&lt;/schemaTargetNamespace&gt;
        &lt;elementFormDefault&gt;qualified&lt;/elementFormDefault&gt;
        &lt;package2Namespace&gt;
          &lt;property&gt;
            &lt;name&gt;com.test.domain&lt;/name&gt;
            &lt;value&gt;http://www.test.com/TestProject&lt;/value&gt;
          &lt;/property&gt;
        &lt;/package2Namespace&gt;
        &lt;outputFileName&gt;target/generated-resources/TestProject.wsdl&lt;/outputFileName&gt;
      &lt;/configuration&gt;
    &lt;/execution&gt;
  &lt;/executions&gt;
&lt;/plugin&gt;
[/sourcecode]</p>
<p>NOTE: The plugin will be invoked automatically in the  generate-sources phase. You can also invoke it directly from the command  line by running the command.</p>
<p>[sourcecode language="xml"]
mvn java2wsdl:java2wsdl
[/sourcecode]
<h3><strong>Problem: Generate web service stub from the wsdl file.</strong></h3>
<strong>Solution:</strong> Use <a href="http://ws.apache.org/axis2/tools/1_2/maven-plugins/maven-wsdl2code-plugin.html">Maven WSDL2Code</a> plugin. This plugin takes as input a WSDL and generates client and  server stubs for calling or implementing a Web service matching the  WSDL.</p>
<p>[sourcecode language="xml"]
&lt;!--
  Generate Java code for the Admin web service (target/wsdl/test.wsdl).
  Places result in target/generated-sources/axis2/wsdl2code/src
--&gt;
&lt;plugin&gt;
  &lt;groupId&gt;org.apache.axis2&lt;/groupId&gt;
  &lt;artifactId&gt;axis2-wsdl2code-maven-plugin&lt;/artifactId&gt;
  &lt;version&gt;1.5&lt;/version&gt;
  &lt;executions&gt;
    &lt;execution&gt;
      &lt;goals&gt;
        &lt;goal&gt;wsdl2code&lt;/goal&gt;
      &lt;/goals&gt;
      &lt;configuration&gt;
        &lt;packageName&gt;com.test.wsclient&lt;/packageName&gt;
        &lt;wsdlFile&gt;${project.build.directory}/wsdl/test.wsdl&lt;/wsdlFile&gt;
        &lt;syncMode&gt;sync&lt;/syncMode&gt;</p>