---
layout: post
header-img: img/default-blog-pic.jpg
author: vashishtha
description: 
post_id: 785
created: 2008/11/08 22:34:54
created_gmt: 2008/11/08 20:34:54
comment_status: open
---

# Current Architectural Frameworks Developments in Flex

Flex provides means to create RIA applications in declarative fashion using MXML. Unlike Swing where you need to do the entire coding in Java, Flex hides a lot of complexity behind MXML tags like JSTL/taglibs do for JSPs. In JSP world, view level scripting is done in JavaScript and presentation layer server side code is written in Java which kind of provides a separation between client side code and server side code. If you really want to do some dynamic stuff on JSPs, either you write some Java code inside JSP (not recommended though) or you use/create taglibs to achieve the same effect. In Flex world, it's all about ActionScript (AS). Irrespective of whether you are writing some scripting or server side code, it's all AS code which kind of creates a confusing situation in front of a developer. It becomes very difficult to separate the scripting code from server side code. That's one of the reasons people complain about Flex as it looks like it doesn't provide a clear separation between scripting code and server side code.

Because of all those reasons, some frameworks have been created in Flex world to provide MVC pattern implementation. One such frameworks is [Cairngorm][1] which is an architectural framework to provide the separation between different MVC layers using some of the very well known design patterns from JEE world. So it may sound quite familiar to you if I talk about [Front Controller][2], [Business Delegate][3], [Service Locator][4], GOF Command Pattern and [Value Object][5] patterns. Flex world still calls Data Transfer Object (DTO) pattern as Value Object because of some reasons. Along with all mentioned patterns it uses one more design pattern which is specific to Flex world and is called [Model Locator][6] design pattern. This pattern is based on singleton pattern though.

Now while I talk about these design patterns, you may wonder that some of these patterns reduces testability a lot because of their singleton nature. Service Locator tries to get the dependencies instead of being injected. To resolve all these problems, you may wonder that there has to be a Spring like dependency injection framework in Flex world too. You are absolutely right. There is one and is called [prana framework][7]. Now if you use prana on top of Cairngorm framework, you automatically remove the need of creating Controller classes, Service Locator, singleton nature of Model Locator etc. Also automatically it enhances the testability of the applications and encourages programming to interfaces.

However while using prana you may come across with a problem. You define the actual implementation of the interface in applicationContext.xml but mxml compiler does not include these dependencies while creating resultant SWF file because it doesn't see these dependencies in the actual code. It may result in a lot of runtime errors. To resolve these problems, generally implementations are included in the main MXML file just for compilation sake which you already know is error prone. I should call it as the limitation of Flex framework as currently it's not able to resolve the dependencies with prana based applicatonContext.xml files. To resolve this, some people create tools to generate the Factory classes from applicationComtext.xml to get actual implementation.

The prana framework is in 0.6 release right now and as you expect it yet again, it requires a similar IDE as you have for Spring (SpringIDE). Even after removing all above problems in Cairngorm with the help of "prana", Cairngorm still has some other problems which then are removed using [cairngorm UM extensions][8].

For unit testing purposes, we have [FlexUnit framework][9] but it doesn't meet some of the requirements of unit testing like testing asynchronous code and other similar problems. To resolve them currently people use [fluint framework][10] which is a unit-test as well as integration test framework. Similar to Selenium web application testing framework Flex world currently has [FlexMonkey][11] to test Flex UI.

Flex build is mostly done on ANT which provides a low level build abstraction compared to Maven 2. Flex world currently lacks good Maven plugins. On similar note, FlexBuilder (an Eclipse based Flex IDE) needs many improvements in IDE in the areas of code refactoring (refactoring support for ActionScript classes other than renaming) and other very usual Eclipse based code generation.

As you already can see that Flex world is going through with a lot of architectural frameworks changes currently. Most of the time, they are Flex specific implementations of already proven existing JEE architectural frameworks/design patterns. In coming time, Flex world is expected to have more matured frameworks which will make the life of a developer easier and software more maintainable.

   [1]: http://opensource.adobe.com/wiki/display/cairngorm/Cairngorm
   [2]: http://java.sun.com/blueprints/corej2eepatterns/Patterns/FrontController.html
   [3]: http://java.sun.com/blueprints/corej2eepatterns/Patterns/BusinessDelegate.html
   [4]: http://java.sun.com/blueprints/corej2eepatterns/Patterns/ServiceLocator.html
   [5]: http://java.sun.com/blueprints/corej2eepatterns/Patterns/TransferObject.html
   [6]: http://www.bigroom.co.uk/blog/improving-cairngorms-modellocator
   [7]: http://www.pranaframework.org/
   [8]: http://code.google.com/p/flexcairngorm/
   [9]: http://code.google.com/p/as3flexunitlib/
   [10]: http://fluint.googlecode.com/ 
   [11]: http://code.google.com/p/flexmonkey/