---
layout: post
header-img: img/default-blog-pic.jpg
author: gbansal
description: 
post_id: 14792
created: 2012/07/24 18:15:42
created_gmt: 2012/07/24 13:15:42
comment_status: open
---

# Geb: Groovy Browser Automation Tool – Part 2

In [Part-1][1] we have covered the installation & usage of Geb, JQuery locators and how using Navigator API we can navigate to different elements of a web page. In part-2 we will see how using in-built Page Object classes of Geb we can leverage Groovy's DSL capabilities to easily define the interesting parts of our pages in a concise, maintainable manner. Along with Page object example we will also cover how easy it is to take snapshot of current’s page HTML and screenshot, screen scrapping and awesome value()/value(String text) method. So let’s start… 

**Page Object Implementation**

Pages define their a) location, b) an “at checker” and c) content (among other things). When we define this information as part of the page it allows us to separate the implementation details from the intention. Let’s see an example. We will be creating 2 Page classes here:

[code] import geb.Page

class HomePage extends Page{ static url = "http://www.bloomberg.com" static at = { title == "Bloomberg - Business, Financial & Economic News, Stock Quotes" } static content = { moreTopNewsHeading {$("div#more_top > h3 > a")} opinionHeading {$("div#opinion_tetris > h3 > a")} personalFinanceHeading {$("div#personal_finance_tetris > h3 > a")} } }

import geb.Page

class NewsPage extends Page{ static url = "news/" static at = { title == "Top U.S & International News Headlines - Bloomberg" } static content = { exclusiveNewsHeading {$("div#exclusive_news > div > h2 > a")} worldwideNewsHeading {$("div#worldwide_news > div > h2 > a")} regionNewsHeading {$("div#regions_news > div > h2 > a")} } } [/code]

**Important Considerations:**

  1. All page objects must inherit from geb.Page class.
  2. "At" Verification : At checkers are subject to “implicit assertions” that check whether the underling browser is at the page that the page class actually represents. This is done via a static at closure
  3. Geb features a DSL for defining page content in a templated fashion, which allows very concise page definitions. Pages define a static closure property called content that describes the page content.
The structure to the content DSL is…

[code] «name» { «definition» } [/code]

Now let’s look at the script:

[code] import geb.Browser

Browser.drive { to HomePage at HomePage
    
    
    assert moreTopNewsHeading.text() == &quot;More Top News&quot;
    assert opinionHeading.text() == &quot;Opinion&quot;
    assert personalFinanceHeading.text() == &quot;Personal Finance&quot;
    
    to NewsPage
    at NewsPage
    
    assert exclusiveNewsHeading.text() == &quot;Exclusive News&quot;
    assert worldwideNewsHeading.text() == &quot;Worldwide News&quot;
    assert regionNewsHeading.text() == &quot;Regions News&quot;
    

}.quit() [/code]

The “to” method utilizes the url property of class that navigates browser to that url. This method does not verify that the browser ends up at the given type. So we need something else to assert whether we are on correct page.

“At” checker then verifies the condition(s) mentioned in the at closure of Page class. This is to ensure ‘fail-fast’ mechanism otherwise subsequent steps may fail because of the simple reason of browser not pointing to expected page.

**Reporting**

It is very easy to create interim reports with Geb. We just need to use “report <>” command and it will take store the complete HTML and screenshot of current page and save those with report name.

[code] Browser.drive(driver: new FirefoxDriver()) { config.reportsDir = new File("reports/geb") go "http://www.grails.org" $("a", text:"Plugins").click() waitFor {title: "Grails - Plugins Portal"} report "Ex4"

}.quit() [/code]

**Screen Scrapping**

Geb can be used for screen scrapping. In case you just want to get some information from web pages and do not want to assert those, it is very much possible in Geb. Usually we need to log search results, left side menu items or top header items on multiple pages. Groovy's collect() method is very helpful in achieving what we want to achieve as it can be used to iterate over collections and transform each element of the collection. Lets see an example where we are capturing plug-in search results from grails.org site.

[code] import geb.Browser import org.openqa.selenium.firefox.FirefoxDriver

Browser.drive(driver: new FirefoxDriver()){ go "http://grails.org/plugins/"
    
    
    $(&quot;input&quot;, name: &quot;q&quot;).value(&quot;geb&quot;)
    $(&quot;input&quot;, class: &quot;searchButton&quot;).click()
    
    waitFor(10){ $(&quot;div&quot;, class:'currentPlugin') }
    
    def pluginNames = $(&quot;div&quot;, class:'currentPlugin').collect { div -&gt;  div.find(&quot;h4&quot;).find(&quot;a&quot;).text()}
    
    println pluginNames
    
    def menuItems = $(&quot;div.mainMenuBarWrapper &gt; ul &gt; li&quot;).collect {li -&gt; li.find(&quot;a&quot;).text() }
    def pluginSorters = $(&quot;ul#pluginSorters &gt; li&quot;).collect {li -&gt; li.find(&quot;a&quot;).text()}
    
    println menuItems
    println pluginSorters
    

}.quit() [/code]

** Setting/getting value to/from different html elements via a single method **

Yes!! You read it right..We are going to see how using a single method you can set and get values to/from different html elements so now you need not to remember different ways to deal with different elements. Let's see an example:

[code] import geb.Browser import org.openqa.selenium.firefox.FirefoxDriver

Browser.drive(driver: new FirefoxDriver()) { go "http://www.olx.in/" $("div#my_olx > a", text:"Register").click() waitFor(20) {$("div#register > h1", text: "Register")}

// TextBox $("input#username").value("xebiatesting") $("input#email").value("xebiatesting@gmail.com") $("input#password").value("xebiatesting")

//Radio $("input#identity_type_2").value("A Professional/Business")

// checkbox $("input#agree_terms").value(true)

// select
    
    
    go &quot;http://www.quikr.com/Register&quot;
    $(&quot;select#selectedCity&quot;).value(&quot;Noida&quot;)
    

// multiple select
    
    
    go &quot;http://www.w3schools.com/tags/tryit.asp?filename=tryhtml_select_multiple&quot;
    withFrame(&quot;view&quot;) {
        //$(&quot;form&quot;).cars = [&quot;Saab&quot;, &quot;Audi&quot;]
        $(&quot;form&quot;).cars = [&quot;Saab&quot;, &quot;Audi&quot;]
    }
    

} [/code]

In last and concluding part (Part 3) we will discuss about dealing with frame/windows, configuration and Geb's integration with different testing framework adapters like Spock, TestNG and JUnit.

   [1]: http://xebee.xebia.in/2012/05/30/geb-groovy-browser-automation-tool-part-1/

## Comments

**[marko](#9245 "2012-07-27 02:58:23"):** Great stuff. Looking forward to part 3.

**[Gaurav Bansal](#9247 "2012-07-27 08:27:09"):** Thanks Marko for liking the series.

