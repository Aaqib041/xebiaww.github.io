---
layout: post
header-img: img/default-blog-pic.jpg
author: vhazratiblog
description: 
post_id: 376
created: 2007/12/31 17:01:40
created_gmt: 2007/12/31 15:01:40
comment_status: open
---

# Introducing Android: The future is mobile!

![android_icon_125.png][1]A couple of weeks back I was at the [TiE Summit 2007][2]. There I could hardly find anyone who was not interested in the mobile space. Mobile phone space definitely is something which cannot be missed given that today the number of mobile phones far out number the number of personal computers. Also, with each passing day they are getting better memory and better computing power. It would soon become a necessity for all major e-commerce sites to offer their services over the mobile phone assuming that they are not already working on it. Apart from this there is a lot of potential for specific mobile phone applications which would help the consumer do better personally and professionally.

**Almost coinciding with the mobile space euphoria, Google released Android.** It is a a fully integrated mobile "software stack" that consists of an operating system, middleware, user-friendly interface and applications. The important objective is to deliver a vastly improved web experience on mobile devices, equal to what people can experience on a desktop computer, in contrast to the limited functionality on today's mobile phones.

At the TiE Summit I also heard many speakers saying that** “portal is dead.”** **The next technology wave is dependent on the platform**. Many applications and services would be built depending on the platform and a business would be successful depending on which platform does it align with. So if you align on the Android platform then assuming that Android is successful, you could be successful as an Android service provider, Android powered device maker etc etc. HTC would be launching Android powered phones in 2008.

Given all of the above I decided to check out the Android offering over the weekend and here are my initial findings  Android is based on linux 2.6 and applications are written using Java, wow!, however interestingly there is no JVM. Instead google uses its own VM called [Dalvik][3]. The Android SDK does not compile your Java source code into Dalvik’s bytecode directly, but it first uses a regular java compiler to generate regular java bytecode and then converts that bytecode into Dalvik’s bytecode. So, Android uses the syntax of the Java platform (the Java “language”, if you wish, which is enough to make java programmers feel at home and IDEs to support the editing smoothly) and the java SE class library but not the Java bytecode or the Java virtual machine to execute it on the phone.

The software stack consists of [4 layers][4]. 

  * Application layer
  * Application framework
  * Libraries and runtime
  * Kernel

Applications consist of four major components: Activity (main UI activities, such as viewing a list of tasks), IntentReceiver (notification registration and event triggering), Service (faceless tasks that run in background) and ContentProvider (sharing data with other applications). As you can see that there is decoupling which provides modularization and extension mechanism. Android provides pretty sophisticated graphics capabilities and has Location manager functionality which would help you to connect with other devices etc.

If all this sounds interesting, for getting started with the understanding about Anatomy of an application, Development tools and application life cycle it would be good to start [here.][5]

Getting started is fairly straight forward, down the sdk from [here][6]. Set up the eclipse plugin and run through the sample application of [HelloAndroid][7] and complete the [tutorial][8] for better understanding.

After going through the above details I decided that the best way to learn is to hit the IDE. What follows is a small application that I developed. Of this application would not get you anywhere close to the [Android Developer Challenge][9] however it did help me learn a few things about the platform in a couple of hours.

The simple application that I developed is a Number guessing application, where the system generates a number between 1 to 99 and the user guesses that. The screens look like this. _The screenshots are from different game plays and may not logically relate to each other, however the flow of the application remains the same._

![Mobile-Menu.png][10]![Guess_Start.png][11]![Guess_number.png][12]![Guess_number_2.png][13]![Guessed.png][14]

The source code for the application can be found [here][15]. Please note that I developed and tested the application on **HVGA-P** emulator. You might have to tweak the application for making it cross functional across emulators.

**Conclusion:** All in all I had a good time working with Android. The eclipse plugin worked well and the emulator speed was great. I could quickly make changes and run the emulator from eclipse and see the results. From a development standpoint, Android is a powerful SDK. It combines XML layout schemes with custom view rendering and provides a lot of extras like maps and inbuilt widgets. On the flip side, I hunted a bit for the source code to attach to my IDE but I could not find that, may be that is slated for a later release. Though the documentation seems to be just enough to get started I was a bit lost when I was hunting for some specific things, I guess as I write this there is some work going on to improve the existing documentation.

**Is 2008 going to be the year of the mobile?**

   [1]: http://xebee.xebia.in/wp-content/uploads/2007/12/android_icon_125.png
   [2]: http://www.tiesummit.org/
   [3]: http://code.google.com/android/what-is-android.html
   [4]: http://code.google.com/android/images/system-architecture.jpg
   [5]: http://code.google.com/android/intro/index.html
   [6]: http://code.google.com/android/index.html
   [7]: http://code.google.com/android/intro/hello-android.html
   [8]: http://code.google.com/android/intro/tutorial.html
   [9]: http://code.google.com/android/adc.html
   [10]: http://xebee.xebia.in/wp-content/uploads/2007/12/Mobile-Menu.png
   [11]: http://xebee.xebia.in/wp-content/uploads/2007/12/Guess_Start.png
   [12]: http://xebee.xebia.in/wp-content/uploads/2007/12/Guess_number.png
   [13]: http://xebee.xebia.in/wp-content/uploads/2007/12/Guess_number_2.png
   [14]: http://xebee.xebia.in/wp-content/uploads/2007/12/Guessed.png
   [15]: http://xebee.xebia.in/wp-content/uploads/2007/12/GuessNumber-tar.gz