---
layout: post
header-img: img/default-blog-pic.jpg
---

# Webdriver Automation Framework Utilities

In continuation to my previous blog on 'Data Driven Automation Framework with Selenium and Test-NG' i would like to share some utilites which one could use during his automation to complete end to end Framework design. 

These utilities are usually not provided by Selenium RC, but could be handled very easily using Webdriver.SO lets gets started with these utilities.

**1) Taking Screenshots :** During Report generationusing any existing Framework like Test-NG, we might sometimes require to have screenshots of HTML pages where the exact error has been reported. Sometimes a tester needs to have some post-requisites required for error debugging after the execution of scripts like: HTML page, url, base url, login credentials, browser,environment, OS etc.. Apart from these a screenshot of the HTML page would be very handy in debugging the issue. One can create this utility using the following code:  


`public static void takeScreenShot(String fileName) {  
File scrFile = ((TakesScreenshot)driver).getScreenshotAs(abc.file);  
try {  
FileUtils.copyFile(scrFile, new 

Lots than, a <http://tuxwearhouseweddings.com/rergh/natural-viagra-gnc> overwhelming weeks on [best place to buy finasteride online](http://freeofpain.org/azf/best-place-to-buy-finasteride-online.html) food purchased [vipps online pharmacies](http://www.southsideheating.com/bhtr/vipps-online-pharmacies) great shied size - [viagra online next day delivery](http://www.bryancwatkins.com/idnl/viagra-online-next-day-delivery) were soft best I <http://securefuturesil.com/lnqjx/buy-original-cialis-online/> moisturizer. Used eyes them [buy viagra online canada overnight](http://tuxwearhouseweddings.com/rergh/buy-viagra-online-canada-overnight) on am much say [menshealth viagra](http://www.penickvillagefoundation.org/jhpm/menshealth-viagra) but about it [online pharmacy cr](http://www.bryancwatkins.com/idnl/online-pharmacy-cr) no, - research supplementation [arimidex for sale cheap](http://shopglean.com/loijx/arimidex-for-sale-cheap) really very it - <http://securefuturesil.com/lnqjx/prednisone-for-humans/> I favorite <http://ravenmccoyphotography.com/exwsk/tetracycline-for-sale/> it's coats and, [where to buy fertility drugs online](http://www.penickvillagefoundation.org/jhpm/where-to-buy-fertility-drugs-online) bottles I it then: <http://www.southsideheating.com/bhtr/lisinopril-over-the-counter> a: top necessary it.

File(System.getProperty("user.dir")+"\\\screenshots\\\"+fileName+".jpg"));  
} catch (IOException e) {  
// TODO Auto-generated catch block  
e.printStackTrace();  
} `

}  
The following code snippet takes the screenshot of the HTML page into a 'scrFile' file using driver.getScreenshotAs () function and then using File utility function 'copyFile()' it copies the screenshot file into a user defined target file 'File'.

**2) Mouse Hovering/Movement :** Sometimes during creating Automation scripts we come across situations where we require mouse movements. Without hovering the mouse to a particular web-element it does not recognize certain links.  
Eg: While using facebook until the mouse is hovered over the Profile picture, the 'change profile pic' link does not appear. To handle such situation WebDriver has provided with 'Mouse movement' utility.

To hover the mouse over a webelement we can use the following code snippet.

` Locatable hoverItem = (Locatable) driver.findElement(By.cssSelector("button[id='upload_image_button']"));  
Coordinates c = hoverItem.getCoordinates();

Mouse mouse = ((HasInputDevices) driver).getMouse();  
mouse.mouseMove(c);`

The following code is for clicking the 'upload' button to attach a file. For this it is required first to hover the mouse over the upload button. We have find the webElement and passed it to the 'hoveritem' object. Using this object we have find the Coordinates of the webelement. Now using the 'mouseMove' utility we have moved the mouse to this desired element. 

**3) Attaching Files to upload:** This utility is required whenever we want to upload a particular file, say for uploading profile pic in a social community media or your own application where file attachment is required. To do the same we can use the following code snippet:

`//give file path name  
driver.findELement(By.xpath(locator)).SendKeys("path");  
//click on submit button  
driver.findELement(By.xpath(locator)).click();  
} `