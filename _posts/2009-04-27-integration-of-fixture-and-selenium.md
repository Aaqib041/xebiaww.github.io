---
layout: post
header-img: img/default-blog-pic.jpg
author: hgupta
description: 
post_id: 1449
created: 2009/04/27 11:15:14
created_gmt: 2009/04/27 10:15:14
comment_status: open
---

# Integration of Fixture and Selenium

<p>Traditionally acceptance test was difficult to integrate with web. Now with combination of fitnesse and selenium you can do the integration easily.</p>
<!--more-->

<p>Selenium is software testing tool for web application. It can run on all platforms and has got client drivers for Python, Ruby, .NET, Perl, Java, and PHP.</p>
<p>FitNesse is acceptance testing tool which allows test cases to be written is english language. It was initially written in java but its now also available in C++, Python, Ruby, Delphi, C#</p>
<p><strong>Installing Selenium</strong>
download the selenium RC server from http://selenium-rc.seleniumhq.org.
To run the server use command 'java -jar selenium-server.jar'.
This will run selenium server on port 4444, the port can be changed by -port option during startup of selenium server.</p>
<p><strong>Selenium Fixture class</strong>
You need to create a class say SeleniumFixture which extends the DoFixture class for creating the custom fixture.
<pre lang="java">public class SeleniumFixture extends DoFixture {</p>
<pre><code>....
....
public boolean startBrowser() {
    if (browser == null) {
        browser = new DefaultSelenium("localhost", 4444, "*iexplore", "http://localhost:8080/");
        browser.start();
    }
    return true;
</code></pre>
<p>}</pre>
This custom class will create instance of DefaultSelenium which is  selenium interface reference. Next you need to expose methods of DefaultSelenium as following.
<pre lang="java">public boolean isTextPresent(String text) {
        return browser.isTextPresent(text);
    }</pre>
<strong>Exposing class methods to fitnesse wiki</strong>
You can call the above methods from fitnesse wiki as following.
<code>|is text|put your text|present|
</code>
This will integrate the selenium with fitnesse.</p>
<p><strong>Example code</strong>
Lets take an example walkthrough of integrating selenium with fitnesse.</p>
<p>Following is code of fitnesse test case that checks for remember me checkbox functionality at login page. Once the checkbox is checked the use name is remembered so next time login there is no need to fill the user name</p>
<p><code>!|com.xebia.xtime.fitnesse.selenium.SeleniumFixture|
|start browser|*iexplore|
|go to url|http://localhost:8080/xtime/|and wait|60000|
|write|ywoe|to text field|j_username|
|write|tomcat|to text field|j_password|
|click|rememberMe|
|click |//input[@name='login']|and wait|60000|
|click|link=Logout| and wait|60000|
|validate|ywoe|in field|j_username|
|write|tomcat|to text field|j_password|
|click |//input[@name='login']| and wait|60000|
|is text|Logged in as: Yuk Woe|present|
|is text|Time Sheet (Create / Update)|present|
</code></p>
<p>Above example code uses methods in SeleniumFixture class as startBrowser, goToUrlAndWait, writeToTextField, click, clickAndWait, validateInField and isTextPresent.</p>
<p>Selenium Fixture code of above example.
<pre lang="java">public class SeleniumFixture extends DoFixture {</p>
<pre><code>   ...
   ...
   ...
public boolean startBrowser(String browserStartCommand) {
    if (browser == null) {
        browser = new DefaultSelenium("localhost", 4444, browserStartCommand, "http://localhost:8080/");
        browser.start();
    }
    return true;
}

public boolean goToUrlAndWait(String url, String waitTime) {
    browser.open(url);
    browser.waitForPageToLoad(waitTime);
    return true;
}
</code></pre>
<p>public boolean writeToTextField(String text, String field) {
        browser.type(field, text);
        return true;
    }</p>
<p>public boolean click(String link) {
        browser.click(link);
        return true;
    }</p>
<pre><code>public boolean clickAndWait(String link, String waitTime) {
    browser.click(link);
    browser.waitForPageToLoad(waitTime);
    return true;
}
</code></pre>
<p>public boolean validateInField(String value, String field) {
        String thisValue = browser.getValue(field);
        if (thisValue.equals(value)) {
            return true;
        } else {
            return false;
        }
    }</p>
<p>public boolean isTextPresent(String text) {
        return browser.isTextPresent(text);
    }
}</pre>
When you integate and run the fitnesse it will open a new browser which is specified in startBrowser method call.
<a href="http://xebee.xebia.in/wp-content/uploads/2009/04/selenium-execution.jpg">
<img style="VERTICAL-ALIGN: middle" src="http://xebee.xebia.in/wp-content/uploads/2009/04/selenium-execution.jpg" alt="" width="240" height="240" /></a></p>
<p><strong>Conclusion</strong>
We have talked about how to integrate the fitnesse with selenium. We covered the changes that needs to be done to create a java class which wraps around DefaultSelenium object and we talked about the way the methods created in java class are exposed to fitnesse world. Towards the end we have taken a real example to understand the integration better.</p>