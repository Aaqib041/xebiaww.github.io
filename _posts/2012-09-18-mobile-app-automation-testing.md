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

While I attended the first XKE here at Xebia, I heard some one saying '_You are not smart... If you dont have a smart phone'_, so Being a smart tester ;) or while trying to be a smart tester I started exploring available tools to test the applications for these Smart Phones. And in this process I came accross a tool which** supports both iOS and Android**, and allows us to test **native or hybrid apps**, on **simulators or real devices** \-- **no jail breaking required**, which is **robust **and enables cross – platform recording and playback. And this great **free and open-source** functional tesing tool is called Monkey Talk. Now you know where the title is coming from. 

**Why monkey talk ?**

Well there are a lot of other available options but I went for Monkey Talk as 

  * It had powerful and robust **record and playback** functions for mobile applications.
  * Fully **cross platform**\-- record on iOS and playback on Android and vice versa.
  * Powerful IDE-- a single **unified IDE** for both iOS and Android based on Eclipse.
  * No jail breaking needed-- runs on stock hardware without alterations.Run on real hardware-- both phones and tablets are supported, either tethered or over the network.
  * **Readable scripts**\-- MonkeyTalk is simple and easy to learn, generating scripts that the average person can understand.
  * **Keyword-driven scripts**\-- MonkeyTalk scripts can be easily extended, even by non-programmers, with custom user-defined commands.
  * **Data-driven scripts**\-- any command can be looped using a CSV data file.
  * **Javascript **\-- generate JavaScript from MonkeyTalk if a full programming language is necessary.
  * **JUnit-compatible XML reports**\-- test suites output the standard XML report, making MonkeyTalk easy to integrate into existing systems.
  * **CI ready**\-- run test suites in a fully automated fashion, from the commandline, perfect for integration into any continuous integration environment.

## How to start Monkeying ?

In this blog I will take you through the set up configuration of Monkey Talk for automation of iOS and Android applications. 

## Android:-

You need to install MonkeyTalk Android Agent into the application which you want to automate.

For this, please follow below steps:

**Steps**

  * Open your Android Project in Eclipse.
  * Right click on the project name and select Configure>>Convert to AspectJ Project.

![][1]

  * Create libs” folder in your Android Project and put MonkeyTalk-agent.jar in “libs” folder. MonkeyTalk-agent.jar can be found in the "agents" folder in the MonkeyTalk package.
  * Right click on MonkeyTalk-agent.jar > AspectJ Tools > Add to Aspectpath.

![][2]

  * Open AndroidManifest.xml and include the following two permissions: o uses-permission android:name="android.permission.INTERNET" o uses-permission android:name="android.permission.GET_TASKS"

![][3]

  * Now RUN your application right click on the Project and Select Run As>>Android application.
  * You would see Android Emulator will appear and your application gets deployed on the emulator or Hardaware.

## iOS:-

You need to install MonkeyTalk iOS Agent into the application which you want to automate. For this, please follow the steps given below:
  * Open your application's project in Xcode.
  * Duplicate your application's build target by right-clicking on it and selecting Duplicate from themenu. A new target will be created called YourApp copy.
  * Rename YourApp copy to something like YourAppMonkey.
  * Add the downloaded MonkeyTalk lib to your project File > Add to “YourApp”... from the menu.
  * When the dialog box appears, navigate to the directory where you unzipped the MonkeyTalk zipfile, and select the MonkeyTalk iOS lib from pathToMonkeyTalkFolder/agents/iOS.
  * Recursively creates groups for any added folders option. Note: It is up to you whether or not youwant to Copy items into destination group's folder.
  * In the Add to Targets box, deselect YourApp and select YourAppMonkey.
  * Click Add. The MonkeyTalk lib should now be visible in your project.
**Configuring Libraries and Build Settings:**

  * Right-click on the YourAppMonkey build target, and select the Build Phases tab.
  * On the Link Binaries with Libraries tab, you will need to add libsqlite3.dylib  CFNetwork.framework and QuartzCore.framework if your application is not already using  them. (These frameworks are required by the MonkeyTalk).
  * XCode will have added references to the libMonkeyTalk.a
  * On the Build Settings tab, scroll down to the Linking section and add to your Other Linker  Flags: -all_load -lstdc++
  * Choose your duplicated test target from the Scheme menu in Xcode and Run on the  Simulator or Device.

   [1]: http://xebee.xebia.in/wp-content/uploads/2012/09/p11-300x118.jpg
   [2]: http://xebee.xebia.in/wp-content/uploads/2012/09/2-300x47.jpg
   [3]: http://xebee.xebia.in/wp-content/uploads/2012/09/3-300x91.jpg

## Comments

**[Subrahmanyam](#9285 "2012-09-18 17:18:02"):** Interesting, Useful and Well written !!

**[Gaurav Bansal](#9287 "2012-09-22 21:08:19"):** Nice to read such a detailed blog on MonkeyTalk. Very well written. Waiting for your next blog on best practices while going for mobile app automation.

**[Utsav](#9289 "2012-09-25 10:13:54"):** Thanks RV and Gaurav, for your thoughts.

