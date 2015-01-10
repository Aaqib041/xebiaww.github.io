---
layout: post
header-img: img/default-blog-pic.jpg
author: jgupta
description: 
post_id: 14082
created: 2013/09/20 14:28:53
created_gmt: 2013/09/20 09:28:53
comment_status: open
---

# UI Automation Best Practices

<p><span style="font-family: Arial,sans-serif">Automated software testing saves a lot of time and money. Every time source code is modified, software tests should be executed to make sure application works as expected. </span></p>
<p><span style="font-family: Arial,sans-serif">Tests should be written at unit, integration and acceptance level. Acceptance tests are mostly UI tests which test the application from user perspective and mirror the user stories. But UI tests can be difficult to manage and should be carefully designed. Below are some of best practices which I have gained through experience. These can be used to make acceptance tests more stable and robust with some of very popular tools like selenium. </span><!--more-->
<ul>
    <li><span style="font-family: Arial,sans-serif">Ensure the test pyramid is correctly followed. Majority of tests should be written at unit level, few integrations tests and <strong>minimal set of acceptance tests.</strong> Acceptance tests are generally slow to execute, expensive and require more maintenance. User workflow related tests should be covered in acceptance but do not try to cover all negative scenarios. Acceptance tests target should be to ensure that key pieces of business functionality are intact.</span></li>
    <li><span style="font-family: Arial,sans-serif"><strong>Carefully select element locaters</strong>. Always try to use ID attribute of html elements instead of name, class or xpath for faster test execution in browsers. If you don’t have IDs, prefer name over class, class over xpath and relative path over absolute path.</span></li>
    <li><span style="font-family: Arial,sans-serif"><strong>Never hardcode sleep</strong> in test code. There is always performance variance in different environments, so instead of using hardcoded sleep, use specific wait methods like Wait.until { ... }, object.when_present.set, object.wait_until_present, object.wait_while_present etc.</span></li>
    <li><span style="font-family: Arial,sans-serif"><strong>Never rely on external dependencies</strong> (For e.g. WebServices, Database) . Use stubbing and setup your own test data.</span></li>
    <li><span style="font-family: Arial,sans-serif"><strong>Make sure tests are independent</strong> and states should not be shared between tests. Always ensure to delete cookies before tests. Avoid using any global variables. </span></li>
    <li><span style="font-family: Arial,sans-serif"><strong>Use Headless browser</strong> in Continuous Integration which should speed up test execution time.</span></li>
    <li><span style="font-family: Arial,sans-serif">Before performing any action on elements, <strong>wait for element to appear</strong> on page. </span></li>
    <li><span style="font-family: Arial,sans-serif"><strong>Use Page Object Design pattern</strong> which provides better maintainability and readability. Also, if UI changes for a page, tests themselves don’t need to change. Only the code in page objects needs to be modified. </span></li>
    <li><span style="font-family: Arial,sans-serif"><strong>Use relative URLs</strong> for pages, if possible and try to use deep linking for pages in tests.</span></li>
    <li><span style="font-family: Arial,sans-serif"><strong>Externalize test data</strong> to text/csv file so that it could be modified with out having to modify source code and perform data driven testing.</span></li>
    <li><span style="font-family: Arial,sans-serif"><strong>Avoid Using Key Press</strong> functions unless you are testing short cuts. If you are using key press for clicking on alerts etc, it might fail if focus is not on that page.</span></li>
    <li><span style="font-family: Arial,sans-serif"><strong>Use Object repository</strong> for centralized data-store for all your elements locators. Follow DRY (do not repeat yourself).</span></li>
</ul>
For more information about Xebia's offerings on Agile Testing Services , Click here (link it to <a href="http://www.xebia.in/application-testing.html">http://www.xebia.in/application-testing.html</a>)</p>

## Comments

**[Nicky](#9448 "2013-10-04 13:51:36"):** Very good list. Especially first point where teams makes mistakes of creating too many ui tests whereas many tests could have been covered with unit/integration tests. Also a vary valid point of using stubbed data. I have seen teams making mistakes of testing with live data of external dependencies and dealing with timeouts, service failures, etc.

**[daluu](#9446 "2013-09-24 04:05:37"):** Good suggestions. Here are some other thoughts on UI automation best practices with respect to page objects, ATDD/TDD with Selenium, and test data management: http://autumnator.wordpress.com/2013/07/11/developing-selenium-tests-with-proper-abstraction/ http://autumnator.wordpress.com/2013/08/02/pseudo-test-driven-development-with-selenium-and-page-object-model-for-developers-and-qa/ http://autumnator.wordpress.com/2013/07/12/avoid-creating-unnecessary-test-data/

