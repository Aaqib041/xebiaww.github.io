---
layout: post
header-img: img/default-blog-pic.jpg
author: jgupta
description: 
post_id: 17315
created: 2014/06/17 01:33:49
created_gmt: 2014/06/16 20:33:49
comment_status: open
---

# Spice up your Test Automation with Test Spicer

<p>In manual testing, we play with our application by running the same tests with different input data. This helps testers to increase test coverage and uncover unexpected bugs. This is not true for automation, where we usually hard code the data we wanted to test into our test function. This reduces the efficiency and effectiveness of our automation scripts which keep executing a scenario again and again with the same set of data. As a result, they rarely find bugs after few executions and bugs that are not in the script execution path or data that we are using in the automation. However, if we add randomly generated data, just like manual testing, we increase our chances of finding new bugs or interesting information with every execution. <strong>Test Spicer</strong> helps to add this spice to our automation.
<!--more--></p>
<p><strong>Test Spicer</strong>
Test Spicer provides a kind of utility to generate random data and makes it easier to explore something new in every test run. It helps to improve the sample data size as its every execution is different than the earlier ones. It also improves the probability of finding defects by using randomly generated data in every run. It leads the project on the path of Effective Testing.</p>
<p><strong>Why? </strong>
Test Automation Scripts with Varied set of data means Effective Testing</p>
<p><strong>What it does?</strong>
Test Spicer is a restful web service. To use TestSpicer, a user needs to sign up on <a title="testspicer" href="http://www.testspicer.com" target="_blank">www.testspicer.com</a> website. The user is then receives an API key. User can then call the appropriate API depending on his requirement. Test Spicer returns the data in either a JSON or XML format. This data can then be used for testing purpose.</p>
<p><strong>Features:</strong>
<ul>
    <li>A restful interface</li>
    <li>Quickly generate random test data</li>
</ul>
<strong>Setting up </strong>
Let's write our first test now... Here we are using Selenium Webdriver with Java. We can use Test Spicer with any platform, tool or language. In this test we have chosen <a title="Xebia Contact Us" href="http://www.xebia.in/contact-us.html" target="_blank">Xebia Contact Us</a> form as the test site. The contact us form contains different input fields that accept different kinds of data. Eg: Name field accepts string, Phone field accepts numbers, Email field accepts email and Message field accepts a long paragraph. We want the automation script to enter a different set of data every time. This can be done by first writing code to call the <a title="Test Spicer RESTful API" href="http://testspicer.com/docs" target="_blank">Test Spicer RESTful API</a>.</p>
<p><strong>Writing code to call the API</strong></p>
<p>[code language="java"]
public class JavaNetURLRESTFulClient {
    String targetURL;
    Map map;</p>
<pre><code>public String getTargetURL(String input){
    if(input.equals(&amp;quot;alphabetString&amp;quot;))
        targetURL = &amp;quot;http://testspicer.com/restapi/v0.1/string/en/10&amp;quot;;
    else if (input.equals(&amp;quot;phone&amp;quot;))
        targetURL = &amp;quot;http://testspicer.com/restapi/v0.1/telephone?countrycode=UK&amp;quot;;
    else if(input.equals(&amp;quot;email&amp;quot;))
        targetURL = &amp;quot;http://testspicer.com/restapi/v0.1/email?domain=com&amp;amp;length=20&amp;quot;;
    else if(input.equals(&amp;quot;para&amp;quot;))
        targetURL = &amp;quot;http://testspicer.com/restapi/v0.1/lorem?language=en&amp;quot;;
    return targetURL;
}

public Map getRandomData(String input){
    getTargetURL(input);
    try {

        URL restServiceURL = new URL(targetURL);

        HttpURLConnection httpConnection = (HttpURLConnection) restServiceURL.openConnection();
        httpConnection.setRequestMethod(&amp;quot;GET&amp;quot;);
        httpConnection.setRequestProperty(&amp;quot;Content-Type&amp;quot;,&amp;quot;application/json&amp;quot;);
        httpConnection.setRequestProperty(&amp;quot;api_key&amp;quot;,&amp;quot;64c20419f2e64b37f123939ba047183v&amp;quot;);

        if (httpConnection.getResponseCode() != 200) {
            throw new RuntimeException(&amp;quot;HTTP GET Request Failed with Error code : &amp;quot;
                    + httpConnection.getResponseCode());
        }

        map = new Gson().fromJson(IOUtils.toString(httpConnection.getInputStream(),&amp;quot;UTF-8&amp;quot;), Map.class);
        httpConnection.disconnect();
    } catch (MalformedURLException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    }
    return map;
}
</code></pre>
<p>}
[/code]</p>
<p><strong>How it works:</strong>
In the above code, we issue a GET request to the end point like “string/en/10” and get 10 character long string. If we want a different length, we just need to modify the length to the desired value.</p>
<p><strong>Writing Selenium Webdriver Test </strong></p>
<p>[code language="java"]
public class ContactUsTest {
    public static void main (String[] args) throws InterruptedException{
        Map map;</p>
<pre><code>    WebDriver driver = new FirefoxDriver();
    driver.get(&amp;quot;http://www.xebia.in/contact-us.html&amp;quot;);

    WebElement name = driver.findElement(By.id(&amp;quot;name&amp;quot;));
    WebElement contact_no = driver.findElement(By.id(&amp;quot;phone&amp;quot;));
    WebElement email = driver.findElement(By.id(&amp;quot;email&amp;quot;));
    WebElement message = driver.findElement(By.id(&amp;quot;message&amp;quot;));

    JavaNetURLRESTFulClient obj = new JavaNetURLRESTFulClient();

    map = obj.getRandomData(&amp;quot;alphabetString&amp;quot;);
    name.sendKeys((String)map.get(&amp;quot;string&amp;quot;));

    map = obj.getRandomData(&amp;quot;phone&amp;quot;);
    contact_no.sendKeys((String)map.get(&amp;quot;number&amp;quot;));

    map = obj.getRandomData(&amp;quot;email&amp;quot;);
    email.sendKeys((String)map.get(&amp;quot;email&amp;quot;));

    map = obj.getRandomData(&amp;quot;para&amp;quot;);
    message.sendKeys((String)map.get(&amp;quot;lorem&amp;quot;));

    driver.quit();
}
</code></pre>
<p>}
[/code]</p>
<p><strong>Steps to run the tests </strong>
We just run our selenium test as a Java application. It should spawn a browser and start executing the test with a given set of data. In the second run, it should type a different set of data.</p>
<p><strong>You should get something like this:</strong>
<a href="http://xebee.xebia.in/wp-content/uploads/2014/06/Xebia_Contact_Us.png"><img class="alignnone size-full wp-image-18410" alt="Xebia_Contact_Us" src="http://xebee.xebia.in/wp-content/uploads/2014/06/Xebia_Contact_Us.png" width="1048" height="595" /></a></p>
<p><strong>Advantages:</strong>
<ul>
    <li>No need to write code to generate random data</li>
    <li>Increase coverage of tests</li>
</ul>
TestSpicer helps to provide test ideas to make sure that apparent things related to quality are never missed.</p>
<p><strong>Similar tools in the market</strong>
<a title="selite" href="https://code.google.com/p/selite/wiki/ProjectHome" target="_blank">selite</a></p>
<p><strong>References </strong>
<a title="testspicer.com" href="www.testspicer.com" target="_blank">http://testspicer.com/</a></p>