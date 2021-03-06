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

Last week, while working on a requirement to create an executable jar in our project, I tried the following two maven plug-ins: ** **

** * **Maven assembly plug-in

** * **Maven onejar plug-in

Using these plug-ins I was able to successfully create an executable jar but my executable was dependent on some Spring framework jars for its effective working and on executing the jar

I encountered some weird exceptions .  _(see a part of stack trace below)_..

_Exception in thread "main" org.springframework.beans.factory.parsing.BeanDefinitionParsingException:_

_ Configuration problem: Unable to locate Spring NamespaceHandler for XML schema namespace [http://www.springframework.org/schema/context] at org.springframework.beans.factory.parsing.FailFastProblemReporter. error(FailFastProblemReporter.java:68) at org.springframework.beans.factory.parsing.ReaderContext.error(ReaderContext.java:85) at org.springframework.beans.factory.parsing.ReaderContext.error(ReaderContext.java:80) at org.springframework.beans.factory.xml.BeanDefinitionParserDelegate. error(BeanDefinitionParserDelegate.java:284) at org.springframework.beans.factory.xml.BeanDefinitionParserDelegate. parseCustomElement(BeanDefinitionParserDelegate.java:1332) at org.springframework.beans.factory.xml.BeanDefinitionParserDelegate. parseCustomElement(BeanDefinitionParserDelegate.java:1325) at org.springframework.beans.factory.xml.DefaultBeanDefinitionDocumentReader. parseBeanDefinitions(DefaultBeanDefinitionDocumentReader.java:135) at org.springframework.beans.factory.xml.DefaultBeanDefinitionDocumentReader. registerBeanDefinitions(DefaultBeanDefinitionDocumentReader.java:93)...._

I was getting these exception because the files in META-INF folder in spring jars were overwriting each other. In our case there are several spring jars all having META-INF/spring.handlers, META-INF/spring.schemas which are used by Spring to handle XML schema namespaces. In final executable jar these files were overwriting each other.

Finally I found an effective maven plugin called [maven shade plugin][1] which resolved the problem.

This plugin provides the capability to package the artifact in an uber-jar, including its dependencies and to shade i.e. rename the packages of some of the dependencies. ** Following are the steps which I followed to resolve this issue:**

  1. Create a resources folder in your src/main(ie src/main/resources)
  2. Create a META-INF folder in your src/main/resources
  3. Make MANIFEST.MF, spring.handlers and spring.schemas files under src/main/resources/META-INF.
  4. Paste the following in plugins section of your pom.xml
[sourcecode lang="java"] <plugin> <groupId>org.apache.maven.plugins</groupId> <artifactId>maven-shade-plugin</artifactId> <version>1.4</version> <executions> <execution> <phase>package</phase> <goals> <goal>shade</goal> </goals> <configuration> <transformers> <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer"> <mainClass> com.company.project.Main </mainClass> </transformer> <transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer"> <resource>META-INF/spring.handlers</resource> </transformer> <transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer"> <resource>META-INF/spring.schemas</resource> </transformer> </transformers> <shadedArtifactAttached>true</shadedArtifactAttached> <!-- optional --> <shadedClassifierName>executable</shadedClassifierName> </configuration> </execution> </executions> </plugin> [/sourcecode]

** **

**5. **Build your project and you will get executable jar.

What actually happened is the problem of overwriting files like spring.handlers and spring.schemas.

**Shades provides a provision to append contents of multiple files into one file.**

While aggregating classes/resources from several artifacts into one uber JAR with overlapping resources, things become complicated.

Shades provides you several “Resource Transformers”, in this case, we used_ **ManifestResourceTransformer **_to set the main class of the executable jar **.**

Some jars contain additional resources (such as .properties files) that have the same file name. To avoid overwriting, you can opt to merge them by appending their content into one file. Here we merged the contents of all the files with that specific name using the **_AppendingTransformer_**. For xml files use XmlAppendingTransformer.

<plugin> <groupId>org.apache.maven.plugins</groupId> <artifactId>maven-shade-plugin</artifactId> <version>1.4</version> <executions> <execution> <phase>package</phase> <goals> <goal>shade</goal> </goals> <configuration> <transformers> <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer"> <mainClass> com.company.project.Main </mainClass> </transformer> <transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer"> <resource>META-INF/spring.handlers</resource> </transformer> <transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer"> <resource>META-INF/spring.schemas</resource> </transformer> </transformers> <shadedArtifactAttached>true</shadedArtifactAttached><!-- optional --> <shadedClassifierName>executable</shadedClassifierName> </configuration> </execution> </executions> </plugin>

   [1]: http://maven.apache.org/plugins/maven-shade-plugin/

## Comments

**[Navin Gupta](#4559 "2010-12-31 17:51:39"):** Thanx for giving a nice solution

**[Rajat Rastogi](#5325 "2011-03-01 02:51:16"):** Great...example of knowledge sharing....

**[Alessandro](#5418 "2011-04-08 12:44:07"):** Exactly the problem I encountered with the maven-assembly-plugin. Thanks, I bookmarked it on delicious!

**[Sergei Vasilyev](#5602 "2011-05-30 15:54:30"):** Thanks! Seems pp.1-3 are not required.

**[Mark](#6040 "2011-10-21 04:40:37"):** Try http://jdotsoft.com/JarClassLoader.php It's simple!

