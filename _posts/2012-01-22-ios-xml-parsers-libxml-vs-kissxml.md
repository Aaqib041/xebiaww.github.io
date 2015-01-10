---
layout: post
header-img: img/default-blog-pic.jpg
author: GauravS
description: 
post_id: 10997
created: 2012/01/22 01:11:16
created_gmt: 2012/01/21 20:11:16
comment_status: open
---

# XML Parsers for iOS: libXML vs KissXML

XML Parsing on iPhone/iPad - Is that something your current sprint revolves around? Well if it does, this blog might help you. While parsing XMLs on device, we need to look into factors like performance and memory consumption. While googling, i found couple of XML parsing APIs including Apple's native [NSXMLParser][1]. NSXMLParser looked like this is all i need. But my requirement was also to 'write' xmls making sure that i do not compromise on performance and memory. On googling more i found couple of APIs which can provide me XML writing capabilities. I chose to explore [libXML2][2] and [KissXML][3].

You need to have some basic settings in order to use any of these APIs. Open your project build settings and search for following two parameters and enter the respective values: 

  * **Header Search Paths:** _/usr/include/libxml2_
  * **Other Linker Flags:**_-lxml2_

and you are good to go!

While the memory consumption by both the APIs are almost same but there is a big difference when it comes to development. libXML2 is written in C, so it's hard to call it as developer-friendly. On the other side, KissXML is written in Objective C, so it's pretty easy to understand and implement as compared to libXML2.

I will show and compare the basic parsing logic provided by both the APIs.

**libXML:**

``` 
 NSString _path = [[NSBundle mainBundle] pathForResource:@"Employee" ofType:@"xml"]; NSData _xmlData = [NSData dataWithContentsOfFile:path]; xmlTextReaderPtr reader = xmlReaderForMemory([xmlData bytes], [xmlData length], [path UTF8String], nil, (XML_PARSE_NOBLANKS | XML_PARSE_NOCDATA | XML_PARSE_NOERROR | XML_PARSE_NOWARNING));
    
    
    if(!reader) {
        NSLog(@&quot;Error loading XML&quot;);
    }
    
    NSString *currentNode = nil;
    NSString *nodeValue = nil;
    
    employeeCollection = [NSMutableArray array];
    
    NSDictionary *employee = nil;
    
    char* temp;
    
    while (true) {
        if (!xmlTextReaderRead(reader)) break;
        switch (xmlTextReaderNodeType(reader)) {
            case XML_READER_TYPE_ELEMENT: {
                temp = (char *) xmlTextReaderConstName(reader);
                currentNode = [NSString stringWithCString:temp encoding:NSUTF8StringEncoding];
                if([currentNode isEqualToString:@&quot;employee&quot;]) {
                    employee = [NSMutableDictionary dictionary];
                    [employeeCollection addObject:employee];
                }
                break;
            }
            case XML_READER_TYPE_TEXT: {
                temp = (char *) xmlTextReaderConstValue(reader);
                nodeValue = [NSString stringWithCString:temp encoding:NSUTF8StringEncoding];
                [employee setValue:nodeValue forKey:currentNode];
                currentNode = nil;
                nodeValue = nil;
                break;
            }
            default:
                break;
        }
    }
    


 ```

**KissXML:** (using XPath)

``` 
 NSString _source = [NSString stringWithContentsOfFile:[[NSBundle mainBundle] pathForResource:@"employee" ofType:@"xml"] encoding:NSUTF8StringEncoding error:&error]; DDXMLDocument _theDocument = [[DDXMLDocument alloc] initWithXMLString:source options:0 error:&error]; NSMutableArray *employeeList = [[NSMutableArray alloc] init];
    
    
    NSArray *employeeXmlResult = [theDocument nodesForXPath:@&quot;/xml/employees/employee&quot; error:&amp;error];
    
    for (DDXMLElement *employee in employeeXmlResult) {
        NSMutableArray *arr = [[NSMutableArray alloc] init];
        arr = (NSMutableArray *)[employee children];
    
        NSString *employeeName = [[employee attributeForName:@&quot;name&quot;] stringValue];
        [employeeList insertObject:employeeName atIndex:0];
    
    }
    


 ```

So was there something in both the code snippets that comes into mind at first? May be the developer-friendliness i was talking above? One of the reason i preferred KissXML was this.

One thing which is important to note here is the performance consideration. While i was testing my project with some extra data stubbed at the backend, i found some pause happening while using KissXML. The pause happened when my application received response for that heavy XML. When i replaced the parsing logic with libXML2, i noticed that the pause went unnoticeable. Though the actual data was not at all heavy, so i simply ignored this test. But this could be an important issue which you can look into before going for KissXML.

   [1]: http://developer.apple.com/library/mac/#documentation/Coc</em>oa/Reference/Foundation/Classes/NSXMLParser_Class/Reference/Reference.html
   [2]: http://xmlsoft.org/
   [3]: https://github.com/robbiehanson/KissXML

## Comments

**[Andrew](#9075 "2012-06-21 14:13:59"):** Very good post! Helped me a lot. But the xml file itself is missing. Though that 's the only disadvantage i noticed. TY!

