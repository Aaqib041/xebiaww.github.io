---
layout: post
header-img: img/default-blog-pic.jpg
author: vaibhav sehgal
description: 
post_id: 8278
created: 2011/04/13 13:15:01
created_gmt: 2011/04/13 08:15:01
comment_status: open
---

# Local Notifications in iPhone

<p>Suppose we want to build an iPhone application that alerts the user at a particular point in time or at repeated intervals of time with some custom message, or that the application runs in the background and performs some task, and alerts the user when the task is complete. This can be accomplished in iPhone using <strong>local notifications,</strong> which were introduced in iOS 4.0. Once a local notification is scheduled by an application, iOS takes care of it and presents it at the appropriate time.</p>
<p>This blog aims to show how to set local notifications in an iPhone application and then moves on to specific use cases where they have to be manipulated. We also talk about which methods are called when a notification is received, depending on the state of the application.
<!--more-->
The following code shows how to set a local notification that will alert the user at 7:00 a.m. everyday.</p>
<p>[sourcecode lang="javascript"]</p>
<p>// Create a fire date for the notification by using calendar components</p>
<p>NSCalendar *calendar = [NSCalendar autoupdatingCurrentCalendar] ;</p>
<p>NSDateComponents *componentsForReferenceDate =</p>
<p>[calendar components:(NSDayCalendarUnit | NSYearCalendarUnit | NSMonthCalendarUnit ) fromDate:[NSDate date]];</p>
<p>[componentsForReferenceDate setDay:1] ;
[componentsForReferenceDate setMonth:1] ;
[componentsForReferenceDate setYear:2011] ;</p>
<p>NSDate *referenceDate = [calendar dateFromComponents:componentsForReferenceDate] ;</p>
<p>// set components for time 7:00 a.m.</p>
<p>NSDateComponents *componentsForFireDate =</p>
<p>[calendar components:(NSYearCalendarUnit | NSHourCalendarUnit | NSMinuteCalendarUnit| NSSecondCalendarUnit ) fromDate: referenceDate];</p>
<p>[componentsForFireDate setHour: 7] ;
[componentsForFireDate setMinute:0] ;
[componentsForFireDate setSecond:0] ;</p>
<p>NSDate *fireDateOfNotification = [calendar dateFromComponents: componentsForFireDate];</p>
<p>// Create the notification</p>
<p>UILocalNotification *notification = [[UILocalNotification alloc]  init] ;</p>
<p>notification.fireDate = fireDateOfNotification ;
notification.timeZone = [NSTimeZone localTimeZone] ;
notification.alertBody = [NSString stringWithFormat: @&quot;Wake up!&quot;] ;
notification.userInfo= [NSDictionary dictionaryWithObject:[NSString stringWithFormat:@&quot;Some information&quot;] forKey:@&quot;information&quot;];
notification.repeatInterval= NSDayCalendarUnit ;
notification.soundName=UILocalNotificationDefaultSoundName;</p>
<p>[[UIApplication sharedApplication] scheduleLocalNotification:notification] ;</p>
<p>[notification release];
[/sourcecode]</p>
<p>The point to be noted in this example is that the <strong>fireDate</strong> of the notification is 1/1/2011 at 7 a.m. . This means that the notification will first fire at 7 a.m. on January 1 . Moreover, the <strong>repeatInterval</strong> property of the notification is set to <strong>NSDayCalendarUnit</strong> (i.e. the notification will repeat after a time interval of one day). Thus the notification will also fire at 7 a.m. on January 2,January 3 and so on for each day. However, if we check the fireDate property of the notification on January 2, then it will still be 7 a.m. on 1/1/2011. However on January 2,  the "<strong>next fire date</strong>" will be 7 a.m. , 2/1/2011.  Unfortunately we cannot access this "next fire date" property.</p>
<p>A classic example of a situation where this might have been of use, is say, when notifications which repeat daily (say two notifications to remind the user to get food items from the market) have been set with fireDate as January 1, and the user launches the application on January 2 and changes some parameter in the application (say, he indicates that today is a holiday and he can't get food items from the market), we need to delete the notifications for the current day (i.e. 2/1/2011). The ideal case would have been to isolate all notifications which would fire on January 2 and delete them only. This would mean that the notifications would still fire on January 3 .In this case we cannot use the fireDate property to isolate notifications that have to fire on January 2, since their fireDate is still set to 1/1/2011.</p>
<p>To accomplish this task, we can use the userInfo property of UILocalNotification. We can set a key and corresponding value in the userInfo Dictionary of the notifications that we set.</p>
<p>[sourcecode lang="javascript"]
notification.userInfo = [NSDictionary dictionaryWithObjectsAndKeys:[NSString stringWithFormat:@&quot;Get Food&quot;], @&quot;foodItem&quot;, nil] ;
[/sourcecode]</p>
<p>Then, we would first get all scheduled notifications by :</p>
<p>[sourcecode lang="javascript"]
[[UIApplication sharedApplication] scheduledLocalNotifications] ;
[/sourcecode]</p>
<p>Now we could loop through this list of scheduled notifications and look for a match according to key. We can thus isolate the notifications and delete them.</p>
<p>[sourcecode lang="javascript"]
NSArray *arrayOfLocalNotifications = [[UIApplication sharedApplication] scheduledLocalNotifications] ;</p>
<p>for (UILocalNotification *localNotification in arrayOfLocalNotifications) {</p>
<pre><code>if ([[localNotification.userInfo valueForKey:@&amp;quot;foodItem&amp;quot;] isEqualToString:@&amp;quot;Get Food&amp;quot;]) {
    [[UIApplication sharedApplication] cancelLocalNotification:localNotification] ; // delete the notification from the system
}
</code></pre>
<p>}
[/sourcecode]</p>
<p>Further, the SDK also gives us methods to know whether a notification was received :
<ul>
    <li>When the user presses the action button on a notification that is received, if the application is running in the <strong>foreground</strong>, or in the <strong>background</strong>, the <strong>application:didReceiveLocalNotification</strong>: method of the application delegate is called.</li>
</ul>
[sourcecode lang="javascript"]
- (void)application:(UIApplication <em>)application didReceiveLocalNotification:(UILocalNotification </em>)notification
{
       //Do something with the notification or its data.
}
[/sourcecode]
<ul>
    <li>However, if the <strong>application has been killed</strong> and then the user presses the action button on the  notification that is received, the <strong>application:didFinishLaunchingWithOptions</strong>: is called . Here we can access the notification by using the <strong>UIApplicationLaunchOptionsLocalNotificationKey</strong> key.</li>
</ul>
[sourcecode lang="javascript"]
- (BOOL)application:(UIApplication <em>)application didFinishLaunchingWithOptions:(NSDictionary </em>)launchOptions
{
    UILocalNotification *localNotification = [launchOptions valueForKey:@&quot;UIApplicationLaunchOptionsLocalNotificationKey&quot;] ;
    // Do something with the notification
}
[/sourcecode]</p>

## Comments

**[Srinivas](#5603 "2011-05-31 16:36:20"):** Hi....Vaibhav Sehgal thanks for this information abt local Notification its very helpful to me to complete my project.. i have a small issue is set the sheduleNotification For every Drinking tea at 9.00 am......is there any Method from Startdate to end date:

**[sumanth kodi](#5609 "2011-06-02 16:11:02"):** very helpfull blog , for me it helped me a lot...,,,,,,

**[Ajay](#5970 "2011-10-03 10:27:18"):** Hi, Thnx for Nice tutorial. Can I set the my application to Open any View , instead of default UIAlertView when any notifications appears ,in background ?

**[Naveen Nagumalli](#5946 "2011-09-27 17:28:12"):** Hi Vaibhav, The article about local notification is good but im not able to retrive the local notification user info when cancelling a particular notification.

**[manoj](#6812 "2012-01-10 18:36:49"):** It was really nice.and i have done local notification with the help of your tutorial. thank u vey much...

**[Charlanne Kallay](#7069 "2012-01-22 18:29:55"):** I am not a programer but am looking for an APP that would alert me at preset intervals to stand instead of sit or to take a walking break over a particular interval of time after which the standing breaks would initiate again. Such an APP is useful for anyone who gets immersed in computer work but is also beneficial for those who already have back or nerve issues that need this kind of practice to be endurable.

