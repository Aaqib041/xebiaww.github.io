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

<p>XML Parsing on iPhone/iPad - Is that something your current sprint revolves around? Well if it does, this blog might help you. While parsing XMLs on device, we need to look into factors like performance and memory consumption. While googling, i found couple of XML parsing APIs including Apple's native <a href="http://developer.apple.com/library/mac/#documentation/Coc&lt;/em&gt;oa/Reference/Foundation/Classes/NSXMLParser_Class/Reference/Reference.html">NSXMLParser</a>. NSXMLParser looked like this is all i need. But my requirement was also to 'write' xmls making sure that i do not compromise on performance and memory. On googling more i found couple of APIs which can provide me XML writing capabilities. I chose to explore <a href="http://xmlsoft.org/">libXML2</a> and <a href="https://github.com/robbiehanson/KissXML">KissXML</a>.</p>
<!--more-->

<p>You need to have some basic settings in order to use any of these APIs. Open your project build settings and search for following two parameters and enter the respective values:
<ul>
    <li><strong>Header Search Paths:</strong> <em>/usr/include/libxml2</em></li>
    <li><strong>Other Linker Flags:</strong><em>-lxml2</em></li>
</ul>
<div>and you are good to go!</div>
While the memory consumption by both the APIs are almost same but there is a big difference when it comes to development. libXML2 is written in C, so it's hard to call it as developer-friendly. On the other side, KissXML is written in Objective C, so it's pretty easy to understand and implement as compared to libXML2.</p>
<p>I will show and compare the basic parsing logic provided by both the APIs.</p>
<p><strong>libXML:</strong></p>
<p>[code]
    NSString <em>path = [[NSBundle mainBundle] pathForResource:@&quot;Employee&quot; ofType:@&quot;xml&quot;];
    NSData </em>xmlData = [NSData dataWithContentsOfFile:path];
    xmlTextReaderPtr reader = xmlReaderForMemory([xmlData bytes],
                                                 [xmlData length],
                                                 [path UTF8String], nil,
                                                 (XML_PARSE_NOBLANKS | XML_PARSE_NOCDATA | XML_PARSE_NOERROR | XML_PARSE_NOWARNING));</p>
<pre><code>if(!reader) {
    NSLog(@&amp;quot;Error loading XML&amp;quot;);
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
            if([currentNode isEqualToString:@&amp;quot;employee&amp;quot;]) {
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
</code></pre>
<p>[/code]</p>
<p><strong>KissXML:</strong> (using XPath)</p>
<p>[code]
    NSString <em>source = [NSString stringWithContentsOfFile:[[NSBundle mainBundle] pathForResource:@&quot;employee&quot; ofType:@&quot;xml&quot;] encoding:NSUTF8StringEncoding error:&amp;error];
    DDXMLDocument </em>theDocument = [[DDXMLDocument alloc] initWithXMLString:source options:0 error:&amp;error];
    NSMutableArray *employeeList = [[NSMutableArray alloc] init];</p>
<pre><code>NSArray *employeeXmlResult = [theDocument nodesForXPath:@&amp;quot;/xml/employees/employee&amp;quot; error:&amp;amp;error];

for (DDXMLElement *employee in employeeXmlResult) {
    NSMutableArray *arr = [[NSMutableArray alloc] init];
    arr = (NSMutableArray *)[employee children];

    NSString *employeeName = [[employee attributeForName:@&amp;quot;name&amp;quot;] stringValue];
    [employeeList insertObject:employeeName atIndex:0];

}
</code></pre>
<p>[/code]</p>
<p>So was there something in both the code snippets that comes into mind at first? May be the developer-friendliness i was talking above? One of the reason i preferred KissXML was this.</p>
<p>One thing which is important to note here is the performance consideration. While i was testing my project with some extra data stubbed at the backend, i found some pause happening while using KissXML. The pause happened when my application received response for that heavy XML. When i replaced the parsing logic with libXML2, i noticed that the pause went unnoticeable. Though the actual data was not at all heavy, so i simply ignored this test. But this could be an important issue which you can look into before going for KissXML.</p>

## Comments

**[Andrew](#9075 "2012-06-21 14:13:59"):** Very good post! Helped me a lot. But the xml file itself is missing. Though that 's the only disadvantage i noticed. TY!

