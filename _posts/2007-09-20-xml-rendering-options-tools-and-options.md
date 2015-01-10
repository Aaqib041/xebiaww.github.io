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

<p>A short overview on some of the XML rendering tools that i have experimented with.
While the applications of XML is a very broad topic, this blog will focus only on some of the tools available to use XML electronic forms creation, submission and exchange.
Example : An electronic purchase order or Invoice exchanged between two partners.
<!--more--></p>
<p><b>MS Infopath</b>
<p>
A product from Microsoft Office team for creating and exchanging electronic forms.</p>
<ul>
 <li>Creating a user interface that looks like a paper electronic form can be done by drag-n-drop of the controls to the workspace.</li>
 <li>Data input fields can be binded to XML Schema elements and the input XML is always validated against it.</li>
 <li>One of the unique power it has is the freedom to write your own XSL/XPATH/XQUERY and render the presentation logic in the way you want.</li>
 <li>Business logic can be written using inbuilt script or it can be integrated with Microsoft Visual Studio.NET. People who have knowledge on VB.net or C# will find it more easy to play around.
 <li>Sender and receiver should have MS Infopath to view the content in readable form. (say paper application form).
 <li>It also allows to add custom toolbars which can call some remote methods on the server.
 <li>Empowers all the backend power of .NET if the code base is integrated to .NET. This enables calling webserivce or email or sharepoint or biztalk very easy.
 <li>Version maintenance is very easy as the form will always ave update link to your original form.
 <li>Has very clear distinction between content and logic.
 <li>Not very expensive, ships along with Microsoft Office.
</ul>

<p>More info about this product can be found <a href="http://office.microsoft.com/infopath/">here</a>.
</p></p>
<p><b>Adobe Form designer (Livecyle designer)</b>
<p>
A product from Adobe Acrobat team for creating and exchanging paper based electronic forms.
<ul>
<li>Holds the power of ePaper (PDF).
<li>Has less distinction between content and logic.
<li>Has the drag-n-drop based form layout generation and the fields can be mapped to XML Schema elements as well.
<li>Business Logic can be written using ECMA java script, but its little difficult to learn on the first go.
<li>Key power is the sender and receiver should simply have the free Adobe Reader 7.0+ installed.
<li>Support webserivces integration and form submission to ASP/CGI.
<li>Not very expensive though the catch is the cost of the Reader enabling server.
<li>Work best for the companies which wants to have complete document workflow management (with the help of Adobe Document Server).
</ul>
More info about this product can be found <a href="http://www.adobe.com/devnet/livecycle/designing_forms.html">here</a>.
</p></p>
<p><b>Custom tools</b>
<p>
<i>Apache FOP:</i>
<blockquote> One of the reliable open source tool built on Java. Its support rendering of XML to PDF and to many other formats.
Based on W3C XSL:FO, so the programmer must have good knowledge on coding xslt and xsl:fo.
Has a clear separation between the data (XML) and the logic (XSLT) that renders it. This helps to fine tune or modify the final layout without reloading or restarting the web application server.
Can be used as a plugin with existing java based web applications. (example: struts, mvc, spring, seam etc.,).
<br />More info about this can be found <a href="http://xml.apache.org/fop">here</a>.
</blockquote>
<i>iText:</i>
<blockquote>Another nice open source tool built on Java. Much faster and portable compared to Apache FOP, but it requires you to learn the way how the layout of the PDF is generated. Found many Java programmers preferred this tool.
One of the drawback as far as i know is, it doesn't support xsl:fo like presentation layer, this means you have reload your application to see new changes applied.
<br />More info about this can be found <a href="http://www.lowagie.com/iText/">here</a>.
</blockquote>
<i>MSXML:</i>
<blockquote>Don’t hate me, but i found the msxml parser much faster in parsing and rendering the xml content to HTML/CSV format.
It would have been great if Microsoft can develop a tool like Apache FOP.
<br />More info about this can be found <a href="http://msdn.microsoft.com/xml/">here</a>.
</blockquote>
<i>Xforms:</i>
<blockquote>Another nice initiative from W3C and I believe this is going to be a lead in long term perspective.
But I am yet to play around with the tools that support xforms.
<br />More info about this can be found <a href="http://www.w3.org/MarkUp/Forms/2003/xforms-for-html-authors">here</a>.
</blockquote>
</p></p>
<p><b>Conclusion:</b>
<br />I found MS Infopath and Apache FOP is best breed as of now. Well this is not the only xml related tools that i have play around, If i find time and if someone see this interesting to know further, then i will write more on other tools as well ;)</p>