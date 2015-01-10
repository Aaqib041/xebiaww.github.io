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

<p><span style="line-height: 1.5em;">Before starting with alfresco custom field search in CMIS I like to tell you about few thing.</span></p>
<p>What is cmis?
How cmis works?
How to create custom model in alfresco?
How to make custom field searchable?
How to search document in cmis using lucene query?</p>
<p><strong>1. What is cmis?</strong></p>
<p>Cmis stands for content management interoperatibility service. CMIS is an open stands for accessing content repository given by OASIS.</p>
<p>There are set of rules or restriction enforced by CMIS for accessing CMIS compitable repository.</p>
<p>Alfresco alfresco support the CMIS standard for accessing the content repository.</p>
<p><strong>2. How cmis works?</strong></p>
<p>There are set of binding that CMIS provide to bind session with CMIS compitable repository.
1. Local session binding.
2. Atom pub binding.
3. Web service binding.</p>
<p>After getting session using user name and password or user token user can get, upload and manupilate content to which he/she is authorized.</p>
<p>User can create new content, upload new content, edit content, delete content, accociate or update metadata with perticular content within the CMIS compitable repository.</p>
<p>There are some limitation when you are working with custom fields and aspects as these are not accessible threw apache cmis librey we have to use alresco open cmis libreary to get additional functionality.</p>
<p><strong>3. How to create custom model in alfresco?</strong></p>
<p>Now this is an practicle example to create an custom model within alfresco.
For creating custom model in alfresco in community version we need to create two file one is cutom model file and other is context file which contain registory for custom model these two files are mandatory for creating new model there are other files too which are created based on needs.</p>
<p>There are two ways of creating new model in alfresco one is by using aspects and other is by using types. We are using the aspects for each the new model in alfresco as aspects are dynamic and reusable while types are not resuable component.</p>
<p>First we will start with creating new model for which we need some files create these files with names:</p>
<ol>
<li>myCustomFields-model.xml</li>
<li>myCustomFields-model-context.xml</li>
<li>
<p>web-client-config-custom.xml
paste them under alfresco/tomcat/shared/classes/alfresco/extension</p>
</li>
<li>
<p>myCustomFields.properties
paste it under alfresco/tomcat/shared/classes/alfresco/messages</p>
</li>
<li>
<p>custom-slingshot-application-context.xml</p>
</li>
<li>share-config-custom.xml
paste them under alfresco/tomcat/shared/classes/alfresco/web-extension</li>
</ol>
<p>myCustomFields-model.xml should contain following content:</p>
<p><code>&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;!-- Definition of Knowledge Base Model --&gt;
&lt;model name="my:model" xmlns="http://www.alfresco.org/model/dictionary/1.0"&gt;
&lt;!-- Optional meta-data about the model --&gt;
&lt;description&gt;Knowledge Base Model&lt;/description&gt;
&lt;author&gt;Will Abson&lt;/author&gt;
&lt;version&gt;1.0&lt;/version&gt;
&lt;!-- Imports are required to allow references to definitions in other models --&gt;
&lt;imports&gt;
&lt;!-- Import Alfresco Dictionary Definitions --&gt;
&lt;import uri="http://www.alfresco.org/model/dictionary/1.0" prefix="d"/&gt;
&lt;!-- Import Alfresco Content Domain Model Definitions --&gt;
&lt;import uri="http://www.alfresco.org/model/content/1.0" prefix="cm"/&gt;
&lt;/imports&gt;
&lt;!-- Introduction of new namespaces defined by this model --&gt;
&lt;namespaces&gt;
&lt;namespace uri="my.model" prefix="my"/&gt;
&lt;/namespaces&gt;
&lt;aspects&gt;
&lt;!-- Definition of new Content Aspect: Knowledge Base Document --&gt;
&lt;aspect name="my:cutomfieldsdata"&gt;
&lt;title&gt;my custom model&lt;/title&gt;
&lt;properties&gt;
&lt;property name="my:metadata_custom_serial_number"&gt;
&lt;type&gt;d:text&lt;/type&gt;
&lt;/property&gt;
&lt;property name="my:metadata_custom_status"&gt;
&lt;type&gt;d:text&lt;/type&gt;
&lt;/property&gt;
&lt;property name="my:metadata_custom_id"&gt;
&lt;type&gt;d:text&lt;/type&gt;
&lt;multiple&gt;true&lt;/multiple&gt;
&lt;index enabled="true"&gt;
&lt;tokenised&gt;true&lt;/tokenised&gt;
&lt;/index&gt;
&lt;/property&gt;
&lt;property name="my:metadata_custom_name"&gt;
&lt;type&gt;d:text&lt;/type&gt;
&lt;multiple&gt;true&lt;/multiple&gt;
&lt;index enabled="true"&gt;
&lt;tokenised&gt;true&lt;/tokenised&gt;
&lt;/index&gt;
&lt;/property&gt;
&lt;property name="my:metadata_custom_code"&gt;
&lt;type&gt;d:text&lt;/type&gt;
&lt;multiple&gt;true&lt;/multiple&gt;
&lt;index enabled="true"&gt;
&lt;tokenised&gt;true&lt;/tokenised&gt;
&lt;/index&gt;
&lt;/property&gt;
&lt;/properties&gt;
&lt;/aspect&gt;
&lt;/aspects&gt;
&lt;/model&gt;
</code>
We used property &lt;index enabled="true"&gt; which make field searchable in full index search. Property &lt;multiple&gt;true&lt;/multiple&gt; is used to contain multiple field within one field in comma separated form for now you can remove this.</p>
<p>myCustomFields-model-context.xml should contain following content:</p>
<p><code>&lt;?xml version='1.0' encoding='UTF-8'?&gt;
&lt;!DOCTYPE beans PUBLIC '-//SPRING//DTD BEAN//EN' 'http://www.springframework.org/dtd/spring-beans.dtd'&gt;
&lt;beans&gt;
&lt;!-- Registration of new models --&gt;
&lt;bean id="extension.kb.dictionaryBootstrap" parent="dictionaryModelBootstrap" depends-on="dictionaryBootstrap"&gt;
&lt;property name="models"&gt;
&lt;list&gt;
&lt;value&gt;alfresco/extension/myCustomFields-model.xml&lt;/value&gt;
&lt;/list&gt;
&lt;/property&gt;
&lt;/bean&gt;
&lt;bean id="extension.kb.resourceBundle" class="org.alfresco.i18n.ResourceBundleBootstrapComponent"&gt;
&lt;property name="resourceBundles"&gt;
&lt;list&gt;
&lt;value&gt;alfresco.messages.myCustomFields&lt;/value&gt;
&lt;/list&gt;
&lt;/property&gt;
&lt;/bean&gt;
&lt;/beans&gt;
</code></p>
<p>web-client-config-custom.xml should contain following content:</p>
<p><code>&lt;alfresco-config&gt;
&lt;config evaluator="string-compare" condition="Advanced Search"&gt;
&lt;advanced-search&gt;
&lt;content-types&gt;
&lt;/content-types&gt;
&lt;folder-types&gt;
&lt;/folder-types&gt;
&lt;custom-properties&gt;
&lt;meta-data aspect="my:cutomfieldsdata" property="my:metadata_custom_serial_number" /&gt;
&lt;meta-data aspect="my:cutomfieldsdata" property="my:metadata_custom_status" /&gt;
&lt;meta-data aspect="my:cutomfieldsdata" property="my:metadata_custom_id" /&gt;
&lt;meta-data aspect="my:cutomfieldsdata" property="my:metadata_custom_name" /&gt;
&lt;meta-data aspect="my:cutomfieldsdata" property="my:metadata_custom_code" /&gt;
&lt;/custom-properties&gt;
&lt;/advanced-search&gt;
&lt;/config&gt;
&lt;/alfresco-config&gt;
</code></p>
<p>this file is use to make each field in model searchable can ne used in alfresco advanced search and cmis field search.</p>
<p>myCustomFields.properties should contain following content:</p>
<p><code>my_model.property.my_metadata_custom_serial_number.title=Serial Number
my_model.property.my_metadata_custom_status.title=Status
my_model.property.my_metadata_custom_id.title=Document ID
my_model.property.my_metadata_custom_name.title=Name
my_model.property.my_metadata_custom_code.title=Code
</code></p>
<p>custom-slingshot-application-context.xml should contain following content:</p>
<p><code>&lt;?xml version='1.0' encoding='UTF-8'?&gt;
&lt;beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:hz="http://www.hazelcast.com/schema/config"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
http://www.hazelcast.com/schema/config
http://www.hazelcast.com/schema/config/hazelcast-spring.xsd"&gt;
&lt;!-- Add Knowledge Base messages --&gt;
&lt;bean id="webscripts.kb.resources" class="org.springframework.extensions.surf.util.ResourceBundleBootstrapComponent"&gt;
&lt;property name="resourceBundles"&gt;
&lt;list&gt;
&lt;value&gt;alfresco.messages.myCustomFields&lt;/value&gt;
&lt;/list&gt;
&lt;/property&gt;
&lt;/bean&gt;
&lt;/beans&gt;
</code></p>
<p>This property file is to access field property title on alfresco share.</p>
<p>share-config-custom.xml should contain following content:
<code>&lt;alfresco-config&gt;
&lt;!-- Repository Library config section --&gt;</p>