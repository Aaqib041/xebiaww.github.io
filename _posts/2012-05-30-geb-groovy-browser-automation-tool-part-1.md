---
layout: post
header-img: img/default-blog-pic.jpg
author: gbansal
description: 
post_id: 14237
created: 2012/05/30 13:09:47
created_gmt: 2012/05/30 08:09:47
comment_status: open
---

# Geb: Groovy Browser Automation Tool - Part 1

<p>There are many open source testing tools that claim to be very easy to use but they are NOT. However this tool, Geb (pronounced “jeb”) is really very easy as far as learning and usage is concerned. Tool has implemented most of the best known <a target="_blank" title="strategies and patterns for Web GUI automation" href="http://xebee.xebia.in/2012/03/30/web-gui-automation-tools-strategies-patterns/">strategies and patterns for Web GUI automation</a> and the <a href="http://www.gebish.org/manual/0.7.0/">documentation</a> is just fantastic.</p>
<p>Test script creation is brought to a new level thanks to a number of features – Power of WebDriver, elegance of jQuery content selection, robustness of Page Object modelling and the expressiveness of the Groovy language. Awesome combination!!
<!--more-->
Now let’s see how a Geb program looks like:</p>
<p>[code]
import geb.Browser
import org.openqa.selenium.firefox.FirefoxDriver</p>
<p>Browser.drive(new Browser(driver: new FirefoxDriver())) {
    go &quot;http://www.twitter.com&quot;
    assert title == &quot;Twitter&quot;
    }
}.quit()
[/code]</p>
<p>There is a Browser class which has static method called drive(). In case we do not specify any parameter to drive() method then it uses HTMLUnit driver and runs the test cases. Otherwise as shown in the example we can pass required parameter to initialize instance of any of FirefoxDriver, InternetExplorerdriver, ChromeDriver and OperaDriver.</p>
<p>Functional testing of web applications has always been a challenge. Tests are expensive to write, painful to read, and relatively hard to maintain.
Geb changed all of that. Geb’s Page Objects and Groovy DSL make tests readable to the point that they’re almost plain English. Beauty of Geb is that it can be integrated with many testing frameworks like JUnit, TestNG, Spock, Cucumber etc. In Part-1 we are covering JQuery locators, how using Navigator object we can navigate to all elements of a web page and the installation &amp; usage of Geb.</p>
<p><strong>JQuery Locators</strong></p>
<p>As the name implies, jQuery focuses on queries. The core of the library allows you to find DOM elements using CSS selector syntax and run methods on that collection. jQuery offers a powerful set of tools for matching a set of elements in a document.</p>
<p>There is a dollar function that can be used anywhere to select content based on CSS selectors, attribute matchers and/or indexes. This $ function returns an object of Navigator class. A Navigator object is in someways analogous to the jQuery data type that represents one or more targeted elements on the page.</p>
<p>Lets take an example from www.twitter.com. Following is the screenshot and inner HTML of Sign Up form on twitter:</p>
<p style="text-align: left;"><a href="http://xebee.xebia.in/2012/05/30/geb-groovy-browser-automation-tool-part-1/twitter-2/" rel="attachment wp-att-14251"><img src="http://xebee.xebia.in/wp-content/uploads/2012/05/Twitter.png" alt="Twitter Sign Up Form" title="Twitter_SignUp" class="size-full wp-image-14251 alignleft" height="218" width="311" /></a></p>

<p><br>
<br>
<br>
<br>
<br>
<br>
<br></p>
<p>[code]
&lt;form action=&quot;https://twitter.com/signup&quot; class=&quot;signup&quot; method=&quot;post&quot;&gt;</p>
<p>&lt;div class=&quot;placeholding-input&quot;&gt;
    &lt;input class=&quot;text-input&quot; autocomplete=&quot;off&quot; name=&quot;user[name]&quot; maxlength=&quot;20&quot; type=&quot;text&quot; /&gt;
    &lt;span class=&quot;placeholder&quot;&gt;Full name&lt;/span&gt;&lt;/div&gt;</p>
<p>&lt;div class=&quot;placeholding-input&quot;&gt;
    &lt;input class=&quot;text-input email-input&quot; autocomplete=&quot;off&quot; name=&quot;user[email]&quot; type=&quot;text&quot; /&gt;
    &lt;span class=&quot;placeholder&quot;&gt;Email&lt;/span&gt;&lt;/div&gt;</p>
<p>&lt;div class=&quot;placeholding-input&quot;&gt;
    &lt;input class=&quot;text-input&quot; name=&quot;user[user_password]&quot; type=&quot;password&quot; /&gt;
    &lt;span class=&quot;placeholder&quot;&gt;Password&lt;/span&gt;&lt;/div&gt;</p>
<p>&lt;input value=&quot;front&quot; name=&quot;context&quot; type=&quot;hidden&quot; /&gt;
&lt;input value=&quot;c2bb63f5839426608c7c5e4610f08b576def8c3c&quot; name=&quot;authenticity_token&quot; type=&quot;hidden&quot; /&gt;</p>
<p>&lt;button type=&quot;submit&quot; class=&quot;btn signup-btn&quot;&gt;
  Sign up for Twitter
&lt;/button&gt;</p>
<p>&lt;/form&gt;
[/code]</p>
<p>Below are some sample JQuery locators that we can use to access html elements:</p>
<p>[code]</p>
<p>// match all 'div' elements on the page
$(&quot;div&quot;)</p>
<p>// match the first 'div' element on the page which is related to 'Full name'
$(&quot;div&quot;, 0)</p>
<p>// match all 'div' elements who have the class 'placeholding-input'</p>
<p>$(&quot;div.placeholding-input&quot;)</p>
<p>// match the second 'div' element with the class 'placeholding-input'</p>
<p>$(&quot;div.placeholding-input&quot;, 1)</p>
<p>// match the input element with a type attribute of value 'password'</p>
<p>$(&quot;input&quot;, type: &quot;password&quot;)</p>
<p>// The parent of the first input</p>
<p>$(&quot;input&quot;, 0).parent()</p>
<p>// All inputs that are nested in a form</p>
<p>$(&quot;form&quot;).find(&quot;input&quot;)</p>
<p>// Please note that above-mentioned methods return Navigator objects that can be used to further refine the content.
[/code]</p>
<p><strong>Navigating the content</strong></p>
<p>Let’s see how we can traverse this html:</p>
<p>[code]
import geb.Browser
import geb.navigator.Navigator
import geb.navigator.NonEmptyNavigator;
import geb.spock.*
import org.openqa.selenium.WebElement
import org.openqa.selenium.firefox.FirefoxDriver</p>
<p>Browser.drive(new Browser(driver: new FirefoxDriver())) {
    go &quot;http://www.twitter.com&quot;
    assert title == &quot;Twitter&quot;</p>
<pre><code>// Navigator is a jQuery-style DOM traversal tool that wraps a set of WebDriver WebElements. Creates a new Navigator instance containing the elements matching the given CSS selector.
Navigator allDivs = $(&amp;quot;form.signup &amp;gt; div&amp;quot;)

// Prints the size of list
println allDivs.size()

// Code to traverse from div to its children then getting WebElement(s) from Navigator object and printing the tag name of element
for(Navigator div : allDivs) {
    Navigator children = div.children()
    for(Navigator child: children) {
        WebElement element = child.getElement(0);
        if(element.getTagName() == &amp;quot;span&amp;quot;) {
            println element.getText()
        } else {
            println element.getTagName()
        }
    }
}
</code></pre>
<p>}.quit()
[/code]</p>
<p>In this example, we have created an instance of Navigator class containing the elements matching the given CSS selector. Now using this Navigator class we traversed from parent to its child Navigator objects to get required WebElement and then printed element’s tag name. See how easy it is and you have complete access each and every html element using Navigator class.  You might be tempting to try your hands on Geb and for that here are the set-up instructions:
<ol>
    <li>Install Groovy from <a href="http://dist.codehaus.org/groovy/distributions/installers/windows/nsis/groovy-1.8.6-installer.exe">http://dist.codehaus.org/groovy/distributions/installers/windows/nsis/groovy-1.8.6-installer.exe</a>  (There is currently an issue where you cannot have spaces in the path where Groovy is installed under windows.  So, instead of accepting the default installation path of "c:\Program Files\Groovy" you will want to change the path to something like "c:\Groovy")</li>
    <li>Install Groovy Plug-in in eclipse from <a href="http://groovy.codehaus.org/Eclipse+Plugin">http://groovy.codehaus.org/Eclipse+Plugin</a></li>
    <li>Enable the following option: Preferences-&gt;Groovy-&gt;Use monospace font for JUnit. This is important for Spock's condition output to be aligned correctly. If this doesn't work immediately, uncheck the option, press 'Apply', and check the option again.</li>
    <li>Download geb-core from <a href="http://repo1.maven.org/maven2/org/codehaus/geb/geb-core/0.7.0/geb-core-0.7.0.jar">http://repo1.maven.org/maven2/org/codehaus/geb/geb-core/0.7.0/geb-core-0.7.0.jar</a></li></p>

## Comments

**[Gaurav Bansal](#9064 "2012-06-18 21:25:24"):** Hi Jaya, Thanks for your nice comment and good question. Geb is a Groovy based tool and we know Groovy and Java are really close cousins so you can definitely run all of your Java code using Geb however to leverage Groovy's awesome feature like its neat shortcut notations and its sensible defaults using which you can remove lot of boiler plate java code, you should start refactoring your code based on Geb's feature set. Regards, Gaurav

**[Jaya](#8972 "2012-06-06 12:31:15"):** Hi Gaurav, You have explained "Geb" tool very nicely in your blog. I am new to this tool and I have a question. I have my automation test suites written in Webdriver using java. Is it possible to migrate these tests to Geb? Thanks.

**[sonalgarg](#9423 "2013-05-30 12:06:43"):** Hi Gaurav,   I am new to Geb. I want to use selenium webdriver with geb. Please tell me selenium2 library for this.   Thanks in Advance

