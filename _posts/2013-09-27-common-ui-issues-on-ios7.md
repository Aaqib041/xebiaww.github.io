---
layout: post
header-img: img/default-blog-pic.jpg
author: Abhay
description: 
post_id: 17223
created: 2013/09/27 19:19:50
created_gmt: 2013/09/27 14:19:50
comment_status: open
---

# Common UI Issues on iOS7

<p>Apple came out with a new version of the iOS operating system recently. iOS 7 has many new features, not the least of which is a new clean UI (which seems inspired by Android &amp; Windows Metro to a large extent). However, I'm not going into the merits or demerits of such a design approach here. My issue is something more basic. I upgraded my devices on the dot when iOS7 went live and, almost immediately, saw that a bunch of the apps I had created were broken (to the eye, at least). Here's my journey of fixing the most common design issues that you, as a iOS developer, might also face from your customers.
<!--more-->
Some common UI elements that are now broken:
<ol>
    <li><span style="line-height: 1.5em;">UI overlap with status bar</span></li>
    <li><span style="line-height: 1.5em;">UiTableView Background colour becomes White</span></li>
    <li><span style="line-height: 1.5em;">Navigation Bar colour  becomes white</span></li>
</ol>
I tinkered with the code around these issues and below are some quick fixes.
<h3>1. UI overlap with status bar</h3>
If we are using storyboard and auto layout features in our development then it will work fine.But in case we are no using these feature then our UI will overlap.</p>
<p>[gallery ids="17234,17233,17232"]</p>
<p>If we want to hide states bar, We generally use this method to hide status bar
<code>[[UIApplication sharedApplication] setStatusBarHidden:YES]; </code>
But now in iOS 7 it will not work more. Now there can be two approaches to achieve it in iOS7.
Either Set "View controller-based status bar appearance" and set it to "NO". in plist.
or
we can call:
<code>(BOOL)prefersStatusBarHidden {
return YES;
}</code>
in application root class.
And if you want status bar to be present in application, there are following ways to handle it:
1) Uncheck "Extend edges under top bar " property in interface builder.
2) Set iOS 6/7 Deltas values  and mention document as iOS6 view from interface builder and set
<h3>2. UiTableView Background color becomes White</h3>
If we want to clear UITableviewCell background colour we commonly use:
<code>[UITableview setBackgroundColor :[UIColor clearColor]];</code> and it will clear UITableView as well as there cells background clear.But when we play our app in ios7 cells show a white background colour.
So we need now to clear table cell background colour as well.
<code>[cell setBackgroundColor:[UIColor clearColor]]</code>
<h3>3. Navigation Bar colour  becomes white</h3>
In iOS7 self.navigationController.navigationBar.tintColor = [UIColor ...] will not work more. The behaviour of tintColor has changed in iOS7. So, to set navigation controller colour, we have to set first of all navigation bar Translucent property false.
Like this
<code>[self.navigationController.navigationBar setTranslucent:NO];
[self.navigationController.navigationBar setBarTintColor:[UIColor colorWithRed:14.0/255.0 green:114.0/255.0 blue:199.0/255.0 alpha:1];</code></p>
<p>For more references, check out the iOS Human Interface Guidelines <a title="https://developer.apple.com/library/ios/design/index.html#//apple_ref/doc/uid/TP40013289" href="https://developer.apple.com/library/ios/design/index.html#//apple_ref/doc/uid/TP40013289" target="_blank">https://developer.apple.com/library/ios/design/index.html#//apple_ref/doc/uid/TP40013289</a></p>