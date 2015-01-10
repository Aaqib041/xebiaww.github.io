---
layout: post
header-img: img/default-blog-pic.jpg
author: gkohli
description: 
post_id: 3926
created: 2010/07/01 19:44:46
created_gmt: 2010/07/01 14:44:46
comment_status: open
---

# Localization of JavaScript messages in Tapestry

I am sure if you are reading this blog, you are aware of Tapestry framework and at least know the basics of it. The purpose of this blog is not to show you how to do simple localization in Tapestry. For that you can read from links at the bottom. In this blog we will directly go to the problem of localizing messages/strings/error codes used in JavaScript code.  These days most of the web application wants to have a rich UI for which they either use JavaScript frameworks like Ext JS (now [Sencha][1]), Jquery UI, GWT or try to build their interface using Flex. Now flex, GWT all these framework are mature enough and already have inbuilt support for localization, but if we have to use some JavaScript framework, then we have to deal with pain of translating all the messages used in our JavaScript code into multiple languages.

Now, [Tapestry][2] as a framework has exposed a lot of its inbuilt components to the developer, which they can easily exploit to make JavaScript localization a bit simpler. One such interface is [Asset][3] class, an asset is any kind resource which can be downloaded to the client browser and they are localized; Tapestry will search for a variation of the file appropriate to the effective locale for the request.

So if we have a JavaScript file upload.js which handles file upload functionality and you have code snippet like this,

**upload.js**

[java]

if(confirm("A file with this name already exists. Do you want to overwrite?")) { document.uploadForm.submit(); }

[/java]

And we want our confirmation message to be localized. Since JavaScript are static resources, we cannot use anything dynamic which can populate the localized message into the JS. So a normal tendency is to include the JavaScript code into JSP or any template page wherein we can localize the message.

In tapestry we can easily put all the messages into a separate JavaScript file upload-messages.js and include that in our main component page.

**UploadFile.java**

[java] //this would locate the localized version of this static resource @Inject @Path("context:/script/upload-messages.js") private Asset uploadmessages;

@Inject @Path("context:/script/upload.js") private Asset upload;

@Environmental private RenderSupport renderSupport;

void afterRender() { //this would include that JS into the main tapestry tml file after rendering the template renderSupport.addScriptLink(upload); renderSupport.addScriptLink(uploadmessages); }

[/java]

**upload-messages.js**

[java]

Upload.Messages = { confirmationMessage : A file with this name already exists. Do you want to overwrite? };

[/java]

**upload-messages_nl.js**

[java]

Upload.Messages = { confirmationMessage : Een bestand met deze naam bestaat reeds. Wilt u overschrijven? };

[/java]

**upload.js [ localized ] **

[java]

if(confirm(Upload.Messages.confirmationMessage)) { document.uploadForm.submit(); }

// Declare a emply object for holding the localized messages. On runtime this would be populated with //correct localized messages. Var  Upload = {}

[/java]

So we store all the messages which needs to be localized in upload object inside upload-messages.js and use that from upload.js.This way we don't have to use any server side code to handle localization.

With the next release of [tapestry 5.2-dev][4], there would be slight change in package structure.RenderSupport would be replaced by more specialized class JavaScriptSupport which would only handle Javascript files. So from that release we would be using 

void
**[importJavascriptLibrary**][5] ([Asset][6] asset) Imports a JavaScript library as part of the rendered page
Links for Localization in Tapestry 

  1. [http://code.google.com/p/shams/wiki/Localization][7]
  2. <http://tapestry.apache.org/tapestry5.1/guide/localization.html>
Page Stats : [pageviews]

   [1]: http://www.sencha.com/
   [2]: http://tapestry.apache.org/tapestry5.1/
   [3]: http://tapestry.apache.org/tapestry5.1/guide/assets.html
   [4]: http://tapestry.apache.org/tapestry5.2-dev/
   [5]: http://tapestry.apache.org/tapestry5.2-dev/apidocs/org/apache/tapestry5/services/javascript/JavascriptSupport.html#importJavascriptLibrary%28org.apache.tapestry5.Asset%29
   [6]: http://tapestry.apache.org/tapestry5.2-dev/apidocs/org/apache/tapestry5/Asset.html (interface in org.apache.tapestry5)
   [7]: http://tapestry.apache.org/tapestry5.1/guide/localization.html