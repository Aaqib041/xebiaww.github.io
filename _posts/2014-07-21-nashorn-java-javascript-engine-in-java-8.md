---
layout: post
header-img: img/default-blog-pic.jpg
author: SOURABH AGGARWAL
description: 
post_id: 18621
created: 2014/07/21 21:32:30
created_gmt: 2014/07/21 16:32:30
comment_status: open
---

# Nashorn (JAVA + Javascript) Engine in Java 8

<p>The Nashorn Javascript Engine is part of Java SE 8 and competes with other standalone engines like Mozilla <a title="rhino" href="https://developer.mozilla.org/en-US/docs/Mozilla/Projects/Rhino">rhino</a>, <a href="https://code.google.com/p/v8/">Google V8</a> (the engine that powers Google Chrome and <a href="http://nodejs.org/">Node.js</a>)etc.</p>
<p>Starting with the JDK 8 <a title="Nashorn" href="http://docs.oracle.com/javase/8/docs/technotes/guides/scripting/nashorn/">Nashorn</a> replaces Rhino as Java’s embedded JavaScript engine. Nashorn supports the full <a href="http://www.ecma-international.org/ecma-262/5.1/">ECMAScript 5.1 specification</a> plus some <a href="https://wiki.openjdk.java.net/display/Nashorn/Nashorn+extensions">extensions</a>. In the near feature its going to support ECMAScript 6. It compiles JavaScript to Java bytecode using new language features based on <a href="https://jcp.org/en/jsr/detail?id=292">JSR 292</a>, including invokedynamic, that were introduced in <a href="http://docs.oracle.com/javase/7/docs/technotes/guides/vm/multiple-language-support.html">JDK 7</a>.</p>
<p>In JDK 6 also Java has shipped with a <a href="http://docs.oracle.com/javase/6/docs/technotes/guides/scripting/programmer_guide/">bundled JavaScript engine</a> based on <a href="https://developer.mozilla.org/docs/Rhino">Mozilla's Rhino</a>. This allow you to call java from javaScript and javaScript from java. It also provide functionality to run javaScript from console using <a href="http://docs.oracle.com/javase/6/docs/technotes/tools/share/jrunscript.html">jrunscript</a>. Its was good at that time to start with ECMAScript 3 as it  lack performance.</p>
<p>Nashorn gives an functionality to mix Java and JavaScript code together and call JavaScript function from java and vice versa with better performance.</p>
<p>Nashorn can be used programmatically using java code or by using jjs tool located in bin folder inside java installation from command line.</p>
<p>Nashorn supports the invocation of javaScript functions defined in your script files directly from java code. You can pass java objects as function arguments and return data back from the function to the calling java method.</p>
<p>In order to call a function you first have to cast the script engine to  <span style="color: #008000;"><strong>Invocable</strong></span>, The <span style="color: #008000;"><strong>Invocable</strong> </span>interface is implemented by the <strong><span style="color: #008000;">NashornScriptEngine </span></strong>implementation and defines a method <strong><span style="color: #008000;">invokeFunction </span></strong>to call a javascript function for a given name.</p>
<p>In this post i am going threw the example using eclipse with java 8 installation.</p>
<p><span style="line-height: 1.5em;">1. Create new java project with name Nashorn with latest JRE 8 libraries.</span>
<span style="line-height: 1.5em;">2. Create new package with name nashorn.</span>
<span style="line-height: 1.5em;">3. Create new class with name TestNashorn with main method under nashorn.</span>
<span style="line-height: 1.5em;">4. Replace or modify class content with following content.</span>
<p class="success" style="padding-left: 30px;"><span style="line-height: 1.5em;">package nashorn;</span></p>
<p class="success" style="padding-left: 30px;">import java.io.FileNotFoundException;
import java.io.FileReader;</p>
<p class="success" style="padding-left: 30px;">import javax.script.Invocable;</p>
<p class="success" style="padding-left: 30px;">import javax.script.ScriptEngine;
import javax.script.ScriptEngineManager;
import javax.script.ScriptException;</p>
<p class="success" style="padding-left: 30px;">public class TestNashorn {</p>
<p class="success" style="padding-left: 30px;">public static void main(String[] args){
ScriptEngine engine = new ScriptEngineManager().getEngineByName("nashorn");
try {
// loading javascript file by script engine
engine.eval(new FileReader("E:/JAVA8/Nashorn/src/nashorn/script.js"));</p>
<p class="success" style="padding-left: 30px;">// changing engine to Invocable to call methods.
Invocable invocable = (Invocable) engine;</p>
<p class="success" style="padding-left: 30px;">System.out.println("Starting String Test................");</p>
<p class="success" style="padding-left: 30px;">// String array
Object[] strings = {"Sourabh Aggarwal"};
//Result Object containing string
Object stringResult;
try {
// invoking javaScript method testString to get string Object
stringResult = invocable.invokeFunction("testString", strings);
// displaying string output returned
System.out.println(stringResult);
// Type of Object returned
System.out.println(stringResult.getClass());
} catch (NoSuchMethodException e) {
e.printStackTrace();
}</p>
<p class="success" style="padding-left: 30px;">System.out.println("Starting Object Test................");</p>
<p class="success" style="padding-left: 30px;">// Test Object is a plain POJO with one field name testField1
TestPojo testPojo = new TestPojo();
testPojo.setTestField1("testFieldData");</p>
<p class="success" style="padding-left: 30px;">// creating object array
Object[] objects = {testPojo};
//Result Object containing object
Object ObjectResult;
try {
// invoking javaScript method testObject to get TestPojo Object
ObjectResult = invocable.invokeFunction("testObject", objects);
// displaying the modified result done by javascript
System.out.println(((TestPojo)ObjectResult).getTestField1());
} catch (NoSuchMethodException e) {
e.printStackTrace();
}</p>
<p class="success" style="padding-left: 30px;">} catch (FileNotFoundException e) {
e.printStackTrace();
} catch (ScriptException e) {
e.printStackTrace();
}
}</p>
<p class="success" style="padding-left: 30px;">}</p>
5. Create new javaScript with name javaScript.js in path E:/JAVA8/Nashorn/src/nashorn/ or with  another name and path which you refer in the above class with following content.
<p class="success" style="padding-left: 30px;">// handling string
var testString = function(name) {
print("In test string mthod of javascript.");
return "Hi, " + name + ". How are you.";
};</p>
<p class="success" style="padding-left: 30px;">// handling object
var testObject = function (object) {
print(object.getTestField1());
object.setTestField1(object.getTestField1() + " changing object value from javascript.");
print("Getting Object type: " + Object.prototype.toString.call(object));
return object;
};</p>
6. Create new POJO class within same package as Nashorn class with name TestPojo and following content.
<p class="success" style="padding-left: 30px;">package nashorn;</p>
<p class="success" style="padding-left: 30px;">public class TestPojo {</p>
private String testField1;
<p class="success" style="padding-left: 30px;">public String getTestField1() {
return testField1;
}</p>
<p class="success" style="padding-left: 30px;">public void setTestField1(String testField1) {
this.testField1 = testField1;
}</p>
<p class="success" style="padding-left: 30px;">}</p>
7. Just run the Nashorn class and see result.
<p class="success" style="padding-left: 30px;">Result:</p>
<p class="success" style="padding-left: 30px;">Starting String Test................
In test string mthod of javascript.
Hi, Sourabh Aggarwal. How are you.
class java.lang.String
Starting Object Test................
testFieldData
Getting Object type: [object nashorn.TestPojo]
testFieldData changing object value from javascript.</p></p>