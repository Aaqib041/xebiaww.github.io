---
layout: post
header-img: img/default-blog-pic.jpg
author: ngaur
description: 
post_id: 18325
created: 2014/05/26 13:51:06
created_gmt: 2014/05/26 08:51:06
comment_status: open
---

# Customizing SOAP-UI made easy!

Hi all, we are going try something interesting in this discussion!

As we all are aware of the fact that '**Test Automation**' is always the hot topic in Quality and Assurance field.This time lets try something which is more valuable to automation.In this blog, I'll be discussing how we can easily customize and extent SOAP UI, and achieve some valuable output by utilizing its core functionality.

**Concept :** Its important to understand the concept before going ahead.Soap UI introduces an extensive factory based mechanism, that allows user to add custom objects to SOAP UI like TestStep, Assertions, Editor View etc.See <http://www.soapui.org/Developers-Corner/custom-factories.html> . In this discussion we will add '**custom test step' ** ie. 'ConnectToLinuxBox' in SOAP UI that will simply connect to any linux box, using these factory extension. SOAP UI uses Apache XMLBean framework for managing the configuration, we will see further how configuration are read by SOAP UI.

Lets get started!

**Step 1 :  'Create a Test Step Factory class'**

As SOAP UI uses factory based mechanism, we need to create a factory class, say 'ConnectToLinuxBoxFactory', and this class should extend 'WsdlTestStepFactory' SOAP UI API class. Lets put it together.

_"public class ConnectToLinuxBoxFactory extends WsdlTestStepFactory_

_{_ _ private static final String STEP_ID = "ConnectToLinuxBox";  //Step id used as a string._

_//Constructor method. _ _ public ConnectToLinuxBoxFactory()_ _ {_ _ // Calls 'WsdlTestStepFactory' constructor with the step id, description etc of custom step._

_ super( STEP_ID, "ConnectToLinuxBox TestStep", "Connect to any linux box", "ConnectToLinuxBox.ico" );_ _ }_

_//@Overridden method._ _ // This is called by SOAP UI when we actually create a test step under test case. _ _ public WsdlTestStep buildTestStep( WsdlTestCase testCase, TestStepConfig config, boolean forLoadTest )_ _ {_ _ return new ConnectToLinuxBoxTestStep( testCase, config, forLoadTest );_ _ }_

_//@Overridden method._ _ public TestStepConfig createNewTestStep( WsdlTestCase testCase, String name )_ _ {_ _ //Creating a factory instance and returning the test step config object._

_ TestStepConfig testStepConfig = TestStepConfig.Factory.newInstance();_ _ testStepConfig.setType( STEP_ID );_ _ testStepConfig.setName( name );_ _ return testStepConfig;_ _ }_

_//@Overridden method. Setting the create flag as true. _ _ public boolean canCreate()_ _ {_ _ return true;_ _ }_ _}"_

**Step 2:  'Implement actual Test Step'.**

Its time to actually implement the Test Step class, try to be with the flow, as this is the actual class that contains logic implementation. Create 'ConnectToLinuxBoxTestStep' class that extends 'WsdlTestStepWithProperties' and also implements 'PropertyExpansionContainer' interface.

_Note -- "Implementing 'PropertyExpansionContainer' is needed to effectively use property expansion in SOAP UI test step."_

Again, lets put it together.

_"public class ConnectToLinuxBoxTestStep extends WsdlTestStepWithProperties implements PropertyExpansionContainer_ _{_ _ // Private members._ _ private String username;_ _ private String password;_ _ private String host;_

_ private Session session= null;_

_ //Constructor method._ _ protected ConnectToLinuxBoxTestStep( WsdlTestCase testCase, TestStepConfig config, boolean forLoadTest )_ _ {_ _ // Calling a super constructor, and passing current test case, and its config object._ _ super( testCase, config, true, forLoadTest );_

_if( !forLoadTest )_ _ {_ _ //If its not used as a load test then set its icon, 'ConnectToLinuxBox.ico', this is only the image that will be in the root of SOAP UI home._ _ setIcon( UISupport.createImageIcon( "ConnectToLinuxBox.ico" ) );_ _ }_ _ //if configuration has not been read, then read config._ _ if( config.getConfig() != null )_ _ {_ _ readConfig( config );_ _ }_ _ }_

_//Implementation of readConfig(), and initializing the properties of test step. _ _ private void readConfig( TestStepConfig config )_ _ {_ _ XmlObjectConfigurationReader reader = new XmlObjectConfigurationReader( config.getConfig() );_ _ username = reader.readString( "username", "" );_ _ password = reader.readString( "password", "" );_ _ host = reader.readString( "host", "" );_

_ }_

_//Implementation of updateConfig()._ _ private void updateConfig()_ _ {_ _ XmlObjectConfigurationBuilder builder = new XmlObjectConfigurationBuilder();_ _ builder.add( "username", username );_ _ builder.add( "password", password );_ _ builder.add( "host", host );_ _ getConfig().setConfig( builder.finish() );_ _ }_