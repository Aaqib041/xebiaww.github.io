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

<p>In <a href="http://xebee.xebia.in/2012/05/30/geb-groovy-browser-automation-tool-part-1/">Part-1</a> we have covered the installation &amp; usage of Geb, JQuery locators and how using Navigator API we can navigate to different elements of a web page. In part-2 we will see how using in-built Page Object classes of Geb we can leverage Groovy's DSL capabilities to easily define the interesting parts of our pages in a concise, maintainable manner. Along with Page object example we will also cover how easy it is to take snapshot of current’s page HTML and screenshot, screen scrapping and awesome value()/value(String text) method. So let’s start…
<!--more--></p>
<p><strong>Page Object Implementation</strong></p>
<p>Pages define their a) location, b) an “at checker” and c) content (among other things). When we define this information as part of the page it allows us to separate the implementation details from the intention. Let’s see an example. We will be creating 2 Page classes here:</p>
<p>[code]
import geb.Page</p>
<p>class HomePage extends Page{
    static url =  &quot;http://www.bloomberg.com&quot;
    static at  = { title == &quot;Bloomberg - Business, Financial &amp; Economic News, Stock Quotes&quot; }
    static content  = {
        moreTopNewsHeading {$(&quot;div#more_top &gt; h3 &gt; a&quot;)}
        opinionHeading {$(&quot;div#opinion_tetris &gt; h3 &gt; a&quot;)}
        personalFinanceHeading {$(&quot;div#personal_finance_tetris &gt;  h3 &gt; a&quot;)}
    }
}</p>
<p>import geb.Page</p>
<p>class NewsPage extends Page{
    static url =  &quot;news/&quot;
    static at  = { title == &quot;Top U.S &amp; International News Headlines - Bloomberg&quot; }
    static content  = {
        exclusiveNewsHeading {$(&quot;div#exclusive_news &gt; div &gt; h2 &gt; a&quot;)}
        worldwideNewsHeading {$(&quot;div#worldwide_news &gt; div &gt; h2 &gt; a&quot;)}
        regionNewsHeading {$(&quot;div#regions_news &gt; div &gt; h2 &gt; a&quot;)}
    }
}
[/code]</p>
<p><strong>Important Considerations:</strong>
<ol>
    <li>All page objects must inherit from geb.Page class.</li>
    <li>"At" Verification : At checkers are subject to “implicit assertions” that  check whether the underling browser is at the page that the page class actually represents. This is done via a static at closure</li>
    <li>Geb features a DSL for defining page content in a templated fashion, which allows very concise page definitions. Pages define a static closure property called content that describes the page content.</li>
</ol>
The structure to the content DSL is…</p>
<p>[code]
«name» { «definition» }
[/code]</p>
<p>Now let’s look at the script:</p>
<p>[code]
import geb.Browser</p>
<p>Browser.drive {
    to HomePage
    at HomePage</p>
<pre><code>assert moreTopNewsHeading.text() == &amp;quot;More Top News&amp;quot;
assert opinionHeading.text() == &amp;quot;Opinion&amp;quot;
assert personalFinanceHeading.text() == &amp;quot;Personal Finance&amp;quot;

to NewsPage
at NewsPage

assert exclusiveNewsHeading.text() == &amp;quot;Exclusive News&amp;quot;
assert worldwideNewsHeading.text() == &amp;quot;Worldwide News&amp;quot;
assert regionNewsHeading.text() == &amp;quot;Regions News&amp;quot;
</code></pre>
<p>}.quit()
[/code]</p>
<p>The “to” method utilizes the url property of class that navigates browser to that url. This method  does not verify that the browser ends up at the given type. So we need something else to assert whether we are on correct page.</p>
<p>“At” checker then verifies the condition(s) mentioned in the at closure of Page class. This is to ensure ‘fail-fast’ mechanism otherwise subsequent steps may fail because of the simple reason of browser not pointing to expected page.</p>
<p><strong>Reporting</strong></p>
<p>It is very easy to create interim reports with Geb. We just need to use “report &lt;<report_name>&gt;” command and it will take store the complete HTML and screenshot of current page and save those with report name.</report_name></p>
<p>[code]
Browser.drive(driver: new FirefoxDriver()) {
    config.reportsDir  = new File(&quot;reports/geb&quot;)
    go &quot;http://www.grails.org&quot;
    $(&quot;a&quot;, text:&quot;Plugins&quot;).click()
    waitFor {title: &quot;Grails - Plugins Portal&quot;}
    report &quot;Ex4&quot;</p>
<p>}.quit()
[/code]</p>
<p><strong>Screen Scrapping</strong></p>
<p>Geb can be used for screen scrapping. In case you just want to get some information from web pages and do not want to assert those, it is very much possible in Geb. Usually we need to log search results, left side menu items or top header items on multiple pages. Groovy's collect() method is very helpful in achieving what we want to achieve as it can be used to iterate over collections and transform each element of the collection. Lets see an example where we are capturing plug-in search results from grails.org site.</p>
<p>[code]
import geb.Browser
import org.openqa.selenium.firefox.FirefoxDriver</p>
<p>Browser.drive(driver: new FirefoxDriver()){
    go &quot;http://grails.org/plugins/&quot;</p>
<pre><code>$(&amp;quot;input&amp;quot;, name: &amp;quot;q&amp;quot;).value(&amp;quot;geb&amp;quot;)
$(&amp;quot;input&amp;quot;, class: &amp;quot;searchButton&amp;quot;).click()

waitFor(10){ $(&amp;quot;div&amp;quot;, class:'currentPlugin') }

def pluginNames = $(&amp;quot;div&amp;quot;, class:'currentPlugin').collect { div -&amp;gt;  div.find(&amp;quot;h4&amp;quot;).find(&amp;quot;a&amp;quot;).text()}

println pluginNames

def menuItems = $(&amp;quot;div.mainMenuBarWrapper &amp;gt; ul &amp;gt; li&amp;quot;).collect {li -&amp;gt; li.find(&amp;quot;a&amp;quot;).text() }
def pluginSorters = $(&amp;quot;ul#pluginSorters &amp;gt; li&amp;quot;).collect {li -&amp;gt; li.find(&amp;quot;a&amp;quot;).text()}

println menuItems
println pluginSorters
</code></pre>
<p>}.quit()
[/code]</p>
<p><strong>
Setting/getting value to/from different html elements via a single method </strong></p>
<p>Yes!! You read it right..We are going to see how using a single method you can set and get values to/from different html elements so now you need not to remember different ways to deal with different elements. Let's see an example:</p>
<p>[code]
import geb.Browser
import org.openqa.selenium.firefox.FirefoxDriver</p>
<p>Browser.drive(driver:  new FirefoxDriver()) {
    go &quot;http://www.olx.in/&quot;
    $(&quot;div#my_olx &gt; a&quot;, text:&quot;Register&quot;).click()
    waitFor(20) {$(&quot;div#register &gt; h1&quot;, text: &quot;Register&quot;)}</p>
<p>// TextBox
    $(&quot;input#username&quot;).value(&quot;xebiatesting&quot;)
    $(&quot;input#email&quot;).value(&quot;xebiatesting@gmail.com&quot;)
    $(&quot;input#password&quot;).value(&quot;xebiatesting&quot;)</p>
<p>//Radio
    $(&quot;input#identity_type_2&quot;).value(&quot;A Professional/Business&quot;)</p>
<p>// checkbox
     $(&quot;input#agree_terms&quot;).value(true)</p>
<p>// select</p>
<pre><code>go &amp;quot;http://www.quikr.com/Register&amp;quot;
$(&amp;quot;select#selectedCity&amp;quot;).value(&amp;quot;Noida&amp;quot;)
</code></pre>
<p>// multiple select</p>
<pre><code>go &amp;quot;http://www.w3schools.com/tags/tryit.asp?filename=tryhtml_select_multiple&amp;quot;
withFrame(&amp;quot;view&amp;quot;) {
    //$(&amp;quot;form&amp;quot;).cars = [&amp;quot;Saab&amp;quot;, &amp;quot;Audi&amp;quot;]
    $(&amp;quot;form&amp;quot;).cars = [&amp;quot;Saab&amp;quot;, &amp;quot;Audi&amp;quot;]
}
</code></pre>
<p>}
[/code]</p>
<p>In last and concluding part (Part 3) we will discuss about dealing with frame/windows, configuration and Geb's integration with different testing framework adapters like Spock, TestNG and JUnit.</p>

## Comments

**[marko](#9245 "2012-07-27 02:58:23"):** Great stuff. Looking forward to part 3.

**[Gaurav Bansal](#9247 "2012-07-27 08:27:09"):** Thanks Marko for liking the series.

