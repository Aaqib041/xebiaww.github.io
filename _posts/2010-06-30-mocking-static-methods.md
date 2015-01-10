---
layout: post
header-img: img/default-blog-pic.jpg
---

# Mocking static methods

The blog at <http://blog.xebia.com/2007/06/21/mocking-static-calls/> mentions numerous ways that enable a developer to mock static calls, however, with the advent of new mocking frameworks mocking static calls is much more simpler, and would avoid changes in the original design that were made to make it testable. [Mockito](http://mockito.org/) is quite a popular mocking framework, but it still doesn't support mocking static method calls. Put [PowerMock](http://code.google.com/p/powermock/) on top of it, and it just becomes icing on the cake.  Power up mockito in three! One: Tell PowerMock to prepare the class for testing, it acts as a signal that this class needs byte-code manipulation. `@PrepareForTest( {StaticMessageGenerator.class})` Two: Enable static mocking for all methods of a class. `PowerMockito.mockStatic(StaticMessageGenerator.class);` Three: Over to mockito `Mockito.when(StaticMessageGenerator.getMessage()).thenReturn("hi");` In general PoweMock is a framework that allows unit testing of Java code that is otherwise difficult to test. PowerMock acts as a addon to existing frameworks namely [EasyMock](http://easymock.org/) and Mockito.

## Comments

**[pligg.com](#4692 "2011-01-04 04:09:28"):** **@ java team => Mocking static methods...** Pour combler le non support des méthodes statiques par Mockito, il est possible de l'associer à powermock pour régler le problème....

