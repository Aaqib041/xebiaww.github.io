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

In manual testing, we play with our application by running the same tests with different input data. This helps testers to increase test coverage and uncover unexpected bugs. This is not true for automation, where we usually hard code the data we wanted to test into our test function. This reduces the efficiency and effectiveness of our automation scripts which keep executing a scenario again and again with the same set of data. As a result, they rarely find bugs after few executions and bugs that are not in the script execution path or data that we are using in the automation. However, if we add randomly generated data, just like manual testing, we increase our chances of finding new bugs or interesting information with every execution. **Test Spicer** helps to add this spice to our automation. 

**Test Spicer** Test Spicer provides a kind of utility to generate random data and makes it easier to explore something new in every test run. It helps to improve the sample data size as its every execution is different than the earlier ones. It also improves the probability of finding defects by using randomly generated data in every run. It leads the project on the path of Effective Testing.

**Why? ** Test Automation Scripts with Varied set of data means Effective Testing

**What it does?** Test Spicer is a restful web service. To use TestSpicer, a user needs to sign up on [www.testspicer.com][1] website. The user is then receives an API key. User can then call the appropriate API depending on his requirement. Test Spicer returns the data in either a JSON or XML format. This data can then be used for testing purpose.

**Features:**

  * A restful interface
  * Quickly generate random test data
**Setting up ** Let's write our first test now... Here we are using Selenium Webdriver with Java. We can use Test Spicer with any platform, tool or language. In this test we have chosen [Xebia Contact Us][2] form as the test site. The contact us form contains different input fields that accept different kinds of data. Eg: Name field accepts string, Phone field accepts numbers, Email field accepts email and Message field accepts a long paragraph. We want the automation script to enter a different set of data every time. This can be done by first writing code to call the [Test Spicer RESTful API][3].

**Writing code to call the API**

[code language="java"] public class JavaNetURLRESTFulClient { String targetURL; Map map;
    
    
    public String getTargetURL(String input){
        if(input.equals(&quot;alphabetString&quot;))
            targetURL = &quot;http://testspicer.com/restapi/v0.1/string/en/10&quot;;
        else if (input.equals(&quot;phone&quot;))
            targetURL = &quot;http://testspicer.com/restapi/v0.1/telephone?countrycode=UK&quot;;
        else if(input.equals(&quot;email&quot;))
            targetURL = &quot;http://testspicer.com/restapi/v0.1/email?domain=com&amp;length=20&quot;;
        else if(input.equals(&quot;para&quot;))
            targetURL = &quot;http://testspicer.com/restapi/v0.1/lorem?language=en&quot;;
        return targetURL;
    }
    
    public Map getRandomData(String input){
        getTargetURL(input);
        try {
    
            URL restServiceURL = new URL(targetURL);
    
            HttpURLConnection httpConnection = (HttpURLConnection) restServiceURL.openConnection();
            httpConnection.setRequestMethod(&quot;GET&quot;);
            httpConnection.setRequestProperty(&quot;Content-Type&quot;,&quot;application/json&quot;);
            httpConnection.setRequestProperty(&quot;api_key&quot;,&quot;64c20419f2e64b37f123939ba047183v&quot;);
    
            if (httpConnection.getResponseCode() != 200) {
                throw new RuntimeException(&quot;HTTP GET Request Failed with Error code : &quot;
                        + httpConnection.getResponseCode());
            }
    
            map = new Gson().fromJson(IOUtils.toString(httpConnection.getInputStream(),&quot;UTF-8&quot;), Map.class);
            httpConnection.disconnect();
        } catch (MalformedURLException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return map;
    }
    

} [/code]

**How it works:** In the above code, we issue a GET request to the end point like “string/en/10” and get 10 character long string. If we want a different length, we just need to modify the length to the desired value.

**Writing Selenium Webdriver Test **

[code language="java"] public class ContactUsTest { public static void main (String[] args) throws InterruptedException{ Map map;
    
    
        WebDriver driver = new FirefoxDriver();
        driver.get(&quot;http://www.xebia.in/contact-us.html&quot;);
    
        WebElement name = driver.findElement(By.id(&quot;name&quot;));
        WebElement contact_no = driver.findElement(By.id(&quot;phone&quot;));
        WebElement email = driver.findElement(By.id(&quot;email&quot;));
        WebElement message = driver.findElement(By.id(&quot;message&quot;));
    
        JavaNetURLRESTFulClient obj = new JavaNetURLRESTFulClient();
    
        map = obj.getRandomData(&quot;alphabetString&quot;);
        name.sendKeys((String)map.get(&quot;string&quot;));
    
        map = obj.getRandomData(&quot;phone&quot;);
        contact_no.sendKeys((String)map.get(&quot;number&quot;));
    
        map = obj.getRandomData(&quot;email&quot;);
        email.sendKeys((String)map.get(&quot;email&quot;));
    
        map = obj.getRandomData(&quot;para&quot;);
        message.sendKeys((String)map.get(&quot;lorem&quot;));
    
        driver.quit();
    }
    

} [/code]

**Steps to run the tests ** We just run our selenium test as a Java application. It should spawn a browser and start executing the test with a given set of data. In the second run, it should type a different set of data.

**You should get something like this:** ![Xebia_Contact_Us][4]

**Advantages:**

  * No need to write code to generate random data
  * Increase coverage of tests
TestSpicer helps to provide test ideas to make sure that apparent things related to quality are never missed.

**Similar tools in the market** [selite][5]

**References ** [http://testspicer.com/][6]

   [1]: http://www.testspicer.com (testspicer)
   [2]: http://www.xebia.in/contact-us.html (Xebia Contact Us)
   [3]: http://testspicer.com/docs (Test Spicer RESTful API)
   [4]: http://xebee.xebia.in/wp-content/uploads/2014/06/Xebia_Contact_Us.png
   [5]: https://code.google.com/p/selite/wiki/ProjectHome (selite)
   [6]: www.testspicer.com (testspicer.com)