---
layout: post
header-img: img/default-blog-pic.jpg
author: saurabhgupta
description: 
post_id: 18830
created: 2014/09/12 08:54:24
created_gmt: 2014/09/12 03:54:24
comment_status: open
---

# SOAP UI Properties

<p>Properties plays the major role for webservices testing in SOAPUI. Properties are used to parameterize your functional testcase executions and helpful to execute the same test case with different data with ease . For example:</p>
<ol>
<li>
<p>Properties can be used to parametrize your request(Soap or Rest) with different data execution</p>
</li>
<li>
<p>Properties can be used to parametrize your endpoints. It help to run your whole test suite with a single change in properties.</p>
</li>
<li>
<p>Properties can be used to share the part of output of one response into another request</p>
</li>
<li>
<p>Properties can be central to use in different test cases of a single suite or to use in different test suite with in a single project or in different project from global location.</p>
</li>
<li>
<p>Properties can be used to keep the authentication credentials which can be easily import in multiple test cases of multiple test suites.</p>
</li>
<li>
<p>Properties can also be used to save the session ids to share in further steps or in other test cases at the time of executions.
<p style="text-align: center"><b><span style="text-decoration: underline"><span style="font-size: x-large">SOAP UI TYPE OF PROPERTIES</span></span></b></p>
<p style="text-align: center"><a href="http://4.bp.blogspot.com/-ngIOnMIwFIY/UZj6TCDUJlI/AAAAAAAAGrw/Z-yddFQAUy8/s1600/TypeOfSoapUIProperties.jpg"><img alt="" src="https://images-blogger-opensocial.googleusercontent.com/gadgets/proxy?url=http%3A%2F%2F4.bp.blogspot.com%2F-ngIOnMIwFIY%2FUZj6TCDUJlI%2FAAAAAAAAGrw%2FZ-yddFQAUy8%2Fs400%2FTypeOfSoapUIProperties.jpg&amp;container=blogger&amp;gadget=a&amp;rewriteMime=image%2F*" width="400" height="181" border="0" /></a></p></p>
</li>
</ol>
<div>

<b><span>Default Property:</span></b>

</div>

<div>
<p class="clean"><span style="font-family: Symbol">·<span style="font-family: 'Times New Roman'">         </span></span><span>These are the set of properties which comes by default with every soapUI installation.</span></p>

</div>

<p><span style="font-family: Symbol">·<span style="font-family: 'Times New Roman'">         </span></span><span>We can change the values of these properties (not in every case) &amp; consume as when needed.</span></p>
<p><b><span>Custom/User Defined Property:</span></b></p>
<p><span style="font-family: Symbol">·<span style="font-family: 'Times New Roman'">         </span></span><span>These are the set of properties which the end user defines as per his requirement.</span>
<div>
<p class="clean"><span style="font-family: Symbol">·<span style="font-family: 'Times New Roman'">         </span></span><span>It can be used a temporary storage for validating the end result of any test (like in assertion).</span></p></p>
<div align="center">

<b><span style="text-decoration: underline"><span style="font-size: x-large">SOAP UI LEVELS OF PROPERTIES</span></span></b>

</div>

<div>

<span># <strong>Global Properties</strong>: specify/define the properties associated with installed version of soapUI. These properties can be access across the project/test suites/ test cases and so on.</span>

# <strong>Project Properties</strong>: specify the properties associated with the current project. This property can be used by all the subset (test suite, test case, test step, script) of the project.

# <strong>Test Suite Properties</strong>: specify the properties associated with the current test suite. This property can be used by all the subset (test case, test step, script) of the test suite.

# <strong>Test Cases Properties</strong>: specify the properties associated with the current test case. This property can be used by all the subset (test step, script) of the test cases.

# <strong>Test Step Properties</strong>: specify the properties associated with the current test step. This property can be used by all the subset (test step, property transfer, script) of the test steps

<span style="text-decoration: underline"><strong>Snapshots:</strong></span>

<a href="http://xebee.xebia.in/wp-content/uploads/2014/08/SoapUI-Properties.jpg"><img class="aligncenter size-medium wp-image-18838" alt="Soapui Properties" src="http://xebee.xebia.in/wp-content/uploads/2014/08/SoapUI-Properties-300x159.jpg" width="300" height="159" /></a>
<p style="text-align: center"><span style="text-decoration: underline;font-size: 18px"><strong>How to use properties as parmaeterization in SoapUI.</strong></span></p>
In Soapui it is very simple to use properties values as a parameter. It syntax is:

${PropertyLocation#propertyName}

For example:

Project: ${#Project#Propertyname}

TestSuite : ${#TestSuite#Propertyname}

TestCase : ${#TestCase#Propertyname}

<span style="text-decoration: underline"><strong>Webservice snapshot:</strong></span>
<p style="text-align: center"><a href="http://xebee.xebia.in/wp-content/uploads/2014/08/Parametrization.jpg"><img class="aligncenter size-large wp-image-18840" alt="How to Parameterize the Soap request" src="http://xebee.xebia.in/wp-content/uploads/2014/08/Parametrization-1024x439.jpg" width="620" height="265" /></a></p>
<p style="text-align: left"><span style="text-decoration: underline"><strong>End Point Parameterization</strong></span>:</p>
<p style="text-align: left"><a href="http://xebee.xebia.in/wp-content/uploads/2014/08/EndPointParametrization.jpg"><img class="aligncenter size-full wp-image-18839" alt="How to parameterize End point" src="http://xebee.xebia.in/wp-content/uploads/2014/08/EndPointParametrization.jpg" width="949" height="244" /></a></p>
<p style="text-align: center"><span style="text-decoration: underline;font-size: 18px"><strong>Groovy for Properties</strong></span></p>
<p style="text-align: left">#------------------------------------------------Project Commands------------------------------------------------</p>
<em><span style="text-decoration: underline">Define Project object</span></em>
<span style="color: #000080">def project = testRunner.testCase.testSuite.project</span>

<span style="text-decoration: underline"><em>Get Default Project Properties</em></span>
i. Project Name
<span style="color: #000080">log.info project.name</span>

ii. Default Scripting Language
<span style="color: #000080">log.info project.getDefaultScriptLanguage()</span>

In Similar fashion we can fetch other default values of project properties like ‘Description (getDescription)’, etc

<span style="text-decoration: underline"><em>Get Custom Project Properties</em></span>
<span style="color: #000080">log.info project.getPropertyValue(“Property1”)</span>

<span style="text-decoration: underline"><em>Change/Set default project property values</em></span>
i. Name
<span style="color: #000080">log.info project.setName("Currency")</span>

ii. Change Default Script Language
<span style="color: #000080">log.info project.setScriptLibrary("Groovy")</span>

In Similar fashion we can set other default values of project properties like ‘Description (setDescription)’, etc

<em><span style="text-decoration: underline">Set Custom Project Properties</span></em>
<span style="color: #000080">log.info project.setPropertyValue(“Property1”, “SampleValue”)</span>

#------------------------------------------Test Suite Commands--------------------------------------------

<em><span style="text-decoration: underline">Define Test Suite object</span></em>
<span style="color: #000080">def sTestSuite = testRunner.testCase.testSuite</span>

<em><span style="text-decoration: underline">Get Default Test Suite Properties</span></em>
i. Get Test Suite Name
<span style="color: #000080">log.info sTestSuite.name</span>

<em><span style="text-decoration: underline">Get Custom TestSuite Properties</span></em>
<span style="color: #000080">log.info sTestSuite.getPropertyValue("SuiteProperty1")</span>

<em><span style="text-decoration: underline">Change default Test Suite property values</span></em>
i. Change the Name of Suite
<span style="color: #000080">log.info sTestSuite.setName("NewDemo")</span>

<em><span style="text-decoration: underline">Set Custom Test Suite Properties</span></em>
<span style="color: #000080">log.info sTestSuite.setPropertyValue("SuiteProperty1", "ChangeSampleValue")</span>