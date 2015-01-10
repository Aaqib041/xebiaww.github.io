---
layout: post
header-img: img/default-blog-pic.jpg
author: dmittal
description: 
post_id: 249
created: 2007/06/18 08:26:21
created_gmt: 2007/06/18 06:26:21
comment_status: open
---

# Mocking collaborating Abstract class using EasyMock partial mocking

<p><meta content="20070618;8271200" name="CREATED" /><meta content="16010102;0" name="CHANGED" />                      <style type="text/css">     <!--        @page { size: 21cm 29.7cm; margin: 2cm }        P { margin-bottom: 0.21cm }     -->     </style>
<p style="margin-bottom: 0cm">There are times when we need to unit-test methods of a concrete subclass, which colloborate with methods of the abstract superclass. The key requirement is that we want to unit-test the subclass methods <strong>in isolation</strong>; without  bothering about the collaborating methods in the abstract superclass.</p>
<br />
<p style="margin-bottom: 0cm">Lets take a look at a simple example : <strong>Dog</strong> extends <strong>Animal</strong>.</p>
<br />
<p style="margin-bottom: 0cm"></p>
<!--more-->
<p style="margin-bottom: 0cm"><strong>// Code for Animal</strong></p>
[java]
package com.xebia.example;</p>
<p>public abstract class Animal {</p>
<pre><code>public void sit() {}
public void stand() {}
public abstract void expressAnger();

public void speak(String message, Integer volumeLevel) {
    // Here goes the code for the method to to speak-out the message with the given volume.
}
</code></pre>
<p>}</p>
<p>[/java]
<p align="left" style="margin-bottom: 0cm"><strong>// Code for Dog</strong></p>
[java]
package com.xebia.example;</p>
<p>public class Dog extends Animal {</p>
<pre><code>public static final String ANGRY_MESSAGE = "BARK-BARK";

public void expressAnger() {
    speak(ANGRY_MESSAGE, 10);
}
</code></pre>
<p>}
[/java]
<p align="left" style="margin-bottom: 0cm">Test cases for Animal class have already been written. Now comes the turn of Dog class – how do we test <strong>expressAnger</strong> method of Dog class in isolation. Well, that's not hard with EasyMock class extension 2.2 partial mocking feature, which allows (as the name suggests) only selected methods of a class to be mocked.</p>
<p style="margin-bottom: 0cm"></p>
<p style="margin-bottom: 0cm">If we take a look at the <strong>expressAnger</strong> method of <strong>Dog </strong>class, the only thing that we want to test is that  <strong>speak</strong> method of <strong>Animal</strong> class is called with the right parameter values. So, we need to mock the colloborating method <strong>speak</strong> in order to unit-test <strong>expressAnger</strong> method.</p>
<p style="margin-bottom: 0cm"></p>
<br />
<p style="margin-bottom: 0cm">Here is the EasyMock test-case which shows how this can be done.</p>
<br />
<p style="margin-bottom: 0cm"></p>
<p style="margin-bottom: 0cm"><strong>// Code for DogTest</strong></p>
[java]
package com.xebia.example;</p>
<p>import static org.easymock.classextension.EasyMock.createMock;
import static org.easymock.classextension.EasyMock.replay;
import static org.easymock.classextension.EasyMock.verify;</p>
<p>import java.lang.reflect.Method;</p>
<p>import junit.framework.TestCase;</p>
<p>public class DogTest extends TestCase {</p>
<pre><code>// Class under test. Also the colloborating class.
Dog dog;

protected void setUp() throws Exception {
    super.setUp();
    // Create a partially-mocked instance of Dog class. The "methods to be mocked" are passed as an array.
    dog = createMock(Dog.class, new Method[] {Dog.class.getMethod("speak", String.class, Integer.class)});
}

public void testExpressAnger() {
    // Set expectations.
    dog.speak(Dog.ANGRY_MESSAGE, new Integer(10));
    // Change mode.
    replay(dog);
    // Call the method under test.
    dog.expressAnger();
    // Verify expectations.
    verify(dog);
}
</code></pre>
<p>}
[/java]
<p style="margin-bottom: 0cm">The test-case is quite simple – instead of mocking out the dependency/collaborating class (the usual case), we have mocked-out the collaborating method.</p>
<br />
<p style="margin-bottom: 0cm">Of-course, the same trick applies when you want to test one method of a class, which calls other methods of the same class. So, you want to keep normal behaviour of the <strong>methodUnderTest </strong>&amp; mock other <strong>collaboratingMethods </strong>of the same class.</p>
<br />
<p style="margin-bottom: 0cm"><u><strong>Conclusion</strong></u></p>
<br />
<p style="margin-bottom: 0cm">EasyMock partial mocking is quite handy for testing in circumstances where a method of a class calls method of the same or superclass; and you need to test methods of the class in isolation of other methods.</p></p>