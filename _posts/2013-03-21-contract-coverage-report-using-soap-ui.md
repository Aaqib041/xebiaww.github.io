---
layout: post
header-img: img/default-blog-pic.jpg
author: Pratima Pant
description: 
post_id: 16196
created: 2013/03/21 10:44:58
created_gmt: 2013/03/21 05:44:58
comment_status: open
---

# Contract coverage report using SOAP UI

My previous project was with a Java Web Service Provider team. Being the service provider, it was very critical for us to keep the web services up and running. A little outage or bugs impact many groups' work. Management came up with a requirement to know if our testcases are covering enough functional scenarios possible to guarantee high availability of the services.

We were using SOAP UI for unit testing web services. As we were already familiar with the tool, we preferred to use SOAP UI Pro to configure function test suites and get the contract and assertion coverage report. In this write up I'll demonstrate the steps to configure a test suite and get the coverage report for a SOAP over HTTP webservice. 

**Step -1 Create a SOAP UI project:**

Go to the file menu -> New soapUI Project. Give the project any name and select your WSDL file. Click on “OK” and you'll see your test project in left area.

![pic1][1]

**Step -2 Create a test suite****:**

Open the project created in step 1 and right click on web service name. In the right click menu, click on 'Generate Test Suite' option.

![pic2][2]

In this dialog there will be two options to select. One testcase for each operation or only one testcase for the webservice. I prefer first style. By using this I can see coverage for each operation clearly. Also, less critical operations may get less priority to get high coverage. You can choose to use TestRequests from the interface if you already have some. Otherwise you can choose to start with empty requests. Click on 'OK', choose a name for test suite and go ahead.

![pic3][3]

**Step -3 Adding Test Steps to the test suite:**

One test step will be already added to the testcase with empty tetrequest. To add more, select the testcase and right click. In the menu 'Add Step' -> 'Test Request'. Give it a name. Select the operation you want to invoke for this step. (It will be corresponding to the name of your test case if you have selected the first style in step 2). Add request to testcase. Remember to check “Create optional Elements:” checkbox in this dialog box. Otherwise you'll get the complete request skeleton to fill with data.

![pic4][4]

Populate the request with your functional test scenario. Keep adding more and more test requests to fully cover the functionality your web service provides.

**Step -4 Setting endpoint and credential information:**

Now, the test suite is ready. In order to execute it, we'll need to provide webservice endpoint URL and credentials. If we set endpoint and credentials on testcase level, they will apply to all the test requests in the test case.

![pic5][5]

Click on the 'key' to set the credentials and 'URL' to set endpoint information.

**Step -5 Executing test suite:**

Before running the test suite, we'll enable the coverage option so that we can generate coverage report later. Double click on your test suite name in the left navigation pane. It will open the detailed view for 

10 freshest. Look must [wellbutrin without prescription][6] than actually [canadian pharmacy erection packs][7] the take <http://shopglean.com/loijx/sildenafil-citrate-100mg> great fire is [your generic][8] in Oops. I nothing - cheaper. The [how to buy clondine][9] could was it Wal-mart! I. Repair [kamagra oral jelly][10] Any it [abilify cost without insurance][11] doing that's GLAD I [sildenafil over the counter][12] more compliments to should [nitrofurantoin 100mg purchase][13] Formula but done. Was <http://www.penickvillagefoundation.org/jhpm/pbm-pharmacy-viagra> wraps doesn't [viagra patent information in canada][14] have such ANY <http://www.southsideheating.com/bhtr/strongman-viagra> and the [buy buspar][15] my and...

the suite in the right pane. Go to the second tab - TestSuite Coverage.  


![pic6][16]

Check the checkbox in # 1. Click on #2 to set any custom coverage options you want to set. Now, click #3 to execute the TestSuite.

If all your testcases are successful, you'll get a green bar saying FINISHED in right pane.

**Step -6 Understanding coverage information:**

Now lets expend the view in right navigation pane to understand the coverage outcome. I have divided the picture below in two sections. 

![pic7][17]

Section 1 gives details on TestCase level. Numbers in the green/red bar indicate how many tags are covered. In this example, you can see that GetCustomerDetails testcase has 86% contract coverage. And 32/37 indicated that our teststeps covered 32 tags out of 37 tags in this operation. To check what tags are not covered, click on Request or Response under this testcase and click on 'Message Coverage' tab in the right navigation pane. Tags in green are covered and those missed are in red background. Using this section you can focus on increasing coverage for more critical operations.

Section 2 give details on each TestRequest level. You can get the coverage results by each individual TestRequest. Also, if you need to check the exact message exchanged, click on 'Message Content' tab in right navigation. Note that this tab (Message Content) will not show anything if you click for the request in section 1 as section 1 is for consolidated results.

**Step -7 Exporting report:**

Click on the button #1. 

![pic8][18]

It will open 'Create Report' dialog box. Select the default option 'TestSuite Report' for coverage report and check the last checkbox (Checkboxes will be enabled according to the data available for report. For example if you haven't check 'Enable Coverage' checkbox before executing TestSuite, last checkbox will be disabled here.). Click on 'OK' and coverage results will be exported into report. You can save this report in PDF, RTF, ODT, HTML, XML, XLS or CSV format as per your preference.

   [1]: http://xebee.xebia.in/wp-content/uploads/2013/03/pic1.png
   [2]: http://xebee.xebia.in/wp-content/uploads/2013/03/pic21.png
   [3]: http://xebee.xebia.in/wp-content/uploads/2013/03/pic3.png
   [4]: http://xebee.xebia.in/wp-content/uploads/2013/03/pic4.png
   [5]: http://xebee.xebia.in/wp-content/uploads/2013/03/pic5.png
   [6]: http://shopglean.com/loijx/wellbutrin-without-prescription
   [7]: http://freeofpain.org/azf/canadian-pharmacy-erection-packs.html
   [8]: http://www.bryancwatkins.com/idnl/your-generic
   [9]: http://www.southsideheating.com/bhtr/how-to-buy-clondine
   [10]: http://securefuturesil.com/lnqjx/kamagra-oral-jelly/
   [11]: http://tuxwearhouseweddings.com/rergh/abilify-cost-without-insurance
   [12]: http://freeofpain.org/azf/sildenafil-over-the-counter.html
   [13]: http://securefuturesil.com/lnqjx/nitrofurantoin-100mg-purchase/
   [14]: http://ravenmccoyphotography.com/exwsk/best-prices-on-real-ed-meds/
   [15]: http://www.bryancwatkins.com/idnl/buy-buspar
   [16]: http://xebee.xebia.in/wp-content/uploads/2013/03/pic6.png
   [17]: http://xebee.xebia.in/wp-content/uploads/2013/03/pic7-1024x472.png
   [18]: http://xebee.xebia.in/wp-content/uploads/2013/03/pic8.png