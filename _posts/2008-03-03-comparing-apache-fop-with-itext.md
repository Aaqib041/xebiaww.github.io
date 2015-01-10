---
layout: post
header-img: img/default-blog-pic.jpg
---

# Comparing Apache FOP with iText

A brief comparision between Apache FOP and iText, especially when to use it for a Java based project. I have also included a small helloworld code snippet of each.  _What is Apache Fop_? **[Apache FOP](http://xmlgraphics.apache.org/fop)** (Formatting Objects Processor) is a print formatter driven by XSL formatting objects (XSL-FO). Fully written in Java. Reads xsl:fo and renders to formats like PDF, PS, PCL, AFP, XML (area tree representation), Print, AWT and PNG, and to a lesser extent, RTF and TXT. _What is iText_? **[iText](http://www.lowagie.com/iText/)** is a library that allows you to generate PDF files on the fly. iText enhance web- and other applications with dynamic PDF document generation and/or manipulation. While the primary target for both of these applications is to generate PDF documents, these two were entirely different Java libraries. Below is my short comparison on when to use which, **Apache FOP**

  * If you want to have a fine grained control over the presentation and the layout of the PDF.   
_Why ? FOP is based on MVC pattern and its uses XSL:FO specification_
  * If your input data is heavily based on XML.   
_Why? Fop main objective is to convert XML to PDF using XSL:FO. While iText have XML2PDF functionality, FOP together with Xalan has much higher control over parsing the XML and rendering the PDF_
  * Apart from PDF, RTF & HTML, If you want to support more output formats like PNG, SVG, TXT and so on.
  * If you DONT want to generate PDF that contains 1000+ pages on every instance.   
_Why? Apache FOP is known for slow processing power. It wont really fit for the cases where you might need to process and create 1000+ pages in a very short time_.
  * If you want to have a externally configurable control over the output format of the PDF.   
_Why? You can keep the style sheet (xsl:fo or xslt) out from your classes or package and tell FOP to use this xsl:fo or xslt while rendering PDF_
  * If you want to have a PDF view accessibility for all your web pages in a web application.   
_ Why? Check out the http://xmlgraphics.apache.org/ site and see how they have integrated Apache Forrest with Apache FOP_ ** iText**

  * If you want to generate PDF on the fly and if you want to add support for annotations, AcroForms, digital signature 
  * If you want to generate 100+ PDF which also contains 1000+ pages.   
_Why? iText is very reliable from processing perspective and its pretty fast in generating the PDF's_
  * If you want to post-process, manipulate existing PDF documents.   
_Why? Because even Apache FOP reckons using iText for this purpose_
  * If you want to encrypt your PDF file   
_Why? iText libraries were pretty well designed for this_
  * While fetching & displaying the database tables on screen (say web or client application), if you want to have instance support for XML, PDF, Excel, CSV outputs.  
_Why ? Checkout [DisplayTag](http://displaytag.sourceforge.net/) features and see how it is using iText for PDF_ **_How is it like writing a simple HelloWorld text printed PDF document generation using iText_ ?**
    
    
    Document document = new Document();
    
    PdfWriter.getInstance(document,
    			new FileOutputStream("HelloWorld.pdf"));
    
    document.open();
    document.add(new Paragraph("Hello World"));
    document.close();
    

To see complete code, click <http://itext.ugent.be/library/com/lowagie/examples/general/HelloWorld.java> **_How is it like writing a simple HelloWorld text printed PDF document generation using Apache FOP_ ?**
    
    
    FopFactory fopFactory = FopFactory.newInstance();
    
    FOUserAgent foUserAgent = fopFactory.newFOUserAgent();
    
    OutputStream out = new java.io.FileOutputStream(pdffile);
    out = new java.io.BufferedOutputStream(out);
    
      Fop fop = fopFactory.newFop(MimeConstants.MIME_PDF, foUserAgent, out);
    
      TransformerFactory factory = TransformerFactory.newInstance();
      Transformer transformer = factory.newTransformer(new StreamSource(new File(xsltfileloc)));
    
      Source src = new StreamSource(new File(xmlfileloc));
      Result res = new SAXResult(fop.getDefaultHandler());
    
      transformer.transform(src, res);
    

For brevity, i have not included the input xml and xslt (xsl:fo) code snippets. To see the complete code, click [FopSource/examples/.../.../ExampleXM2PDF.java](http://svn.apache.org/viewvc/xmlgraphics/fop/tags/fop-0_94/examples/embedding/java/embedding/ExampleXML2PDF.java?revision=567305&view=markup) **Note :** Well, the above points were based on my experience and exposure, if you have something new or see something misinterpreted, then please posts it as your comment.