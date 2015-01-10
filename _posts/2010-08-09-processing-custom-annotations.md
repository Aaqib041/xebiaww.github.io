---
layout: post
header-img: img/default-blog-pic.jpg
---

# Processing Custom Annotations 

**How to write custom annotations using APT?**   


Annotations provide data about a program that is not part of the program itself. The more advanced uses of annotations include writing an annotation processor that can read a Java program and take actions based on its annotations. To facilitate this task, release 5.0 of the JDK includes an annotation processing tool, called apt. In release 6 of the JDK, the functionality of apt is a standard part of the Java compiler. 

  
**Using APT-Annotation Processing Tool**

  1. JDK tool for processing source annotations
  2. cleaner model of source and types than doclet
  3. support recursive processing of generated files
  4. Multiple processors (vs single doclet)

Each processor implements the _AnnotationProcessor_ interface in the package com.sun.mirror.apt. This interface has one method i.e. process used by the apt tool to invoke the processor. A processor will "process" one or more annotation types. A processor instance is returned by its corresponding factory - an _AnnotationProcessorFactory_. The apt tool calls the factory's _getProcessorFor_ method to get hold of the processor. During this call, the tool provides an _AnnotationProcessorEnvironment_. In the environment the processor will find everything it needs to get started, including references to the program structure on which it is operating, and the means to communicate and cooperate with the apt tool by creating new files and passing on warning and error messages.

**Steps for creating a custom processor**

  1. Write an annotation processor, extends _AbstractProcessor_
    * Provide implementation for _process _method-public void process();
    * Implement the annotation processing logic
    * Use environment from _AnnotationProcessorFactory_; iterate through types being processed
  2. add tools.jar to classpath
  3. Invoke apt
  4. Use Mirror packages for representing types and processing declaration and types.
**APT Configuration**

  1. Configuration is done by creating text file containing fully qualified name of the AP under file META-INF/services/javax.annotation.processing.Processor. Sample file content is shown below: [sourcecode language="java"]com.abc.ConstructorWithoutArgsProcessor[/sourcecode]
  2. Jar the AP classes and META-INF
[sourcecode language="java"] jar cvMif myProcessor.jar -C com/* META-INF/services/* [/sourcecode]  **Sample Annotation Processor Source Code** [sourcecode language="java"] public class MyCustomProcessor extends AbstractProcessor { public boolean process(Set annotations, RoundEnvironment env) { for (TypeElement type : annotations) { doProcess(env, type); } return true; } [/sourcecode] Let's write a sample annotation [sourcecode language="java"] @Inherited @Documented @Retention(RetentionPolicy.SOURCE) @Target(ElementType.TYPE) public @interface ConstructorWithoutArgs { } [/sourcecode] Here is the code of custom processor: [sourcecode language="java"] @SupportedAnnotationTypes("com.abc.ConstructorWithoutArgs") @SupportedSourceVersion(SourceVersion.RELEASE_6) public class ConstructorWithoutArgsProcessor extends AbstractProcessor { public boolean process(Set<? extends TypeElement> annotations, RoundEnvironment env) { for (TypeElement type : annotations) { processClassesWithoutConstructorArgs(env, type); } return true; } private void processClassesWithoutConstructorArgs(RoundEnvironment env, TypeElement type) { for (Element element : env.getElementsAnnotatedWith(type)) { processClass(element); } } private void processClass(Element element) { if (!doesClassContainConstructorWithoutArgs(element)) { processingEnv.getMessager().printMessage(Diagnostic.Kind.ERROR, "Class " \+ element + " needs a No-Args Constructor"); } } private boolean doesClassContainConstructorWithoutArgs(Element el) { for (Element subelement : el.getEnclosedElements()) { if (subelement.getKind() == ElementKind.CONSTRUCTOR && subelement.getModifiers().contains(Modifier.PUBLIC)) { TypeMirror mirror = subelement.asType(); if (mirror.accept(noArgsVisitor, null)) return true; } } return false; } private static final TypeVisitor<Boolean, Void> noArgsVisitor = new SimpleTypeVisitor6<Boolean, Void>() { public Boolean visitExecutable(ExecutableType t, Void v) { return t.getParameterTypes().isEmpty(); } }; } [/sourcecode] In the doesClassContainConstructorWithoutArgs() method, we iterate through all of the class elements and pick out the public constructors. A class that does not have any will automatically fail the test. Note that when we do not specify any constructor, Java automatically adds a default no-args constructor. Once we have found our constructor element, we visit it with to determine whether the argument list is empty. Using the Visitor pattern in this case helps us to avoid a downcast. Once we have all these files packaged in a jar file, let's call it myProcessor.jar, we can add a processing parameter to our call to javac: [sourcecode language="java"] javac -classpath .;myProcessor.jar -processorpath myProcessor.jar com/*.java [/sourcecode] Let's test this with few test cases: [sourcecode language="java"] @ConstructorWithoutArgs public abstract class SuperClassWithoutArgs { public SuperClassWithoutArgs() { } } // Passes public class PublicConstructorWithoutArgs extends SuperClassWithoutArgs { public PublicConstructorWithoutArgs() { } } // Fails public class NonPublicConstructor extends SuperClassWithoutArgs { NonPublicConstructor() { } } // Fails public class WrongConstructor extends SuperClassWithoutArgs { public WrongConstructor(String aString) { } } [/sourcecode] When we compile these classes with the ConstructorWithoutArgsProcessor, we see the following compiler errors: error: Class NonPublicConstructor needs a No-Args Constructor error: Class WrongConstructor needs a No-Args Constructor 2 errors **Limitations of APT**

  1. no processing of local variable annotations
  2. no inheritance of annotations
In this way, we can write custom annotations which may require specific logic for any certain business needs like persistence, auditing etc. In Java SE 6, annotations can be written only on method parameters and the declarations of packages, classes, methods, fields, and local variables. [JSR 308](http://types.cs.washington.edu/jsr308/) extends Java to allow annotations on nearly any use of a type, and on type parameter declarations. JSR 308 uses a simple prefix syntax for type annotations, with a special case for receiver types and for constructor results. The [Checker Framework](http://types.cs.washington.edu/checker-framework/) is one way to create plug-ins that do pluggable type-checking for the type qualifiers. A pluggable type-checker enables you to detect certain bugs in your code, or to prove that they are not present and this verification happens at compile time. The Checker Framework enhances Java’s type system to make it more powerful and useful. This lets software developers detect and prevent errors in their Java programs.