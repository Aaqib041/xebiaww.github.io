---
layout: post
header-img: img/default-blog-pic.jpg
author: srentala
description: 
post_id: 15325
created: 2012/11/05 11:15:53
created_gmt: 2012/11/05 06:15:53
comment_status: open
---

# Mouse Movements in WebDriver

While automating a website application, we require to handle mouse movements for various different purposes. In this blog, I have tried to cover all the possible scenarios of mouse movements. Let’s look at the various scenarios one by one. **Handling CSS Menus in Websites using Webdriver** During Automating websites we testers face certain problems. One of the problems which we all might have faced is handling CSS Context Menus. 

What are CSS Context Menus? CSS menus are the Menus under the Links which get only visible when Mouse is hovered over the Link. The common mistake which we all do is to get the locator (CSS/Xpath/id/Name) of the menu which is under the link and try to use it. And when we run the script, Webdriver is unable to find the Web element in the browser. The reason is that element is hidden to webdriver, and it only becomes visible when the mouse is hovered over the parent link. There are certain ways through which we can overcome this problem. I will be discussing few of the solutions to this problem.  **Solution 1 : ** During selenium RC days, we used to have only two interfaces – Mouse and Keyboard for mouse and keyboard operations. Now in Webdriver they use Actions Class which uses both these interfaces. ‘Actions’ class has many inbuilt functions for mouse movements. So we need to first move the mouse till the parent Link, so that the CSS menus are enabled and then we can use the locator of the web element to click. We can do that by doing: `Webdriver driver = new FirefoxDriver(); Driver.get(“http://www.abc.com”); WebElement w = driver.findelement(By.xpath(“”); Actions a = new Actions(driver); a.moveToElement(w).build().perform(); driver.findelement(By.xpath(“”).click();` Sometimes even this does not work. That is because the native events are not enabled in the browser. So for that we need to enable it by doing: `FirefoxProfile p = new FirefoxProfile(); p.setEnableNativeEvents(true);`

**Solution 2:** When we are using driver commands it internally is calling the Javascript snippet for that particular event. Sometimes when we face problems in the above mentioned solution, we can have a simple alternative solution. The solution is that we have the leverage of calling the JS in webdriver. On any click event, a JS is called. When we view the page source of the page and search for any CSS Link, it will show the HTML somewhat like this: 
” onmouseover=”focus(a1)” on mouseout=”focusout()”; This tells that on hovering the mouse on this particular link call the JS function “focus(a1)”. So we have the freedom in web driver to call the JS directly. We can do that by: `Webdriver driver = new FirefoxDriver(); driver.get(“http://www.abc.com”); ((JavascriptExecutor)driver.executeScript(“focus(a1)”);`

**Drag a web element to a specific location: ** We can again use the Actions class to perform this mouse action. WebElement w = driver.findelement(By.xpath(“”); Actions a = new Actions(driver); a.dragAndDropBy(w,,).build().perform(); where x and y coordinates are the coordinates to where the element is to be shifted.

**Drag and Drop a Web Element to another WebElement:** For this again Action Class has a provision to do it like: WebElement source = driver.findelement(By.xpath(“”); WebElement target = driver.findelement(By.xpath(“”);

`Actions a = new Actions(driver); a.dragAndDrop(source,target).build().perform();` where  and  are the web elements identified by their locators.

**Working with sliders:** We sometimes have to automate the slider movements. The same can be done in the following way: WebElement scroller = driver.findelement(By.xpath(“”); `Actions a = new Actions(); Int x= scroller.getLocation().x; a.clickAndHold(scroller).dragAndDropBy(scroller,x,300).build.perform();` here the x coordinate would not change since it is a horizontal slider, so we just have to change the y-coordinate and we can slide the slider to the desired position. Webdriver provides many other mouse movements which RC lacks. The implementations of these methods are also very easy and fast. I have applied all these mouse movements in my existing projects and found very useful and easy.