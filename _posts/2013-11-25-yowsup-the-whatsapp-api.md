---
layout: post
header-img: img/default-blog-pic.jpg
author: dbansal
description: 
post_id: 17631
created: 2013/11/25 22:06:31
created_gmt: 2013/11/25 17:06:31
comment_status: open
---

# Yowsup - The WhatsApp API

<p style="text-align: justify"><!-- P { margin-bottom: 0.21cm; }A:link {  } -->WhatsApp ! Do I even need to introduce it ? For a normal smartphone user, WhatsApp is the replacement of SMS service as it is free and runs on internet data of the phone. It allows us to send text as well as multimedia messages, create groups, share status etc. Lets see it formally:</p>

<p style="text-align: justify">According to <a href="http://www.whatsapp.com/">WhatsApp.com</a>:</p>

<blockquote>
<p style="padding-left: 30px;text-align: justify">“<em>WhatsApp Messenger is a cross-platform mobile messaging app which allows you to exchange messages without having to pay for SMS.</em>”</p>
</blockquote>

<p style="text-align: justify">But WhatsApp is not just another brick in the wall in terms of communication apps available in today's IT world. It has become “a must to have app” for any smartphone user.</p>

<p style="text-align: justify">According to a <a href="http://www.nextiphonenews.com/2013/11/is-facebook-inc-fb-losing-its-position/">news report</a>:</p>

<blockquote>
<p style="padding-left: 30px;text-align: justify">“<em>According to a recent survey by the statistics portal Statista, which interviewed 2,000 Apple Inc. iPhone users in Germany, <b>67 percent of users reported using WhatsApp,</b> with <b>Facebook Inc. finishing second with 63-percent use</b>.”</em></p>
</blockquote>

<p style="text-align: justify">So, we can say that if we were to replace SMS with any other communication medium, our first choice should be WhatsApp. But why to replace SMS ? Reasons are straightforward: cost, lack of media support(MMS is even costlier), cross country/region SMSes are costlier, etc. WhatsApp works on Internet which is available on almost any smartphone. Also, subscription fee is free for one year and after that a nominal charge per year.</p>

<p style="text-align: justify">So, as a developer, why should I bother about all this ?</p>

<p style="text-align: justify">You must, if your app is still dependent on emails and SMS API's for notifications and other communications. Emails are not as reliable. Someone may forget to check his email but would never miss a WhatsApp message.</p>

<p style="text-align: justify">Ok but how do I use it ?</p>

<p style="text-align: justify"><strong><span style="font-size: 22px">Yowsup !</span></strong></p>

<blockquote>
<p style="padding-left: 30px;text-align: justify" align="JUSTIFY"><em>“<a href="https://github.com/tgalal/yowsup/wiki/About-and-Features#wiki-about">Yowsup</a> is a cross platform python library that allows you to do all the previous (what WhatsApp can do) in your own app. Yowsup allows you to login and use the Whatsapp service and provides you with all capabilities of an official Whatsapp client, allowing you to create a full-fledged custom Whatsapp client.”</em></p>
</blockquote>

<p style="text-align: justify">Yowsup can be used in your application to provide all the functionalities of Whatsapp service. It allows for two way communication i.e. it can send as well as receive messages from other users just like any other WhatsApp client.</p>

<p style="text-align: justify">Basic Yowsup library provides a set of signals that you can register callbacks to. Connection Manager is responsible for calling these callbacks on the corresponding event. Library also provides a set of methods that you can use to send information(message, acknowledgement, media, typing signal etc) to the WhatsApp server.</p>

<p style="text-align: justify">Yowsup package comes bundled with some examples implemented on top of this library which fulfill basic tasks required to interact with WhatsApp server. However, you are always free to modify it according to your needs.</p>

<p style="text-align: justify">But it is python library, and I am using Java ?</p>

<p style="text-align: justify"><strong><span style="font-size: 22px">Integration with Java</span></strong></p>

<p style="text-align: justify">It comes with a CLI client that can help in integrating it with any other application that is not even written in python. Lets go through the following steps and see how can we embed this library in a Java application:</p>

<p style="text-align: justify"><em>(Note: I am using Linux. Similar steps can be followed on Windows machine).</em></p>

<ol style="text-align: justify">
<ol>
<ol>
    <li>Download the Yowsup library from the “Download Zip” link on <a href="https://github.com/tgalal/yowsup">Yowsup github page. </a>Extract its “<b>src”</b> folder in home directory <code>~/Yowsup/</code>. It should contain 2 directories(Examples, Yowsup) and 3 files.Set environment variable $PYTHONPATH to the above location.</li>
    <li>First of all we have to register a number on Whatsapp server. For this you should have an unused mobile number that should be specifically obtained for your application. (Why ? Lets keep this question for later). Edit <code>config.example</code>: put your country code in “cc” (without + or 00), mobile number obtained in “phone”, random text in “id” (14 digit). Leave password as blank.</li>
    <li>Now, open a terminal and navigate to <code>~/Yowsup/</code> and enter:
<pre>$ python yowsup-cli -c config.example -r sms</pre>
Note: if you get error like:
<pre>Traceback (most recent call last):
File yowsup-cli, line 306, in &lt;module&gt;
elif args["dbus"]:
KeyError: 'dbus'</pre>
then edit yowsup-cli and uncomment line number 207
However, if you do not receive sms use <a href="https://coderus.openrepos.net/whitesoft/whatsapp_sms">this link</a> to generate 6 digit verification code</li>
    <li>After you recieve 6 digit verification code on that number like <code>abc-def</code>(You would need a phone for that), Enter command:
<pre>$python yowsup-cli -c config.example -R abc-def</pre>
you will receive some key value pairs in output, one of which <code>pw:</code> contains your password.

Copy this password in your <code>config.example</code>'s password property.</li>
    <li>Now you are ready to send and recieve messages. For trial send a message to your personal number using following command:
<pre>$python yowsup-cli -c config.example -s &lt;your personal number including cc&gt; &lt;message&gt;</pre>
</li>
    <li>To embed it in Java, all you have to do is call this command from Java's process builder framework. For this, first create a shell script to send message. Name it <code>sendMessage.sh</code> which looks like:
<pre> #!/bin/bash
 python yowsup-cli -c config.example -s $1 "$2"</pre>
Make it executable by command: chmod +x sendMessage.sh</li>
    <li>Open a java editor and type below code in the main method (modify paths according to environment):
<pre>String message = "The message goes here";
String[] sendCommand = { "/bin/bash", “path/to/sendMessage.sh”, "&lt;to number including cc&gt;", message };
ProcessBuilder processBuilder = new ProcessBuilder();processBuilder.directory(new File(&lt;YOWSUP_HOME&gt;));
processBuilder.environment().put("PYTHONPATH", &lt;YOWSUP_HOME&gt;);
processBuilder.command(sendCommand);
System.out.println(processBuilder.environment());
Process process = processBuilder.start();
process.waitFor();</pre>
</li>
    <li>Run the above program and your message will be sent to the intended receiver.</li>
    <li>You can read the <code> process.getInputStream()</code>
to get the output of the script you executed. For ex-
<pre>InputStream stream = process.getInputStream();
BufferedReader br = new BufferedReader(new InputStreamReader(stream));
String str = null;
while ((str = br.readLine()) != null) {
    System.out.println(str);
}</pre>
</li>
</ol>

<p></ol>
</ol>
<p style="text-align: justify">Now, coming back to the question, why should I use a new unused mobile number. The answer is simple: one phone number can not be registered on two WhatsApp accounts. So as soon as you install Whatsapp on the phone which is using this number it will again verify your account and assign a new password. So, your application will become unusable because the password stored in config file is no longer valid.</p>
<p style="text-align: justify"><span style="font-size: 20px"><strong>Conclusion</strong></span></p></p>

## Comments

**[virtualpathum](#9465 "2014-03-17 19:25:58"):** Hi I just tried as you instructed but Im getting the following error E:\yowsup-master\src>python yowsup-cli -c config.example -r sms Traceback (most recent call last): File "E:\yowsup-master\src\Yowsup\Common\watime.py", line 24, in from dateutil import tz ImportError: No module named 'dateutil' During handling of the above exception, another exception occurred: Traceback (most recent call last): File "yowsup-cli", line 33, in from Examples.CmdClient import WhatsappCmdClient File "E:\yowsup-master\src\Examples\CmdClient.py", line 21, in from Yowsup.connectionmanager import YowsupConnectionManager File "E:\yowsup-master\src\Yowsup\connectionmanager.py", line 29, in from Yowsup.Common.watime import WATime File "E:\yowsup-master\src\Yowsup\Common\watime.py", line 26, in from .dateutil import tz ImportError: No module named 'Yowsup.Common.dateutil' Appreciate if you could help me to rectify this error. Thanks

**[Deepak](#9466 "2014-03-17 19:43:37"):** Please install [datetime][1] module in python.

   [1]: http://labix.org/python-dateutil (dateutil)

**[virtualpathum](#9467 "2014-03-17 19:55:53"):** I already installed datetime but still getting the same error.

**[Deepak](#9468 "2014-03-17 22:23:11"):** which version of python are you using ?

**[virtualpathum](#9469 "2014-03-18 06:47:35"):** I'm using Python 3.4 I did a easy_install python-dateutil and now it seems ok but now it is giving this error ImportError: cannot import name 'tz' What might be the reason? I don't know anything about Python. I just googling and searching for answers.

**[virtualpathum](#9472 "2014-03-18 19:15:18"):** Hey I'm still struggling to fix this issue. Appreciate if you could give me some idea. Thanks

**[virtualpathum](#9473 "2014-03-18 20:58:04"):** Ok I was able to figure out the issue and it was a stupid mistake. Now I can run the code. I configured the config.example as instructed and run the first command to get the code but It gave me the following error E:\yowsup-master\src>python yowsup-cli -c config.example -r sms reason: b'bad_param' param: b'number' status: b'fail' Then I used the given URL to get the code and I got the code as a sms Now when Im running the second command to get the password it is giving me the following error E:\yowsup-master\src>python yowsup-cli -c config.example -r 000-000 (dummy code) coderequest accepts only sms or voice as a value What might be the reason for this Thanks

**[Deepak](#9474 "2014-03-19 07:53:42"):** You should use -R and not -r in second step of registration.

**[virtualpathum](#9475 "2014-03-19 19:42:52"):** I was able to register and got the password as well. But when Im trying to send msgs it gives me Auth Failed error.

**[Deepak](#9476 "2014-03-19 20:25:29"):** Did you copy the password (including the trailing '=', if any) in config.example file ? Is country code correctly specified ? Does phone field includes country code ? Config.example file should look something like: cc=91 phone=9199xxxxxx96 id=123456789101112 password=IC1uYdT1SpTsX9XKeXj494agAxE= Please check above properties in config.example. Let me know if the problem still persists.

**[virtualpathum](#9477 "2014-03-19 20:36:11"):** Here is my config file cc=60 phone=60xxxxxxxx8 id=qwertyuiopasdf password=nczAJqnhBDRE2GVWssjldy70+QA= this is thee command I used to send message python yowsup-cli -c config.example -s 60xxxxxxxxx test still the same error

**[Deepak](#9478 "2014-03-19 22:13:52"):** Now, the only issue that I can think of is that the password has been generated again by putting the SIM card of the phone number in a Whatsapp installed phone. Actually once you generate a password using the above procedure, the whatsapp installed in the phone stops working as the password stored in phone becomes invalid (after the regeneration as mentioned above). So when you try to use whatsapp app in phone it again does registration and generates a new password. However this procedure is not visible as it is all done by whatsapp application in phone ,internally. Thus the manually generated password becomes invalid. So try to generate new password and remove whatsapp app from the phone. Then try to send the message as described above.

**[virtualpathum](#9479 "2014-03-20 19:11:05"):** Hi, It's working now. I regenerated the pin number and the password now I can send the messages. Thank you very much for your support.

**[virtualstep](#9480 "2014-04-06 21:54:55"):** Hi Deepak, thanks for the great guidance post for this topic. May i know is it possible to send image via the yowsup-cli API? Can you guide me on that? Thank you in advance.

**[ramvijay35](#9486 "2014-06-08 22:59:32"):** Is it possible to integrate yowsup in android application .If yes how to approach .Useful tips or links will be helpful ....I tried googling but dint help much . Thanz in advance

**[Deepak](#9487 "2014-06-09 09:12:22"):** I am afraid that I do not have any idea about Android with Yowsup. As far as the approach is concerned, it would be best if you are a python developer and can develop the whole application in python and then integrating Yowsup will just be a matter of calling yowsup from python classes/functions. This link might help further. https://ep2013.europython.eu/conference/talks/developing-android-apps-completely-in-python However, if you are not a python developer, and are writing android applications in Java, then the approach used in above blog might just work. But you would need something like http://qpython.com/ here.

**[manet](#9491 "2014-06-25 17:55:59"):** Hello! Every time I send messages through the API They block me after a few posts, is there a way that they do not block the posting? Please!!!

**[ramvijay35](#9489 "2014-06-12 13:16:07"):** Thanz a lot for ur response ... I ll check those out .. And now again I need ur help . I tried using methods in yowsup library .And I could able to get last time user went offline with presence_request method .But what I actually need is user's current status (available or unavailable) . I couldn't able to get that .Presence_subscribe method is also doesn't give any reply back .So need ur help .I need to get user's current online status .How to prooceed. Googling doesn't help much ...so waiting for response .... Thanz in advance

**[Deepak](#9492 "2014-06-26 08:39:29"):** Check this [link][1] and search for a similar question. I remember, I have read about some scenarios where the posts are blocked because the frequency of sending messages appears to be more than what a human can achieve.

   [1]: https://github.com/tgalal/yowsup/issues

**[Deepak](#9493 "2014-06-26 08:50:10"):** Try to use "presence_available". Check [Subscribe to users presence][1]

   [1]: https://github.com/tgalal/yowsup/issues/169

**[manet](#9494 "2014-06-26 12:53:15"):** Hey thanks for the reply but it does not give me a solution how I can send messages through the API Without Sim attitude?

