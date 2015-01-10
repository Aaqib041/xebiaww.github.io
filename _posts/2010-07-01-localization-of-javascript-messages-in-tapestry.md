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

<p>I am sure if you are reading this blog, you are aware of Tapestry framework and at least know the basics of it. The purpose of this blog is not to show you how to do simple localization in Tapestry. For that you can read from links at the bottom. In this blog we will directly go to the problem of localizing messages/strings/error codes used in JavaScript code.
<!--more-->
These days most of the web application wants to have a rich UI for which they either use JavaScript frameworks like Ext JS (now <a href="http://www.sencha.com/">Sencha</a>), Jquery UI, GWT or try to build their interface using Flex. Now flex, GWT all these framework are mature enough and already have inbuilt support for localization, but if we have to use some JavaScript framework, then we have to deal with pain of translating all the messages used in our JavaScript code into multiple languages.</p>
<p>Now, <a href="http://tapestry.apache.org/tapestry5.1/">Tapestry</a> as a framework has exposed a lot of its inbuilt components to the developer, which they can easily exploit to make JavaScript localization a bit simpler. One such interface is <a href="http://tapestry.apache.org/tapestry5.1/guide/assets.html">Asset</a> class, an asset is any kind resource which can be downloaded to the client browser and they are localized; Tapestry will search for a variation of the file appropriate to the effective locale for the request.</p>
<p>So if we have a JavaScript file upload.js which handles file upload functionality and you have code snippet like this,</p>
<p><strong>upload.js</strong></p>
<p>[java]</p>
<p>if(confirm(&quot;A file with this name already exists. Do you want to overwrite?&quot;))
{
document.uploadForm.submit();
}</p>
<p>[/java]</p>
<p>And we want our confirmation message to be localized. Since JavaScript are static resources, we cannot use anything dynamic which can populate the localized message into the JS. So a normal tendency is to include the JavaScript code into JSP or any template page wherein we can localize the message.</p>
<p>In tapestry we can easily put all the messages into a separate JavaScript file upload-messages.js and include that in our main component page.</p>
<p><strong>UploadFile.java</strong></p>
<p>[java]
//this would locate the localized version of this static resource
@Inject
@Path(&quot;context:/script/upload-messages.js&quot;)
private Asset uploadmessages;</p>
<p>@Inject
@Path(&quot;context:/script/upload.js&quot;)
private Asset upload;</p>
<p>@Environmental
private RenderSupport renderSupport;</p>
<p>void afterRender() {
//this would include that JS into the main tapestry tml file after rendering the template
renderSupport.addScriptLink(upload);
renderSupport.addScriptLink(uploadmessages);
}</p>
<p>[/java]</p>
<p><strong>upload-messages.js</strong></p>
<p>[java]</p>
<p>Upload.Messages = {
confirmationMessage : A file with this name already exists. Do you want to overwrite?
};</p>
<p>[/java]</p>
<p><strong>upload-messages_nl.js</strong></p>
<p>[java]</p>
<p>Upload.Messages = {
confirmationMessage : Een bestand met deze naam bestaat reeds. Wilt u overschrijven?
};</p>
<p>[/java]</p>
<p><strong>upload.js [ localized ]
</strong></p>
<p>[java]</p>
<p>if(confirm(Upload.Messages.confirmationMessage))
{
document.uploadForm.submit();
}</p>
<p>// Declare a emply object for holding the localized messages. On runtime this would be populated with //correct localized messages.
Var  Upload = {}</p>
<p>[/java]</p>
<p>So we store all the messages which needs to be localized in upload object inside upload-messages.js and use that from upload.js.This way we don't have to use any server side code to handle localization.</p>
<p>With the next release of <a href="http://tapestry.apache.org/tapestry5.2-dev/">tapestry 5.2-dev</a>, there would be slight change in package structure.RenderSupport would be replaced by more specialized class JavaScriptSupport which would only handle Javascript files. So from that release we would be using
<table border="1" cellspacing="0" cellpadding="10" width="100%">
<tbody>
<tr>
<td width="10%" valign="top">void</td>
<td><a href="http://tapestry.apache.org/tapestry5.2-dev/apidocs/org/apache/tapestry5/services/javascript/JavascriptSupport.html#importJavascriptLibrary%28org.apache.tapestry5.Asset%29"><strong>importJavascriptLibrary</strong></a> (<a title="interface in org.apache.tapestry5" href="http://tapestry.apache.org/tapestry5.2-dev/apidocs/org/apache/tapestry5/Asset.html">Asset</a> asset)
Imports a   JavaScript library as part of the rendered page</td>
</tr>
</tbody>
</table>
Links for Localization in Tapestry
<ol>
    <li><a href="http://tapestry.apache.org/tapestry5.1/guide/localization.html">http://code.google.com/p/shams/wiki/Localization</a></li>
    <li><a href="http://tapestry.apache.org/tapestry5.1/guide/localization.html">http://tapestry.apache.org/tapestry5.1/guide/localization.html</a></li>
</ol>
Page Stats : [pageviews]</p>