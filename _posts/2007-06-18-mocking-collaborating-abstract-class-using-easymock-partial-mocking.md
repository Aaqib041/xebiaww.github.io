---
layout: post
header-img: img/default-blog-pic.jpg
---

# Mocking collaborating Abstract class using EasyMock partial mocking

There are times when we need to unit-test methods of a concrete subclass, which colloborate with methods of the abstract superclass. The key requirement is that we want to unit-test the subclass methods **in isolation**; without bothering about the collaborating methods in the abstract superclass.

  


Lets take a look at a simple example : **Dog** extends **Animal**.

  


**// Code for Animal**

[java] package com.xebia.example; public abstract class Animal { public void sit() {} public void stand() {} public abstract void expressAnger(); public void speak(String message, Integer volumeLevel) { // Here goes the code for the method to to speak-out the message with the given volume. } } [/java] 

**// Code for Dog**

[java] package com.xebia.example; public class Dog extends Animal { public static final String ANGRY_MESSAGE = "BARK-BARK"; public void expressAnger() { speak(ANGRY_MESSAGE, 10); } } [/java] 

Test cases for Animal class have already been written. Now comes the turn of Dog class – how do we test **expressAnger** method of Dog class in isolation. Well, that's not hard with EasyMock class extension 2.2 partial mocking feature, which allows (as the name suggests) only selected methods of a class to be mocked.

If we take a look at the **expressAnger** method of **Dog **class, the only thing that we want to test is that **speak** method of **Animal** class is called with the right parameter values. So, we need to mock the colloborating method **speak** in order to unit-test **expressAnger** method.

  


Here is the EasyMock test-case which shows how this can be done.

  


**// Code for DogTest**

[java] package com.xebia.example; import static org.easymock.classextension.EasyMock.createMock; import static org.easymock.classextension.EasyMock.replay; import static org.easymock.classextension.EasyMock.verify; import java.lang.reflect.Method; import junit.framework.TestCase; public class DogTest extends TestCase { // Class under test. Also the colloborating class. Dog dog; protected void setUp() throws Exception { super.setUp(); // Create a partially-mocked instance of Dog class. The "methods to be mocked" are passed as an array. dog = createMock(Dog.class, new Method[] {Dog.class.getMethod("speak", String.class, Integer.class)}); } public void testExpressAnger() { // Set expectations. dog.speak(Dog.ANGRY_MESSAGE, new Integer(10)); // Change mode. replay(dog); // Call the method under test. dog.expressAnger(); // Verify expectations. verify(dog); } } [/java] 

The test-case is quite simple – instead of mocking out the dependency/collaborating class (the usual case), we have mocked-out the collaborating method.

  


Of-course, the same trick applies when you want to test one method of a class, which calls other methods of the same class. So, you want to keep normal behaviour of the **methodUnderTest **& mock other **collaboratingMethods **of the same class.

  


_**Conclusion**_

  


EasyMock partial mocking is quite handy for testing in circumstances where a method of a class calls method of the same or superclass; and you need to test methods of the class in isolation of other methods.