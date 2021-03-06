---
layout: post
header-img: img/default-blog-pic.jpg
author: Priyanka
description: 
post_id: 13037
created: 2012/04/06 17:00:41
created_gmt: 2012/04/06 12:00:41
comment_status: open
---

# Maximize Browser Window in Selenium Web Driver

Nowadays, automation testers are shifting from selenium1 to selenium web driver because of its many advantages. I am new to automation and had used selenium1 earlier. I have recently started using selenium web driver and while automating my first test, I wanted to maximize my browser window but then I found out that web driver has no direct command of maximizing window like we have in selenium1.  Although we can use selenium commands in webdriver by using webbacked driver function. I used selenium.maximize() function but it was not working on my machine. I knew that it is not the only way of maximizing browser window. There could be other ways also. I have mentioned four different ways of maximizing browser window in selenium web driver in this blog . If in any case, one of them is not working for you then have other options which can be tried out.

  1. The very first method which is given in their documentation is using windowMaximize() command of selenium instance.

``` 


selenium = new WebDriverBackedSelenium(driver,url); selenium.windowMaximize();


 ```

  1. We can use robot class to invoke keyboard action to maximize browser window. Robot class  is used to generate native system input events for the purposes of test automation, self-running demos  and other applications where control of the mouse and keyboard is needed. Robot can be used to facilitate automated testing of Java platform implementations. I used Robot class in my test case to simulate keyboard keys combination ALT+ SPACE and then used down arrows keys to select maximize option in menu.

``` 


Robot robot = new Robot();

// Press ALT and SPACE Keys

robot.keyPress(KeyEvent.VK_ALT); robot.keyPress(KeyEvent.VK_SPACE); robot.keyRelease(KeyEvent.VK_ALT); robot.keyRelease(KeyEvent.VK_SPACE); Thread.sleep(1000); //Press down arrow keys to move to select Maximize option in menu

robot.keyPress(KeyEvent.VK_DOWN); robot.keyRelease(KeyEvent.VK_DOWN); Thread.sleep(100);

robot.keyPress(KeyEvent.VK_DOWN); robot.keyRelease(KeyEvent.VK_DOWN); Thread.sleep(100);

robot.keyPress(KeyEvent.VK_DOWN); robot.keyRelease(KeyEvent.VK_DOWN); Thread.sleep(100);

robot.keyPress(KeyEvent.VK_DOWN); robot.keyRelease(KeyEvent.VK_DOWN); Thread.sleep(100); //Press enter to invoke the Maximize menu option

robot.keyPress(KeyEvent.VK_ENTER); robot.keyRelease(KeyEvent.VK_ENTER); }


 ```

  1. Sikuli is a robust and powerful tool to automate and tests user interfaces screenshots. Sikuli commands can be easily integrated with java code. We just need to download sikul-script jar and configure in build path. After configuring in build path, create and initialize an instance of Screen object.

``` 


Screen screen = new Screen();


 ```

Capture image which you want to get clicked during execution of your test case. For Example:- Take a snapshot of maximize button on browser and save it anywhere on your disk or in any folder of your project and then give its path in click command just like below:-

``` 
 screen.click("Image Path");


 ```

  1. Fourth way is to use Toolkit utility which query the native operating system directly and is platform independent. In addition to this, the code below will maximize the browser window according to your system's current resolution.

``` 


driver.manage().window().setPosition(new Point(0,0)); java.awt.Dimension screenSize = java.awt.Toolkit.getDefaultToolkit().getScreenSize(); Dimension dim = new Dimension((int) screenSize.getWidth(), (int) screenSize.getHeight()); driver.manage().window().setSize(dim);


 ```

I don't say these above are the only ways to maximize browser window . I am sure there are other ways also. I have mentioned all of them in one blog so that it would be helpful for readers to try and know different ways. It would be really helpful to me as well as to other readers if anybody of you can also suggest tried and tested other ways of doing it and  paste your codes in comments.

## Comments

**[Simmi](#8659 "2012-05-02 16:23:23"):** Hey Vijendra, Priyanka @Priyanka: Thanks for collating all possible solutions for window maximize at one place. :) @Vijendra: Thanks for F11 solution, it worked well for me. :)

**[StackTrace](#8980 "2012-06-07 19:51:53"):** for those working with c# develop webdriver extension and use as driver.MaximizeWindow() Extension Code: [DllImport("user32.dll")] public static extern bool ShowWindowAsync(IntPtr hWnd, int nCmdShow); public static void MaximizeWindow(this IWebDriver driver) { Process[] processes = Process.GetProcessesByName(Common.GetBrowserType()); foreach (Process p in processes) { if (p.MainWindowTitle.Contains(driver.Title)) { ShowWindowAsync(p.MainWindowHandle, 3); } } } or simply use _driver.Manage().Window.Maximize(); [only webdriver > 2.21 other option for chrome: var options = new ChromeOptions(); options.AddArgument("start-maximized");

**[Vijendra Aithal](#8520 "2012-04-20 23:15:04"):** driver.manage().window().maximize(); --> works in latest version. Still I like the F11 approach, coz it helps me get maximum screen area when I call screenCapture function when there is an error in automation run. So Priyanka , there is an easy way with Selenium2 (2.20 and lower versions) Webdriver ... for most of the times we login with username and password ... just send the F11 key alongwith it driver.findElement(By.xpath("xpath of User name web element")).sendKeys("Username01",Keys.F11); Even otherwise, once the driver opens URL , just add following as next statement driver.findElement(By.xpath("//*[@*]")).sendKeys(Keys.F11); This would be equivalent of opening a browser and pressing F11

**[Priyanka](#8553 "2012-04-23 18:24:54"):** Thanks for the info Vijendra.. I will try this out and will tell you

**[Santosh](#8551 "2012-04-23 15:56:54"):** Thanks for the Very useful information.

**[NX](#8430 "2012-04-12 11:30:01"):** Thanks for sharung these valuable knowledge base. Keep going. God bless.

**[Nimesh](#8669 "2012-05-03 10:18:11"):** Thanks...:)

**[Priyanka](#8484 "2012-04-18 10:01:06"):** Thanks for the info Alexey.

**[Alexey](#8478 "2012-04-17 19:32:12"):** Hi, Selenium 2.21 includes bundled possibility to maximize window. Looks something like that: WebDriver driver = new FirefoxDriver(); driver.manage().window().maximize();

