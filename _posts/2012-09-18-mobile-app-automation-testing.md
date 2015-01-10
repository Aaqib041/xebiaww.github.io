---
layout: post
header-img: img/default-blog-pic.jpg
author: usinha
description: 
post_id: 14884
created: 2012/09/18 17:02:08
created_gmt: 2012/09/18 12:02:08
comment_status: open
---

# When Monkeys Talk (Mobile App Automated Tesing - I)

<p>While I attended the first XKE here at Xebia, I heard some one saying '<i>You are not smart... If you dont have a smart phone'</i>, so Being a smart tester ;) or while trying to be a smart tester I started exploring available tools to test the applications for these Smart Phones. And in this process I came accross a tool which<b> supports both  iOS and Android</b>, and allows us to test <strong>native or hybrid apps</strong>, on <strong>simulators or real devices</strong> -- <strong>no jail breaking required</strong>, which is <strong>robust </strong>and enables cross – platform recording and playback. And this great <strong>free and open-source</strong> functional tesing tool is called Monkey Talk. Now you know where the title is coming from.
<!--more--></p>
<p><strong><span style="font-family: Arial, sans-serif;font-size: medium">Why monkey talk ?</span></strong></p>
<p>Well there are a lot of other available options but I went for Monkey Talk as
<ul>
    <li>It had powerful     and robust <strong>record and playback</strong> functions for mobile applications.</li>
    <li>Fully <strong>cross     platform</strong>-- record on iOS and playback on Android and vice versa.</li>
    <li>Powerful IDE-- a    single <strong>unified IDE</strong> for both iOS and Android based on Eclipse.</li>
    <li>No jail breaking    needed-- runs on stock hardware without alterations.Run on real     hardware-- both phones and tablets are supported, either tethered or    over the network.</li>
    <li><strong>Readable scripts</strong>--     MonkeyTalk is simple and easy to learn, generating scripts that the     average person can understand.</li>
    <li><strong>Keyword-driven  scripts</strong>-- MonkeyTalk scripts can be easily extended, even by   non-programmers, with custom user-defined commands.</li>
    <li><strong>Data-driven     scripts</strong>-- any command can be looped using a CSV data file.</li>
    <li><strong>Javascript </strong>-- generate JavaScript from MonkeyTalk if a full programming    language is necessary.</li>
    <li><strong>JUnit-compatible    XML reports</strong>-- test suites output the standard XML report, making   MonkeyTalk easy to integrate into existing systems.</li>
    <li><strong>CI ready</strong>-- run     test suites in a fully automated fashion, from the commandline,     perfect for integration into any continuous integration environment.</li>
</ul>
<h2>How to start Monkeying ?</h2>
In this blog I will take you through the set up configuration of Monkey Talk for automation of  iOS and Android applications.
<h2><span style="font-family: Arial, sans-serif">Android:-</span></h2>
You need to install MonkeyTalk Android Agent into the application which you want to automate.</p>
<p>For this, please follow below steps:</p>
<p><b>Steps</b>
<ul>
    <li>Open your Android   Project in Eclipse.</li>
    <li>Right click on the  project name and select Configure&gt;&gt;Convert to AspectJ Project.</li>
</ul>
<div><a rel="attachment wp-att-14914" href="http://xebee.xebia.in/2012/09/18/mobile-app-automation-testing/p1-2/"><img class="aligncenter size-medium wp-image-14914" height="118" width="300" src="http://xebee.xebia.in/wp-content/uploads/2012/09/p11-300x118.jpg" /></a></div>
<div></div>
<ul>
    <li><span style="color: #000000"><span style="font-family: 'Times New Roman', serif"><span style="font-size: small">Create</span></span></span><span style="color: #000000"><span style="font-family: 'Times New Roman', serif"><span style="font-size: small"> </span></span></span>libs” folder in your Android Project and put   MonkeyTalk-agent.jar in “libs” folder. MonkeyTalk-agent.jar can     be found in the "agents" folder in the MonkeyTalk package.</li>
    <li>Right click on  MonkeyTalk-agent.jar &gt; AspectJ Tools &gt; Add to Aspectpath.</li>
</ul>
<div><a rel="attachment wp-att-14915" href="http://xebee.xebia.in/2012/09/18/mobile-app-automation-testing/2-9/"><img class="aligncenter size-medium wp-image-14915" height="47" width="300" src="http://xebee.xebia.in/wp-content/uploads/2012/09/2-300x47.jpg" /></a></div>
<ul>
    <li>Open    AndroidManifest.xml and include the following two permissions:
o   uses-permission android:name="android.permission.INTERNET"
o   uses-permission android:name="android.permission.GET_TASKS"</li>
</ul>
<div><a rel="attachment wp-att-14916" href="http://xebee.xebia.in/2012/09/18/mobile-app-automation-testing/3-4/"><img class="aligncenter size-medium wp-image-14916" height="91" width="300" src="http://xebee.xebia.in/wp-content/uploads/2012/09/3-300x91.jpg" /></a></div>
<ul>
    <li>Now RUN your    application right click on the Project and Select Run As&gt;&gt;Android     application.</li>
    <li>You would see   Android Emulator will appear and your application gets deployed on  the emulator or Hardaware.</li>
</ul>
<h2><span style="font-family: Arial, sans-serif">iOS:-</span></h2>
<ul>You need to install     MonkeyTalk iOS Agent into the application which you want to     automate. For this, please follow the steps given below:</ul>
<ul>
    <li>Open your   application's project in Xcode.</li>
    <li>Duplicate your  application's build target by right-clicking on it and selecting    Duplicate from themenu. A new target    will be created called YourApp copy.</li>
    <li>Rename YourApp  copy to something like YourAppMonkey.</li>
    <li>Add the downloaded  MonkeyTalk lib to your project File &gt; Add to “YourApp”...    from the menu.</li>
    <li>When the dialog     box appears, navigate to the directory where you unzipped the   MonkeyTalk zipfile, and select  the MonkeyTalk iOS lib from pathToMonkeyTalkFolder/agents/iOS.</li>
    <li>Recursively     creates groups for any added folders option. Note: It is up to you  whether or not youwant to Copy items    into destination group's folder.</li>
    <li>In the Add to   Targets box, deselect YourApp and select YourAppMonkey.</li>
    <li>Click Add. The  MonkeyTalk lib should now be visible in your project.</li>
</ul>
<span style="color: #000000"><strong><span style="font-family: 'Times New Roman', serif"><span style="font-size: small">Configuring Libraries and Build Settings:</span></span></strong></span>
<ul>
    <li><span style="color: #000000"><span style="font-family: TimesNewRoman, serif"><span style="font-size: small">Right-click     on the YourAppMonkey build target, and select the Build Phases tab.</span></span></span></li>
    <li><span style="color: #000000"><span style="font-family: TimesNewRoman, serif"><span style="font-size: small">On  the Link Binaries with Libraries tab, you will need to add  libsqlite3.dylib </span></span></span><span style="color: #000000"> <span style="font-family: TimesNewRoman, serif"><span style="font-size: small">CFNetwork.framework  and QuartzCore.framework if your application is not already using </span></span></span><span style="color: #000000"> <span style="font-family: TimesNewRoman, serif"><span style="font-size: small">them.   (These frameworks are required by the MonkeyTalk).</span></span></span></li>
    <li><span style="color: #000000"><span style="font-family: TimesNewRoman, serif"><span style="font-size: small">XCode   will have added references to the libMonkeyTalk.a</span></span></span></li>
    <li><span style="color: #000000"><span style="font-family: TimesNewRoman, serif"><span style="font-size: small">On  the Build Settings tab, scroll down to the Linking section and add  to your Other Linker </span></span></span><span style="color: #000000"> <span style="font-family: TimesNewRoman, serif"><span style="font-size: small">Flags:   -all_load -lstdc++</span></span></span></li>
    <li><span style="color: #000000"><span style="font-family: TimesNewRoman, serif"><span style="font-size: small">Choose  your duplicated test target from the Scheme menu in Xcode and Run on    the </span></span></span><span style="color: #000000"> <span style="font-family: TimesNewRoman, serif"><span style="font-size: small">Simulator     or Device.</span></span></span></li></p>

## Comments

**[Subrahmanyam](#9285 "2012-09-18 17:18:02"):** Interesting, Useful and Well written !!

**[Gaurav Bansal](#9287 "2012-09-22 21:08:19"):** Nice to read such a detailed blog on MonkeyTalk. Very well written. Waiting for your next blog on best practices while going for mobile app automation.

**[Utsav](#9289 "2012-09-25 10:13:54"):** Thanks RV and Gaurav, for your thoughts.

