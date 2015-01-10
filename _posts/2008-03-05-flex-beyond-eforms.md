---
layout: post
header-img: img/default-blog-pic.jpg
author: balajidl
description: 
post_id: 436
created: 2008/03/05 20:45:49
created_gmt: 2008/03/05 18:45:49
comment_status: open
---

# Flex Beyond -- eForms

<p>Around 6 months back me and <a href="http://vikashazrati.wordpress.com/about/">Vikas Hazrati</a> gave a <a href="http://www.xebiaindia.com/in/your-career/our-culture/your-colleagues">XTR</a> on Adobe Flex to my colleagues in Xebia India.</p>
<p>We took some resources from <a href="http://www.adobe.com/devnet/">Adobe Dev Net </a>site and eventually found a cool <a href="http://www.adobe.com/devnet/flex/articles/java_testdrive.html">article</a> explaining how Flex Data Services works with Spring using Spring Remoting features and so on.
We were quite amazed with the UI capabilities of Flex with its server side integration. If you look at that article now, its start with a disclaimer saying "Effective with the release of Adobe LiveCycle ES, the Adobe Flex Data Services 2 server product has been rebranded as a Solution Component of LiveCycle ES."
<!--more--></p>
<p>I did heard about <a href="http://www.adobe.com/products/livecycle/">Adobe Livecyle ES</a> before but at that time it was called as <b>Adode Document server</b>. Adobe keeps changing its product name for reasons only they can understand &amp; agree, but the point is that the architecture and the concept of the
Adobe LiveCycle ES is getting very matured and stunning.</p>
<p><b>What is Adobe LiveCycle ES</b> ?<blockquote>Adobe® LiveCycle® ES (Enterprise Suite) software is an integrated J2EE server solution that blends electronic forms, process management, document security, and document generation to help you create and deliver rich and engaging applications that reduce paperwork, accelerate decision-making, and help ensure regulatory compliance.
Read more from http://www.adobe.com/products/livecycle/</blockquote></p>
<p><strong>Ok, now what Flex has to do with LiveCycle ES ?</strong>
Just have a look at the below diagram showing the technology summary of LiveCycle ES.
<img src="http://www.avoka.com/ad_livecycle_es/images/marketecture_diagram_558x340.jpg" alt=""/>
[Image linked directly from the site Avoka]</p>
<p>If you look at it carefully and if you are a vivid Java developer, you might have noted that the server-side of LiveCycle is based on J2EE.
LiveCycle reckons using Flex or PDF as its UI.
<b>What!...PDF as an UI?, but how can it provide a rich interface that Flex or HTML can give ?</b>
Yes. Have a look at <a href="http://www.adobe.com/products/livecycle/designer/">Adobe LiveCycle Designer</a> .
<b>What is Adobe LiveCycle Designer</b><blockquote> Using Adobe® LiveCycle® Designer ES software, you can create form and document templates that combine high-fidelity presentation with XML data handling. This gives you the ability to create dynamic forms that closely mirror the paper forms they will replace. A unified design environment with an intuitive graphic interface makes it easy to quickly design and maintain templates, define business logic, and preview them in real time before they are deployed.
 Read more at http://www.adobe.com/products/livecycle/designer/</blockquote>
While you can use Flex for 'shopping cart' kind of user interactions, you can use PDF (precisely called Interactive PDF) for form filling and submission interactions.</p>
<p><b>Well!, Where this can be useful and why this sounds like a complex solution ?</b>
Actually no. The Flex and PDF is just a small part of the LiveCycle ES and the other components in LiveCycle ES together provide solution for Enterprise Business Process Automation, RIA, eForms, Financial Services, Governments, Manufacturing and so on.</p>
<p>IMHO, one of the marketing factor for LiveCycle ES product is PDF. Be it manufacturing business process or Financial or eBusiness, you might require to produce a paper during the workflow.
for example Commercial Invoice, receipts, Purchase order and so on. PDF is the best way to reproduce a paper document in electronic format.
Adobe Reader is installed in 80% of PC (as per Adobe). This enables the end users of LiveCycle ES, especially the SME's to participate in workflow as a client using free Adobe Reader.</p>
<p>If possible I will try to cover more details about the LiveCycle ES components on my next blog.;)</p>