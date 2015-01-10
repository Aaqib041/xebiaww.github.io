---
layout: post
header-img: img/default-blog-pic.jpg
author: KaranNangru
description: 
post_id: 5590
created: 2010/10/11 17:51:26
created_gmt: 2010/10/11 12:51:26
comment_status: open
---

<!--Delimiting the scope of work involved in merging Cairngorm based Flex Modules and Applications.....-->

# FLEX: Modular Application Development Using Cairngorm Architecture

<p>Over the past few years, Cairngorm architecture has been hugely embraced for developing enterprise Flex applications. While there are a lot of sources available online that demonstrate building flex applications using Cairngorm Architecture, not many serve the cause if you intend to develop <em>Modular Flex Applications using Cairngorm Architecture.</em></p>
<p><strong>Brief description of Flex Modules:</strong>
<em>Flex Modules are code functionality compiled to dynamically-loadable SWF files that can be loaded and unloaded by an application at run-time.
Modules let you split your application into several pieces, or modules. The main application, or loader, can dynamically load other modules that it requires, when it needs them. It does not have to load all modules when it starts, nor does it have to load any modules if the user does not interact with them. </em></p>
<p><em><!--more-->
</em></p>
<p><strong>Pictorial description of Cairngorm Architecture:</strong></p>
<p>[caption id="attachment_29" align="alignnone" width="614" caption="Cairngorm Architecture"]<a href="http://karannangru.files.wordpress.com/2010/10/cairngormarchitecture.png"><img class="size-full wp-image-29" title="CairngormArchitecture" src="http://karannangru.files.wordpress.com/2010/10/cairngormarchitecture.png" alt="Cairngorm Architecture" width="614" height="446" /></a>[/caption]</p>
<p><em>Consider the following scenario: </em>Your team is working on a flex project that is part of the bigger picture(a baseline Flex Application). You might develop your project using an 'independent cairngorm architecture(One that does not interact with the bigger picture)'. <em>Concisely, you are developing a flex module that will be called from a flex application.</em>
In such scenarios, if the module does not have much dependency on the parent application then the module-application integration is many a times postponed till the module starts bearing the fruits, thereby delaying the bitter task of merging architectures(This should generally be a task that should be initiated as soon as you analyze the module design).</p>
<p><strong>While merging Flex Modules and Application that are independently designed using Cairngorm Architecture, following classes should be paid attention to and handled as follows:</strong>
<ul>
    <li><em>Controller</em>:</li>
</ul>
<em>'The Controller is the most sophisticated part of the Cairngorm architecture. The Controller layer is implemented as a singleton FrontController. The FrontController instance, which receives every View-generated event, dispatches the events to the assigned Command class based on the event's declared type.'</em>
Modules can use the controller of the base class. In such a case, all commands of the module need to be registered with the main controller at the time of module load, and, should thereby be removed when the module is unloaded.</p>
<p>[sourcecode language="java"]
package xyz
{
import com.adobe.cairngorm.control.FrontController;
import mx.collections.ArrayCollection;
import .......control.<em>;
import .......commands.</em>;
import .......CommandDesc;</p>
<p>public class BaseApplicationController extends FrontController
{
public function BaseApplicationController()
{
initialiseCommands();
}</p>
<p>public function initialiseCommands() : void
{
//Add all the application level commands
addCommand( baseEventName1, baseCommandClass1 );
addCommand( baseEventName2, baseCommandClass2 );
addCommand( baseEventName3, baseCommandClass3 );
addCommand( baseEventName4, baseCommandClass4 );</p>
<p>}</p>
<p>//Add all module level commands
public function addCommands(commandList:ArrayCollection):void{
for(var i:int=0;i
var commDes:CommandDesc =commandList.getItemAt(i) as CommandDesc;
this.addCommand(commDes.getEventName(),commDes.getCommand());
}
}</p>
<p>//Remove all module level commands
public function removeCommands(commandList:ArrayCollection):void{
for(var i:int=0;i
var commDes:CommandDesc =commandList.getItemAt(i) as CommandDesc;
this.removeCommand(commDes.getEventName());
}
}</p>
<p>}
}
[/sourcecode]</p>
<p>Where CommandDesc.as contains the module level event name and the corresponding Command class:</p>
<p>[sourcecode language="java"]
package abc
{
    import com.adobe.cairngorm.commands.Command;</p>
<pre><code>public class CommandDesc
{
    private var eventName:String;
    private var command:Class;

    public function getEventName():String{
        return eventName;
    }
    public function setEventName(eventName:String):void{
        this.eventName = eventName;
    }
    public function getCommand():Class{
        return command;
    }
    public function setCommand(command:Class):void{
        this.command = command;
    }
}
</code></pre>
<p>}
[/sourcecode]
<ul>
    <li><em>Model:</em></li>
</ul>
<em>'In a Cairngorm Model, related data are stored in Value Objects (VOs), while simple variables can be stored as direct properties of the ModelLocator class. A static reference to the ModelLocator singleton instance is used by the View layers to locate the required data.'</em>
The modelLocator directive is such,that, its always better not to merge modelLocator implementations of the module and the application (since modelLocator contain a lot of contextual information)</p>
<p><em>Following is a change that needs to be incorporated on the module side, as well as the application side:</em>
Since modelLocator is singleton, we should create a modelLocator implementation in the interface gateway(generally compiled as a SWC) that is used for the application-modules interaction. Since this interface is used by the application SWF to communicate with all module's SWF that it needs to use, a singleton modelLocator in the interface is maintainable and can contain various modules and the application specific modelLocator. (I will soon be posting a blog on modular interaction using SWC gateway interfaces).</p>
<p>Interface level ModelLocator that should implement com.adobe.cairngorm.model.ModelLocator:</p>
<p>[sourcecode language="java"]
package externalInterface
{
    import com.adobe.cairngorm.model.ModelLocator;</p>
<pre><code>public class InterfaceModelLocator implements com.adobe.cairngorm.model.ModelLocator
{
    public static var interfaceModelLocator:InterfaceModelLocator;
    private var _modelMap:Object = new Object();

    public function addModel(key:String, model:Object):void{
        _modelMap[key] = model;
    }

    public function getModel(key:String):Object{
        return _modelMap[key];
    }

    public function deleteModel(key:String):void{
        _modelMap[key] = null;
    }

    public function InterfaceModelLocator(){
        if(interfaceModelLocator != null){
            throw new Error( &amp;quot;Only one ModelLocator instance should be instantiated&amp;quot; );
        }
    }

    public static function getInstance():InterfaceModelLocator{
        if ( interfaceModelLocator == null )
        {
            interfaceModelLocator = new InterfaceModelLocator();
        }
        return interfaceModelLocator;
    }
}
</code></pre>
<p>}
[/sourcecode]</p>
<p>Application level ModelLocator that should use the interface level ModelLocator:</p>
<p>[sourcecode language="java"]
[Bindable]
public class ApplicationModelLocator extends InterfaceModelLocator
{
private static var modelLocator : ApplicationModelLocator;</p>
<p>public static function getInstance() : ApplicationModelLocator
{
     if ( modelLocator == null )
    {
      modelLocator = new ApplicationModelLocator();
      InterfaceModelLocator.getInstance().addModel(&quot;ApplicationLevelModelName&quot;,modelLocator);
    }</p>
<pre><code>return ApplicationModelLocator(
InterfaceModelLocator.getInstance().getModel(&amp;quot;ApplicationLevelModelName&amp;quot;));
</code></pre>
<p>}
}
[/sourcecode]</p>
<p>The module level ModelLocator should be coded on similar lines.
<ul>
    <li><em>Styles:</em></li>
</ul>
css usage varies from project to project.  All CSS related changes of the application and the module could be kept in a single .css file.
<ul>
    <li><em>Command:</em></li>
</ul></p>

## Comments

**[Karan Nangru](#5343 "2011-03-06 01:10:16"):** Make sure of the following... >If you are using a common singleton ModelLocator for the main application as well as the Module, then clean up the module on module unload. >Remove all module specific commands from the controller. >Cleaning up the gateway >Releasing the IModuleInfo

**[Bobo](#5291 "2011-02-14 14:00:42"):** I try to implement cairngorm in this way in my modular flex application it works but my module are not completely unloaded. My module contains a datagrid and all VO binded in the grid item persist. Any suggestions?

**[adrian sule](#5900 "2011-08-31 19:40:21"):** I would recommend upgrading to Cairngorm 3, and avoid a couple of pitfalls with C2. (Adobe is also recommending this)

