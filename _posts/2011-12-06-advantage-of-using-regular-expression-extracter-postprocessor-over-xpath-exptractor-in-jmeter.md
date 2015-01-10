---
layout: post
header-img: img/default-blog-pic.jpg
author: Priyanka
description: 
post_id: 10406
created: 2011/12/06 16:04:54
created_gmt: 2011/12/06 11:04:54
comment_status: open
---

# Advantage of using Regular Expression Extracter postprocessor over XPath exptractor in Jmeter

In my recent project I needed to do performance testing of SSO log-in with 1000 unique users  with JMeter as performance testing tool. With JMeter, it is very easy to pass values from response of one request to subsequent request by using its post-processor requests. During project time I came up with one scenario where values were required to be read from HTTP request and then those values were required to be passed  to next request as input parameters. Please read scenario description which is given below:-

**Scenario:-**** **It was required to extract two values from response of first HTTP request and passing those to subsequent request as parameters. First value was required to be extracted from **body response** of  HTTP request and other value was required to be extracted from its **header response**. Those values were:-

  1. **sessionid** –it was required to be extracted from body response of first HTTP request.
  2. **node number** – it was required to be extracted from its header response.

**Solution:-**

The above problem can be solved by using JMeter’s **Regular Expression Extractor** which can be used to extract values from different response fields like response body, response headers, URL, response code, response message and then will store the extracted results into the given variable name in subsequent request.

**Advantage of Regular Expression Extractor over XPath extractor:-**

XPath Extractor can also be used to extract values from response but there is advantage of using Regular Expression Extractor as it can read values from response body, response headers, URL, response code, response message but XPath Extractor can extract values from response body only. Secondly, using regular expression results in fast speed of execution as compared to Xpath extractors because regular expression matching is simpler and fast and it is a good replacement of XPath matching which is comparatively slower.

**Check step by step use of Regular Expression Extractor and XPath Extractor in test plan below:-**

The above problem has been showcased below by using both Regular Expression Extractor and XPath Extractor. Let’s discuss the steps to create a test plan which illustrates use of both types of extractors which detailing.

  1. Create a Test Plan with HTTP sampler.

  2. Now add XPath Extractor Post Processor to the first sampler request by right clicking on that request and enter the required reference name and XPath query. XPath Extractor has three fields, which are reference name, XPath query and default value. Let’s discuss what do these terms mean:-

![][1]

**Reference name**:-It is a variable name given by user which stores the extracted value. In above example "sessionId" is being used as reference name.

**XPath query**:- Functions like local-name() and namespace-uri() can be used in XPath query to match the local tag name and the URI associated with its namespace in JMeter.

**Default value**: As name suggests it carries a default value of reference name in case XQuery fails to extract value and we do not want our test case to fail. It is not a mandatory field like above two.

  1. Add **Regular Expression Extractor** to first sampler request to extract the value of interest into parameter given in reference name. It is added to first HTTP request to extract value of node number from its header response. It has five fields. Let’s understand what do these terms mean:-

![][2]

**Reference name**:- In above screenshot “**node”** is reference variable.

**Regular expression**:-User can express any regular expression to search for matching string of text. Regular expression used in above screenshot “node=(.*?)” matches group name node in response containing values as any character 0 or more times

**Template**:- The Template is used to create a string from the matches found. Its value can be $1$ and so on depending on which block we want to match. In my case above I wanted it to match with first string block which should match with “(.*?)” regular expression and gets save in variable name “node”. This is the reason I have taken template value as “$1$”.

**Match no**:- It Indicates which match to use. The regular expression may match multiple times. In our case there was only one match so I can choose any number either 0 which means for random match or any positive integer which represent nth match. Its good to use positive number here to specify clear match.

**Default value**:- It is not a mandatory field like above four.

  1. Add next sampler request where you want to use these variables. In screenshot below where ${sessionid} and ${node} are passed as variables in sakai.session and node parameters respectively. Remember, whenever a variable value needs to be passed it should be passed with $ sign and variable name in brackets.

![][3]

  1. When Test Plan is run, Regular Expression and XPath Extractor will both execute with their sampler request in its scope and will extract values in given reference names from sampler request and then transfer them to subsequent request.

   [1]: http://xebee.xebia.in/wp-content/uploads/2011/12/XPath_Extractor5-1024x350.png (XPath_Extractor)
   [2]: http://xebee.xebia.in/wp-content/uploads/2011/12/Regular_Expression-1024x344.png (Regular_Expression)
   [3]: http://xebee.xebia.in/wp-content/uploads/2011/12/Parameters-1024x402.png (Parameters)

## Comments

**[Priyanka](#7107 "2012-01-24 16:45:50"):** Hi Durga, The answer to your question is :- yes you should use Reg-exp for your script. I agree that it should be generating unique ticket for every user log-in. But there must be some part which is common also. For example:- If "ticket=" is common in each response then you can give "ticket=(.*?)" as regular expression in your script and I think should solve your problem. Add Cookie Manager also in your script. Please feel free if you have any questions. Thanks, Priyanka Priyanka

**[Durga](#7079 "2012-01-23 10:13:00"):** Hi, This post has been quite useful. I have one question for you, I am testing an webapp using JMeter and in this every time any user logs in it generates an "ticket=ST-523-????????????-cas" like this and its unique for every user login and its valid for entire session unless u logout. Just tell me when I am running this script for 50 Users (by just mentioning in the Number of Threads(Users) in ThreadGroup panel as 50) do I need to use Regex ? does it required to generate this "ticket" 50 times ? I have been using Cache Manager just HTTP Request Default in the script, is this enough or do I have to use Regex?

**[Durga](#7274 "2012-02-01 12:48:40"):** Hi Priyanka, First of all Thanks for your prompt reply. The thing i am about to ask you is really more than a favor., actually i have never used this regex in any of my scripts till - I do not have any idea where exactly to insert this Post processor in my script and all the settings to be done before actually 'running' script. If you can provide step by step on how to use this Regex by taking any application example like irctc or any other web app. i would be very glad. Yes i have googled it out but they are all very confusing. Please help me on this regard. Thanks, Durga

**[pawan](#7901 "2012-03-12 15:05:25"):** Post Regex above thread group and give reference in url

**[Priyanka](#7944 "2012-03-16 15:52:08"):** Hi Durga, You have to insert post processor just below you http call from where you want to extract values. Please see the screenshots mentioned in my blog. I have provided step by step description.

