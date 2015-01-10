---
layout: post
header-img: img/default-blog-pic.jpg
author: ngaur
description: 
post_id: 15439
created: 2012/12/06 21:32:59
created_gmt: 2012/12/06 16:32:59
comment_status: open
---

# Hand-Coding CodedUI

<p style="padding-left: 30px;">Various popular software vendors like HP, IBM and other has offered their solution in QA industry for automation testing, specifically of user interfaces, which actually move the QA and testing to a new level.</p>

<p style="padding-left: 30px;">Microsoft had also been jumped into this area with the introduction of CodedUI, as integrated component of Visual Studio Premium and Ultimate.</p>

<p style="padding-left: 30px;">CodedUI has been extensively used as an alternative to HP's Quick Test Professional, IBM Rational Functional Tester and other lovely tools out there.</p>

<p style="padding-left: 30px;">The anatomy of Coded UI is quite simple , i'm not getting into the details of that.</p>

<p style="padding-left: 30px;"><span style="background-color: #ffffff;">My key point to discuss here is that how to use hand-coded snippet for CodedUI test instead of using hard-coded UIMap.uitest xml files for actions and properties.</span></p>

<p style="padding-left: 30px;"><strong>Requirements of Hand-coded tests:</strong></p>

<!--more-->

<ul style="padding-left: 30px;">
    <li>Recorded tests are hard to maintain.These test can't be integrated to any framework architecture and has very little scope of alterations.</li>
    <li>Hand coded test gives the freedom of using any test design pattern to test. like using data driven design, keyword driven design or hybrid pattern.</li>
    <li>Recorded test doesn't assure you the right result always.</li>
    <li>Anyway to implement the logic and iterations you need to come down to code.</li>
    <li>User may not have command on the Recorded script, so locating errors is difficult.</li>
</ul>

<p style="padding-left: 30px;"><strong>How to hand-code : </strong></p>

<p style="padding-left: 30px;">"Microsoft.VisualStudio.TestTools.UITesting" namespace provide the API to access different types of objects. please follow <span style="color: #0066cc;">http://msdn.microsoft.com/en-us/library/microsoft.visualstudio.testtools.uitesting.aspx </span> for API reference.</p>

<p style="padding-left: 60px;">Now lets see a short piece of code for a calculator application and how it can be used at different framework architectures.Suppose a test class is created as follows. <strong>Read the lines of code carefully, as the object properties are used in description later.</strong></p>

<p style="padding-left: 60px;"><span style="color: #009900;">[TestMethod]</span></p>

<p style="padding-left: 60px;"><span style="color: #009900;">public void CodedUITestMethod1()        {</span></p>

<p style="padding-left: 60px;"><span style="color: #009900;">// To generate code for this test, select "Generate Code for Coded UI Test" from the shortcut menu and select one of the menu items.            // For more information on generated code, see http://go.microsoft.com/fwlink/?LinkId=179463</span></p>

<p style="padding-left: 60px;"><span style="color: #009900;">try            {</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">UITestControl calcWindow = Microsoft.VisualStudio.TestTools.UITesting.ApplicationUnderTest.Launch(&lt;path to calculator exe&gt;);</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">//Getting the window, by specifiying its property and adding to SearchProperties Object.</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">calcWindow.SearchProperties.Add(WinControl.PropertyNames.Name, "Calculator", WinControl.PropertyNames.ClassName, "CalcFrame", WinControl.PropertyNames.ControlType, "Window");</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">calcWindow.WaitForControlExist(5000);  //Wait for window to exist.</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">calcWindow.DrawHighlight(); //Highlight the window.</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">//Now to click on '8' there is hierarchy of windows. SO lets first get the item window and then get the button to click.</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">//getting the Item window.</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">WinWindow UIItemWindow = new WinWindow(calcWindow);</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">//Get Itemwindow by passing its parent.</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">//Specifying and adding the property of item window</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">UIItemWindow.SearchProperties.Add(WinControl.PropertyNames.ControlId, "138", WinControl.PropertyNames.ControlType, "Window");</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">//Now Getting the button 8 obj, specifying the Item Window as parent.</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">WinButton UIItem8Button = new WinButton(UIItemWindow);</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">UIItem8Button.TechnologyName = "MSAA";</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">UIItem8Button.SearchConfigurations.Add(SearchConfiguration.ExpandWhileSearching);</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">UIItem8Button.SearchProperties.Add(WinControl.PropertyNames.ControlType, "Button");</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">UIItem8Button.DrawHighlight();</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">Mouse.Click(UIItem8Button);</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">//Similarly Getting Text Window and assert that now it shows 8.</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">WinWindow UIItem0Window = new WinWindow(calcWindow);</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">UIItem0Window.SearchProperties.Add(WinControl.PropertyNames.ControlId, "150");</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">WinText UiItem0Text = new WinText(UIItem0Window); //getting the Text window.</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">UiItem0Text.SearchProperties.Add(WinControl.PropertyNames.ControlType, "Text"); //Specifying and adding its property.</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">Assert.AreEqual(UiItem0Text.DisplayText, "8");assert for its display text.</span></p>

<p style="text-align: left; padding-left: 60px;"><span style="color: #009900;">}</span></p>

<p style="padding-left: 60px;"><span style="color: #009900;">catch (Exception e)            {                Console.WriteLine(e.Message);            }</span></p>

<p style="padding-left: 30px;">Above code snippet is the basic script that doesn't use typical CodedUI anatomy architecture still it doesn't use hardcoded UIMap.uitest xml defenations for object identifications and actions.</p>

<p style="padding-left: 30px;">Now its time to go <strong>"Scriptless"</strong>,so that test writing doesn't require tool specific API knowledge.So start converting the above script, by incorporating it in a Hybrid design pattern.</p>

<p style="padding-left: 30px;">Following thing need to be done :</p>

<p style="padding-left: 30px;">1. <strong>Object Identification :</strong></p>

<p style="padding-left: 30px;">Prepare a simple object repository which can be any file like Excel etc.</p>

<p style="padding-left: 30px;">From the code above we can see that object requires the parent object to locate.So plan the notation in such a way like</p>

## Comments

**[Burdette Lamar](#9481 "2014-04-06 22:05:17"):** Thanks for this pose\t ngaur. I'm doing hand-coded CUIT, which I blog about over at BurdetteLamar.wordpress.com.

