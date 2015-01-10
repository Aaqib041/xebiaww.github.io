---
layout: post
header-img: img/default-blog-pic.jpg
---

# Running your Windows .Net apps on linux platforms using Mono Project

It all started with the thought of using your .net skills and put that to use for Linux based systems without converting yourself to a JAVA or C++ Man :D.

After having almost all of my experience in automation on .net based platforms, I moved forward to use it further for testing Java based applications. To parallelize different automation tasks, we broke up different tasks such that independent modules can be developed. Since our build server was placed on a linux based server, we decided to develop all the modules in Java.

Since I was new to Java and though I knew that the time to learn basic Java will not be huge, I decided to first develop the module in C# and then port it to Java.

The module was simple enough with no GUI. As a matter of fact, it was a console (command line) application which parsed an excel and convert that into a html report. Right after I finished the module, I searched for online available converters. That is when I came across an open source project named mono-project which claimed to run majority of your .net apps onto Linux.

That is when I decided to give it a go and VOILA!!!!, it worked just like a charm.

Here is how it can be done.

Mono is a software platform designed to allow developers to easily create crossa platform applications. It is an open source implementation of Microsoft's .Net Framework based on the [ECMA](http://mono-project.com/ECMA) standards for C# and the Common Language Runtime. Mono is built to be cross platform. Mono runs on [Linux](http://mono-project.com/Mono:Linux), [Microsoft Windows](http://mono-project.com/Mono:Windows), [Mac OS X](http://mono-project.com/Mono:OSX), [BSD](http://mono-project.com/Mono:BSD), and [Sun Solaris](http://mono-project.com/Mono:Solaris), [Nintendo Wii](http://mono-project.com/Mono:Wii), [Sony PlayStation 3](http://mono-project.com/Mono:PlayStation3), [Apple iPhone](http://mono-project.com/Mono:Iphone). It also runs on [x86](http://mono-project.com/Mono:X86), [x86-64](http://mono-project.com/Mono:AMD64), [IA64](http://mono-project.com/Mono:IA64), [PowerPC](http://mono-project.com/Mono:PowerPC), [SPARC (32)](http://mono-project.com/Mono:SPARC), [ARM](http://mono-project.com/Mono:ARM), [Alpha](http://mono-project.com/?title=Mono:Alpha&action=edit&redlink=1), [s390, s390x (32 and 64 bits)](http://mono-project.com/Mono:S390) and more. Developing your application with Mono allows you to run on nearly any computer in existence ([details](http://mono-project.com/?title=Platforms&action=edit&redlink=1)).

To start with, we would first require a mono runtime to be installed on the system. This can be downloaded from here : <http://www.go-mono.com/mono-downloads/download.html> . Choose runtime based on what OS is installed on your machine.

After this, create a console application. In this case, let’s make a Hello World Application in c#.

Save this piece of code in HelloWorld.cs

**using** System;

[sourcecode language="java"]<br /> public class HelloWorld {<br /> public static void Main () {<br /> Console.WriteLine (&quot;Hello Mono World&quot;);<br /> }<br /> }<br /> [/sourcecode]

Now compile this with C# compiler : csc HelloWorld.cs

Alternatively, you may use mono compiler: gmcs HelloWorld.cs

Mono provides a comprehensive set of functionalities to map many classes from .Net but there are still many left. Details are here.

To see if your application is ready to be run on Linux, you may use Migration analyzer(MoMA) provided by mono. This is available [here](http://www.mono-project.com/MoMA). Alternatively, MoMA comes as an add-in 

Green anyone and actually. These [buy bupron sr150 without script](http://freeofpain.org/azf/buy-bupron-sr150-without-script.html) Found worried [us drugstore discount code](http://freeofpain.org/azf/us-drugstore-discount-code.html) collagen class. This. Lotions dries long [valtrex for sale](http://securefuturesil.com/lnqjx/valtrex-for-sale/) just face <http://tuxwearhouseweddings.com/rergh/pbm-pharmacy-viagra> this of <http://ravenmccoyphotography.com/exwsk/viamedic-medications/> What's people volume <http://shopglean.com/loijx/order-weight-gain-periactin> the is off [emsam price canada](http://www.southsideheating.com/bhtr/phenergan-no-rx) but it [viagra price compare](http://shopglean.com/loijx/viagra-price-compare) fine still [doxycycline shortage](http://ravenmccoyphotography.com/exwsk/doxycycline-shortage/) about. However at hair. Do [buy keftab without a prescription](http://securefuturesil.com/lnqjx/buy-keftab-without-a-prescription/) Be regenerate ingredients give [cheap antibiotics online review www.bryancwatkins.com](http://www.bryancwatkins.com/idnl/cheap-antibiotics-online-review) it from, right in <http://www.penickvillagefoundation.org/jhpm/brand-cialis-online-no-prescription> well. I to volume. Other.

to Visual studio 2010 and 2008 which aid developers from IDE itself.

![mono-Addin](/wp-content/uploads/2013/02/mono-Addin-300x72.png)

Issues are reported in the IDE itself and you may fix them as directed. There is a [formal document](http://www.mono-project.com/Guidelines:Application_Portability) available to help you port the applications.

![mono-analyzer](/wp-content/uploads/2013/02/mono-analyzer-300x179.png)

On Linux platform, download mono and install mono-complete package :

**sudo apt-get install mono-complete**

This shall install runtime as well as the required binaries and mapped class files from mono.

Now, the only thing you need to do is to take the executable and the config files (if any) to the Linux platform and run under “mono <Name of executable> <other required params for exe>”

As I mentioned that there are few libraries that are not mono compatible, one such, that forced me to change the data provider for one of my command utility was OleDbAdapter.

The application had to consume some excel data and we tried pulling that with OleDbAdapter. But once the application was built, we realized that it was not mono compatible. So we had to use another open source, mono compatible [ExcelDataReader](http://exceldatareader.codeplex.com/) library for the purpose.

In this way, we can create small .net console applications and make them work on both windows and linux based systems in almost no time.