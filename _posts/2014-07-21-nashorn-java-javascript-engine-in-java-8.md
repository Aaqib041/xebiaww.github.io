---
layout: post
header-img: img/default-blog-pic.jpg
---

# Nashorn (JAVA + Javascript) Engine in Java 8

The Nashorn Javascript Engine is part of Java SE 8 and competes with other standalone engines like Mozilla [rhino](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/Rhino), [Google V8](https://code.google.com/p/v8/) (the engine that powers Google Chrome and [Node.js](http://nodejs.org/))etc. Starting with the JDK 8 [Nashorn](http://docs.oracle.com/javase/8/docs/technotes/guides/scripting/nashorn/) replaces Rhino as Java’s embedded JavaScript engine. Nashorn supports the full [ECMAScript 5.1 specification](http://www.ecma-international.org/ecma-262/5.1/) plus some [extensions](https://wiki.openjdk.java.net/display/Nashorn/Nashorn+extensions). In the near feature its going to support ECMAScript 6. It compiles JavaScript to Java bytecode using new language features based on [JSR 292](https://jcp.org/en/jsr/detail?id=292), including invokedynamic, that were introduced in [JDK 7](http://docs.oracle.com/javase/7/docs/technotes/guides/vm/multiple-language-support.html). In JDK 6 also Java has shipped with a [bundled JavaScript engine](http://docs.oracle.com/javase/6/docs/technotes/guides/scripting/programmer_guide/) based on [Mozilla's Rhino](https://developer.mozilla.org/docs/Rhino). This allow you to call java from javaScript and javaScript from java. It also provide functionality to run javaScript from console using [jrunscript](http://docs.oracle.com/javase/6/docs/technotes/tools/share/jrunscript.html). Its was good at that time to start with ECMAScript 3 as it  lack performance. Nashorn gives an functionality to mix Java and JavaScript code together and call JavaScript function from java and vice versa with better performance. Nashorn can be used programmatically using java code or by using jjs tool located in bin folder inside java installation from command line. Nashorn supports the invocation of javaScript functions defined in your script files directly from java code. You can pass java objects as function arguments and return data back from the function to the calling java method. In order to call a function you first have to cast the script engine to  **Invocable**, The **Invocable** interface is implemented by the **NashornScriptEngine **implementation and defines a method **invokeFunction **to call a javascript function for a given name. In this post i am going threw the example using eclipse with java 8 installation. 1\. Create new java project with name Nashorn with latest JRE 8 libraries. 2\. Create new package with name nashorn. 3\. Create new class with name TestNashorn with main method under nashorn. 4\. Replace or modify class content with following content.

package nashorn;

import java.io.FileNotFoundException; import java.io.FileReader;

import javax.script.Invocable;

import javax.script.ScriptEngine; import javax.script.ScriptEngineManager; import javax.script.ScriptException;

public class TestNashorn {

public static void main(String[] args){ ScriptEngine engine = new ScriptEngineManager().getEngineByName("nashorn"); try { // loading javascript file by script engine engine.eval(new FileReader("E:/JAVA8/Nashorn/src/nashorn/script.js"));

// changing engine to Invocable to call methods. Invocable invocable = (Invocable) engine;

System.out.println("Starting String Test................");

// String array Object[] strings = {"Sourabh Aggarwal"}; //Result Object containing string Object stringResult; try { // invoking javaScript method testString to get string Object stringResult = invocable.invokeFunction("testString", strings); // displaying string output returned System.out.println(stringResult); // Type of Object returned System.out.println(stringResult.getClass()); } catch (NoSuchMethodException e) { e.printStackTrace(); }

System.out.println("Starting Object Test................");

// Test Object is a plain POJO with one field name testField1 TestPojo testPojo = new TestPojo(); testPojo.setTestField1("testFieldData");

// creating object array Object[] objects = {testPojo}; //Result Object containing object Object ObjectResult; try { // invoking javaScript method testObject to get TestPojo Object ObjectResult = invocable.invokeFunction("testObject", objects); // displaying the modified result done by javascript System.out.println(((TestPojo)ObjectResult).getTestField1()); } catch (NoSuchMethodException e) { e.printStackTrace(); }

} catch (FileNotFoundException e) { e.printStackTrace(); } catch (ScriptException e) { e.printStackTrace(); } }

}

5\. Create new javaScript with name javaScript.js in path E:/JAVA8/Nashorn/src/nashorn/ or with  another name and path which you refer in the above class with following content. 

// handling string var testString = function(name) { print("In test string mthod of javascript."); return "Hi, " + name + ". How are you."; };

// handling object var testObject = function (object) { print(object.getTestField1()); object.setTestField1(object.getTestField1() + " changing object value from javascript."); print("Getting Object type: " + Object.prototype.toString.call(object)); return object; };

6\. Create new POJO class within same package as Nashorn class with name TestPojo and following content. 

package nashorn;

public class TestPojo {

private String testField1; 

public String getTestField1() { return testField1; }

public void setTestField1(String testField1) { this.testField1 = testField1; }

}

7\. Just run the Nashorn class and see result. 

Result:

Starting String Test................ In test string mthod of javascript. Hi, Sourabh Aggarwal. How are you. class java.lang.String Starting Object Test................ testFieldData Getting Object type: [object nashorn.TestPojo] testFieldData changing object value from javascript.