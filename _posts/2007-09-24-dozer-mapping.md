---
layout: post
header-img: img/default-blog-pic.jpg
author: ajain
description: 
post_id: 291
created: 2007/09/24 10:57:20
created_gmt: 2007/09/24 08:57:20
comment_status: open
---

# Dozer Mapping

<p>Dozer is a Java Bean to Java Bean mapper that recursively copies data from one object to another. Typically, these Java Beans will be of different complex types. This blog will explain you how to convert one Java Bean into another Java Bean by using context type mapping[Dozer mappings], also you can convert one variable type into another variable type by defining them into XML file.
<!--more--></p>
<p>Dozer supports different type of mappings:
1. Simple property mapping
2. Complex type mapping
3. Bi-directional mapping
4. implicit-explicit mapping
5. Recursive mappings
Dozer supports mapping for collection attributes, which is required to element level mapping.
Many a times we need a context based mapping with Dozer so as to control number of fields conversion. Let’s take an example of converting</p>
<p><strong>SourceClass </strong>to <strong>DestinationClass</strong>.
[java]public class SourceClass {
    private String sourceStringForInteger;
    private String sourceStringForLong;
    private String sourceStringForint;
    private String sourceStringForString;
    private List sourceCol;
    …
}</p>
<p>public class DestinationClass {
    private Integer sourceStringForInteger;
    private Long sourceStringForLong;
    private int sourceStringForint;
    private String sourceStringForString;
    private List destCol;
    …
}[/java]</p>
<p>You can see <strong>SourceClass </strong>contains <strong>sourceCol </strong>variable of type List, which we are converting into <strong>destCol </strong>variable of type List in <strong>DestinationClass</strong>.
Here <strong>sourceCol </strong>is an object type of <strong>SourceChildClass </strong>and <strong>destCol </strong>is an object of <strong>DestinationChildClass</strong>.
[java]
public class SourceChildClass {
    private String prop1;
    private String prop2;
    private String prop3;
    …
}</p>
<p>public class DestinationChildClass {
    private Integer prop1;
    private Integer prop2;
    private Integer prop3;
    …
}[/java]</p>
<p><strong>Usecase 1 [Simple conversion of source object into destination object.]</strong></p>
<p>We will use map-id attribute to name the cases.
In one service call/conversion we want to convert SourceClass into DestinationClass (say caseA)
[xml]<mapping map-id="caseA">
      <class-a>package.SourceClass</class-a>
      <class-b>package.DestinationClass</class-b>
      <field>
       <a>sourceCol</a>
       <b>destCol</b>
       <a-hint>package.SourceChildClass</a-hint>
       <b-hint>package.DestinationChildClass</b-hint>
      </field>
</mapping>[/xml]
In this mapping</p>
<ol>
<li>Tag &lt;class-a&gt; value source class</li>
<li>Tag &lt;class-b&gt; value destination</li>
<li>Tag &lt;a&gt; value collection variable in source class</li>
<li>Tag &lt;b&gt; value collection variable in destination class</li>
<li>Tag &lt;a-hint&gt; Object type of collection defines as tag &lt;a&gt; value.</li>
<li>Tag &lt;b-hint&gt; Object type of collection defines as tag &lt;b&gt; value.</li>
<li>Other SourceClass fields will implicitly convert into DestinationClass fields irrespective of there data types, like sourceStringForInteger a variable of String type in SourceClass, dozer will convert this variable into Integer type in DestinationClass.</li>
</ol>
<p>Let’s see we want to exclude <strong>sourceStringForInteger </strong>variable define <strong>SourceClass </strong>(say caseB).</p>
<p>[xml]<mapping map-id="caseB">
      <class-a>package.SourceClass</class-a>
      <class-b>package.DestinationClass</class-b>
      <field>
       <a>sourceCol</a>
       <b>destCol</b>
       <a-hint>package.SourceChildClass</a-hint>
       <b-hint>package.DestinationChildClass</b-hint>
      </field>[/xml]
      <strong>[xml]<field-exclude>
            <a> sourceStringForInteger </a>
            <b> sourceStringForInteger </b>
          </field-exclude>
   </mapping>[/xml]</strong></p>
<p>In this mapping
 Add <field-exclude> tag and define variable names, which you want to exclude.</p>
<p>Let’s see how code look like. sourceClass is an object of type <strong>SourceClass </strong>which has to be converted into <strong>DestinationClass </strong></p>
<pre><code>       [java]DestinationClass destinationClass = (DestinationClass) mapper.map (sourceClass, DestinationClass.class,"caseA");[/java]
</code></pre>
<p>Definition of mapper is done as bean in Spring.</p>
<p>Here we pass a third string argument indicating the name of the mapping to be used by the Dozer. This argument will be used for converting from SourceClass to DestinationClass.
While doing reverse conversion means conversion form DestinationClass to SourceClass we have to add <strong>PRIME</strong> to name of mapping.</p>
<pre><code>        SourceClass sourceClass = (SourceClass) mapper.map (destinationClass, SourceClass.class,"caseAPRIME");
</code></pre>
<p><strong>Usecase 2 [Complex conversion of source object into destination object.] </strong></p>
<p>Let’s see a complex scenario. Say we want to exclude <strong>prop1 </strong>variable defined in <strong>SourceChildClass </strong>to be converter for caseA. So for this we have to tell the dozer to use a different mapping while converting the object belonging to collection. We could name that mapping as collCaseA, and in the field tag we will use map-id to indicate the name.
[xml]<mapping map-id="caseA">
      <class-a>package.SourceClass</class-a>
      <class-b>package.DestinationClass</class-b>
       &lt;! -- Uses mapping named childCaseA for conversion of objects in the collection -- &gt;
      <field map-id="collCaseA">
       <a>sourceCol</a>
       <b>destCol</b>
       <a-hint>package.SourceChildClass</a-hint>
       <b-hint>package.DesctinationChildClass</b-hint>
      </field>
 </mapping>[/xml]
<strong>[xml]
 <mapping map-id="collCaseA">
      <class-a>package.SourceChildClass</class-a>
      <class-b>package.DestinationChildClass</class-b>
      <field-exclude>
         <a>prop1</a>
         <b>prop1</b>
      </field-exclude>
 </mapping>[/xml]</strong></p>
<p>In this mapping
The change done is from Usecase-1, CaseA in adding new mapping to define exclude specific field from collection conversion under<strong> &lt; mapping map-id="collCaseA"&gt;</strong> tag.</p>
<p>Similarly for caseB we want to exclude <strong>prop2 </strong>variable define in <strong>SourceChildClass</strong>, so we call it as collCaseB.</p>
<p>[xml]<mapping map-id="caseB">
  <class-a>package.SourceClass</class-a>
  <class-b>package.DestinationClass</class-b>
  <field map-id="collCaseB">
   <a>sourceCol</a>
   <b>destCol</b>
   <a-hint>package.SourceChildClass</a-hint>
   <b-hint>package.DestinationChildClass</b-hint>
  </field>
  <field-exclude>
   <a>sourceStringForInteger</a>
   <b>sourceStringForInteger</b>
  </field-exclude>
</mapping>
[/xml]
<strong>[xml]<mapping map-id="collCaseB">
    <class-a>package.SourceChildClass</class-a>
    <class-b>package.DestinationChildClass</class-b>
    <field-exclude>
      <a>prop2</a>
      <b>prop2</b>
    </field-exclude>
</mapping>[/xml]</strong></p>
<p>In this mapping
The change done is from Usecase-1, CaseB in adding new mapping to define exclude specific field from collection conversion under <strong>&lt;mapping map-id="collCaseB"&gt; </strong>tag.</p>
<p>Conversion code would remain same for Usecase 2 as in Usecase 1.</p>
<p><em><strong>Conclusion:</strong> </em> You have seen various cases of context based mapping conversion. For more information you can refer following resources.
•   <a href="http://dozer.sourceforge.net/documentation/deepmapping.html">Deep Mappings </a>
•   <a href="http://dozer.sourceforge.net/">http://dozer.sourceforge.net/</a>
If you have any specific question, please comment back. I will try to reply with answer.</p>