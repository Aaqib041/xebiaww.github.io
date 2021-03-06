---
layout: post
header-img: img/default-blog-pic.jpg
author: SOURABH AGGARWAL
description: 
post_id: 19239
created: 2014/11/20 16:18:38
created_gmt: 2014/11/20 11:18:38
comment_status: open
---

# Alfresco custom field search in CMIS approach.

Before starting with alfresco custom field search in CMIS I like to tell you about few thing.

What is cmis? How cmis works? How to create custom model in alfresco? How to make custom field searchable? How to search document in cmis using lucene query?

**1\. What is cmis?**

Cmis stands for content management interoperatibility service. CMIS is an open stands for accessing content repository given by OASIS.

There are set of rules or restriction enforced by CMIS for accessing CMIS compitable repository.

Alfresco alfresco support the CMIS standard for accessing the content repository.

**2\. How cmis works?**

There are set of binding that CMIS provide to bind session with CMIS compitable repository. 1\. Local session binding. 2\. Atom pub binding. 3\. Web service binding.

After getting session using user name and password or user token user can get, upload and manupilate content to which he/she is authorized.

User can create new content, upload new content, edit content, delete content, accociate or update metadata with perticular content within the CMIS compitable repository.

There are some limitation when you are working with custom fields and aspects as these are not accessible threw apache cmis librey we have to use alresco open cmis libreary to get additional functionality.

**3\. How to create custom model in alfresco?**

Now this is an practicle example to create an custom model within alfresco. For creating custom model in alfresco in community version we need to create two file one is cutom model file and other is context file which contain registory for custom model these two files are mandatory for creating new model there are other files too which are created based on needs.

There are two ways of creating new model in alfresco one is by using aspects and other is by using types. We are using the aspects for each the new model in alfresco as aspects are dynamic and reusable while types are not resuable component.

First we will start with creating new model for which we need some files create these files with names:

  1. myCustomFields-model.xml
  2. myCustomFields-model-context.xml
  3. web-client-config-custom.xml paste them under alfresco/tomcat/shared/classes/alfresco/extension

  4. myCustomFields.properties paste it under alfresco/tomcat/shared/classes/alfresco/messages

  5. custom-slingshot-application-context.xml

  6. share-config-custom.xml paste them under alfresco/tomcat/shared/classes/alfresco/web-extension

myCustomFields-model.xml should contain following content:

`<?xml version="1.0" encoding="UTF-8"?> <!-- Definition of Knowledge Base Model --> <model name="my:model" xmlns="http://www.alfresco.org/model/dictionary/1.0"> <!-- Optional meta-data about the model --> <description>Knowledge Base Model</description> <author>Will Abson</author> <version>1.0</version> <!-- Imports are required to allow references to definitions in other models --> <imports> <!-- Import Alfresco Dictionary Definitions --> <import uri="http://www.alfresco.org/model/dictionary/1.0" prefix="d"/> <!-- Import Alfresco Content Domain Model Definitions --> <import uri="http://www.alfresco.org/model/content/1.0" prefix="cm"/> </imports> <!-- Introduction of new namespaces defined by this model --> <namespaces> <namespace uri="my.model" prefix="my"/> </namespaces> <aspects> <!-- Definition of new Content Aspect: Knowledge Base Document --> <aspect name="my:cutomfieldsdata"> <title>my custom model</title> <properties> <property name="my:metadata_custom_serial_number"> <type>d:text</type> </property> <property name="my:metadata_custom_status"> <type>d:text</type> </property> <property name="my:metadata_custom_id"> <type>d:text</type> <multiple>true</multiple> <index enabled="true"> <tokenised>true</tokenised> </index> </property> <property name="my:metadata_custom_name"> <type>d:text</type> <multiple>true</multiple> <index enabled="true"> <tokenised>true</tokenised> </index> </property> <property name="my:metadata_custom_code"> <type>d:text</type> <multiple>true</multiple> <index enabled="true"> <tokenised>true</tokenised> </index> </property> </properties> </aspect> </aspects> </model> ` We used property <index enabled="true"> which make field searchable in full index search. Property <multiple>true</multiple> is used to contain multiple field within one field in comma separated form for now you can remove this.

myCustomFields-model-context.xml should contain following content:

`<?xml version='1.0' encoding='UTF-8'?> <!DOCTYPE beans PUBLIC '-//SPRING//DTD BEAN//EN' 'http://www.springframework.org/dtd/spring-beans.dtd'> <beans> <!-- Registration of new models --> <bean id="extension.kb.dictionaryBootstrap" parent="dictionaryModelBootstrap" depends-on="dictionaryBootstrap"> <property name="models"> <list> <value>alfresco/extension/myCustomFields-model.xml</value> </list> </property> </bean> <bean id="extension.kb.resourceBundle" class="org.alfresco.i18n.ResourceBundleBootstrapComponent"> <property name="resourceBundles"> <list> <value>alfresco.messages.myCustomFields</value> </list> </property> </bean> </beans> `

web-client-config-custom.xml should contain following content:

`<alfresco-config> <config evaluator="string-compare" condition="Advanced Search"> <advanced-search> <content-types> </content-types> <folder-types> </folder-types> <custom-properties> <meta-data aspect="my:cutomfieldsdata" property="my:metadata_custom_serial_number" /> <meta-data aspect="my:cutomfieldsdata" property="my:metadata_custom_status" /> <meta-data aspect="my:cutomfieldsdata" property="my:metadata_custom_id" /> <meta-data aspect="my:cutomfieldsdata" property="my:metadata_custom_name" /> <meta-data aspect="my:cutomfieldsdata" property="my:metadata_custom_code" /> </custom-properties> </advanced-search> </config> </alfresco-config> `

this file is use to make each field in model searchable can ne used in alfresco advanced search and cmis field search.

myCustomFields.properties should contain following content:

`my_model.property.my_metadata_custom_serial_number.title=Serial Number my_model.property.my_metadata_custom_status.title=Status my_model.property.my_metadata_custom_id.title=Document ID my_model.property.my_metadata_custom_name.title=Name my_model.property.my_metadata_custom_code.title=Code `

custom-slingshot-application-context.xml should contain following content:

`<?xml version='1.0' encoding='UTF-8'?> <beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:hz="http://www.hazelcast.com/schema/config" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.5.xsd http://www.hazelcast.com/schema/config http://www.hazelcast.com/schema/config/hazelcast-spring.xsd"> <!-- Add Knowledge Base messages --> <bean id="webscripts.kb.resources" class="org.springframework.extensions.surf.util.ResourceBundleBootstrapComponent"> <property name="resourceBundles"> <list> <value>alfresco.messages.myCustomFields</value> </list> </property> </bean> </beans> `

This property file is to access field property title on alfresco share.

share-config-custom.xml should contain following content: `<alfresco-config> <!-- Repository Library config section -->