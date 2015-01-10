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

One of the key requirements of any modern day enterprise product is to have the capability of Automation. While there are many products in the market today that provide UI automation testing (read Record and Play) but when it comes to automate flex applications, RIA Test <http://www.riatest.com> is as close as you can get to have a perfect solution in place.

We have been using RIATest for quite some time in our product 'DeployIt' and i must say we have reaped huge benefits from it. Deployit by the way is XebiaLabs product for automated deployments. While RIATest IDE is very intuitive and RIATest itself supports automation for all basic UI components (and also Spark components) . It means that we don't have to do anything special to record and replay the events originating from those components. Unfortunately, things are not very simple if you want to automate your own components which extends basic flex components. Things heat up even more in case you wish to automate any third party components.

While RIATest has a default support for basic UI components and TabNavigator component is no exception. In this part 1, we will try to analyze how does RIATest support automation for TabNavigator in order to learn the technicalities of automation delegates. If you go to your RIATest installation, you will find a flex_classes.xml. This file contains the information in the form of 'ClassInfo' elements which tells the RiaTest Engine about all properties that can be verified for a particular component and which all events can be recorded and replayed.

[sourcecode language="xml"] <ClassInfo Name="FlexTabNavigator" Extends="FlexViewStack" SupportsTabularData="true"> <Internal Class="mx.events::ItemClickEvent" Type="itemClick"/> <Events> <Event Name="itemClick"> <Internal Class="mx.events::ItemClickEvent" Type="itemClick"/>         <Property Name="relatedObject"> <PropertyType Type="String" Codec="tab"/> </Property> </Event> </Events> <Properties>       <Property Name="horizontalAlign" Verify="true">         <PropertyType Type="String" />       </Property>       <Property Name="horizontalGap" Verify="true">         <PropertyType Type="Number" />       </Property>       <Property Name="tabHeight" Verify="true">         <PropertyType Type="Number" />       </Property>       <Property Name="tabWidth" Verify="true">       <PropertyType Type="Number" />     </Property>     </Properties> </ClassInfo> [/sourcecode]

For TabNavigator component, the 'Name' attribute of the classInfo element shows the logical name for the component that will be used by RIATest in the scripts. The 'Extends' attribute shows the logical name of the component that TabNavigator extends. In this case it is FlexViewStack and the component is ViewStack. The actual class name is provided in 'Class' attribute of 'Internal' element. All the events that need to be automated for TabNavigator are provided in the 'Event' element under 'Events'. Here 'Name' attribute depicts the logical name of the event to be used by RIATest in automation scripts. The 'Class' attribute of the 'Internal' tag under <Event> shows the fully qualified name of the actual event class while 'Type' attribute shows the actual event type. Then it is specified that this event needs to be passed an object which is actually the selected Tab. We use a special attribute called 'Codec' with value 'tab'. The 'Properties' tag contains different 'Property' elements containing the 'Name' and 'Type' of the component properties which you want to verify in RIATest scripts.

For each ClassInfo element in flex_classes.xml, you will find a corresponding Automation class called Delegate in the automation.swc which is either included in Flex sdk or you have to include it as a library to support automation. Since we were using Flex sdk 3.5 this swc was bundled as part of the sdk. For TabNavigator, the automation delegate class is TabNavigatorAutomationImpl.

[sourcecode language="actionscript3"] use namespace mx_internal;

[Mixin]

public class TabNavigatorAutomationImpl extends ViewStackAutomationImpl { include "../../../core/Version.as";
    
    
    public static function init(root:DisplayObject):void
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
    

}

[/sourcecode]

This Automation Delegate class _**TabNavigatorAutomationImpl **_extends _**ViewStackAutomationImpl **_and it implements _**IAutomationObject **_interface . Remember that in its ClassInfo FlexTabNavigator extends FlexViewStack.  So its Automation class also matches the defined ClassInfo.  One of the most important methods of this delegate class is the static init method which takes the DisplayObject as its parameters.  This DisplayObject is the SystemManager of the application and this method registers this delegate class for the TabNavigator component class with automation manager.  The other important method in this delegate class is the overridden _componentInitialized_ method which gets called after the component, which is TabNavigator in our case, gets initialized.  It is in this method only that an EventListener can be added. Notice that in this case, an event listener is added to the ''TabBar' of this TabNavigator component which listens for any recordable event. A handler method named _tabBar_recordHandler_ is added. In this handler, _recordAutomatableEvent _of the _UIComponentAutomationImpl _class is called, passing the event which will call the _recordAutomatableEvent _of the _AutomationManager_.

For replaying  this event, we need to override _replayAutomatableEvent _which has the Event as its parameter. The event that was recorded earlier needs to be dispatched from that component. In this case, the ItemClickEvent needs to be replayed by dispatching a MouseEvent to the item (TabBar) that was clicked.  This MouseEvent.CLICK is handled ultimately in the _replayAutomatableEvent _of _UIComponentAutomationImpl _which is in the parent hierarchy of _TabNavigatorAutomationImpl _class.

[sourcecode language="actionscript3"] public function replayAutomatableEvent(event:Event):Boolean { var help:IAutomationObjectHelper = Automation.automationObjectHelper; if (event is MouseEvent && event.type == MouseEvent.CLICK) return help.replayClick(uiComponent, event as MouseEvent);

[/sourcecode]