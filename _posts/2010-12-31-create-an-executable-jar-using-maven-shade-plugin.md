---
layout: post
header-img: img/default-blog-pic.jpg
author: samarth rastogi
description: 
post_id: 6961
created: 2010/12/31 10:15:41
created_gmt: 2010/12/31 05:15:41
comment_status: open
---

# Create an executable jar using maven shade plugin

<p><span style="color: #000000;">Last week, while working on a requirement to create an executable jar in our project, I tried the following two maven plug-ins:</span> <span style="color: #000000;"><strong> </strong></span></p>
<p><span style="color: #000000;"><strong> * </strong>Maven assembly plug-in</span></p>
<p><span style="color: #000000;"><strong> * </strong>Maven onejar plug-in</span></p>
<p><span style="color: #000000;">Using these plug-ins I was able to successfully create an executable jar but my executable was dependent on some Spring framework jars for its effective working and on executing the jar</span></p>
<p><span style="color: #000000;">I encountered some weird exceptions .  <em>(see a part of stack trace below)</em>..</span></p>
<p><em><span style="color: #ff0000;">Exception in thread "main" org.springframework.beans.factory.parsing.BeanDefinitionParsingException:</span></em></p>
<p><em><span style="color: #ff0000;"><!--more-->
Configuration problem: Unable to locate Spring NamespaceHandler
for XML schema namespace [http://www.springframework.org/schema/context]
at org.springframework.beans.factory.parsing.FailFastProblemReporter.
error(FailFastProblemReporter.java:68)
at org.springframework.beans.factory.parsing.ReaderContext.error(ReaderContext.java:85)
at org.springframework.beans.factory.parsing.ReaderContext.error(ReaderContext.java:80)
at org.springframework.beans.factory.xml.BeanDefinitionParserDelegate.
error(BeanDefinitionParserDelegate.java:284)
at org.springframework.beans.factory.xml.BeanDefinitionParserDelegate.
parseCustomElement(BeanDefinitionParserDelegate.java:1332)
at org.springframework.beans.factory.xml.BeanDefinitionParserDelegate.
parseCustomElement(BeanDefinitionParserDelegate.java:1325)
at org.springframework.beans.factory.xml.DefaultBeanDefinitionDocumentReader.
parseBeanDefinitions(DefaultBeanDefinitionDocumentReader.java:135)
at org.springframework.beans.factory.xml.DefaultBeanDefinitionDocumentReader.
registerBeanDefinitions(DefaultBeanDefinitionDocumentReader.java:93)....</span></em></p>
<p><span style="color: #000000;">I was getting these exception because the files in META-INF folder in spring jars were overwriting each other. </span><span style="color: #000000;">In our case there are several spring jars all having META-INF/spring.handlers, META-INF/spring.schemas which are used by Spring to handle XML schema namespaces. In final executable jar these files were overwriting each other.</span></p>
<p><span style="color: #000000;">Finally I found an effective maven plugin called <a href="http://maven.apache.org/plugins/maven-shade-plugin/" target="_blank">maven shade plugin</a> which resolved the problem.</span></p>
<p><span style="color: #000000;">This plugin provides the capability to package the artifact in an uber-jar, including its dependencies and to shade i.e. rename the packages of some of the dependencies.
<span style="text-decoration: underline;"><strong>
Following are the steps which I followed to resolve this issue:</strong></span></span>
<ol>
    <li><span style="color: #000000;">Create a resources folder in your src/main(ie src/main/resources)</span></li>
    <li><span style="color: #000000;">Create a META-INF folder in your src/main/resources</span></li>
    <li><span style="color: #000000;">Make MANIFEST.MF, spring.handlers and spring.schemas files under src/main/resources/META-INF.</span></li>
    <li><span style="color: #000000;">Paste the following in plugins section of your pom.xml</span></li>
</ol>
[sourcecode lang="java"]
&lt;plugin&gt;
 &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
 &lt;artifactId&gt;maven-shade-plugin&lt;/artifactId&gt;
 &lt;version&gt;1.4&lt;/version&gt;
 &lt;executions&gt;
  &lt;execution&gt;
   &lt;phase&gt;package&lt;/phase&gt;
       &lt;goals&gt;
          &lt;goal&gt;shade&lt;/goal&gt;
       &lt;/goals&gt;
   &lt;configuration&gt;
    &lt;transformers&gt;
     &lt;transformer
   implementation=&quot;org.apache.maven.plugins.shade.resource.ManifestResourceTransformer&quot;&gt;
      &lt;mainClass&gt; com.company.project.Main &lt;/mainClass&gt;
     &lt;/transformer&gt;
     &lt;transformer
      implementation=&quot;org.apache.maven.plugins.shade.resource.AppendingTransformer&quot;&gt;
      &lt;resource&gt;META-INF/spring.handlers&lt;/resource&gt;
     &lt;/transformer&gt;
     &lt;transformer
      implementation=&quot;org.apache.maven.plugins.shade.resource.AppendingTransformer&quot;&gt;
     &lt;resource&gt;META-INF/spring.schemas&lt;/resource&gt;
     &lt;/transformer&gt;
    &lt;/transformers&gt;
    &lt;shadedArtifactAttached&gt;true&lt;/shadedArtifactAttached&gt;
    &lt;!-- optional --&gt;
    &lt;shadedClassifierName&gt;executable&lt;/shadedClassifierName&gt;
     &lt;/configuration&gt;
   &lt;/execution&gt;
  &lt;/executions&gt;
&lt;/plugin&gt;
[/sourcecode]</p>
<p><strong> </strong></p>
<p><span style="color: #000000;"><strong><span style="font-weight: normal;">5.</span> </strong>Build your project and you will get executable jar.</span></p>
<p><span style="color: #000000;">What actually happened is the problem of overwriting files like spring.handlers and spring.schemas.</span></p>
<p><span style="color: #000000;"> <strong>Shades provides a provision to append contents of multiple files into one file.</strong></span></p>
<p><span style="color: #000000;">While aggregating classes/resources from several artifacts into one uber JAR with overlapping resources, things become complicated.</span></p>
<p><span style="color: #000000;">Shades provides you several “Resource Transformers”, in this case, we used<em> <strong>ManifestResourceTransformer </strong></em>to set the main class of the executable jar <strong>.</strong></span></p>
<p><span style="color: #000000;">Some jars contain additional resources (such as .properties files) that have the same file name. To avoid overwriting, you can opt to merge them by appending their content into one file. Here we merged the contents of all the files with that specific name using the <strong><em>AppendingTransformer</em></strong>. For xml files use XmlAppendingTransformer.</span>
<div id="_mcePaste" style="position: absolute; left: -10000px; top: 684px; width: 1px; height: 1px; overflow: hidden;"><span style="color: #000000;">&lt;plugin&gt;
&lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
&lt;artifactId&gt;maven-shade-plugin&lt;/artifactId&gt;
&lt;version&gt;1.4&lt;/version&gt;
&lt;executions&gt;
&lt;execution&gt;
&lt;phase&gt;package&lt;/phase&gt;
&lt;goals&gt;
&lt;goal&gt;shade&lt;/goal&gt;
&lt;/goals&gt;
&lt;configuration&gt;
&lt;transformers&gt;
&lt;transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer"&gt;
&lt;mainClass&gt; com.company.project.Main &lt;/mainClass&gt;
&lt;/transformer&gt;
&lt;transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer"&gt;
&lt;resource&gt;META-INF/spring.handlers&lt;/resource&gt;
&lt;/transformer&gt;
&lt;transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer"&gt;
&lt;resource&gt;META-INF/spring.schemas&lt;/resource&gt;
&lt;/transformer&gt;
&lt;/transformers&gt;
&lt;shadedArtifactAttached&gt;true&lt;/shadedArtifactAttached&gt;&lt;!-- optional --&gt;
&lt;shadedClassifierName&gt;executable&lt;/shadedClassifierName&gt;
&lt;/configuration&gt;
&lt;/execution&gt;
&lt;/executions&gt;
&lt;/plugin&gt; </span></div></p>

## Comments

**[Navin Gupta](#4559 "2010-12-31 17:51:39"):** Thanx for giving a nice solution

**[Rajat Rastogi](#5325 "2011-03-01 02:51:16"):** Great...example of knowledge sharing....

**[Alessandro](#5418 "2011-04-08 12:44:07"):** Exactly the problem I encountered with the maven-assembly-plugin. Thanks, I bookmarked it on delicious!

**[Sergei Vasilyev](#5602 "2011-05-30 15:54:30"):** Thanks! Seems pp.1-3 are not required.

**[Mark](#6040 "2011-10-21 04:40:37"):** Try http://jdotsoft.com/JarClassLoader.php It's simple!

