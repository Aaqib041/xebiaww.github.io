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

<p>Hi all, we are going try something interesting in this discussion!</p>
<p>As we all are aware of the fact that '<strong>Test Automation</strong>' is always the hot topic in Quality and Assurance field.This time lets try something which is more valuable to automation.In this blog, I'll be discussing how we can easily customize and extent SOAP UI, and achieve some valuable output by utilizing its core functionality.</p>
<p><span style="text-decoration: underline"><strong>Concept :</strong></span>
Its important to understand the concept before going ahead.Soap UI introduces an extensive factory based mechanism, that allows user to add custom objects to SOAP UI like TestStep, Assertions, Editor View etc.See <a title="Custom Factories" href="http://www.soapui.org/Developers-Corner/custom-factories.html" target="_blank">http://www.soapui.org/Developers-Corner/custom-factories.html</a> .
In this discussion we will add '<strong><span style="color: #000000">custom test step' </span></strong> ie. 'ConnectToLinuxBox' in SOAP UI that will simply connect to any linux box, using these factory extension. SOAP UI uses Apache XMLBean framework for managing the configuration, we will see further how configuration are read by SOAP UI.</p>
<p>Lets get started!</p>
<p><strong>Step 1 :  'Create a Test Step Factory class'</strong></p>
<p>As SOAP UI uses factory based mechanism, we need to create a factory class, say 'ConnectToLinuxBoxFactory', and this class should extend 'WsdlTestStepFactory' SOAP UI API class. Lets put it together.</p>
<p><span style="line-height: 1.5em"><em><span style="font-size: 28px">"</span><span style="color: #003366">public class ConnectToLinuxBoxFactory extends WsdlTestStepFactory</span></em></span></p>
<p><span style="color: #003366"><em>{</em></span>
<span style="color: #003366"><em> private static final String STEP_ID = "ConnectToLinuxBox";  <span style="color: #008000">//Step id used as a string</span>.</em></span></p>
<p><span style="color: #003366"><em><span style="color: #008000">//Constructor method.</span> </em></span>
<span style="color: #003366"><em> public ConnectToLinuxBoxFactory()</em></span>
<span style="color: #003366"><em> {</em></span>
<span style="color: #008000"><em> // Calls 'WsdlTestStepFactory' constructor with the step id, description etc of custom step.</em></span></p>
<p><span style="color: #003366"><em> super( STEP_ID, "ConnectToLinuxBox TestStep", "Connect to any linux box", "ConnectToLinuxBox.ico" );</em></span>
<span style="color: #003366"><em> }</em></span></p>
<p><span style="color: #008000"><em>//@Overridden method.</em></span>
<span style="color: #003366"><em><span style="color: #008000"> // This is called by SOAP UI when we actually create a test step under test case.</span> </em></span>
<span style="color: #003366"><em> public WsdlTestStep buildTestStep( WsdlTestCase testCase, TestStepConfig config, boolean forLoadTest )</em></span>
<span style="color: #003366"><em> {</em></span>
<span style="color: #003366"><em> return new ConnectToLinuxBoxTestStep( testCase, config, forLoadTest );</em></span>
<span style="color: #003366"><em> }</em></span></p>
<p><span style="color: #008000"><em>//@Overridden method.</em></span>
<span style="color: #003366"><em> public TestStepConfig createNewTestStep( WsdlTestCase testCase, String name )</em></span>
<span style="color: #003366"><em> {</em></span>
<span style="color: #008000"><em> //Creating a factory instance and returning the test step config object.</em></span></p>
<p><span style="color: #003366"><em> TestStepConfig testStepConfig = TestStepConfig.Factory.newInstance();</em></span>
<span style="color: #003366"><em> testStepConfig.setType( STEP_ID );</em></span>
<span style="color: #003366"><em> testStepConfig.setName( name );</em></span>
<span style="color: #003366"><em> return testStepConfig;</em></span>
<span style="color: #003366"><em> }</em></span></p>
<p><span style="color: #003366"><em><span style="color: #008000">//@Overridden method. Setting the create flag as true.</span> </em></span>
<span style="color: #003366"><em> public boolean canCreate()</em></span>
<span style="color: #003366"><em> {</em></span>
<span style="color: #003366"><em> return true;</em></span>
<span style="color: #003366"><em> }</em></span>
<em><span style="color: #003366">}</span><span style="font-size: 28px">"</span></em></p>
<p><strong>Step 2:  'Implement actual Test Step'.</strong></p>
<p>Its time to actually implement the Test Step class, try to be with the flow, as this is the actual class that contains logic implementation. Create 'ConnectToLinuxBoxTestStep' class that extends 'WsdlTestStepWithProperties' and also implements 'PropertyExpansionContainer' interface.</p>
<p><em>Note -- "Implementing 'PropertyExpansionContainer' is needed to effectively use property expansion in SOAP UI test step."</em></p>
<p>Again, lets put it together.</p>
<p><em><span style="font-size: 28px">"</span><span style="color: #003366">public class ConnectToLinuxBoxTestStep extends WsdlTestStepWithProperties implements PropertyExpansionContainer</span></em>
<span style="color: #003366"><em>{</em></span>
<span style="color: #008000"><em> // Private members.</em></span>
<span style="color: #003366"><em> private String username;</em></span>
<span style="color: #003366"><em> private String password;</em></span>
<span style="color: #003366"><em> private String host;</em></span></p>
<p><span style="color: #003366"><em> private Session session= null;</em></span></p>
<p><span style="color: #008000"><em> //Constructor method.</em></span>
<span style="color: #003366"><em> protected ConnectToLinuxBoxTestStep( WsdlTestCase testCase, TestStepConfig config, boolean forLoadTest )</em></span>
<span style="color: #003366"><em> {</em></span>
<span style="color: #008000"><em> // Calling a super constructor, and passing current test case, and its config object.</em></span>
<span style="color: #003366"><em> super( testCase, config, true, forLoadTest );</em></span></p>
<p><span style="color: #003366"><em>if( !forLoadTest )</em></span>
<span style="color: #003366"><em> {</em></span>
<span style="color: #008000"><em> //If its not used as a load test then set its icon, 'ConnectToLinuxBox.ico', this is only the image that will be in the root of SOAP UI home.</em></span>
<span style="color: #003366"><em> setIcon( UISupport.createImageIcon( "ConnectToLinuxBox.ico" ) );</em></span>
<span style="color: #003366"><em> }</em></span>
<span style="color: #008000"><em> //if configuration has not been read, then read config.</em></span>
<span style="color: #003366"><em> if( config.getConfig() != null )</em></span>
<span style="color: #003366"><em> {</em></span>
<span style="color: #003366"><em> readConfig( config );</em></span>
<span style="color: #003366"><em> }</em></span>
<span style="color: #003366"><em> }</em></span></p>
<p><span style="color: #008000"><em>//Implementation of readConfig(), and initializing the properties of test step. </em></span>
<span style="color: #003366"><em> private void readConfig( TestStepConfig config )</em></span>
<span style="color: #003366"><em> {</em></span>
<span style="color: #003366"><em> XmlObjectConfigurationReader reader = new XmlObjectConfigurationReader( config.getConfig() );</em></span>
<span style="color: #003366"><em> username = reader.readString( "username", "" );</em></span>
<span style="color: #003366"><em> password = reader.readString( "password", "" );</em></span>
<span style="color: #003366"><em> host = reader.readString( "host", "" );</em></span></p>
<p><span style="color: #003366"><em> }</em></span></p>
<p><span style="color: #008000"><em>//Implementation of updateConfig().</em></span>
<span style="color: #003366"><em> private void updateConfig()</em></span>
<span style="color: #003366"><em> {</em></span>
<span style="color: #003366"><em> XmlObjectConfigurationBuilder builder = new XmlObjectConfigurationBuilder();</em></span>
<span style="color: #003366"><em> builder.add( "username", username );</em></span>
<span style="color: #003366"><em> builder.add( "password", password );</em></span>
<span style="color: #003366"><em> builder.add( "host", host );</em></span>
<span style="color: #003366"><em> getConfig().setConfig( builder.finish() );</em></span>
<span style="color: #003366"><em> }</em></span></p>