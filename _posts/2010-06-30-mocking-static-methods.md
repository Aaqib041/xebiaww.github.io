---
layout: post
header-img: img/default-blog-pic.jpg
author: saket_vishal
description: 
post_id: 3941
created: 2010/06/30 14:08:12
created_gmt: 2010/06/30 09:08:12
comment_status: open
---

# Mocking static methods

<p>The blog at <a href="http://blog.xebia.com/2007/06/21/mocking-static-calls/">http://blog.xebia.com/2007/06/21/mocking-static-calls/</a> mentions numerous ways that enable a developer to mock static calls, however, with the advent of new mocking frameworks mocking static calls is much more simpler, and would avoid changes in the original design that were made to make it testable. <a href="http://mockito.org/">Mockito</a> is quite a popular mocking framework, but it still doesn't support  mocking static method calls. Put <a href="http://code.google.com/p/powermock/">PowerMock</a> on top of it, and it just becomes icing on the cake.
<!--more--></p>
<p>Power up mockito in three!</p>
<p>One: Tell PowerMock to prepare the class for testing, it acts as a signal that this class needs byte-code manipulation.
<code>@PrepareForTest( {StaticMessageGenerator.class})</code></p>
<p>Two: Enable static mocking for all methods of a class.
<code>PowerMockito.mockStatic(StaticMessageGenerator.class);</code></p>
<p>Three: Over to mockito
<code>Mockito.when(StaticMessageGenerator.getMessage()).thenReturn("hi");</code></p>
<p>In general PoweMock is a framework that allows unit testing of Java code that is otherwise difficult to test. PowerMock acts as a addon to existing frameworks namely <a href="http://easymock.org/">EasyMock</a> and Mockito.</p>

## Comments

**[pligg.com](#4692 "2011-01-04 04:09:28"):** **@ java team => Mocking static methods...** Pour combler le non support des méthodes statiques par Mockito, il est possible de l'associer à powermock pour régler le problème....

