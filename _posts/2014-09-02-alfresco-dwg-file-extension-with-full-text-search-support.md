---
layout: post
header-img: img/default-blog-pic.jpg
author: SOURABH AGGARWAL
description: 
post_id: 18919
created: 2014/09/02 16:03:23
created_gmt: 2014/09/02 11:03:23
comment_status: open
---

# Alfresco DWG file extension with full text search support

Engineering drawing is one of the most widely used content with Extension .DWG or .DXF .

First thing to know about is that what is DWG file?

DWG file is binary file which is being used to store two and three dimensional design or data. There are some other format that belong to the same format like .BAK, .DWS, .DWT, .SV$ etc. but DWG is the native format for most of the CAD software used like IntelliCAD, AotoCAD etc.

From the past we are employing many of the popular DMS to save engineering drawing and getting it back when ever its required.

The are many blogs or article that define how we can preview DWG file in Alfresco using custom transformers.

Most important thing about my article is that i am not only focusing about how we can preview DWG file in alfresco using custom transformer but also how we can search DWG file using full text search in Alfresco.

As we all know that Alfresco uses Lucene and Solr for searching content within Alfresco directory when content is being uploaded to Alfresco, Alfresco uses custom transformer to transform that content to text and create Lucene or solr indexes for searching that content using Full text search.

But import thing to notice here is that all OCR(Optical character recognition) used to transform image in DWG to TXT are not 100% accurate but they gives best output.

Main purpose of this article is that you must be able to create a complete DWG previewer along with full text search so that we can search content inside DWG file.

There are many plugins available in the market for previewing DWG file but they may or may not give you the best output you required.

First I will explain you how alfresco content transformer and complex transformer works.

**CONTENT TRANSFORMER :-**

Content transformer is used in alfresco to transform one file type to another using some tools already configured with alfresco or some external tools that are integrated with alfresco.

Like :- I take an example of JPG file that is being converted to PDF file. 

![C1][1]

Here JPG to PDF transformer is used to transform JPG image to PDF file but it not Searchable PDF at all because there is no OCR(Optical character recognition) tool integrated to check content within the image file.

By integrating any OCR(Optical character recognition) tool that can convert JPG to TXT we overcome that problem.

**COMPLEX TRANSFORMER :-**

Complex transformer also work in the same manner as content transformer the difference between those two is that there is intermediate file in between transformation process. 

File there are chain of transformer being employed to transform one file to another file.

Like :- I take an example of JPG file that is being converted into SWF(Flash File) file(there may be a case when there is one tool used to convert JPG to SWF I am not taking that in consideration).

![CU][2]

Here the JPG image is first convert into PDF image using JPG to PDF converter configured with Alfresco system and then PDF generated will be again converted into SWF file using PDF to SWF converter configured. Note that if there is director converter for transforming JPG to SWF configured with Alfresco then its not a complex transformation process.

I start with my example:

First of all up to Alfresco 4.0 SWF previewer is used to preview document by converted into SWF flash File from Alfresco 5.0 PDF previewer is used instead of SWF flash previewer used to preview document by converting it into PDF file so to make rendition process fast.

For our DWG previewer with Full text generation process we need two transformation process.

1\. DWG file to PDF file or DWG file to SWF file - Used for previewing DWG file in browser.

2\. DWG file to TXT file – Used for full text search inside DWG file.

**First :-**

   [1]: http://xebee.xebia.in/wp-content/uploads/2014/09/C1-300x269.png
   [2]: http://xebee.xebia.in/wp-content/uploads/2014/09/CU-300x140.png