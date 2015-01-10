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

Various popular software vendors like HP, IBM and other has offered their solution in QA industry for automation testing, specifically of user interfaces, which actually move the QA and testing to a new level.

Microsoft had also been jumped into this area with the introduction of CodedUI, as integrated component of Visual Studio Premium and Ultimate.

CodedUI has been extensively used as an alternative to HP's Quick Test Professional, IBM Rational Functional Tester and other lovely tools out there.

The anatomy of Coded UI is quite simple , i'm not getting into the details of that.

My key point to discuss here is that how to use hand-coded snippet for CodedUI test instead of using hard-coded UIMap.uitest xml files for actions and properties.

**Requirements of Hand-coded tests:**

  * Recorded tests are hard to maintain.These test can't be integrated to any framework architecture and has very little scope of alterations.
  * Hand coded test gives the freedom of using any test design pattern to test. like using data driven design, keyword driven design or hybrid pattern.
  * Recorded test doesn't assure you the right result always.
  * Anyway to implement the logic and iterations you need to come down to code.
  * User may not have command on the Recorded script, so locating errors is difficult.

**How to hand-code : **

"Microsoft.VisualStudio.TestTools.UITesting" namespace provide the API to access different types of objects. please follow http://msdn.microsoft.com/en-us/library/microsoft.visualstudio.testtools.uitesting.aspx  for API reference.

Now lets see a short piece of code for a calculator application and how it can be used at different framework architectures.Suppose a test class is created as follows. **Read the lines of code carefully, as the object properties are used in description later.**

[TestMethod]

public void CodedUITestMethod1()        {

// To generate code for this test, select "Generate Code for Coded UI Test" from the shortcut menu and select one of the menu items.            // For more information on generated code, see http://go.microsoft.com/fwlink/?LinkId=179463

try            {

UITestControl calcWindow = Microsoft.VisualStudio.TestTools.UITesting.ApplicationUnderTest.Launch(<path to calculator exe>);

//Getting the window, by specifiying its property and adding to SearchProperties Object.

calcWindow.SearchProperties.Add(WinControl.PropertyNames.Name, "Calculator", WinControl.PropertyNames.ClassName, "CalcFrame", WinControl.PropertyNames.ControlType, "Window");

calcWindow.WaitForControlExist(5000);  //Wait for window to exist.

calcWindow.DrawHighlight(); //Highlight the window.

//Now to click on '8' there is hierarchy of windows. SO lets first get the item window and then get the button to click.

//getting the Item window.

WinWindow UIItemWindow = new WinWindow(calcWindow);

//Get Itemwindow by passing its parent.

//Specifying and adding the property of item window

UIItemWindow.SearchProperties.Add(WinControl.PropertyNames.ControlId, "138", WinControl.PropertyNames.ControlType, "Window");

//Now Getting the button 8 obj, specifying the Item Window as parent.

WinButton UIItem8Button = new WinButton(UIItemWindow);

UIItem8Button.TechnologyName = "MSAA";

UIItem8Button.SearchConfigurations.Add(SearchConfiguration.ExpandWhileSearching);

UIItem8Button.SearchProperties.Add(WinControl.PropertyNames.ControlType, "Button");

UIItem8Button.DrawHighlight();

Mouse.Click(UIItem8Button);

//Similarly Getting Text Window and assert that now it shows 8.

WinWindow UIItem0Window = new WinWindow(calcWindow);

UIItem0Window.SearchProperties.Add(WinControl.PropertyNames.ControlId, "150");

WinText UiItem0Text = new WinText(UIItem0Window); //getting the Text window.

UiItem0Text.SearchProperties.Add(WinControl.PropertyNames.ControlType, "Text"); //Specifying and adding its property.

Assert.AreEqual(UiItem0Text.DisplayText, "8");assert for its display text.

}

catch (Exception e)            {                Console.WriteLine(e.Message);            }

Above code snippet is the basic script that doesn't use typical CodedUI anatomy architecture still it doesn't use hardcoded UIMap.uitest xml defenations for object identifications and actions.

Now its time to go **"Scriptless"**,so that test writing doesn't require tool specific API knowledge.So start converting the above script, by incorporating it in a Hybrid design pattern.

Following thing need to be done :

1\. **Object Identification :**

Prepare a simple object repository which can be any file like Excel etc.

From the code above we can see that object requires the parent object to locate.So plan the notation in such a way like

## Comments

**[Burdette Lamar](#9481 "2014-04-06 22:05:17"):** Thanks for this pose\t ngaur. I'm doing hand-coded CUIT, which I blog about over at BurdetteLamar.wordpress.com.

