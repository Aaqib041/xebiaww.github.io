---
layout: post
header-img: img/default-blog-pic.jpg
author: jitens
description: 
post_id: 17118
created: 2013/09/10 15:54:16
created_gmt: 2013/09/10 10:54:16
comment_status: open
---

# Android Permissions : An invisible Trap ?

<p style="text-align: justify;">As the world moves in the era of Smartphones, People are now using Smartphones for almost everything they did earlier on their PC's. The biggest advantage of this transformation has been the inclusion on non-technical people in the growth of Technological advancements.</p>

<p style="text-align: justify;"> If we carefully look at the smartphones and the way they have changed our lives we will realise they have become a literally become an ocean of our lifestyle details and personal memories. The Kodak moments of Life are now being captured by Smart Phone cameras. Mobile Galleries have become new personal Photo Albums storage. The Letters or Telegrams have now vanished and SMS and Emails are the new way to communicate. Instead of signing Guestbook or Registers in Hotels or Shops, we are now using Check in features in smart Phone apps. Personal Meetings with friends are being increasingly replaced by Google Hangouts or Skype calls. Phone calls are being replaced by Voice chat apps like Viber, WeChat and Whatsapp. And all of these transformations are happening with increasingly rising capacity and features of Smartphones. And everything mentioned above when done using one single smartphones is being captured and stored forever. For us to look back and for someone else to spy on .Since all of these apps come FREE, we love to quickly download them and install them without bothering what mess we are landing into or what backdoors we are opening in our virtual lives.</p>

<p style="text-align: justify;">Let’s take the case of Andorid. With Millions of Free apps being available at Google appstore and 1000s new being added every week, there is virtually a flood of applications but with none or almost failed control mechanisms. The automated behaviour scans or bots being used by Google to do the behaviour analysis of these apps to filter out malicious ones are failing miserably. Hackers are easily tricking Google scanners and bots and 1000s of malicious apps are now available on Google Play or Appstore.<strong> All you need to do is to fool the Bots and Scanner for 5 minutes because that is the max time Google normally scans an application before uploading on Google Play.</strong></p>

<p style="text-align: justify;">Now interesting I don’t want to talk about these malicious apps as they at the outset are specifically designed to steal, infect or spy. Today I will be writing about the risk and dangers genuine, reputed apps are offering. The ones like Google, Facebook, Whatsapp, Wechat and games like Temple Run. Normally app downloads by Normal users are only triggered by 3 major factors.</p>

<p style="text-align: justify;">1) Usability<br />
2) Reputation<br />
3) Brand</p>

<p style="text-align: justify;">Take the example of Facebook. We simply download or update whatever Facebook produces. We install Facebook apps just coz of Facebook's reputation and brand. Same is case with Google, Tencents or EA sports. We often do not check what does applications offered by these big names are actually can or are actually doing on our phones at backend.</p>

<p style="text-align: justify;">Since the majority of users of these applications are non techies or to be liberal Lets include techies also. We Users hardly care and bother about the permissions we give to an app during installation. Permissions screen while installing an app is often treated like some terms and agreement Contract or some Just 'press next button' screen to install these applications .<strong>These same mindless permissions granting have today created a racket of malicious activities even by reputed applications.</strong></p>

<p style="text-align: justify;">During a recent study we came across many games which ask users to permit them to read and write their SMS, Phone book, Call Register and Gallery. Now what business these games have with your Gallery is not a million dollar Question. Permission taken for Genuine Reasons may be used without Knowledge in future too.</p>

<p style="text-align: justify;">Let’s take example of Whatsapp, The most popular instant messaging app. It takes permission to read and write SMS messages. Reason? Because it says it wants to send and read a confirmation SMS for the verification of user during installation. Fair enough! But what is the guarantee of Whatsapp not reading your messages after installation?????</p>

<p style="text-align: justify;">We tested this scenario on a Demo app by first uploading it with all fair code on Google Play to send an SMS during installation. Later we released an update with a malicious module to read and upload a copy of every SMS device received. Now none of the anti-virus or malware scanner was able to detect this malicious activity. Reason: User had given the SMS permission to the app during first installation and since that permission is a Universal Permission for its Infinite executions. Worst even a reputed Behaviour analysis tool was not able to raise any alarm.</p>

<p>Now take the example of Facebook and see what all permissions it takes during installation <a href="http://xebee.xebia.in/index.php/2013/09/10/android-permissions-an-invisible-trap/screenshot_2013-09-10-14-45-27/" rel="attachment wp-att-17119"><img class="wp-image-17119 alignleft" alt="Screenshot_2013-09-10-14-45-27" src="http://xebee.xebia.in/wp-content/uploads/2013/09/Screenshot_2013-09-10-14-45-27.png" width="288" height="461" /></a> <a href="http://xebee.xebia.in/index.php/2013/09/10/android-permissions-an-invisible-trap/screenshot_2013-09-10-14-45-57/" rel="attachment wp-att-17120"><img class="aligncenter  wp-image-17120" alt="Screenshot_2013-09-10-14-45-57" src="http://xebee.xebia.in/wp-content/uploads/2013/09/Screenshot_2013-09-10-14-45-57.png" width="288" height="461" /></a></p>

<p style="text-align: justify;"> Now initially some of them may look fishy and cast doubt but Facebook has all nice reasons to explain each of them. and even if one is to somehow get their app source code today everything might seem alright.  But then this is where the trick lies. Because if tomorrow Facebook is to modify their code and use those same permissions for unwarranted activities without your knowledge (They have permission to download updates without notifications :) ). They will have an easy ride because as per Android Architecture and Google policies An update installation does not require verification or revalidation of permissions by the user. Updates are simply installed on one click with no behaviour change notifications to the user.</p>

<p style="text-align: justify;">The reason i am writing and targeting Android permission architecture is that it needs a complete makeover. We discovered that a reputed app 'WeChat' from Tencent Holdings in China was in fact using this same permissions loophole to do mass espionage for a foreign government. (I will be writing about it in Detail in the Next blog). But what’s the solution. Stop installing apps? Because automated scanner and bots are failing, because Reputations, Brand and Number of Users are all  now failed Benchmark’s for trusting an application.</p>

<p style="text-align: justify;"><strong>May be it’s a wise idea to Split Google Permission with a Number of accompanies check. Things like attempts lock, Usage boundaries, May be using duration limiting philosophy by giving Time Barred Permissions or providing API access logs to the user for every app regularly.  Giving or revoking selective permissions without breaking the application may be another solution.</strong></p>

<p style="text-align: justify;">Google has invited me to speak on the Android Permission architecture in Annual Google Devfest in Singapore next Month. One Man's Mind is often a half filled pot with a lot of space for more ideas. Feel free to share with me your ideas on Permission architecture overhaul in Android and make it a safer place.</p>

<p>http://www.xebia.in/mobile-application-development.html</p>

## Comments

**[bgarvelink](#9440 "2013-09-10 16:16:21"):** Great post! I agree with your assessment that the Android permissions model is broken in its current form (Windows Phone's model is similar) The "on install" permission request is awkward not just because of the reasons you mention above, but also for app developers because the permission request comes completely out of context. Imagine adding a "find a nearby branch office" feature to an existing B2C app: you'll have to justify this permission request to updating users somehow and people who are unwilling to grant it will stop updating your app. I myself have stopped updating Meetup.com since they started asking for Calendar access. The "on access" model in iOS allows finer grained access control and the permission requests happen in context and users can make a much more informed decision. There's hope for Android though. The hidden Application Permissions screen that was uncovered in 4.3 has apps appearing after the first time they use any kind of privileged action. This is, by itself useless, because any app would have "made off with the loot" before you have a chance to block it. Clearly, an upcoming Android release will come with just-in-time permission pop-ups to complement the maintenance screen that's already in Android 4.3. I also agree with you that apps are currently far too greedy in asking permissions. Your Whatsapp SMS example is a good one, there's no justification for this and indeed, Whatsapp for iOS and Windows Phone do perfectly fine without it. This requires a cultural change among app developers that technology can't solve on its own, although I'm sure the brutal transparency inherent in on-access pop-ups is a helpful stimulus here. That leaves just Windows Phone to get its permissions act together.

**[Jiten](#9441 "2013-09-10 16:35:16"):** Yeah Just in time permissions may also be a good idea for one time usage features :) lets connect on twitter. My handle is jiten_jain

