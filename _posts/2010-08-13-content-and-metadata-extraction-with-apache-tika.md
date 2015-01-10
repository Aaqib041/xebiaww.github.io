---
layout: post
header-img: img/default-blog-pic.jpg
author: rsharma
description: 
post_id: 4324
created: 2010/08/13 15:57:45
created_gmt: 2010/08/13 10:57:45
comment_status: open
---

# Content and Metadata Extraction with Apache Tika

<p style="text-align: justify;">Often we  have the requirement of reading different types of files e.g. CSV, PDF, XLS files and there can be  many more types need to be read in an  application.</p>

<p style="text-align: justify;">One way, we try to implement such a requirement is by  using separate readers for all of them and  may use different frameworks for  analyzing each of them e.g. if our application requires PDF, XML and XLS file parsing, we can create a FileReader interface. Then we can  have different implementations of this interface for each of the types and use PDFBox for reading PDF files, Apache POI for XLS files and SAX for XML  files.<!--more--></p>

<p>[sourcecode language="java"]
// Read PDF using PDFBox
PDDocument pdDoc = new PDDocument(cosDoc);
PDFTextStripper stripper = new PDFTextStripper();
String docText = stripper.getText(pdDoc);
.......
//Read XLS using POI
POIFSFileSystem fileSystem = new POIFSFileSystem(is);
HSSFWorkbook workBook = new HSSFWorkbook(fileSystem);
....`
// Read XML using  SAX
SAXParserFactory spf = SAXParserFactory.newInstance();
SAXParser parser = spf.newSAXParser();
[/sourcecode]</p>
<p>This poses additional set of issues like :
<ul>
    <li>We must configure each and every type separately.</li>
    <li>We may be using different APIs for reading different types, this poses additional challenge of  learning and configuring separate APIs for each of types.</li>
</ul>
As the list of types  that your application needs to handle grows, these two issues become more and more visible.
<h3>Solution : <a href="http://tika.apache.org/">Apache Tika</a></h3>
<p style="text-align: justify;"><a href="http://tika.apache.org/">Apache Tika</a> is one project that can assist in reducing, if not eliminating, these pains. The projects site suggests :</p>
<p style="padding-left: 60px; text-align: justify;">"<em>Apache  Tika as toolkit for detecting and extracting meta-data and  structured  text content from various documents using existing parser  libraries</em>"</p>
<p style="text-align: justify;">Basically  the Tika API is a single shot API to read data from multiple types,  internally it configures and uses the various existing libraries for  reading the data eg. POI for Excel stuff.</p></p>
<h4><strong>The Parser API</strong></h4>

<p style="text-align: justify;">The <a href="http://tika.apache.org/0.7/api/org/apache/tika/parser/Parser.html">Parser</a> interface is the key component of  Tika library, that can be used to parse data . You need to create an instance of  <a href="http://tika.apache.org/0.7/api/org/apache/tika/parser/Parser.html">Parser</a> and call the parse API to get the required data.  The project currently  supports around 14 different formats e.g. text,  pdf,  audio, video,  image, archives, ms-office, xmls, emails etc. For  each of the  supported  format the package has a <a href="http://tika.apache.org/0.7/api/org/apache/tika/parser/Parser.html">Parser </a>which  it employs to read the format. You can either use each of these  different parsers for reading different types of data or the API also  provides a <a href="http://tika.apache.org/0.7/api/org/apache/tika/parser/AutoDetectParser.html">AutoDetectParser</a> that can detect the type of stream and  use the corresponding parser.</p>

<p style="text-align: justify;">The  Parser.parse method takes the stream, related meta-data                      as input and a few more arguments, and outputs the results and extra                      meta-data. The API is not concerned about the specs  of the stream i.e. how it is  opened etc. The code using the Parser  should take care of the stream  maintenance i.e opening and closing.</p>

<p>[sourcecode language="java"]
@Test
public void testPDFParser() throws Exception {
 String resourceLocation = &amp;quot;Tika.pdf&amp;quot;;
 InputStream input = this.getClass().getClassLoader().getResourceAsStream(resourceLocation);
 ContentHandler textHandler = new BodyContentHandler();
 Metadata metadata = new Metadata();
 PDFParser parser = new PDFParser();
 parser.parse(input, textHandler, metadata, new ParseContext());
 input.close();
 System.out.println(&amp;quot;Title: &amp;quot; + metadata.get(&amp;quot;title&amp;quot;));
 System.out.println(&amp;quot;Author: &amp;quot; + metadata.get(&amp;quot;Author&amp;quot;));
 System.out.println(&amp;quot;format: &amp;quot; + metadata.get(&amp;quot;source&amp;quot;));
 System.out.println(&amp;quot;content: &amp;quot; + textHandler.toString());
}
@Test
public void testAutoDetectParser() throws Exception {
 InputStream input = this.getClass().getResourceAsStream(&amp;quot;TestTikaAPI.class&amp;quot;);
 ContentHandler textHandler = new BodyContentHandler();
 Metadata metadata = new Metadata();
 Parser parser = new AutoDetectParser();
 parser.parse(input, textHandler, metadata, new ParseContext());
 input.close();
 System.out.println(&amp;quot;Title: &amp;quot; + metadata.get(&amp;quot;title&amp;quot;));
 System.out.println(&amp;quot;Author: &amp;quot; + metadata.get(&amp;quot;Author&amp;quot;));
 }
[/sourcecode]
<h4>The Value-Adds</h4>
<p style="text-align: justify;">The Parser API gives back meta data that confirms to various metadata formats like <a href="http://dublincore.org/">Dublin core</a> and <a href="http://creativecommons.org/">Creative commons</a>. If you are not concerned with the meta-data then you can use the simpler <a href="http://tika.apache.org/0.7/api/org/apache/tika/utils/ParseUtils.html">ParseUtils </a>API for extracting only text.</p></p>
<p>[sourcecode language="java"]
@Test
public void testTikaParserUtils() throws Exception {
 String resourceLocation = &amp;quot;target/test-classes/Tika_EN.txt&amp;quot;;
 String content = ParseUtils.getStringContent(new File(resourceLocation), new TikaConfig());
 System.out.println(content);
}
[/sourcecode]
<p style="text-align: justify;">The API also provides a bunch of value add APIs like  language detection, stream detection etc.</p></p>
<p>[sourcecode language="java"]
@Test
public void testTypeDetector() throws Exception {
 String resourceLocation = &amp;quot;Tika.pdf&amp;quot;;
 InputStream input = this.getClass().getClassLoader().getResourceAsStream(resourceLocation);
 Detector detector = new TypeDetector();
 MediaType media = detector.detect(input, new Metadata());
 System.out.println(&amp;quot;Extact Type: &amp;quot; + media.getType());
 System.out.println(&amp;quot;Sub Type: &amp;quot; + media.getBaseType());
 }</p>
<p>@Test
public void testLanguageIdentifier() throws Exception {
 String resourceLocation = &amp;quot;Tika_EN.txt&amp;quot;;
 InputStream input = this.getClass().getClassLoader().getResourceAsStream(resourceLocation);
 ContentHandler textHandler = new BodyContentHandler();
 Metadata metadata = new Metadata();
 Parser parser = new AutoDetectParser();
 parser.parse(input, textHandler, metadata, new ParseContext());
 input.close();
 LanguageIdentifier languageIdentifier = new LanguageIdentifier(textHandler.toString());
 System.out.println(&amp;quot;found language :&amp;quot;+ languageIdentifier.getLanguage() + &amp;quot; certainity : &amp;quot; + languageIdentifier.isReasonablyCertain());
 }
[/sourcecode]</p>