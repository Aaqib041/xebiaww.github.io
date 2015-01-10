---
layout: post
header-img: img/default-blog-pic.jpg
author: srentala
description: 
post_id: 15687
created: 2013/02/17 20:57:32
created_gmt: 2013/02/17 15:57:32
comment_status: open
---

# Migrating from JUnit to TestNG

It’s been quite a long time developers and testers using JUnit as a Unit test Framework or a Reporting Framework. JUnit has emerged as a very powerful open source tool and is still continuing with its abilities and enhanced versions. But lately with the emergence of TestNG, things have taken a slight deviation. TestNG emerged as a more powerful tool with much more extra features and utilities than JUnit. In this blog I will be focusing in how the emergence has been useful , key differences between them and how is TestNG far more better than JUnit. Let us start with some differences when we are using JUnit and TestNG for different parameters. The differences will be between JUnit-4 vs. TestNG-6 

**1) Object Parameterization:** Parameterization helps to take different sets of values and check the outputs for each of them. 

**a) @Parameters In JUnit :** `@RunWith(value = Parameterized.class) public class TestClass { int score; public TestClass(int score) { this.score=score; } @Parameters public static Collection<Object[]> data() { Object[][] data = new Object[][] { { 50 }, {100 }, {150 } }; return Arrays.asList(data); } @Test public void giveTest() { System.out.println("Score is : " + score); } }`

Here we declare @Parameters annotation and the parameters defined are passed into the constructor to initialize the ‘score’ data variable. Moreover the return type of the class is ‘List[]’ and the type of the data is limited to Strings only. To make a unit test for the above created class we make the following: `public class TestJunit { @Test @Parameters(value="score") public void Test(int score) { System.out.println("Score is : " + score); } }` ** b) @DataProvider in TestNG:** On the other hand in TestNG, we can have return type for parameters not only as Strings but also as integers and Objects.

`@Test(dataProvider="abc") public void xyz (String username, String password, String email) { System.out.println(username + "---" +password + "----" + email); System.out.println("hiiii"); } @DataProvider public Object[][] abc() { Object[][] data = new Object[2][3]; // Data Set 1 data[0][0]= "username1"; data[0][1]= "password1"; data[0][2]="email1"; //Data Set 2 data[1][0]= "username2"; data[1][1]= "password2"; data[1][2]="email2";
    
    
    return data;
    

}` This way the parameterization process becomes less hassle free and easy to implement with just @DataProvider include in your class.

**2) Dependent on Methods /Groups** JUnit does not support any type of dependencies on Methods/Groups. Through this feature you can make one test method dependent on a different test method(s) or methods of same group. Eg: For a Dependent Method, check the following code snippet: `public void main_function(String name) { System.out.println("Executing 'main_function' method"); String a = "subrahmanyam1"; Assert.assertEquals(name, a); } @Test (dependsOnMethods = {"main_function"}) public void sub_function() { System.out.println("Executing 'sub_function' method"); } ` For a Dependent Group, check the following code snippet:
    
    
    <code>@Test(groups = {"main.1"})
    public void abc(String x)
    {
        String a = "Subrahmanyam1";
        Assert.assertEquals(x, a);
        System.out.println("Executing 1");
    }
    
    @Test(groups = {"main.2"})
    public void abc1(String y)
    {
        String b = "Subrahmanyam2";
        Assert.assertEquals(y, b);
        System.out.println("Executing 2");
    }
    
    @Test(dependsOnGroups = {"main.*"})
        public void check()
    {
        System.out.println("Executed 4");
    }</code>
    

In the dependencies case, until the parent class is not executed and passed the dependent method will not be processed. It will be marked as Skipped if the flow has not reached the dependent method.

**3) Running a set of Class (Suite Class)** In JUnit , if one wishes to run a set of classes, then he has to make a separate class all together and include all the classes which he wants to run, but however he cannot wish to run selective unit tests within that class. It has to run the whole class not matter what. `@RunWith(Suite.class) @Suite.SuiteClasses({ ClassA.class, ClassB.class }) public class ClassC { …. }` This tells the system that first run the Class C and then execute the ClassA and ClassB simultaneously.

On the other hand, TestNG has the leverage to use different ways of calling class and individual methods inside it: **a)Through testng.xml:** `&ltsuite name="My suite"&gt &lttest name="Check"&gt &ltclasses&gt &ltclass name="org.package.ClassA " /&gt &ltclass name=" org.package.ClassB " /&gt &lt/classes&gt &lt/test&gt &lt/suite&gt`

**b)We can also include individual methods or groups within a class.** `&ltsuite name="My suite"&gt &lttest name="Check"&gt &ltgroups&gt &ltrun&gt &ltinclude name="Group1"/&gt &lt/run&gt &lt/groups&gt &ltclasses&gt &ltclass name="org.package.ClassA" /&gt &ltclass name="org.package.ClassB" /&gt &lt/classes&gt &lt/test&gt &lt/suite&gt`

This will run all the methods of ClassA and ClassB which belong to group ‘GroupA’ and run them.

**4)Different Annotation Support:** There are many extra annotation support in TestNG than JUnit which provides a more leverage to use them in Testing Framework. Few of them are: a) @Before/AfterSuite: This annotation is used before and after every suite. A suite is a collection of different Tests, each of which could be collection of classes or methods or groups. b) @Before/AfterTest: This annotation is executed before and after every test in a class. c) @Before/AfterGroups: This annotation is executed before and after every group (method). These annotations are not there in JUnit as it supports only @Before/AfterClass and @Before/After which is equivalent to @Before/AfterMethod in TestNg.

TestNG also supports many other features like:

**1) Class Level Annotations:** TestNG unlike JUnit supports many Class level annotations like: a) @Test(alwaysrun=true) : If set true, this test will run even if the dependent methods are missing or failed. b) @Test(Description =”This Sanity Test”): It provides the description of a particular test . c) @Test(priority =1): It sets the priority of running a test if there are multiple test in a class which are to be run. d) @Test(singleThreaded=true): The test will run sequentially or else it will run parallel. And many more.

**2) Running Failed Test Cases:** TestNG supports a feature of running the failed test cases. It created a xml of the failed test cases automatically and once the failed test cases are fixed by the developers we can run that testng-failed.xml. **3) Running JUnit Test cases**: If a framework is designed using JUnit, then TestNG can be used in several manners to convert the JUNIT test cases to TestNG. a) Get the TestNG jar and right click on any JUnit test case -> Convert it into a TestNG test case. b) In the testng.xml , include the following snippet:

` &lt test name="Test1" junit="true"&gt &ltclasses&gt &ltclass name ="com.testng2.ClassK" &gt &ltclasses&gt`

We just have to include junit parameter equal to ‘true’.

**4) TestNG can take the advantage of Listeners:** a) Iannotation Transforemers: Use this Listener when we to change the parameters of any test at the run time . b) Iannotation Transforers2: Use this Listener if one wishes to change the parameters of any test other than @Test at the run time. c) Ihookable: Use this listener if one wishes to execute the ‘run()’ method instead of @Test. It is used in JAAS applications. d) IReporter: Use this listener to generate Inbuilt Reports.

So, When TestNG is supporting so many features which even the latest JUnit version does not support then why not migrate from JUNIT to TESTNG. One can definitely give it a try.