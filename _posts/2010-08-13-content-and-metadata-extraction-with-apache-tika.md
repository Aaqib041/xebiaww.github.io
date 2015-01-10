---
layout: post
header-img: img/default-blog-pic.jpg
---

# Content and Metadata Extraction with Apache Tika

Often we  have the requirement of reading different types of files e.g. CSV, PDF, XLS files and there can be  many more types need to be read in an application.

One way, we try to implement such a requirement is by using separate readers for all of them and  may use different frameworks for analyzing each of them e.g. if our application requires PDF, XML and XLS file parsing, we can create a FileReader interface. Then we can  have different implementations of this interface for each of the types and use PDFBox for reading PDF files, Apache POI for XLS files and SAX for XML  files.

[sourcecode language="java"] // Read PDF using PDFBox PDDocument pdDoc = new PDDocument(cosDoc); PDFTextStripper stripper = new PDFTextStripper(); String docText = stripper.getText(pdDoc); ....... //Read XLS using POI POIFSFileSystem fileSystem = new POIFSFileSystem(is); HSSFWorkbook workBook = new HSSFWorkbook(fileSystem); ....` // Read XML using SAX SAXParserFactory spf = SAXParserFactory.newInstance(); SAXParser parser = spf.newSAXParser(); [/sourcecode] This poses additional set of issues like : 

  * We must configure each and every type separately.
  * We may be using different APIs for reading different types, this poses additional challenge of learning and configuring separate APIs for each of types.
As the list of types that your application needs to handle grows, these two issues become more and more visible. 

### Solution : [Apache Tika](http://tika.apache.org/)

[Apache Tika](http://tika.apache.org/) is one project that can assist in reducing, if not eliminating, these pains. The projects site suggests :

"_Apache Tika as toolkit for detecting and extracting meta-data and structured text content from various documents using existing parser libraries_"

Basically the Tika API is a single shot API to read data from multiple types, internally it configures and uses the various existing libraries for reading the data eg. POI for Excel stuff.

#### **The Parser API**

The [Parser](http://tika.apache.org/0.7/api/org/apache/tika/parser/Parser.html) interface is the key component of  Tika library, that can be used to parse data . You need to create an instance of  [Parser](http://tika.apache.org/0.7/api/org/apache/tika/parser/Parser.html) and call the parse API to get the required data. The project currently supports around 14 different formats e.g. text, pdf,  audio, video, image, archives, ms-office, xmls, emails etc. For each of the  supported format the package has a [Parser ](http://tika.apache.org/0.7/api/org/apache/tika/parser/Parser.html)which it employs to read the format. You can either use each of these different parsers for reading different types of data or the API also provides a [AutoDetectParser](http://tika.apache.org/0.7/api/org/apache/tika/parser/AutoDetectParser.html) that can detect the type of stream and  use the corresponding parser.

The Parser.parse method takes the stream, related meta-data as input and a few more arguments, and outputs the results and extra meta-data. The API is not concerned about the specs of the stream i.e. how it is opened etc. The code using the Parser should take care of the stream maintenance i.e opening and closing.

[sourcecode language="java"] @Test public void testPDFParser() throws Exception { String resourceLocation = &quot;Tika.pdf&quot;; InputStream input = this.getClass().getClassLoader().getResourceAsStream(resourceLocation); ContentHandler textHandler = new BodyContentHandler(); Metadata metadata = new Metadata(); PDFParser parser = new PDFParser(); parser.parse(input, textHandler, metadata, new ParseContext()); input.close(); System.out.println(&quot;Title: &quot; + metadata.get(&quot;title&quot;)); System.out.println(&quot;Author: &quot; + metadata.get(&quot;Author&quot;)); System.out.println(&quot;format: &quot; + metadata.get(&quot;source&quot;)); System.out.println(&quot;content: &quot; + textHandler.toString()); } @Test public void testAutoDetectParser() throws Exception { InputStream input = this.getClass().getResourceAsStream(&quot;TestTikaAPI.class&quot;); ContentHandler textHandler = new BodyContentHandler(); Metadata metadata = new Metadata(); Parser parser = new AutoDetectParser(); parser.parse(input, textHandler, metadata, new ParseContext()); input.close(); System.out.println(&quot;Title: &quot; + metadata.get(&quot;title&quot;)); System.out.println(&quot;Author: &quot; + metadata.get(&quot;Author&quot;)); } [/sourcecode] 

#### The Value-Adds

The Parser API gives back meta data that confirms to various metadata formats like [Dublin core](http://dublincore.org/) and [Creative commons](http://creativecommons.org/). If you are not concerned with the meta-data then you can use the simpler [ParseUtils ](http://tika.apache.org/0.7/api/org/apache/tika/utils/ParseUtils.html)API for extracting only text.

[sourcecode language="java"] @Test public void testTikaParserUtils() throws Exception { String resourceLocation = &quot;target/test-classes/Tika_EN.txt&quot;; String content = ParseUtils.getStringContent(new File(resourceLocation), new TikaConfig()); System.out.println(content); } [/sourcecode] 

The API also provides a bunch of value add APIs like  language detection, stream detection etc.

[sourcecode language="java"] @Test public void testTypeDetector() throws Exception { String resourceLocation = &quot;Tika.pdf&quot;; InputStream input = this.getClass().getClassLoader().getResourceAsStream(resourceLocation); Detector detector = new TypeDetector(); MediaType media = detector.detect(input, new Metadata()); System.out.println(&quot;Extact Type: &quot; + media.getType()); System.out.println(&quot;Sub Type: &quot; + media.getBaseType()); } @Test public void testLanguageIdentifier() throws Exception { String resourceLocation = &quot;Tika_EN.txt&quot;; InputStream input = this.getClass().getClassLoader().getResourceAsStream(resourceLocation); ContentHandler textHandler = new BodyContentHandler(); Metadata metadata = new Metadata(); Parser parser = new AutoDetectParser(); parser.parse(input, textHandler, metadata, new ParseContext()); input.close(); LanguageIdentifier languageIdentifier = new LanguageIdentifier(textHandler.toString()); System.out.println(&quot;found language :&quot;+ languageIdentifier.getLanguage() + &quot; certainity : &quot; + languageIdentifier.isReasonablyCertain()); } [/sourcecode]