---
layout: post
header-img: img/default-blog-pic.jpg
author: srentala
description: 
post_id: 15768
created: 2012/12/20 10:38:41
created_gmt: 2012/12/20 05:38:41
comment_status: open
---

# Tools and Frameworks for testing Web Services

<p>In this blog I am going to explain what all tools are available in the market to test the SOAP/REST Web Services. What are the pros and cons of each of them and which tool can be used for which testing purpose?
Recently I had an opportunity to test the implementation of Web Services of our application over REST. I started exploring quite a few tools which are available in market and which are being used in demand.</p>
<p>After a thorough exploration I found three main tools/ frameworks which can be used to test the Web Services. Let us start digging into REST and SOAP.</p>
<p><strong>What is REST?</strong> The acronym stands for ‘Representational State Transfer’. It means that every link/URL of a website represents a type of Object, and this object can be called using different HTTP methods: GET, POST, PUT, DELETE etc. Rest is nothing but a technology which leverages the HTTP protocol to fetch its web resources. So once these Web Services are hosted/ deployed on the server, our work begins. Let us start with the overview of the tools which can be used to test RESTful Services.
<!--more--></p>
<p><strong>What is SOAP?</strong> It is a protocol based on XML for exchange of information or accessing Web Services over HTTP. SOAP Xml contains four main elements – Envelope (which identifies XML document as SOAP message), Header element, Body Element and Fault element. The request and response are in the form of XMLs.
Let us start exploring the tool/frameworks which can be used to test these Web Services:</p>
<p><strong>FITNESSE</strong>: It is a light weight open source framework which basically compares customers’ expectations to the actual results (in case of Restful Web service testing it check the expected result with the actual response received by the Web service. It uses Fit or SLIM as its Test System. To create a framework on Fitnesse we need to follow the below mentioned steps:</p>
<p><strong>1)</strong> Download fitnesse.jar and run the jar using the command ‘java –jar fitnesse.jar’.
<strong>2)</strong> Open the local host -&gt; Create a Project-&gt; Create a Test Case.
<strong>3)</strong> Create a wiki page. E.g:
<code>a. |Check Tag Name|
b. |Sno|tagIdentifier|tagName?|
c. |1|2341149|Alcohol|
d. |2|2341146|Category|
e. |3|2338638|Medium|</code></p>
<p>A Wiki Table will be created in the format:</p>
<p><code>Sno tagIdentifier tagName?
1 2341149 Alcohol
2 2341146 Category
3 2338638 Medium</code></p>
<p><strong>4)</strong> ‘tagIdentifier’ will be the expected result. ‘tagName?’ will be the actual result.
5) Create a project in your IDE which will have test methods for GET, POST and PUT methods.
a. Create a Class with getters and setter for the variables which will be used.
b. Create a method which will be called in the fitnesse server to compare the results with the expected values.</p>
<p><code>
public class CheckTagName {</code></p>
<p>&nbsp;</p>
<p><code> private Long tagIdentifier;
private int Sno;
public int getSno() {
return Sno;
}
public void setSno(int sno) {
this.Sno = sno;
}
public Long getTagIdentifier() {
return tagIdentifier;
}
public void setTagIdentifier(Long tagIdentifier) {
this.tagIdentifier = tagIdentifier;
}
public String tagName()
{
ClientConfig config = new DefaultClientConfig();
config.getFeatures().put(JSONConfiguration.FEATURE_POJO_MAPPING,Boolean.TRUE);
Client client = Client.create(config);
WebResource service = client.resource("http://localhost:9090/rest-web/tagsUtil/tags/"+ getTagIdentifier());
TagDTO tagDTO = service.accept(MediaType.APPLICATION_JSON).get(TagDTO.class);
return tagDTO.getName();
}
</code></p>
<p>c. Make the Jar of the Project and include it in the classpath in fitnesse server (to identify the methods inside the project).
d. Run the test Case to see if the expected and actual results are same.</p>
<p><strong>When to use Fitnesse?</strong>
1) When we want to make our own framework through which we can create our own test cases, reporting and drive your test cases the way you want.
2) Use any programming language to automate complex scenarios.
3) When we need to handle Actual and Expected Results in a tabular manner.</p>
<p><strong>SOAP –UI:</strong> It is an open source functional testing tool basically to test Web Services. It’s a complete testing tool which offers us to use different features without any need for extensive coding. It covers various types of Automated Testing – Functional, Mocking, Load and Security Testing. Since it’s an open source tool it allows us to perform basic testing of a Web Service without any much effort. We can create Assertions over the tests using a single click.</p>
<p><strong>SOAP UI Pro version</strong> allows us to use many other advanced features which we will be discussing soon.
Following are the main assertions or tests that SOAP UI supports which otherwise had to be called by creating our own test methods in other frameworks:
<strong>1) Simple Contains:</strong> Looks for a pattern and see whether the response contains the following text or not.
<strong>2) Simple Not Contains:</strong> Looks for a pattern and checks that the given text is not contained the response.
<strong>3) Response SLA:</strong> Checks whether a response is coming within the time specified in milliseconds and passes or fail the assertion accordingly.
<strong>4) XPath Match:</strong> The response shown in the outline editor of SOAP UI shows the hierarchical structure, and for each node we can create an XPath assertion.
E.g.: Let’s say we have a response:
<code> &amp;ltId: response&amp;gt
&amp;ltname&amp;gtPeter&amp;lt/name&amp;gt
&amp;ltId:response&amp;gt&amp;lt/code&amp;gt</code></p>
<p>And I want to apply XPath Match over this node, I will specify:
<strong>XPath Expression </strong>: //id:response
<strong>Expected Result:</strong>Peter</p>
<p>We can also allow wildcard usages on the expected expressions.</p>
<p><strong>5) XQuery Match:</strong> We can create simple queries on the Xpaths and compare them against the expected results:
e.g: We have a response like :
<code> &amp;ltbank&amp;gt
&amp;ltamount&amp;gt1000&amp;lt/amount&amp;gt
&amp;lt/bank&amp;gt
&amp;ltbank&amp;gt
&amp;ltamount&amp;gt3500&amp;lt/amount&amp;gt
&amp;lt/bank&amp;gt
&amp;ltbank&amp;gt
&amp;ltamount&amp;gt8555&amp;lt/amount&amp;gt
&amp;lt/bank&amp;gt&amp;lt/code&amp;gt</code></p>
<p>And we want to retrieve all the amount for nodes.
We specify :
<code>
&amp;ltresult&amp;lt
{ for $x in //bank
Return {data($x/amount/text())}
}
&amp;lt/result&amp;gt
</code></p>
<p>In the ‘Xpath Query expression’ and expect the output to be :
<code>
&amp;ltresult&amp;gt
&amp;ltamount&amp;gt1000&amp;lt/amount&amp;gt
&amp;ltamount&amp;gt3500&amp;lt/amount&amp;gt
&amp;ltamount&amp;gt8555&amp;lt/amount&amp;gt
&amp;lt/result&amp;gt
</code></p>
<p><strong>6) Script Assertions:</strong> Apart from the Query expressions applied on Xpaths we can create our own scripts and compare them with the expected values in Groovy or Java Script.</p>
<p>There are many other numerous features which SOAP UI provides which are inbuilt and can be applied to any response tag/element.</p>
<p>SOAP UI Pro provides advanced features like:
<strong>1) Use of Editors</strong>:The Outline Editor is used when figuring out the structure and the Form editor is used when we want to send the actual requests.
<strong>2) Data Source Driven testing</strong>: We can import various data sources -text files, XML, Groovy, Excel, Directory, JDBC (Relational Database), and the Internal Grid data source to fed into the test.
<strong>3) Point and Click Testing</strong>: It provides some extra assertions like -count, declare etc which can be automatically created in no time.
<strong>4) Coverage</strong>: It provides graphical coverage representation of the test cases.
<strong>5) Security Testing</strong>: Provides features of creating a set of Vulnerability test cases.
<strong>6) Refactoring:</strong> Easy refactoring using 'Search and replace'.
<strong>7) SQL Builder</strong> Creates SQL queries and statements using GUI.
<strong>8) Reporting:</strong> Generates detailed Reports of Test Suites, Test cases into any standard format.
<strong>9) Requirements Mapping:</strong> Provides a way to map requirements against Test Cases.</p>