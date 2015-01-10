---
layout: post
header-img: img/default-blog-pic.jpg
author: SOURABH AGGARWAL
description: 
post_id: 19253
created: 2014/11/26 11:45:07
created_gmt: 2014/11/26 06:45:07
comment_status: open
---

# Alfresco working with aspects in CMIS

Alfresco is one of the most popular and widely used open source content management system.

Alfresco comes in both enterprise and community version. At the time of writing this blog latest version for **Enterprise** edition is **4.2.3.1**. At the time of writing this blog latest version for **Community** edition is **5.0.b**.

Before starting with this blog all should be familiar with CMIS and how to use CMIS along with alfresco. For getting started with CMIS with alfresco and other content repository you must first read our blog on**[ Alfresco working with CMIS (File, Folder and Search operation)][1]. **

**First of all what is aspects?**

  * Aspects most likely related to content modeling in Alfresco which itself is one of the biggest feature of the alfresco system. Aspects as the like the additional information that we can dynamically attach to any content within the alfresco repository. Aspects scan be linked to the content individually to add aspect properties to the content or can be called within or used within the types in alfresco content model to add the properties that aspect is holding.
  * Alfresco comes with many aspects by default we can create our own aspects as per requirement.
  
**Why aspects are used?**

Aspects basically used to add dynamicity to the content modelling in alfresco means aspects can be dynamically added and removed from the content or folder within the alfresco repository. Aspects are much like the super class in java which are used by its sub classes to add all the functionality that super class holding. 

Before starting how to use aspects in alfresco using CMIS we must be aware about how to create aspects in alfresco this we have already done in our previous blog ****[Alfresco** custom field search in CMIS approach**][2].** **

For using alfresco aspects in CMIS we have to use alfresco-opencmis-extension-0.3.jar along with CMIS library for content repository access.

First we have to create session that can be used to access aspects and apply aspects to the new content if not already having that aspect.

**1\. Creating alfresco session CMIS : **

```
// default factory implementation SessionFactory factory = SessionFactoryImpl.newInstance(); // map to contain connection parameter any cmis repository using atom pub Map<String, String> parameter = new HashMap<String, String>(); parameter.put(SessionParameter.USER, "admin"); parameter.put(SessionParameter.PASSWORD, "admin"); parameter.put(SessionParameter.ATOMPUB_URL, "http://localhost:9090/alfresco/service/cmis"); parameter.put(SessionParameter.BINDING_TYPE, BindingType.ATOMPUB.value()); parameter.put(SessionParameter.REPOSITORY_ID, "6fc907f9-6790-46b5-9c25-d6855ff8ff5c"); parameter.put(SessionParameter.OBJECT_FACTORY_CLASS, "org.alfresco.cmis.client.impl.AlfrescoObjectFactoryImpl"); // getting Instance from User Credential try { // create session Session session = factory.createSession(parameter); } catch (Exception e) { e.printStackTrace(); } } 
```

Here we have passed one more parameter in parameter map during session creation

```
parameter.put(SessionParameter.OBJECT_FACTORY_CLASS, "org.alfresco.cmis.client.impl.AlfrescoObjectFactoryImpl");
```

to deal with aspects, alfrescoDocument or AlfrescoFolder etc.

**2\. Get the existing folder object it can be AlfrescoFolder or cmis Folder object.**

```
try{ OperationContext context = session.createOperationContext(); context.setRenditionFilterString("cmis:thumbnail"); Folder cmisFolder = (Folder) session.getObjectByPath("/", context); }catch (Exception e){ e.printStackTrace(); } 
```

Now you can either create the new folder and create new document within that folder or use existing folder for document creation. But make sure that user creating document must have access to that particular folder.

**3\. Creating document in the existing folder.**

```
// Setting properties for new document to be created within repository Map<String, Object> propertiesDocument = new HashMap<String, Object>(); propertiesDocument.put(PropertyIds.NAME, "test_Aspect.pdf"); propertiesDocument.put(PropertyIds.OBJECT_TYPE_ID, "cmis:document"); FileInputStream oInputStream = new FileInputStream("/home/sourabh/Desktop/test_aspects.pdf"); // Reading the content in Bytes stream and Writing it to new file in ContentStream contentStream = new ContentStreamImpl("test_Aspect.pdf", null, "application/pdf", oInputStream); // creating document with name title and description Document document = folder.createDocument(propertiesDocument, contentStream, VersioningState.MAJOR); 
```

Now new document is uploaded with name test_aspects.pdf in the root directory. You  can get complete detail for the document uploaded from the document object.

**4\. Check for aspect whether applied to new document or not if no apply custom aspect to document created.**

For this we have to first convert CMIS Document to AlfrescoDocument object then we have add aspect for that document.

```
// converting cmis document to alfresco document AlfrescoDocument alfrescoDocument = (AlfrescoDocument) document; if (!alfrescoDocument.hasAspect("P:my:cutomfieldsdata")) { // adding custom meta data type aspect to document alfrescoDocument.addAspect("P:my:cutomfieldsdata"); // Setting properties for new document to be created within repository Map<String, Object> customPropertiesDocument = new HashMap<String, Object>(); // Set agreement serial number customPropertiesDocument.put("my:metadata_custom_serial_number", "serial number"); customPropertiesDocument.put("my:metadata_custom_status", "status"); customPropertiesDocument.put("my:metadata_custom_id", "id"); customPropertiesDocument.put("my:metadata_custom_name", "name"); customPropertiesDocument.put("my:metadata_custom_code", "code");

// updating the document with the new meta data fields CmisObject updatedDoucment = alfrescoDocument.updateProperties(customPropertiesDocument); System.out.println("document" + updatedDoucment.getName() + " successfully updated"); } else{ System.out.println("document already updated"); }
```

Document is updated with our custom aspect we can add already defined aspects also to the document.

In this we have checked whether our custom aspects is already linked with document or not if not then add custom aspect to document and then update custom properties for document.

**[For more hacks about alfresco visit][3]..................**

   [1]: http://xebee.xebia.in/?p=19243&preview=true
   [2]: http://xebee.xebia.in/index.php/2014/11/20/alfresco-custom-field-search-in-cmis-approach/
   [3]: http://xebee.xebia.in/index.php/tag/alfresco/