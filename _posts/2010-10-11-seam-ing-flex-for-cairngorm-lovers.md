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

<p>Seam, Flex, Flamingo, Cairngorm - Looking for a combination of these techie stuff. In this blog, i will talk about how to write Flamingo-specific code using Cairngorm. Talking about these four technologies individually –</p>
<p><a href="http://www.adobe.com/products/flex/?sdid=GXZOB" target="_blank"><span style="text-decoration: underline;">Flex</span></a> – SDK released by Adobe Systems for the development and deployment of cross-platform RIA.</p>
<p><a href="http://seamframework.org/" target="_blank"><span style="text-decoration: underline;">Seam</span></a> – Web application framework developed by JBoss.</p>
<p><a href="http://www.exadel.com/web/portal/flamingo" target="_blank"><span style="text-decoration: underline;">Flamingo</span></a> – Integration library and framework for building Flex/JavaFX based user interfaces for your JBoss Seam/Spring Framework applications.</p>
<p><a href="http://opensource.adobe.com/wiki/display/cairngorm/Cairngorm" target="_blank"><span style="text-decoration: underline;">Cairngorm</span></a> – Open source framework for application architecture in Adobe Flex.</p>
<p>Writing code in Flex as per Cairngorm would not  be an alarming task, unless it includes Flamingo's API. Flamingo allows  your Flex application to interact with Seam. We use RemoteObject class  in Flex to call JAVA methods on the server-side. Giving it a proper  destination allows a Flex application to talk to Java methods (that  obviously requires some more configurations). Unfortunately routine  RemoteObject is not a good option to choose while talking to Seam.  Before presenting the 'good' option lets try to understand why we need  something more that a RemoteObject. Seam based applications are not just  simple Java files, but they have a concept of Session-based objects.</p>
<p><em><!--more--></em>Flamingo actually extends RemoteObject, and make it's own – 'SeamRemoteObject'. Lets have a look at it:</p>
<p>[java]
public dynamic class SeamRemoteObject extends RemoteObject {</p>
<p>public override function getOperation(name:String):AbstractOperation {
    var o:Object = _operations[name];
    var op:AbstractOperation = (o is AbstractOperation) ? AbstractOperation(o) : null;
    if (op == null)
        {
           op = new SeamOperation(this, name);
           _operations[name] = op;
           op.asyncRequest = asyncRequest;
        }
    return op;
    }
}
[/java]</p>
<p>If you look at 'getOperation', you can sense what Flamingo does. Flamingo override this method and make the simple operation as a 'SeamOperation'. Now here comes the advantage. SeamOperation overrides the 'invoke' method and returns a seam-specific IMessage and AsyncToken object.</p>
<p>[java]
mx_internal override function invoke(msg:IMessage, token:AsyncToken=null):AsyncToken {
    msg.headers[CONVERSATION_TAG] = SeamRemoteObject.getConversationId();
    return super.invoke(msg, token);
}
[/java]</p>
<p>As we can see, 'msg' stores the conversation id through SeamRemoteObject. Also, inside the constructor of the same class, they add an listener for handling the Result:</p>
<p>Inside the result handler, SeamRemoteObject then call it's static method, 'setConversationId()' -</p>
<p>[java]
SeamRemoteObject.setConversationId(event.message.headers[CONVERSATION_TAG] != null ? event.message.headers[CONVERSATION_TAG] : &quot;&quot;);
[/java]</p>
<p>Getting back to Flex – The main classes that we need to focus upon are Services and Delegate. As per Cairngorm, 'service' class should contain the server side calls. While using Flamingo, instead of Flex's RemoteObject, we would use SeamRemoteObject to make the call. So the code would be -</p>
<p>[java]
&lt;flamingo:SeamRemoteObject id=&quot;seamRemoteObject&quot;
        destination=&quot;destination&quot;/&gt;
[/java]</p>
<p>Don't forget to include the namespace for flamingo.</p>
<p>That's all you need to do in Services class, now coming to Delegate class – As usual, you would have a public method inside, where you would call the Java method. We can have a variable of type RemoteObject, and instantiate it in the constructor. Writing the next step as a part of cairngorm-routine,</p>
<p>[java]
this.service = ServiceLocator.getInstance().getRemoteObject(&quot;seamRemoteObject&quot;);
[/java]</p>
<p>Now if you look carefully at the above line, ServiceLocator.getInstance().getRemoteObject() would return a value of type SeamRemoteObject, but our variable 'service' is of type RemoteObject. To avoid the 'known' error, we just need to typecast the return value of type RemoteObject. The code would be -</p>
<p>[java]
this.service =  RemoteObject(ServiceLocator.getInstance().getRemoteObject(&quot;seamRemoteObject&quot;));
[/java]</p>
<p>You can then call the Java method through the 'service' variable -</p>
<p>[java]
var token:AsyncToken = service.javaMethod();
token.addResponder(responder);
[/java]</p>
<p>After this, you can carry with the normal Cairngorm procedure, i.e. control would reach either the result/fault inside the Command class.</p>
<p>I know it's a vast topic to discuss, so feel free to pour in your comments.</p>