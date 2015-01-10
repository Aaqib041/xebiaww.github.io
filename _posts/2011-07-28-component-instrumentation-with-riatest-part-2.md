---
layout: post
header-img: img/default-blog-pic.jpg
author: Amit Sharma
description: 
post_id: 9389
created: 2011/07/28 01:11:52
created_gmt: 2011/07/27 20:11:52
comment_status: open
---

# Component Instrumentation with RIATest (Part-2)

In the first part of this series <http://xebee.xebia.in/2011/07/27/component-instrumentation-with-ria-test-part-1/#more-9326> , we discussed how RIATest has a default support for automation of TabNavigator component. Now lets apply our learnings and try to automate a third party component SuperTabNavigator(STN) which is a part of FlexLib components. The SuperTabNavigator is an extension of the TabNavigator navigation container and it functions exactly like the TabNavigator, but adds some functionality. Added functionality includes: 1\. Draggable, re-orderable tabs 2\. Closable tabs 3\. Scrolling tab bar if too many tabs are open 4\. Drop-down list of tabs.

By looking at the code of SuperTabNavigator, found at <http://code.google.com/p/flexlib/source/browse/trunk/src/flexlib/containers/SuperTabNavigator.as?r=212> , it becomes evident (hardly any surprises here) that it extends TabNavigator component. Also notice from the code that SuperTabNavigator component dispatches two events namely 'tabClose' and 'tabReordered'. In this part, we will only focus on the 'tabClose' event and the 'tabReader' automation is left for the reader as an exercise. The 'tabClose' event is dispatched whenever a user clicks the close icon present on the tab. Without automating the STN, we wont be able to record and replay this Event with RIATest. Ok so what do we have to do in order to automate SuperTabNavigator component's 'tabClose' event? Basically we have to perform following three steps in order to enable automation for STN. These are :- 1\. Create a SuperTabNavigatorClassInfo.xml containing the ClassInfo details like events to be automated and properties to be verified. You need to create this xml inside a specific folder named 'classinfo' inside your RIAtest project Folder.

[sourcecode language="xml"]

<AllClasses>  
<ClassInfo Name="SuperTabNavigator" Extends="FlexTabNavigator">  
<Internal Class="flexlib.containers.SuperTabNavigator"/> <Events> <Event Name="tabClose"> <Internal Class="flexlib.events::SuperTabEvent" Type="tabClose"/> <Property Name="tabIndex"> <PropertyType Type="Number"/> </Property> </Event> </Events> <Properties> <Property Name="popUpButtonPolicy" Verify="true"> <PropertyType Type="String" /> </Property> <Property Name="dragEnabled" Verify="true"> <PropertyType Type="Boolean" /> </Property> <Property Name="dropEnabled" Verify="true"> <PropertyType Type="Boolean" /> </Property> <Property Name="closePolicy" Verify="true"> <PropertyType Type="String" /> </Property> </Properties>

</ClassInfo>  
</AllClasses>

[/sourcecode]

In the classInfo above, the logical name of the STN is 'SuperTabNavigator' which will be used by RIATest engine. It extends 'FlexTabNavigator' (understandably so) which is defined in the flex_classes.xml in RIATest installation. The fully qualified name of the SuperTabNavagator is flexlib.containers.SuperTabNavigator. The event we want to automate is of type 'tabClose' while the fully qualified name of the Event is flexlib.events::SuperTabEvent. The logical name of the event to be used by RIATest for automation is 'tabClose'. The SuperTabEvent (have a look at the code found in automation.swc) needs the tab index as a parameter. So we have a property with name 'tabIndex' and type 'Number' for the 'Event' tag in classInfo. Also we have some properties of the SuperTabNavigator to be verified in the RIATest scripts.

  1. Create a SuperTabNavigatorAutomationImpl class in your Flex Project source which should have the necessary methods overridden for record and replay of the events advertised in its classInfo.

[sourcecode language="actionscript3"] package com.xebialabs.deployit.riatestdelegates { import flash.display.DisplayObject; import flash.events.Event;
    
    
    import flexlib.containers.SuperTabNavigator;
    import flexlib.controls.SuperTabBar;
    import flexlib.controls.tabBarClasses.SuperTab;
    import flexlib.events.SuperTabEvent;
    
    import mx.automation.Automation;
    import mx.automation.IAutomationObject;
    import mx.automation.IAutomationObjectHelper;
    import mx.automation.delegates.containers.TabNavigatorAutomationImpl;
    import mx.controls.Button;
    import mx.core.mx_internal;
    
    use namespace mx_internal;
    
    [Mixin]
    public class SuperTabNavigatorAutomationImpl extends TabNavigatorAutomationImpl
    {
    
        private var superTabNavigator:SuperTabNavigator;
    
        //--------------------------------------------------------------------------
        //
        //  Class methods
        //
        //--------------------------------------------------------------------------
    
        /**
         *  Registers the delegate class for a component class with automation manager.
         *
         *  @param root The SystemManger of the application.
         */
        public static function init(root:DisplayObject):void
        {
            Automation.registerDelegateClass(SuperTabNavigator, SuperTabNavigatorAutomationImpl);
        }
    
        public function SuperTabNavigatorAutomationImpl(obj:SuperTabNavigator)
        {
            superTabNavigator = obj;
            super(obj);
    
        }
    
        //--------------------------------------------------------------------------
        //
        //  Event handlers
        //
        //--------------------------------------------------------------------------
    
        /**
         *  Method which gets called after the component has been initialized.
         *  This can be used to access any sub-components and act on the component.
         */
        override protected function componentInitialized():void
        {
            super.componentInitialized();
            superTabNavigator.addEventListener(SuperTabEvent.TAB_CLOSE, tabCloseHandler, false, 0, true);
        }
    
        private function tabCloseHandler(superTabEvent:SuperTabEvent):void {
            recordAutomatableEvent(superTabEvent);
        }
    
        /**
         *  Replays SuperTabEvents by dispatching a MouseEvent to the item that was
         *  clicked. In case of ItemClickEvent it is passed on to the super class to handle.
         *
         *  @param interaction The event to replay.
         *
         *  @return &lt;code&gt;true&lt;/code&gt; if the replay was successful. Otherwise, returns &lt;code&gt;false&lt;/code&gt;.
         */
        override public function replayAutomatableEvent(interaction:Event):Boolean
        {
            if(interaction is SuperTabEvent) {
                var tabCloseEvent:SuperTabEvent = interaction as SuperTabEvent;
                var help:IAutomationObjectHelper = Automation.automationObjectHelper;
    
                var replayer:IAutomationObject = superTabNavigator.getTabBar() as IAutomationObject;
                var superTab:Button = superTabNavigator.getTabAt(tabCloseEvent.tabIndex);
                var closeButton:DisplayObject;
                for(var i:int = 0; i&lt; superTab.numChildren; i++) {
                    closeButton = superTab.getChildAt(i);
                    if(closeButton is Button) {
                        break;
                    }
                }
                help.replayClick(closeButton);
                return true;
            }else {
                return super.replayAutomatableEvent(interaction);
            }
        }
    }
    

} [/sourcecode]

Since SuperTabNavigator extends TabNavigator, the SuperTabNavigatorAutomationImpl extends TabNavigatorAutomationImpl. We register this class as the automation delegate for the SuperTabNavigator component in the static method init. Notice that we have a [Mixin] metadata tag just above our class. When you put the [Mixin] metadata just above your class definition and add a static init function to the class, like we have in our class, the static init function will be called as soon as your application loads (assuming this class is referenced somewhere in the app). This is useful if you have some code that you want to run before any of the other code in the class.

## Comments

**[Amar Deep Singh](#5879 "2011-08-25 12:20:42"):** Hi Amit, Nice blog, i liked it.

**[Amar Deep Singh](#5880 "2011-08-25 12:24:28"):** Hi Amit, it was really very interesting and helpful blog

