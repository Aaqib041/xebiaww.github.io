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

Dozer is a Java Bean to Java Bean mapper that recursively copies data from one object to another. Typically, these Java Beans will be of different complex types. This blog will explain you how to convert one Java Bean into another Java Bean by using context type mapping[Dozer mappings], also you can convert one variable type into another variable type by defining them into XML file. 

Dozer supports different type of mappings: 1\. Simple property mapping 2\. Complex type mapping 3\. Bi-directional mapping 4\. implicit-explicit mapping 5\. Recursive mappings Dozer supports mapping for collection attributes, which is required to element level mapping. Many a times we need a context based mapping with Dozer so as to control number of fields conversion. Let’s take an example of converting

**SourceClass **to **DestinationClass**. [java]public class SourceClass { private String sourceStringForInteger; private String sourceStringForLong; private String sourceStringForint; private String sourceStringForString; private List sourceCol; … }

public class DestinationClass { private Integer sourceStringForInteger; private Long sourceStringForLong; private int sourceStringForint; private String sourceStringForString; private List destCol; … }[/java]

You can see **SourceClass **contains **sourceCol **variable of type List, which we are converting into **destCol **variable of type List in **DestinationClass**. Here **sourceCol **is an object type of **SourceChildClass **and **destCol **is an object of **DestinationChildClass**. [java] public class SourceChildClass { private String prop1; private String prop2; private String prop3; … }

public class DestinationChildClass { private Integer prop1; private Integer prop2; private Integer prop3; … }[/java]

**Usecase 1 [Simple conversion of source object into destination object.]**

We will use map-id attribute to name the cases. In one service call/conversion we want to convert SourceClass into DestinationClass (say caseA) [xml] package.SourceClass package.DestinationClass sourceCol **destCol** package.SourceChildClass package.DestinationChildClass [/xml] In this mapping

  1. Tag <class-a> value source class
  2. Tag <class-b> value destination
  3. Tag <a> value collection variable in source class
  4. Tag <b> value collection variable in destination class
  5. Tag <a-hint> Object type of collection defines as tag <a> value.
  6. Tag <b-hint> Object type of collection defines as tag <b> value.
  7. Other SourceClass fields will implicitly convert into DestinationClass fields irrespective of there data types, like sourceStringForInteger a variable of String type in SourceClass, dozer will convert this variable into Integer type in DestinationClass.

Let’s see we want to exclude **sourceStringForInteger **variable define **SourceClass **(say caseB).

[xml] package.SourceClass package.DestinationClass sourceCol **destCol** package.SourceChildClass package.DestinationChildClass [/xml] **[xml] sourceStringForInteger  ** sourceStringForInteger ** [/xml]**

In this mapping Add  tag and define variable names, which you want to exclude.

Let’s see how code look like. sourceClass is an object of type **SourceClass **which has to be converted into **DestinationClass **
    
    
           [java]DestinationClass destinationClass = (DestinationClass) mapper.map (sourceClass, DestinationClass.class,"caseA");[/java]
    

Definition of mapper is done as bean in Spring.

Here we pass a third string argument indicating the name of the mapping to be used by the Dozer. This argument will be used for converting from SourceClass to DestinationClass. While doing reverse conversion means conversion form DestinationClass to SourceClass we have to add **PRIME** to name of mapping.
    
    
            SourceClass sourceClass = (SourceClass) mapper.map (destinationClass, SourceClass.class,"caseAPRIME");
    

**Usecase 2 [Complex conversion of source object into destination object.] **

Let’s see a complex scenario. Say we want to exclude **prop1 **variable defined in **SourceChildClass **to be converter for caseA. So for this we have to tell the dozer to use a different mapping while converting the object belonging to collection. We could name that mapping as collCaseA, and in the field tag we will use map-id to indicate the name. [xml] package.SourceClass package.DestinationClass <! -- Uses mapping named childCaseA for conversion of objects in the collection -- > sourceCol **destCol** package.SourceChildClass package.DesctinationChildClass [/xml] **[xml]  package.SourceChildClass package.DestinationChildClass prop1 **prop1** [/xml]**

In this mapping The change done is from Usecase-1, CaseA in adding new mapping to define exclude specific field from collection conversion under** < mapping map-id="collCaseA">** tag.

Similarly for caseB we want to exclude **prop2 **variable define in **SourceChildClass**, so we call it as collCaseB.

[xml] package.SourceClass package.DestinationClass sourceCol **destCol** package.SourceChildClass package.DestinationChildClass sourceStringForInteger **sourceStringForInteger** [/xml] **[xml] package.SourceChildClass package.DestinationChildClass prop2 **prop2** [/xml]**

In this mapping The change done is from Usecase-1, CaseB in adding new mapping to define exclude specific field from collection conversion under **<mapping map-id="collCaseB"> **tag.

Conversion code would remain same for Usecase 2 as in Usecase 1.

_**Conclusion:** _ You have seen various cases of context based mapping conversion. For more information you can refer following resources. • [Deep Mappings ][1] • <http://dozer.sourceforge.net/> If you have any specific question, please comment back. I will try to reply with answer.

   [1]: http://dozer.sourceforge.net/documentation/deepmapping.html