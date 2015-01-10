---
layout: post
header-img: img/default-blog-pic.jpg
author: sprasadjha
description: 
post_id: 3929
created: 2010/07/08 11:57:21
created_gmt: 2010/07/08 06:57:21
comment_status: open
---

# Localization in Tapestry 

<!--        @page { margin: 0.79in }        P { margin-bottom: 0.08in }         H1 { margin-bottom: 0.08in }        H1.western { font-family: "Arial", sans-serif; font-size: 16pt }        H1.cjk { font-family: "Arial", sans-serif; font-size: 16pt }        H1.ctl { font-family: "Tahoma"; font-size: 16pt }       H3 { margin-bottom: 0.08in }        H3.western { font-family: "Arial", sans-serif }         H3.cjk { font-family: "Arial", sans-serif }         PRE.cjk { font-family: "Arial", sans-serif, monospace }         H5 { margin-bottom: 0.08in }        H5.western { font-family: "Arial", sans-serif; font-size: 11pt }        H5.cjk {font-family: "Arial", sans-serif; font-size: 11pt }         H5.ctl { font-size: 11pt } -->

<p style="margin-bottom: 0in;">In my current project I have been assigned the task of internationalizing the application. The application is using Tapestry as the MVC framework. In the process of localization I came across a number of technical hurdles for which I had to Google, read some books or consult other developers to find a solution. In this blog I intend to capture all such details which I found, some times easily and sometimes with a bit of effort. I would begin with brief discussion of basics and then deal with specific cases and their solutions.</p>

<p style="margin-bottom: 0in;"><!--more--></p>

<p style="margin-bottom: 0in;">&nbsp;</p>

<h1>Localization in Tapestry 5</h1>

<p style="margin-bottom: 0in;">&nbsp;</p>

<h4>Step 1: Enable multi-lingual support :</h4>

<p style="margin-bottom: 0in;"><span style="color: #000000;"><span><span style="font-size: small;">Tapestry needs to be informed about the languages it needs to support. If the application supports English, French and German then contributeApplicationDefaults method of the AppModule class should have following code</span></span></span></p>

<p>[sourcecode language="java"]
public static void contributeApplicationDefaults( MappedConfiguration configuration){
configuration.add(&quot;tapestry.supported-locales&quot;, &quot;en,fr,de&quot;);
   }
[/sourcecode]
<h4>Step 2: Create locale specific property files</h4>
<p style="margin-bottom: 0in;">Create properties files for each language that application supports. For all languages which application does not support create a default property file which would be used to render the GUI in default language used to create the  default property file.</p>
<p style="margin-bottom: 0in;">&nbsp;</p>
<p style="margin-bottom: 0in;">The conventions to be followed are as follows</p></p>
<ol>
    <li>
<p style="margin-bottom: 0in;">If page/component is     PageName.tml create PageName.properties. This will be treated as the    default property file.</p>
</li>
    <li>
<p style="margin-bottom: 0in;">Create PageName_fr.properties and    PageName_de.properties (to support French and German).</p>
</li>
    <li>
<p style="margin-bottom: 0in;">Place the properties files in the    same package as the tml's java file (PageName.java in this case).</p>
<p style="margin-bottom: 0in;">&nbsp;</p>
</li>
</ol>

<p style="margin-bottom: 0in;">Note: It is a good practice to place the properties files along with resources and not with .java files. This makes it easy to manage the properties files. The resources can be easily given to ( generally ) non technical guys for translation.</p>

<h4>Step 3: Use the locale keys in tml files</h4>

<p style="margin-bottom: 0in;">In the .tml files replace the strings that need to be localized with the following construct.</p>

<p style="margin-bottom: 0in;">&nbsp;</p>

<p>[sourcecode language="xml"]
${message:key.in.proprties.file}
[/sourcecode]
<p style="margin-bottom: 0in;">Tapestry framework will take care of replacing the key with appropriate localized value.</p>
<p style="margin-bottom: 0in;">&nbsp;</p></p>
<h4>Specific Cases : Questions &amp; Answers</h4>

<p style="margin-bottom: 0in;">The above mentioned steps are the basic building blocks required to equip Tapestry to support i18n. But there are specific cases which require special attention. I will deal them as questions and answers</p>

<h5 style="background: #e7d3ec; border: solid 1px #c7beca; padding: 5px; margin: 5px 0px;">How to define commonly used strings in one properties file?</h5>

<p style="margin-bottom: 0in;">There will be some strings which get repeated number of times in the application. If you follow the steps detailed above then the common keys will get repeated in more than one properties files. To overcome this problem, in Tapestry there is a concept of application-wide or root message catalog. There are two conventions which needs to be followed for creating root message catalog</p>

<ol>
    <li>
<p style="margin-bottom: 0in;">Place the file in WEB-INF    directory.</p>
</li>
    <li>
<p style="margin-bottom: 0in;">The name of the file should be   same as the name of the Tapestry filter defined in the web.xml file.    If the definition in web.xml is as follows then the name the root   message catalog should be app.properties.</p>
<p style="margin-bottom: 0in;">&nbsp;</p>

[sourcecode language="xml"]

app
com.abc.xyz.lmnFilter

[/sourcecode]
<p style="margin-bottom: 0in;">&nbsp;</p>
</li>
</ol>

<h5 style="background: #e7d3ec; border: solid 1px #c7beca; padding: 5px; margin: 5px 0px;">How to create parameterized message key?</h5>

<p style="margin-bottom: 0in;">&nbsp;</p>

<p style="margin-bottom: 0in;">While deleting an entity an alert should appear with a message “You are about to delete entity &lt;Entity Name&gt;. Are you sure ?” How can I create a single key for such messages?</p>

<p style="margin-bottom: 0in;">&nbsp;</p>

<p style="margin-bottom: 0in;">Instead of breaking this message into two parts and creating two keys for the message a single key can be created, as follows:</p>

<p style="margin-bottom: 0in;">Entry in properties file</p>

<p style="margin-bottom: 0in;">&nbsp;</p>

<p>[sourcecode language="xml"]
confirm.delete.key=You are about to delete entity '%s'. Are you sure ?
[/sourcecode]
<p style="margin-bottom: 0in;">&nbsp;</p>
<p style="margin-bottom: 0in;">Entry in the tml</p>
<p style="margin-bottom: 0in;">&nbsp;</p></p>
<p>[sourcecode language="xml"]
${format:confirm.delete.key=deleteEntityName}
[/sourcecode]
<p style="margin-bottom: 0in;">&nbsp;</p>
<p style="margin-bottom: 0in;">The above command will pull the value of the key from the message bundle and will call getDeleteEntityName() method of the tml's java file and then replace the '%s' in the message string with the value returned by the method.</p>
<p style="margin-bottom: 0in;">&nbsp;</p>
<p style="margin-bottom: 0in;">&nbsp;</p></p>
<h5 style="background: #e7d3ec; border: solid 1px #c7beca; padding: 5px; margin: 5px 0px;">How to localize the default validation and error messages of the tapestry framework?</h5>

<p style="margin-bottom: 0in;">&nbsp;</p>

<p style="margin-bottom: 0in;"><span style="color: #000000;">Tapestry places the default error message catalog in org.apache.tapestry5.corelib.components package. By default Tapestry supports some languages like French, German etc. To add default error message support for a new language</span> add a message catalog in org.apache.tapestry.internal package. The name of the default  message catalog file is Errors.properties. You can create the locale specific version of the error messages as described in step 2 above.</p>

<p style="margin-bottom: 0in;">&nbsp;</p>

<p style="margin-bottom: 0in;">Similarly to localize the validation messages you most add a message catalog in org.apache.tapestry.internal package. The message catalog name is ValidationMessages.properties.</p>

<p style="margin-bottom: 0in;">&nbsp;</p>

<p style="margin-bottom: 0in;">To create localized files one needs to know the key names in the localized files. To do that you can look the content of the file in jar.</p>

<p style="margin-bottom: 0in;">To put this message catalog in package other than default package add following code to contributeValidationMessagesSource(Configuration&lt;String&gt;  configuration) :</p>

<p>[sourcecode language="java"]
public void contributeValidationMessagesSource(Configuration configuration) {
configuration.add(&quot;path/to/your/package/ValidationMessages.properties&quot;);
}
[/sourcecode]
<p style="margin-bottom: 0in;">&nbsp;</p></p>

## Comments

**[Nuno Monteiro](#8404 "2012-04-09 20:51:32"):** Hello, I'm trying to internationalize my webapp that was built using Tapestry 5. I have lots of parameterized messages. So I'm trying to use what you describe on step "How to create parameterized message key?". However, when I use the format: binding expression I receive this error: Could not convert 'format:welcome-message=myName' into a component parameter binding: Error parsing property expression 'format:welcome-message=myName': line 1:6 missing EOF at ':'. I have myName as a page @Property and I have the welcome-message key in the page catalog. Tapestry do not recognize the format: as it recognize the message: parameter binding. Can you help me, please? Thank you, Nuno Can you help me?

