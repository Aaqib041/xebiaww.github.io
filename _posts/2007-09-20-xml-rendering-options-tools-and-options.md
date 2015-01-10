---
layout: post
header-img: img/default-blog-pic.jpg
author: balajidl
description: 
post_id: 204
created: 2007/09/20 14:05:24
created_gmt: 2007/09/20 12:05:24
comment_status: open
---

# XML Rendering - Tools and Options

A short overview on some of the XML rendering tools that i have experimented with. While the applications of XML is a very broad topic, this blog will focus only on some of the tools available to use XML electronic forms creation, submission and exchange. Example : An electronic purchase order or Invoice exchanged between two partners. 

**MS Infopath**

A product from Microsoft Office team for creating and exchanging electronic forms.

  * Creating a user interface that looks like a paper electronic form can be done by drag-n-drop of the controls to the workspace.
  * Data input fields can be binded to XML Schema elements and the input XML is always validated against it.
  * One of the unique power it has is the freedom to write your own XSL/XPATH/XQUERY and render the presentation logic in the way you want.
  * Business logic can be written using inbuilt script or it can be integrated with Microsoft Visual Studio.NET. People who have knowledge on VB.net or C# will find it more easy to play around. 
  * Sender and receiver should have MS Infopath to view the content in readable form. (say paper application form). 
  * It also allows to add custom toolbars which can call some remote methods on the server. 
  * Empowers all the backend power of .NET if the code base is integrated to .NET. This enables calling webserivce or email or sharepoint or biztalk very easy. 
  * Version maintenance is very easy as the form will always ave update link to your original form. 
  * Has very clear distinction between content and logic. 
  * Not very expensive, ships along with Microsoft Office. 

More info about this product can be found [here][1]. 

**Adobe Form designer (Livecyle designer)**

A product from Adobe Acrobat team for creating and exchanging paper based electronic forms. 

  * Holds the power of ePaper (PDF). 
  * Has less distinction between content and logic. 
  * Has the drag-n-drop based form layout generation and the fields can be mapped to XML Schema elements as well. 
  * Business Logic can be written using ECMA java script, but its little difficult to learn on the first go. 
  * Key power is the sender and receiver should simply have the free Adobe Reader 7.0+ installed. 
  * Support webserivces integration and form submission to ASP/CGI. 
  * Not very expensive though the catch is the cost of the Reader enabling server. 
  * Work best for the companies which wants to have complete document workflow management (with the help of Adobe Document Server).  More info about this product can be found [here][2]. 

**Custom tools**

_Apache FOP:_

> One of the reliable open source tool built on Java. Its support rendering of XML to PDF and to many other formats. Based on W3C XSL:FO, so the programmer must have good knowledge on coding xslt and xsl:fo. Has a clear separation between the data (XML) and the logic (XSLT) that renders it. This helps to fine tune or modify the final layout without reloading or restarting the web application server. Can be used as a plugin with existing java based web applications. (example: struts, mvc, spring, seam etc.,).   
More info about this can be found [here][3]. 

_iText:_

> Another nice open source tool built on Java. Much faster and portable compared to Apache FOP, but it requires you to learn the way how the layout of the PDF is generated. Found many Java programmers preferred this tool. One of the drawback as far as i know is, it doesn't support xsl:fo like presentation layer, this means you have reload your application to see new changes applied.   
More info about this can be found [here][4]. 

_MSXML:_

> Don’t hate me, but i found the msxml parser much faster in parsing and rendering the xml content to HTML/CSV format. It would have been great if Microsoft can develop a tool like Apache FOP.   
More info about this can be found [here][5]. 

_Xforms:_

> Another nice initiative from W3C and I believe this is going to be a lead in long term perspective. But I am yet to play around with the tools that support xforms.   
More info about this can be found [here][6]. 

**Conclusion:**   
I found MS Infopath and Apache FOP is best breed as of now. Well this is not the only xml related tools that i have play around, If i find time and if someone see this interesting to know further, then i will write more on other tools as well ;)

   [1]: http://office.microsoft.com/infopath/
   [2]: http://www.adobe.com/devnet/livecycle/designing_forms.html
   [3]: http://xml.apache.org/fop
   [4]: http://www.lowagie.com/iText/
   [5]: http://msdn.microsoft.com/xml/
   [6]: http://www.w3.org/MarkUp/Forms/2003/xforms-for-html-authors