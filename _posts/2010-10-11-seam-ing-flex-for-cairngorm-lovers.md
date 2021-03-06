---
layout: post
header-img: img/default-blog-pic.jpg
author: GauravS
description: 
post_id: 5610
created: 2010/10/11 21:07:31
created_gmt: 2010/10/11 16:07:31
comment_status: open
---

# Seam-ing Flex for Cairngorm Lovers

Seam, Flex, Flamingo, Cairngorm - Looking for a combination of these techie stuff. In this blog, i will talk about how to write Flamingo-specific code using Cairngorm. Talking about these four technologies individually –

[Flex][1] – SDK released by Adobe Systems for the development and deployment of cross-platform RIA.

[Seam][2] – Web application framework developed by JBoss.

[Flamingo][3] – Integration library and framework for building Flex/JavaFX based user interfaces for your JBoss Seam/Spring Framework applications.

[Cairngorm][4] – Open source framework for application architecture in Adobe Flex.

Writing code in Flex as per Cairngorm would not be an alarming task, unless it includes Flamingo's API. Flamingo allows your Flex application to interact with Seam. We use RemoteObject class in Flex to call JAVA methods on the server-side. Giving it a proper destination allows a Flex application to talk to Java methods (that obviously requires some more configurations). Unfortunately routine RemoteObject is not a good option to choose while talking to Seam. Before presenting the 'good' option lets try to understand why we need something more that a RemoteObject. Seam based applications are not just simple Java files, but they have a concept of Session-based objects.

__Flamingo actually extends RemoteObject, and make it's own – 'SeamRemoteObject'. Lets have a look at it:

[java] public dynamic class SeamRemoteObject extends RemoteObject {

public override function getOperation(name:String):AbstractOperation { var o:Object = _operations[name]; var op:AbstractOperation = (o is AbstractOperation) ? AbstractOperation(o) : null; if (op == null) { op = new SeamOperation(this, name); _operations[name] = op; op.asyncRequest = asyncRequest; } return op; } } [/java]

If you look at 'getOperation', you can sense what Flamingo does. Flamingo override this method and make the simple operation as a 'SeamOperation'. Now here comes the advantage. SeamOperation overrides the 'invoke' method and returns a seam-specific IMessage and AsyncToken object.

[java] mx_internal override function invoke(msg:IMessage, token:AsyncToken=null):AsyncToken { msg.headers[CONVERSATION_TAG] = SeamRemoteObject.getConversationId(); return super.invoke(msg, token); } [/java]

As we can see, 'msg' stores the conversation id through SeamRemoteObject. Also, inside the constructor of the same class, they add an listener for handling the Result:

Inside the result handler, SeamRemoteObject then call it's static method, 'setConversationId()' -

[java] SeamRemoteObject.setConversationId(event.message.headers[CONVERSATION_TAG] != null ? event.message.headers[CONVERSATION_TAG] : ""); [/java]

Getting back to Flex – The main classes that we need to focus upon are Services and Delegate. As per Cairngorm, 'service' class should contain the server side calls. While using Flamingo, instead of Flex's RemoteObject, we would use SeamRemoteObject to make the call. So the code would be -

[java] <flamingo:SeamRemoteObject id="seamRemoteObject" destination="destination"/> [/java]

Don't forget to include the namespace for flamingo.

That's all you need to do in Services class, now coming to Delegate class – As usual, you would have a public method inside, where you would call the Java method. We can have a variable of type RemoteObject, and instantiate it in the constructor. Writing the next step as a part of cairngorm-routine,

[java] this.service = ServiceLocator.getInstance().getRemoteObject("seamRemoteObject"); [/java]

Now if you look carefully at the above line, ServiceLocator.getInstance().getRemoteObject() would return a value of type SeamRemoteObject, but our variable 'service' is of type RemoteObject. To avoid the 'known' error, we just need to typecast the return value of type RemoteObject. The code would be -

[java] this.service = RemoteObject(ServiceLocator.getInstance().getRemoteObject("seamRemoteObject")); [/java]

You can then call the Java method through the 'service' variable -

[java] var token:AsyncToken = service.javaMethod(); token.addResponder(responder); [/java]

After this, you can carry with the normal Cairngorm procedure, i.e. control would reach either the result/fault inside the Command class.

I know it's a vast topic to discuss, so feel free to pour in your comments.

   [1]: http://www.adobe.com/products/flex/?sdid=GXZOB
   [2]: http://seamframework.org/
   [3]: http://www.exadel.com/web/portal/flamingo
   [4]: http://opensource.adobe.com/wiki/display/cairngorm/Cairngorm