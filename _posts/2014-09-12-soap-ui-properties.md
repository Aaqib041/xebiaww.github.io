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

Properties plays the major role for webservices testing in SOAPUI. Properties are used to parameterize your functional testcase executions and helpful to execute the same test case with different data with ease . For example:

  1. Properties can be used to parametrize your request(Soap or Rest) with different data execution

  2. Properties can be used to parametrize your endpoints. It help to run your whole test suite with a single change in properties.

  3. Properties can be used to share the part of output of one response into another request

  4. Properties can be central to use in different test cases of a single suite or to use in different test suite with in a single project or in different project from global location.

  5. Properties can be used to keep the authentication credentials which can be easily import in multiple test cases of multiple test suites.

  6. Properties can also be used to save the session ids to share in further steps or in other test cases at the time of executions. 

**SOAP UI TYPE OF PROPERTIES**

![][1]

**Default Property:**

·         These are the set of properties which comes by default with every soapUI installation.

·         We can change the values of these properties (not in every case) & consume as when needed.

**Custom/User Defined Property:**

·         These are the set of properties which the end user defines as per his requirement.

·         It can be used a temporary storage for validating the end result of any test (like in assertion).

##SOAP UI LEVELS OF PROPERTIES

###Global Properties: 
specify/define the properties associated with installed version of soapUI. These properties can be access across the project/test suites/ test cases and so on. 
### Project Properties: 
specify the properties associated with the current project. This property can be used by all the subset (test suite, test case, test step, script) of the project. 
###Test Suite Properties: 
specify the properties associated with the current test suite. This property can be used by all the subset (test case, test step, script) of the test suite. 
### Test Cases Properties: 
specify the properties associated with the current test case. This property can be used by all the subset (test step, script) of the test cases. 
###Test Step Properties: 
specify the properties associated with the current test step. This property can be used by all the subset (test step, property transfer, script) of the test steps 

##Snapshots ![Soapui Properties][2]

**How to use properties as parmaeterization in SoapUI.**

In Soapui it is very simple to use properties values as a parameter. It syntax is: ${PropertyLocation#propertyName} For example: Project: ${#Project#Propertyname} TestSuite : ${#TestSuite#Propertyname} TestCase : ${#TestCase#Propertyname} **Webservice snapshot:**

![How to Parameterize the Soap request][3]

**End Point Parameterization**:

![How to parameterize End point][4]

**Groovy for Properties**

#------------------------------------------------Project Commands------------------------------------------------

```
_Define Project object_ def project = testRunner.testCase.testSuite.project _Get Default Project Properties_ i. Project Name log.info project.name ii. Default Scripting Language log.info project.getDefaultScriptLanguage() In Similar fashion we can fetch other default values of project properties like ‘Description (getDescription)’, etc _Get Custom Project Properties_ log.info project.getPropertyValue(“Property1”) _Change/Set default project property values_ i. Name log.info project.setName("Currency") ii. Change Default Script Language log.info project.setScriptLibrary("Groovy") In Similar fashion we can set other default values of project properties like ‘Description (setDescription)’, etc _Set Custom Project Properties_ log.info project.setPropertyValue(“Property1”, “SampleValue”) #------------------------------------------Test Suite Commands-------------------------------------------- _Define Test Suite object_ def sTestSuite = testRunner.testCase.testSuite _Get Default Test Suite Properties_ i. Get Test Suite Name log.info sTestSuite.name _Get Custom TestSuite Properties_ log.info sTestSuite.getPropertyValue("SuiteProperty1") _Change default Test Suite property values_ i. Change the Name of Suite log.info sTestSuite.setName("NewDemo") _Set Custom Test Suite Properties_ log.info sTestSuite.setPropertyValue("SuiteProperty1", "ChangeSampleValue")
```

   [1]: https://images-blogger-opensocial.googleusercontent.com/gadgets/proxy?url=http%3A%2F%2F4.bp.blogspot.com%2F-ngIOnMIwFIY%2FUZj6TCDUJlI%2FAAAAAAAAGrw%2FZ-yddFQAUy8%2Fs400%2FTypeOfSoapUIProperties.jpg&container=blogger&gadget=a&rewriteMime=image%2F*
   [2]: http://xebee.xebia.in/wp-content/uploads/2014/08/SoapUI-Properties-300x159.jpg
   [3]: http://xebee.xebia.in/wp-content/uploads/2014/08/Parametrization-1024x439.jpg
   [4]: http://xebee.xebia.in/wp-content/uploads/2014/08/EndPointParametrization.jpg