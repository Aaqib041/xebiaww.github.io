---
layout: post
header-img: img/default-blog-pic.jpg
author: ssinghal
description: 
post_id: 14345
created: 2012/06/06 15:02:56
created_gmt: 2012/06/06 10:02:56
comment_status: open
---

# Quick Start with Automated Functional Testing of Grails Application

### **1\. Introduction**

There are many plug-ins available by which one can automate functional tests for grails application, notably [webtest][1], [functional-test][2], [geb][3], [Selenium-RC ][4]and [Webdriver][5]. Though we haven’t tried all, but explored grails-webdriver plug-in and used for automation.

**Why Grails-Webdriver plug-in? And the reasons are:**

• Easy Grails-Webdriver integration helps to quick start with functional testing of web application • No need to learn any specific tool, one must have some knowledge of webdriver and programming language (Java or Groovy) • Support for Page Object Pattern in Webdriver to implement high-level abstraction and to hide HTML details • Ability to run individual (or multiple) tests directly through IDE like any other JUnit test. • Ability to run functional tests in both HtmlUnit (for speed) and real web browsers like Firefox, IE and Chrome. • Auto Reporting in grails after test execution. 

### **2\. Prerequisites**

**Tools required:**

• [Selenium IDE][6]: Selenium IDE is used to record the test script quickly. • [Firebug][7] • IDE as [SpringSourceToolSuite][8] or [IntelliJ][9]: IDE is used to maintain and write test scripts in a structured way.

**Install Plug-in:** It is very easy to install webdriver plug-in in the grails main project or separate test project via IDE or command line by using following command:

**grails install-plugin webdriver**

Once the plug-in is installed, we have a separate functional folder along with unit and integrate folder in test folder. Now we are ready to go for automated functional tests. 

### **3\. Getting Started**

Consider a Test Scenario, **“To login in the application successfully.”**

![][10]

** **

** **

** **

** **

** **

** **

** **

** **

**Step 1: Install Selenium IDE Plug-in in Firefox and start recording test script to enter login credentials and to click Login button.**

![][11]

** **

** **

** **

** **

** **

** **

** **

** **

** **

** **

**Step 2: Export recorded test script into available format:** Selenium IDE provides various formats notably, JUnit 4 (Webdriver); JUnit 3 (RC); JUnit 4(RC); and so on. Once login script is recorded, we exported the script to JUnit 4 (Webdriver) format.

![][12]

** **

** **

** **

** **

** **

** **

** **

** **

** **

** **

**Step 3: Create test class as test/functional/tests/LoginTest.groovy to paste exported test script.**

[sourcecode type="java"] import org.junit._; import static org.junit.Assert._; import org.openqa.selenium.*; import org.openqa.selenium.firefox.FirefoxDriver; import org.openqa.selenium.support.ui.Select;

public class LoginTest {
    
    
    private WebDriver driver;
    private String baseUrl;
    
    @Before
     public void setUp() throws Exception {
    
        driver = new FirefoxDriver();
        baseUrl = &quot;http://localhost:8080/&quot;;
        driver.manage().timeouts().implicitlyWait(30, TimeUnit.SECONDS);
    }
    

@Test public void testLogin() throws Exception {
    
    
        driver.get(baseUrl + &quot;/login/auth&quot;);
        assertEquals(&quot;Test Application&quot;, driver.getTitle());
        driver.findElement(By.id(&quot;username&quot;)).clear();
        driver.findElement(By.id(&quot;username&quot;)).sendKeys(&quot;admin&quot;);
        driver.findElement(By.id(&quot;password&quot;)).clear();
        driver.findElement(By.id(&quot;password&quot;)).sendKeys(&quot;admin&quot;);
        driver.findElement(By.cssSelector(&quot;button.cta_button&quot;)).click();
    }
    
    @After
    public void tearDown() throws Exception {
    
        driver.quit();
    }
    

} [/sourcecode]

** **

**Step4: Create Login Page Object class as test/functional/pages/LoginPage.groovy**

[sourcecode type="java"] import org.openqa.selenium.* import org.codehaus.groovy.grails.plugins.webdriver.WebDriverHelper

class LoginPage {
    
    
    // Login Page constants
    final static String LOGIN_PAGE_URL = &quot;/login/auth&quot;;
    final static String LOGIN_BUTTON_CSS = &quot;button.cta_button&quot;;
    final static String USERNAME_ID = &quot;username&quot;;
    final static String PASSWORD_ID = &quot;password&quot;;
    final static String REMEMBER_ME_ID = &quot;remember_me&quot;;
    final static String LOGIN_BUTTON_TEXT = &quot;Login&quot;;
    final static String FORGOT_PASSWORD_TEXT = &quot;Forgot Password&quot;;
    
    String username;
    String password;
    WebDriver driver;
    
    public LoginPage() {}
    
    public LoginPage(String username, String password, WebDriver driver) {
    
        this.username = username;
    this.password = password;
        this.driver = driver;
    }
    
    public void openLoginPage(WebDriverHelper webdriver) {
    
        webdriver.open(LOGIN_PAGE_URL);
    }
    
    public void login() {
    
        enterLoginDetails();
        loginButton();
    }
    
    public void enterLoginDetails() {
    
        driver.findElement(By.id(USERNAME_ID)).clear();
        driver.findElement(By.id(USERNAME_ID)).sendKeys(username, Keys.F11);
        driver.findElement(By.id(PASSWORD_ID)).clear();
    driver.findElement(By.id(PASSWORD_ID)).sendKeys(password);
    }
    
    public void loginButton() {
        driver.findElement(By.cssSelector(LOGIN_BUTTON_CSS)).click();
    }
    

} [/sourcecode]

 

Here the LoginPage Class contains both actions implemented as methods (example enterLoginDetails(), login()) and web elements(like button, text fields, checkbox), which are themselves separate objects. Also If in future there is some change in the HTML of Login Page (for example, change the ID of an element, or add an extra div for example) we only have to modify a specific part of our LoginPage object and not every test that interacted with that element.

** **

**Step5: Add assertions and modify test script:** Look at the modified test script class “test/functional/tests/LoginTest.groovy “.

[sourcecode type="java"] import org.openqa.selenium.* import pages.LoginPage import utils.Constants import org.codehaus.groovy.grails.plugins.webdriver.WebDriverHelper import org.junit.Rule import org.openqa.selenium.support.ui.WebDriverWait

   [1]: http://grails.org/plugin/webtest
   [2]: http://grails.org/plugin/functional-test
   [3]: http://www.grails.org/plugin/geb
   [4]: http://grails.org/plugin/selenium-rc
   [5]: http://www.grails.org/plugin/webdriver
   [6]: http://seleniumhq.org/download/
   [7]: http://getfirebug.com/downloads
   [8]: http://www.springsource.org/springsource-tool-suite-download
   [9]: http://www.jetbrains.com/idea/
   [10]: http://xebee.xebia.in/wp-content/uploads/2012/06/Login_Screen-300x195.png (Image1: Login_Screen)
   [11]: http://xebee.xebia.in/wp-content/uploads/2012/06/Login_Test_IDE-300x228.png (Image2: Recorded Login Test )
   [12]: http://xebee.xebia.in/wp-content/uploads/2012/06/available-formats-300x225.png (Image3: Available Formats in Selenium IDE)

## Comments

**[Simmi Singhal](#9183 "2012-07-19 09:55:42"):** Hi Kanwalpreet, Thanks for your appreciation. Can you please elaborate more on what kind of text you want to fetch from a page after any CRUD operation. Is it a success message on top or... ?? Its not clear to me. But as per my understanding, you can use following driver's built-in function to get the text: String text = driver.findElement(By.xpath("XPath of the message text").getText(); You can use css Selector or Id or any of css locator with By. I hope this would serve your purpose.. :) Thanks :)

**[Kanwalpreet Singh](#9180 "2012-07-18 18:15:30"):** Hi , Nice tutorial,helped me alot.I am not able to get the message text from a page after doing create/update.It will be greate if you help me sort it out. Thanks in advance.

