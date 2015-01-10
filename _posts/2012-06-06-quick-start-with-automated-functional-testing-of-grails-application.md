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

<h3><strong>1. Introduction</strong></h3>

<p>There are many plug-ins available by which one can automate functional tests for grails application, notably <a href="http://grails.org/plugin/webtest">webtest</a>, <a href="http://grails.org/plugin/functional-test">functional-test</a>, <a href="http://www.grails.org/plugin/geb">geb</a>, <a href="http://grails.org/plugin/selenium-rc">Selenium-RC </a>and <a href="http://www.grails.org/plugin/webdriver">Webdriver</a>. Though we haven’t tried all, but explored grails-webdriver plug-in and used for automation.</p>
<p><strong>Why Grails-Webdriver plug-in? And the reasons are:</strong></p>
<p>• Easy Grails-Webdriver integration helps to quick start with functional testing of web application
• No need to learn any specific tool, one must have some knowledge of webdriver and programming language (Java or Groovy)
• Support for Page Object Pattern in Webdriver to implement high-level abstraction and to hide HTML details
• Ability to run individual (or multiple) tests directly through IDE like any other JUnit test.
• Ability to run functional tests in both HtmlUnit (for speed) and real web browsers like Firefox, IE and Chrome.
• Auto Reporting in grails after test execution.
<!--more-->
<h3><strong>2.  Prerequisites</strong></h3>
<strong>Tools required:</strong></p>
<p>• <a href="http://seleniumhq.org/download/">Selenium IDE</a>: Selenium IDE is used to record the test script quickly.
• <a href="http://getfirebug.com/downloads">Firebug</a>
• IDE as <a href="http://www.springsource.org/springsource-tool-suite-download">SpringSourceToolSuite</a> or <a href="http://www.jetbrains.com/idea/">IntelliJ</a>: IDE is used to maintain and write test scripts in a structured way.</p>
<p><strong>Install Plug-in:</strong> It is very easy to install webdriver plug-in in the grails main project or separate test project via IDE or command line by using following command:</p>
<p><strong>grails install-plugin webdriver</strong></p>
<p>Once the plug-in is installed, we have a separate functional folder along with unit and integrate folder in test folder. Now we are ready to go for automated functional tests.
<h3><strong>3. Getting Started</strong></h3>
Consider a Test Scenario, <strong>“To login in the application successfully.”</strong></p>
<p><a href="http://xebee.xebia.in/2012/06/06/quick-start-with-automated-functional-testing-of-grails-application/login_screen/" rel="attachment wp-att-14348"><img src="http://xebee.xebia.in/wp-content/uploads/2012/06/Login_Screen-300x195.png" title="Image1: Login_Screen" class="size-medium wp-image-14348 alignleft" height="195" width="300" /></a></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>Step 1: Install Selenium IDE Plug-in in Firefox and start recording test script to enter login credentials and to click Login button.</strong></p>
<p><a href="http://xebee.xebia.in/2012/06/06/quick-start-with-automated-functional-testing-of-grails-application/login_test_ide/" rel="attachment wp-att-14357"><img src="http://xebee.xebia.in/wp-content/uploads/2012/06/Login_Test_IDE-300x228.png" title="Image2: Recorded Login Test " class="size-medium wp-image-14357 alignleft" height="228" width="300" /></a></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>Step 2: Export recorded test script into available format:</strong> Selenium IDE provides various formats notably, JUnit 4 (Webdriver); JUnit 3 (RC); JUnit 4(RC); and so on. Once login script is recorded, we exported the script to JUnit 4 (Webdriver) format.</p>
<p><a href="http://xebee.xebia.in/2012/06/06/quick-start-with-automated-functional-testing-of-grails-application/available-formats/" rel="attachment wp-att-14363"><img src="http://xebee.xebia.in/wp-content/uploads/2012/06/available-formats-300x225.png" title="Image3: Available Formats in Selenium IDE" class="size-medium wp-image-14363 alignleft" height="225" width="300" /></a></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>
</strong></p>
<p><strong>Step 3: Create test class as test/functional/tests/LoginTest.groovy to paste exported test script.</strong></p>
<p>[sourcecode type="java"]
import org.junit.<em>;
import static org.junit.Assert.</em>;
import org.openqa.selenium.*;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.support.ui.Select;</p>
<p>public class LoginTest {</p>
<pre><code>private WebDriver driver;
private String baseUrl;

@Before
 public void setUp() throws Exception {

    driver = new FirefoxDriver();
    baseUrl = &amp;quot;http://localhost:8080/&amp;quot;;
    driver.manage().timeouts().implicitlyWait(30, TimeUnit.SECONDS);
}
</code></pre>
<p>@Test
    public void testLogin() throws Exception {</p>
<pre><code>    driver.get(baseUrl + &amp;quot;/login/auth&amp;quot;);
    assertEquals(&amp;quot;Test Application&amp;quot;, driver.getTitle());
    driver.findElement(By.id(&amp;quot;username&amp;quot;)).clear();
    driver.findElement(By.id(&amp;quot;username&amp;quot;)).sendKeys(&amp;quot;admin&amp;quot;);
    driver.findElement(By.id(&amp;quot;password&amp;quot;)).clear();
    driver.findElement(By.id(&amp;quot;password&amp;quot;)).sendKeys(&amp;quot;admin&amp;quot;);
    driver.findElement(By.cssSelector(&amp;quot;button.cta_button&amp;quot;)).click();
}

@After
public void tearDown() throws Exception {

    driver.quit();
}
</code></pre>
<p>}
[/sourcecode]</p>
<p><strong>
</strong></p>
<p><strong>Step4: Create Login Page Object class as test/functional/pages/LoginPage.groovy</strong></p>
<p>[sourcecode type="java"]
import org.openqa.selenium.*
import org.codehaus.groovy.grails.plugins.webdriver.WebDriverHelper</p>
<p>class LoginPage {</p>
<pre><code>// Login Page constants
final static String LOGIN_PAGE_URL = &amp;quot;/login/auth&amp;quot;;
final static String LOGIN_BUTTON_CSS = &amp;quot;button.cta_button&amp;quot;;
final static String USERNAME_ID = &amp;quot;username&amp;quot;;
final static String PASSWORD_ID = &amp;quot;password&amp;quot;;
final static String REMEMBER_ME_ID = &amp;quot;remember_me&amp;quot;;
final static String LOGIN_BUTTON_TEXT = &amp;quot;Login&amp;quot;;
final static String FORGOT_PASSWORD_TEXT = &amp;quot;Forgot Password&amp;quot;;

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
</code></pre>
<p>}
[/sourcecode]</p>
<p>&nbsp;</p>
<p>Here the LoginPage Class contains both actions implemented as methods (example enterLoginDetails(), login()) and web elements(like button, text fields, checkbox), which are themselves separate objects. Also If in future there is some change in the HTML of Login Page (for example, change the ID of an element, or add an extra div for example) we only have to modify a specific part of our LoginPage object and not every test that interacted with that element.</p>
<p><strong>
</strong></p>
<p><strong>Step5: Add assertions and modify test script:</strong> Look at the modified test script class “test/functional/tests/LoginTest.groovy “.</p>
<p>[sourcecode type="java"]
import org.openqa.selenium.*
import pages.LoginPage
import utils.Constants
import org.codehaus.groovy.grails.plugins.webdriver.WebDriverHelper
import org.junit.Rule
import org.openqa.selenium.support.ui.WebDriverWait</p>

## Comments

**[Simmi Singhal](#9183 "2012-07-19 09:55:42"):** Hi Kanwalpreet, Thanks for your appreciation. Can you please elaborate more on what kind of text you want to fetch from a page after any CRUD operation. Is it a success message on top or... ?? Its not clear to me. But as per my understanding, you can use following driver's built-in function to get the text: String text = driver.findElement(By.xpath("XPath of the message text").getText(); You can use css Selector or Id or any of css locator with By. I hope this would serve your purpose.. :) Thanks :)

**[Kanwalpreet Singh](#9180 "2012-07-18 18:15:30"):** Hi , Nice tutorial,helped me alot.I am not able to get the message text from a page after doing create/update.It will be greate if you help me sort it out. Thanks in advance.

