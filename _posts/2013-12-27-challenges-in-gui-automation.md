---
layout: post
header-img: img/default-blog-pic.jpg
author: Khyati
description: 
post_id: 17314
created: 2013/12/27 19:13:11
created_gmt: 2013/12/27 14:13:11
comment_status: open
---

# Challenges in GUI automation

<p dir="ltr" style="text-align: left">The current trend in user interfaces is geared towards graphical user interfaces (GUIs). GUIs are very complicated and hence GUI testing is very time consuming. Automation is a requirement for testing any larger graphical user interfaces, but automating GUI tests isn't a forthright task. Automation is not that easy at first.</p>

<p dir="ltr" style="text-align: left"><em>Testing always under time constraint:</em></p>

<p dir="ltr" style="text-align: left">Every software comes to a team with deadlines and delivery dates. And that's the major challenge of automation, There is huge list of stories and tasks including understanding product requirements,writing test cases,writing automated test scripts ,executing them, make them run as a whole may be as a part of daily build,etc which needs to complete within specified time lines.</p>

<p dir="ltr" style="text-align: left"><em>Proper knowledge of the requirements:</em></p>

<p dir="ltr" style="text-align: left">Most of the time  business analysts are responsible for communicating with customers for understanding the requirements. What if tester fails to understand that requirements? Will she be able to test the application properly? Testers require good listening and understanding capabilities along with communication skills.</p>

<!--more-->

<p dir="ltr" style="text-align: left"><span style="color: #003366"><em>Automation Strategy , Framework picking and <em>Testing tool Selection</em><strong>:</strong></em></span></p>

<p dir="ltr" style="text-align: left">Availability of various tools in market and that too open source is making life of a tester more complicated rather than easier. In the starting decisions like which framework to choose? Which tools will best work here? Which coding language supports that tools? arises and that is the first and foremost task which needs to be done before starting automation.</p>

<p dir="ltr" style="text-align: left"><em>Testers centering on detecting easy bugs:</em></p>

<p dir="ltr" style="text-align: left">There are many organizations and customers in which testers have to provide a list of bugs and at the end of day that can act as a performance indicator for them. Finding easy bugs those don’t require deep understanding and testing but to make a website or a software more robust we need to dig into more complex issues which could be be rare though important,tough to find and remains unnoticeable if not given proper attention. According to me to include each test in automation test cases is next to impossible.But automating the majority gives you the time to focus on the “strange” few scenarios that automated tests cannot see.</p>

<p dir="ltr" style="text-align: left"><span style="color: #003366"><em>Relationship with business analysts:</em></span></p>

<p dir="ltr" style="text-align: left">Developing relations ,by this I mean continuous communication with the developer. So this need a mature and experienced tester who can optimistically handle the situation by her involvement in doing tasks in their own format. The problem become more intense when questions arises like if this bug is actually a bug or a part of functionality itself!</p>

<p dir="ltr" style="text-align: left"><em><span style="color: #003366">How to handle lack of experience in testers</span></em></p>

<p dir="ltr" style="text-align: left">In Agile testing particularly,the teams are mix of different experienced people all around not only QA's but also developers,so the problem is to come on a common track where every individual can contribute equally. Which thus,results in issues like time management ,inapt , undone work.</p>

<p dir="ltr" style="text-align: left"><span style="color: #003366"><em>Priority of test cases:Which tests to execute first?</em></span></p>

<p dir="ltr" style="text-align: left">Decision like which test cases should be executed and with what priority? Which tests are more  important over others? This requires good experience to work under pressure. When and where to start automation? How and when to stop testing?  When to pause the testing? GUI is quite fuzzy and brittle,it should target most important and utmost areas covering basics first at ground level like Home page , Log in page ,etc and then moving to the other high level areas</p>

<p dir="ltr" style="text-align: left"><span style="color: #003366"><em>Should we target the automation of 'X' module:</em></span></p>

<p dir="ltr" style="text-align: left">Should automating this application is required ,at all? How much time will it take? At what level automation should be done? Does automation really works? Decision of automation or manual testing will need to address the pros and cons of each process.For every module we should have a discussion over it as automation testing is like development only where QA team has to decide proper development tool, development language, correct designing ,etc. We also have to decide which module should be target to automate and which we should leave.</p>

<p dir="ltr" style="text-align: left"><span style="color: #003366"><em>Simulating real user experience and handling complicated elements and functionalities: </em></span></p>

<p dir="ltr" style="text-align: left">There are multiple issues like handling pop up ,say ,whether you want to proceed , <em>clicking on a selected color of a slice of pie</em> , clicking on a highlighted text,etc are most of the common issues which I have see most of time while adding test steps in an automation framework.</p>

<p dir="ltr" style="text-align: left"><span style="color: #003366"><em>Reuse of Test scripts:</em></span></p>

<p dir="ltr" style="text-align: left">Application development methods are dynamic ,making it difficult to manage the test tools and test scripts. Test script migration or reuse is very essential but difficult task. GUI is an ever changing part of any software ,and updating GUI often break your test cases. Small alteration might break whole test suite.</p>

<p dir="ltr" style="text-align: left"><em>Robust handling of environment and response slowness:</em></p>

<p dir="ltr" style="text-align: left">This is something which cant be resolved easily as wait functions sometimes makes overall run of test suite very very slow. For instance, if there is a web site say IRCTC which sometimes gives quick response and often user have to struggle for getting a ticket booked ,on contrary, Google which is the best and fast search engine on web. So the point is to automate web sites having different response time is a bit tricky and difficult.</p>

<p dir="ltr" style="text-align: left"><span style="color: #003366"><em>Multiple browsers and cross browser support:</em></span></p>

<p dir="ltr" style="text-align: left">This issue is not specifically for automation testing but also valid for manual testing. Testing over multiple browsers is always a head-ache. So as to make automation test out of it,a tester need to add browser compatibility executable in the project , desired capabilities of each browser ,etc. For example:</p>

<p>[code]
public void initDriver() {
 try {
 if (Property.IsRemoteExecution.equalsIgnoreCase(&quot;true&quot;)) {
 String remoteURL;
 DesiredCapabilities capabilities = new DesiredCapabilities();
 if (Property.RemoteURL.toLowerCase().contains(&quot;saucelabs&quot;)) {
 // set all saucelab capabilities here.
 }
 remoteURL = Property.RemoteURL + &quot;/wd/hub&quot;;</p>
<p>URL uri = new URL(remoteURL);
 if (browserName.equalsIgnoreCase(&quot;firefox&quot;)) {
 FirefoxProfile remoteProfile = new FirefoxProfile();
 remoteProfile.setPreference(
 &quot;webdriver_assume_untrusted_issuer&quot;, false);
 remoteProfile.setAcceptUntrustedCertificates(true);
 remoteProfile.setEnableNativeEvents(false);
 capabilities.setBrowserName(&quot;firefox&quot;);
 capabilities.setCapability(&quot;firefox_profile&quot;, remoteProfile
 .toString().toString());
 driver = new RemoteWebDriver(uri, capabilities);
 } else if (browserName.equalsIgnoreCase(&quot;internetexplorer&quot;)) {
 capabilities.setBrowserName(&quot;internet explorer&quot;);
 capabilities.setCapability(&quot;ignoreProtectedModeSettings&quot;,
 true);</p>