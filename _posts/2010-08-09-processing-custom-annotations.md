---
layout: post
header-img: img/default-blog-pic.jpg
author: jdhingra
description: 
post_id: 4263
created: 2010/08/09 12:39:55
created_gmt: 2010/08/09 07:39:55
comment_status: open
---

# Processing Custom Annotations 

<p><strong>How to write custom annotations using APT?</strong>
<br><div>
Annotations provide data about a program that is not part of the program itself. The more advanced uses of annotations include writing an annotation processor that can read a Java program and take actions based on its annotations. To facilitate this task, release 5.0 of the JDK includes an annotation processing tool, called apt. In release 6 of the JDK, the functionality of apt is a standard part of the Java compiler.
<!--more-->
<div style="width: 700px; text-align: left; margin-left: 0px;">
<br><strong>Using APT-Annotation Processing Tool</strong>
<ol>
    <li> JDK tool for processing source annotations</li>
    <li> cleaner model of source and types than doclet</li>
    <li> support recursive processing of generated files</li>
    <li> Multiple processors (vs single doclet)</li>
</ol>
</div>
Each processor implements the <em>AnnotationProcessor</em> interface in the package com.sun.mirror.apt. This interface has one method i.e. process used by the apt tool to invoke the processor. A processor will "process" one or more annotation types. A processor instance is returned by its corresponding factory - an <em>AnnotationProcessorFactory</em>. The apt tool calls the factory's <em>getProcessorFor</em> method to get hold of the processor. During this call, the tool provides an <em>AnnotationProcessorEnvironment</em>. In the environment the processor will find everything it needs to get started, including references to the program structure on which it is operating, and the means to communicate and cooperate with the apt tool by creating new files and passing on warning and error messages.</p>
<strong>Steps for creating a custom processor</strong>
<ol>
    <li> Write an annotation processor, extends <em>AbstractProcessor</em>
<ul>
    <li> Provide implementation for <em>process </em>method-public void process();</li>
    <li> Implement the annotation processing logic</li>
    <li> Use environment from <em>AnnotationProcessorFactory</em>; iterate through types being processed</li>
</ul>
</li>
    <li>add tools.jar to classpath</li>
    <li>Invoke apt</li>
    <li>Use Mirror packages for representing types and processing declaration and types.</li>
</ol>
<strong>APT Configuration</strong>
<ol>
    <li> Configuration is done by creating text file containing fully qualified name of the AP under file 
              META-INF/services/javax.annotation.processing.Processor. Sample file content is shown below:
[sourcecode language="java"]com.abc.ConstructorWithoutArgsProcessor[/sourcecode]</li>
    <li> Jar the AP classes and META-INF</li>
[sourcecode language="java"]
jar cvMif myProcessor.jar -C com/<em> META-INF/services/</em>
[/sourcecode]
</ol>
<strong>Sample Annotation Processor Source Code</strong></p>
<p>[sourcecode language="java"]
public class MyCustomProcessor extends AbstractProcessor {
public boolean process(Set annotations,
RoundEnvironment env) {
for (TypeElement type : annotations) {
doProcess(env, type);
}
return true;
}
[/sourcecode]
Let's write a sample annotation
[sourcecode language="java"]
@Inherited
@Documented
@Retention(RetentionPolicy.SOURCE)
@Target(ElementType.TYPE)
public @interface ConstructorWithoutArgs {
}
[/sourcecode]
Here is the code of custom processor:
[sourcecode language="java"]
@SupportedAnnotationTypes(&quot;com.abc.ConstructorWithoutArgs&quot;)
@SupportedSourceVersion(SourceVersion.RELEASE_6)
public class ConstructorWithoutArgsProcessor extends AbstractProcessor {
  public boolean process(Set&lt;? extends TypeElement&gt; annotations, RoundEnvironment env) {
    for (TypeElement type : annotations) {
      processClassesWithoutConstructorArgs(env, type);
    }
    return true;
  }</p>
<p>private void processClassesWithoutConstructorArgs(RoundEnvironment env, TypeElement type) {
    for (Element element : env.getElementsAnnotatedWith(type)) {
      processClass(element);
    }
  }</p>
<p>private void processClass(Element element) {
    if (!doesClassContainConstructorWithoutArgs(element)) {
      processingEnv.getMessager().printMessage(Diagnostic.Kind.ERROR,
          &quot;Class &quot; + element + &quot; needs a No-Args Constructor&quot;);
    }
  }</p>
<p>private boolean doesClassContainConstructorWithoutArgs(Element el) {
    for (Element subelement : el.getEnclosedElements()) {
      if (subelement.getKind() == ElementKind.CONSTRUCTOR &amp;&amp;
          subelement.getModifiers().contains(Modifier.PUBLIC)) {
        TypeMirror mirror = subelement.asType();
        if (mirror.accept(noArgsVisitor, null)) return true;
      }
    }
    return false;
  }</p>
<p>private static final TypeVisitor&lt;Boolean, Void&gt; noArgsVisitor =
      new SimpleTypeVisitor6&lt;Boolean, Void&gt;() {
        public Boolean visitExecutable(ExecutableType t, Void v) {
          return t.getParameterTypes().isEmpty();
        }
      };
}
[/sourcecode]
In the doesClassContainConstructorWithoutArgs() method, we iterate through all of the class elements and pick out the public constructors. A class that does not have any will automatically fail the test. Note that when we do not specify any constructor, Java automatically adds a default no-args constructor. </p>
<p>Once we have found our constructor element, we visit it with to determine whether the argument list is empty. Using the Visitor pattern in this case helps us to avoid a downcast. 
Once we have all these files packaged in a jar file, let's call it myProcessor.jar, we can add a processing parameter to our call to javac: 
[sourcecode language="java"]
javac -classpath .;myProcessor.jar -processorpath myProcessor.jar com/*.java
[/sourcecode]</p>
<p>Let's test this with few test cases: 
[sourcecode language="java"]
@ConstructorWithoutArgs
public abstract class SuperClassWithoutArgs {
  public SuperClassWithoutArgs() {
  }
}</p>
<p>// Passes
public class PublicConstructorWithoutArgs extends SuperClassWithoutArgs {
  public PublicConstructorWithoutArgs() {
  }
}</p>
<p>// Fails
public class NonPublicConstructor extends SuperClassWithoutArgs {
  NonPublicConstructor() {
  }
}</p>
<p>// Fails
public class WrongConstructor extends SuperClassWithoutArgs {
  public WrongConstructor(String aString) {
  }
}
[/sourcecode]
  When we compile these classes with the ConstructorWithoutArgsProcessor, we see the following compiler errors: </p>
<pre><code>error: Class NonPublicConstructor needs a No-Args Constructor
error: Class WrongConstructor needs a No-Args Constructor
2 errors
</code></pre>
<p><strong>Limitations of APT</strong>
<ol>
    <li> no processing of local variable annotations</li>
    <li> no inheritance of annotations</li>
</ol>
In this way, we can write custom annotations which may require specific logic for any certain business needs like persistence, auditing etc. In Java SE 6, annotations can be written only on method parameters and the declarations of packages, classes, methods, fields, and local variables. <a href="http://types.cs.washington.edu/jsr308/">JSR 308</a> extends Java to allow annotations on nearly any use of a type, and on type parameter declarations. JSR 308 uses a simple prefix syntax for type annotations, with a special case for receiver types and for constructor results.
The <a href="http://types.cs.washington.edu/checker-framework/">Checker Framework</a> is one way to create plug-ins that do pluggable type-checking for the type qualifiers. A pluggable type-checker enables you to detect certain bugs in your code, or to prove that they are not present and this verification happens at compile time. The Checker Framework enhances Java’s type system to make it more powerful and useful. This lets software developers detect and prevent errors in their Java programs.
</div></p>