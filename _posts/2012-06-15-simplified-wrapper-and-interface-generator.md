---
layout: post
header-img: img/default-blog-pic.jpg
author: Shruti Khattar
description: 
post_id: 14485
created: 2012/06/15 22:41:19
created_gmt: 2012/06/15 17:41:19
comment_status: open
---

# Simplified Wrapper and Interface Generator

In this blog, I am going to share some insights on the software development tool called SWIG- Simplified Wrapper and Interface Generator. SWIG works as an interface compiler, through which you can use the C/C++ native libraries and code in your programming language. This tool has customized features, specific to almost every native, scripting or AI language, to allow C/C++ code to be called from it. It also supports cross language polymorphism. The question that irks is, why would we need it? Let us try and find it out by taking an example of one of the most popular choice of language among software developers - Java. Being one of the most robust and a rich development platform available at our disposal today, it still lacks some important features and has some limitations. Some of them are:  • Java does not expose the lower level system facilities to the programmers. The details of the underlying operating system and hardware devices are abstracted by the JVM. C/C++ provides you a convenient access to both of them. • The Date/Time API in Java. With every new release, the developers are hoping for a better and enhanced version of this library with better presentation, sensible semantics and manipulation capabilities. But, till now, it is disappointing. Though, there are some alternate libraries available, like Joda Time, but that also suffers from some limitations, especially while customizing a new time zone. The libraries in C and C++ are better structured. • Memory management. Because we do not have pointers arithmetic in Java, there is no direct way we can access or manipulate the memory location which is being assigned to the object being created. The destruction of objects cannot be governed by the code, it is entirely in the hands of garbage collector. • In some cases, templates in C++ are more powerful than Generics framework in Java. Example, template functions can accept default parameters, generic functions do not work with default parameters. • The operator overloading feature is very limited. It is not even close to the versatility of the same feature in C++ How to use SWIG in java? Simple! Just download and configure the SWIG plug-in in your editor. The dependency for the tool has to be included in the POM.

``` 

    
    
               org.apache.maven.plugins
               maven-swig-plugin
               1.3.40-1-SNAPSHOT
    


 ```

The C/C++ code is transformed into a corresponding glue code or proxy Java classes, which acts as a wrapper to bind the code into the target language (Java in this case). The C/C++ source code have to be a part of an interface file with .i extension, where each component(s) has to be incorporated with %include directive. We can generate the interface to throw a java exception from C++function. This allows to utilize exhaustive exception handling feature in java.

This will generate a java proxy interface as below. public class Example {

``` 


# include

template<int n> Class Sample{ public: Sample *calculate(int a, int b); void main(){ Sample sample= new Sample(); int factorial= sample.fact(5); int mod= sample.my_mod(7,3); cout<< mod };

Sample:: int fact(int n) { if (n <= 1) cout<<1; else cout<< n*fact(n-1); }

Sample:: int my_mod(int x, int y) { cout<< (x%y); } 
 ```

Let this file be saved as Sample.cpp. The corresponding interface file, which would serve as an interface to swig tool would be:

``` 
 /_sample.i _/ %module sample %include template <int n> %include time.h %{ extern double My_variable; extern int fact(int n); extern int my_mod(int x, int y); %} extern double My_variable; extern int fact(int n); extern int my_mod(int x, int y); %javaexception("java.lang.IllegalArgumentException") calculate { try { $action } catch (std::some_exception &e) { jclass clazz = jenv->FindClass("java/lang/IllegalArgumentException"); jenv->ThrowNew(clazz, "Illegal argument exception"); return $null; } }


 ```

Now, the command:

``` 
 swig -c++ -java sample.i 
 ```

It will create a wrapper sample_wrap.cxx or a corresponding JNI code for the C/C++ function, and it is good to be used in your Java code

``` 
 class JavaApp { public static void main(String argv[]) { System.loadLibrary("sample"); public static int getFact(int n) { return sampleJNI.fact(int n); public Example calculate(int a, int b) throws java.lang.IllegalArgumentException; } } 
 ```

Enums are fully supported and come in 3 categories- Type Safe, proper java enum and type unsafe enums.

A way to generate simple java enum is as below.

``` 
 %include "enums.swg"

%typemap(javain) enum SWIGTYPE "$javainput.ordinal()" %typemap(javaout) enum SWIGTYPE { return $javaclassname.class.getEnumConstants()[$jnicall]; } %typemap(javabody) enum SWIGTYPE ""

%inline %{ enum Direction { NORTH, SOUTH, EAST, WEST }; void setDirection(Direction direction); Direction getDirection(); %}

This will Generate following java type enum class.

public enum Direction { NORTH, SOUTH, EAST, WEST; }

public static void setDirection(Direction direction) { exampleJNI.setDirection(direction.ordinal()); }

public static Direction getDirection() { return Direction.class.getEnumConstants()[exampleJNI.getDirection()]; }
 ```

There is a lot that you can do with this tool, like, restricting the premature garbage collection, rapid prototyping, interactive debugging and so on. But, it is only recommended to be used if your application is using it moderately or needs it extensively. Otherwise, SWIG would be an overkill.