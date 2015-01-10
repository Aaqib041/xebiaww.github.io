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

In my current project I have been assigned the task of internationalizing the application. The application is using Tapestry as the MVC framework. In the process of localization I came across a number of technical hurdles for which I had to Google, read some books or consult other developers to find a solution. In this blog I intend to capture all such details which I found, some times easily and sometimes with a bit of effort. I would begin with brief discussion of basics and then deal with specific cases and their solutions.

 

# Localization in Tapestry 5

 

#### Step 1: Enable multi-lingual support :

Tapestry needs to be informed about the languages it needs to support. If the application supports English, French and German then contributeApplicationDefaults method of the AppModule class should have following code

[sourcecode language="java"] public static void contributeApplicationDefaults( MappedConfiguration configuration){ configuration.add("tapestry.supported-locales", "en,fr,de"); } [/sourcecode] 

#### Step 2: Create locale specific property files

Create properties files for each language that application supports. For all languages which application does not support create a default property file which would be used to render the GUI in default language used to create the  default property file.

 

The conventions to be followed are as follows

  1. If page/component is PageName.tml create PageName.properties. This will be treated as the default property file.

  2. Create PageName_fr.properties and PageName_de.properties (to support French and German).

  3. Place the properties files in the same package as the tml's java file (PageName.java in this case).

 

Note: It is a good practice to place the properties files along with resources and not with .java files. This makes it easy to manage the properties files. The resources can be easily given to ( generally ) non technical guys for translation.

#### Step 3: Use the locale keys in tml files

In the .tml files replace the strings that need to be localized with the following construct.

 

[sourcecode language="xml"] ${message:key.in.proprties.file} [/sourcecode] 

Tapestry framework will take care of replacing the key with appropriate localized value.

 

#### Specific Cases : Questions & Answers

The above mentioned steps are the basic building blocks required to equip Tapestry to support i18n. But there are specific cases which require special attention. I will deal them as questions and answers

##### How to define commonly used strings in one properties file?

There will be some strings which get repeated number of times in the application. If you follow the steps detailed above then the common keys will get repeated in more than one properties files. To overcome this problem, in Tapestry there is a concept of application-wide or root message catalog. There are two conventions which needs to be followed for creating root message catalog

  1. Place the file in WEB-INF directory.

  2. The name of the file should be same as the name of the Tapestry filter defined in the web.xml file. If the definition in web.xml is as follows then the name the root message catalog should be app.properties.

 

[sourcecode language="xml"] app com.abc.xyz.lmnFilter [/sourcecode] 

 

##### How to create parameterized message key?

 

While deleting an entity an alert should appear with a message “You are about to delete entity <Entity Name>. Are you sure ?” How can I create a single key for such messages?

 

Instead of breaking this message into two parts and creating two keys for the message a single key can be created, as follows:

Entry in properties file

 

[sourcecode language="xml"] confirm.delete.key=You are about to delete entity '%s'. Are you sure ? [/sourcecode] 

 

Entry in the tml

 

[sourcecode language="xml"] ${format:confirm.delete.key=deleteEntityName} [/sourcecode] 

 

The above command will pull the value of the key from the message bundle and will call getDeleteEntityName() method of the tml's java file and then replace the '%s' in the message string with the value returned by the method.

 

 

##### How to localize the default validation and error messages of the tapestry framework?

 

Tapestry places the default error message catalog in org.apache.tapestry5.corelib.components package. By default Tapestry supports some languages like French, German etc. To add default error message support for a new language add a message catalog in org.apache.tapestry.internal package. The name of the default message catalog file is Errors.properties. You can create the locale specific version of the error messages as described in step 2 above.

 

Similarly to localize the validation messages you most add a message catalog in org.apache.tapestry.internal package. The message catalog name is ValidationMessages.properties.

 

To create localized files one needs to know the key names in the localized files. To do that you can look the content of the file in jar.

To put this message catalog in package other than default package add following code to contributeValidationMessagesSource(Configuration<String> configuration) :

[sourcecode language="java"] public void contributeValidationMessagesSource(Configuration configuration) { configuration.add("path/to/your/package/ValidationMessages.properties"); } [/sourcecode]

## Comments

**[Nuno Monteiro](#8404 "2012-04-09 20:51:32"):** Hello, I'm trying to internationalize my webapp that was built using Tapestry 5. I have lots of parameterized messages. So I'm trying to use what you describe on step "How to create parameterized message key?". However, when I use the format: binding expression I receive this error: Could not convert 'format:welcome-message=myName' into a component parameter binding: Error parsing property expression 'format:welcome-message=myName': line 1:6 missing EOF at ':'. I have myName as a page @Property and I have the welcome-message key in the page catalog. Tapestry do not recognize the format: as it recognize the message: parameter binding. Can you help me, please? Thank you, Nuno Can you help me?

