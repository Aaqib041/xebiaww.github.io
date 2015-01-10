---
layout: post
header-img: img/default-blog-pic.jpg
---

# Challenges in GUI automation

The current trend in user interfaces is geared towards graphical user interfaces (GUIs). GUIs are very complicated and hence GUI testing is very time consuming. Automation is a requirement for testing any larger graphical user interfaces, but automating GUI tests isn't a forthright task. Automation is not that easy at first.

_Testing always under time constraint:_

Every software comes to a team with deadlines and delivery dates. And that's the major challenge of automation, There is huge list of stories and tasks including understanding product requirements,writing test cases,writing automated test scripts ,executing them, make them run as a whole may be as a part of daily build,etc which needs to complete within specified time lines.

_Proper knowledge of the requirements:_

Most of the time  business analysts are responsible for communicating with customers for understanding the requirements. What if tester fails to understand that requirements? Will she be able to test the application properly? Testers require good listening and understanding capabilities along with communication skills.

_Automation Strategy , Framework picking and _Testing tool Selection_**:**_

Availability of various tools in market and that too open source is making life of a tester more complicated rather than easier. In the starting decisions like which framework to choose? Which tools will best work here? Which coding language supports that tools? arises and that is the first and foremost task which needs to be done before starting automation.

_Testers centering on detecting easy bugs:_

There are many organizations and customers in which testers have to provide a list of bugs and at the end of day that can act as a performance indicator for them. Finding easy bugs those don’t require deep understanding and testing but to make a website or a software more robust we need to dig into more complex issues which could be be rare though important,tough to find and remains unnoticeable if not given proper attention. According to me to include each test in automation test cases is next to impossible.But automating the majority gives you the time to focus on the “strange” few scenarios that automated tests cannot see.

_Relationship with business analysts:_

Developing relations ,by this I mean continuous communication with the developer. So this need a mature and experienced tester who can optimistically handle the situation by her involvement in doing tasks in their own format. The problem become more intense when questions arises like if this bug is actually a bug or a part of functionality itself!

_How to handle lack of experience in testers_

In Agile testing particularly,the teams are mix of different experienced people all around not only QA's but also developers,so the problem is to come on a common track where every individual can contribute equally. Which thus,results in issues like time management ,inapt , undone work.

_Priority of test cases:Which tests to execute first?_

Decision like which test cases should be executed and with what priority? Which tests are more  important over others? This requires good experience to work under pressure. When and where to start automation? How and when to stop testing?  When to pause the testing? GUI is quite fuzzy and brittle,it should target most important and utmost areas covering basics first at ground level like Home page , Log in page ,etc and then moving to the other high level areas

_Should we target the automation of 'X' module:_

Should automating this application is required ,at all? How much time will it take? At what level automation should be done? Does automation really works? Decision of automation or manual testing will need to address the pros and cons of each process.For every module we should have a discussion over it as automation testing is like development only where QA team has to decide proper development tool, development language, correct designing ,etc. We also have to decide which module should be target to automate and which we should leave.

_Simulating real user experience and handling complicated elements and functionalities: _

There are multiple issues like handling pop up ,say ,whether you want to proceed , _clicking on a selected color of a slice of pie_ , clicking on a highlighted text,etc are most of the common issues which I have see most of time while adding test steps in an automation framework.

_Reuse of Test scripts:_

Application development methods are dynamic ,making it difficult to manage the test tools and test scripts. Test script migration or reuse is very essential but difficult task. GUI is an ever changing part of any software ,and updating GUI often break your test cases. Small alteration might break whole test suite.

_Robust handling of environment and response slowness:_

This is something which cant be resolved easily as wait functions sometimes makes overall run of test suite very very slow. For instance, if there is a web site say IRCTC which sometimes gives quick response and often user have to struggle for getting a ticket booked ,on contrary, Google which is the best and fast search engine on web. So the point is to automate web sites having different response time is a bit tricky and difficult.

_Multiple browsers and cross browser support:_

This issue is not specifically for automation testing but also valid for manual testing. Testing over multiple browsers is always a head-ache. So as to make automation test out of it,a tester need to add browser compatibility executable in the project , desired capabilities of each browser ,etc. For example:

[code] public void initDriver() { try { if (Property.IsRemoteExecution.equalsIgnoreCase("true")) { String remoteURL; DesiredCapabilities capabilities = new DesiredCapabilities(); if (Property.RemoteURL.toLowerCase().contains("saucelabs")) { // set all saucelab capabilities here. } remoteURL = Property.RemoteURL + "/wd/hub"; URL uri = new URL(remoteURL); if (browserName.equalsIgnoreCase("firefox")) { FirefoxProfile remoteProfile = new FirefoxProfile(); remoteProfile.setPreference( "webdriver_assume_untrusted_issuer", false); remoteProfile.setAcceptUntrustedCertificates(true); remoteProfile.setEnableNativeEvents(false); capabilities.setBrowserName("firefox"); capabilities.setCapability("firefox_profile", remoteProfile .toString().toString()); driver = new RemoteWebDriver(uri, capabilities); } else if (browserName.equalsIgnoreCase("internetexplorer")) { capabilities.setBrowserName("internet explorer"); capabilities.setCapability("ignoreProtectedModeSettings", true);