---
layout: post
header-img: img/default-blog-pic.jpg
author: Khyati
description: 
post_id: 16512
created: 2013/03/31 23:31:24
created_gmt: 2013/03/31 18:31:24
comment_status: open
---

# How to handle TimeOuts , implicit and explicit waits in Selenium?

The most common problem which I used to face while running selenium test scripts is because of the intervention of threads .I went through many forums and communities to see the solution of this problem but was not able to  find the best way out to resolve this problem. So just gave a thought, to write a blog stating the solution which I applied in my project to resolve the time out problems.

While working in a project I faced the same and resolved it via writing a code and separating a class of thread return and thread timeout handling.

Many of the selenium users might have seen this ERROR erstwhile running the scripts.

**ERROR: Timed out after 30000ms**

This problem is seen in Selenium IDE ,RC and Webdriver as well.

_Problem: This occurs often when one uses click method and selenium take time to find the element on web page or when more than one simultaneous clicks are placed on a same class one after another. _ Here is an example, ` selenium.click(" XXX  "); selenium.waitForPageToLoad("20000"); selenium.click(" XXX ");`

or

`selenium.click(" XXX  "); selenium.click(" XXX  "); ` _One of the solution could be if user try finding the locator on web page before clicking on it._ Solution :

To stop the processing of the infinite loop of the thread which was looking for the element (which wasnot visible at that time on the web page)and because of which it was not able to try detecting that element again,we can use the below code.

` public class TimeoutThread extends Thread { final private int timeout; public TimeoutThread(int seconds) { this.timeout = seconds; }

public void run() { try { Thread.sleep(timeout * 1000);

// Timeout occurred ThreadReturn.save(new TimeoutException("Time out occured waiting for page to load"));

// Stop WebDriver.get Robot robot = new Robot(); /_escape key basically press teh escape key from keyboard if the timeout exceeds the particular time which was defined by the user_/ robot.keyPress(KeyEvent.VK_ESCAPE);

} catch (InterruptedException ex) { return; } catch (AWTException ex) { System.out.println("Error occurned pressing ESC"); ex.printStackTrace(); } } } `

_Why implicit and where explicit waits ?_

**Implicit wait **\- Implicit wait time is applied to all elements in your script and if an element appear before specified time than script will start executing and start running the rest of the steps of scripts otherwise script will throw **NoSuchElementException**. Implicit Wait sets internally a timeout that will be used for all successive WebElement searches. Best way to use in setup method.

`WebDriver driver = new FirefoxDriver(); driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);`

**Thread.sleep()** \- It will sleep the thread/execution- time for script.It is actually not a good way to use in script .It sleeps without any condition.

**Explicit waits**:Explicit waits are more smarter and brainy waits that are restricted to a particular web element. It is more extensible at the prospect that one can set it up to wait for any suitable condition. Using explicit waits you are essentially narrating WebDriver the maximum it has to wait for specific time before it stops working.Explicit wait time is practical only for individual definite component.In Explicit waits ,you can configure, how often you need to examine state. `WebDriverWait.until(condition-that-finds-the-element) `

Occasionally implicit wait seems to get overridden and wait time is very little. I have found while Selenium Webdriver testing that implicit waits are good to use but now or then you have to use explicit wait . There’s a train of thought that sometimes comes up here, let’s just wait for the whole page to load and be done with it. Though it is not advisable to make an explicit call for a thread to sleep , but again there isn't a good way around it. However, there are other Selenium provided wait options that help viz. **waitForPageToLoad ** and **waitForFrameToLoad ** have proved notably advantageous.

Happy Coding!