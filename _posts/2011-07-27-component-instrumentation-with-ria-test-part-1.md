---
layout: post
header-img: img/default-blog-pic.jpg
author: Amit Sharma
description: 
post_id: 9326
created: 2011/07/27 14:53:33
created_gmt: 2011/07/27 09:53:33
comment_status: open
---

# Component Instrumentation with RIA Test (Part-1)

<p>One of the key requirements of any modern day enterprise product is to have the capability of Automation. While there are many products in the market today that provide UI automation testing (read Record and Play) but when it comes to automate flex applications, RIA Test <a href="http://www.riatest.com">http://www.riatest.com</a> is as close as you can get to have a perfect solution in place.</p>
<p>We have been using RIATest for quite some time in our product 'DeployIt' and i must say we have reaped huge benefits from it. Deployit by the way is XebiaLabs product for automated deployments.
While RIATest IDE is very intuitive and RIATest itself supports automation for all basic UI components (and also Spark components) . It means that we don't have to do anything special to record and replay the events originating from those components. Unfortunately, things are not very simple if you want to automate  your own components which extends basic flex components. Things heat up even more in case you wish to automate any third party components.</p>
<!--more-->

<p>While RIATest has a default support for basic UI components and TabNavigator component is no exception. In this part 1, we will try to analyze how does RIATest support automation for TabNavigator in order to learn the technicalities of automation delegates.
If you go to your RIATest installation, you will find a flex_classes.xml. This file contains the information in the form of 'ClassInfo' elements which tells the RiaTest Engine about all properties that can be verified for a particular component and which all events can be recorded and replayed.</p>
<p>[sourcecode language="xml"]
&lt;ClassInfo Name=&quot;FlexTabNavigator&quot; Extends=&quot;FlexViewStack&quot; SupportsTabularData=&quot;true&quot;&gt;
      &lt;Internal Class=&quot;mx.events::ItemClickEvent&quot; Type=&quot;itemClick&quot;/&gt;
      &lt;Events&gt;
        &lt;Event Name=&quot;itemClick&quot;&gt;
           &lt;Internal Class=&quot;mx.events::ItemClickEvent&quot; Type=&quot;itemClick&quot;/&gt;
           &lt;Property Name=&quot;relatedObject&quot;&gt;
             &lt;PropertyType Type=&quot;String&quot; Codec=&quot;tab&quot;/&gt;
           &lt;/Property&gt;
        &lt;/Event&gt;
      &lt;/Events&gt;
       &lt;Properties&gt;
         &lt;Property Name=&quot;horizontalAlign&quot; Verify=&quot;true&quot;&gt;
           &lt;PropertyType Type=&quot;String&quot; /&gt;
         &lt;/Property&gt;
         &lt;Property Name=&quot;horizontalGap&quot; Verify=&quot;true&quot;&gt;
           &lt;PropertyType Type=&quot;Number&quot; /&gt;
         &lt;/Property&gt;
         &lt;Property Name=&quot;tabHeight&quot; Verify=&quot;true&quot;&gt;
           &lt;PropertyType Type=&quot;Number&quot; /&gt;
         &lt;/Property&gt;
        &lt;Property Name=&quot;tabWidth&quot; Verify=&quot;true&quot;&gt;
          &lt;PropertyType Type=&quot;Number&quot; /&gt;
        &lt;/Property&gt;
      &lt;/Properties&gt;
&lt;/ClassInfo&gt;
[/sourcecode]</p>
<p>For TabNavigator component, the 'Name' attribute of the classInfo element shows the logical name for the component that will be used by RIATest in the scripts. The 'Extends' attribute shows the logical name of the component that TabNavigator extends. In this case it is FlexViewStack and the component is ViewStack. The actual class name is provided in 'Class' attribute of 'Internal' element. All the events that need to be automated for TabNavigator are provided in the 'Event' element under 'Events'. Here 'Name' attribute depicts the logical name of the event to be used by RIATest in automation scripts. The 'Class' attribute of the 'Internal' tag under &lt;Event&gt; shows the fully qualified name of the actual event class while 'Type' attribute shows the actual event type. Then it is specified that this event needs to be passed an object which is actually the selected Tab. We use   a special attribute called 'Codec' with value 'tab'. The 'Properties' tag contains different 'Property' elements containing the 'Name' and 'Type' of the component properties which you want to verify in RIATest scripts.</p>
<p>For each ClassInfo element in flex_classes.xml, you will find a corresponding Automation class called Delegate in the automation.swc which is either included in Flex sdk or you have to include it as a library to support automation. Since we were using Flex sdk 3.5 this swc was bundled as part of the sdk. For TabNavigator, the automation delegate class is TabNavigatorAutomationImpl.</p>
<p>[sourcecode language="actionscript3"]
use namespace mx_internal;</p>
<p>[Mixin]</p>
<p>public class TabNavigatorAutomationImpl extends ViewStackAutomationImpl
{
    include &quot;../../../core/Version.as&quot;;</p>
<pre><code>public static function init(root:DisplayObject):void
{
    Automation.registerDelegateClass(TabNavigator, TabNavigatorAutomationImpl);
}

public function TabNavigatorAutomationImpl(obj:TabNavigator)
{
    super(obj);
}

protected function get tabNavigator():TabNavigator
{
    return uiComponent as TabNavigator;
}

override public function get automationTabularData():Object
{
    var delegate:IAutomationObject
            = tabNavigator.getTabBar() as IAutomationObject;

    return delegate.automationTabularData;
}

override public function replayAutomatableEvent(interaction:Event):Boolean
{
    var replayer:IAutomationObject =
                tabNavigator.getTabBar() as IAutomationObject ;
    return replayer.replayAutomatableEvent(interaction);
}

override protected function componentInitialized():void
{
    super.componentInitialized();
    tabNavigator.getTabBar().addEventListener(AutomationRecordEvent.RECORD,
                                tabBar_recordHandler, false, 0, true);
}

private function tabBar_recordHandler(event:AutomationRecordEvent):void
{
    recordAutomatableEvent(event.replayableEvent);
}

}
</code></pre>
<p>}</p>
<p>[/sourcecode]</p>
<p>This Automation Delegate class <em><strong>TabNavigatorAutomationImpl </strong></em>extends <em><strong>ViewStackAutomationImpl </strong></em>and it implements <em><strong>IAutomationObject </strong></em>interface . Remember that in its ClassInfo FlexTabNavigator extends FlexViewStack.  So its Automation class also matches the defined ClassInfo.  One of the most important methods of this delegate class is the static init method which takes the DisplayObject as its parameters.  This DisplayObject is the SystemManager of the application and this method registers this delegate class for the TabNavigator component class with automation manager.  The other important method in this delegate class is the overridden <em>componentInitialized</em> method which gets called after the component, which is TabNavigator in our case, gets initialized.  It is in this method only that an EventListener can be added. Notice that in this case, an event listener is added to the ''TabBar' of this TabNavigator component which listens for any recordable event. A handler method named <em>tabBar_recordHandler</em> is added. In this handler, <em>recordAutomatableEvent </em>of the <em>UIComponentAutomationImpl </em>class is called, passing the event which will call the <em>recordAutomatableEvent </em>of the <em>AutomationManager</em>.</p>
<p>For replaying  this event, we need to override <em>replayAutomatableEvent </em>which has the Event as its parameter. The event that was recorded earlier needs to be dispatched from that component. In this case, the ItemClickEvent needs to be replayed by dispatching a MouseEvent to the item (TabBar) that was clicked.  This MouseEvent.CLICK is handled ultimately in the <em>replayAutomatableEvent </em>of <em>UIComponentAutomationImpl </em>which is in the parent hierarchy of <em>TabNavigatorAutomationImpl </em>class.</p>
<p>[sourcecode language="actionscript3"]
public function replayAutomatableEvent(event:Event):Boolean
{
var help:IAutomationObjectHelper = Automation.automationObjectHelper;
if (event is MouseEvent &amp;&amp; event.type == MouseEvent.CLICK)
return help.replayClick(uiComponent, event as MouseEvent);</p>
<p>[/sourcecode]</p>