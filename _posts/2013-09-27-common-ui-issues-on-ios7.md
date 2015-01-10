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

Apple came out with a new version of the iOS operating system recently. iOS 7 has many new features, not the least of which is a new clean UI (which seems inspired by Android & Windows Metro to a large extent). However, I'm not going into the merits or demerits of such a design approach here. My issue is something more basic. I upgraded my devices on the dot when iOS7 went live and, almost immediately, saw that a bunch of the apps I had created were broken (to the eye, at least). Here's my journey of fixing the most common design issues that you, as a iOS developer, might also face from your customers.  Some common UI elements that are now broken: 

  1. UI overlap with status bar
  2. UiTableView Background colour becomes White
  3. Navigation Bar colour  becomes white
I tinkered with the code around these issues and below are some quick fixes. 

### 1\. UI overlap with status bar

If we are using storyboard and auto layout features in our development then it will work fine.But in case we are no using these feature then our UI will overlap.

[gallery ids="17234,17233,17232"]

If we want to hide states bar, We generally use this method to hide status bar `[[UIApplication sharedApplication] setStatusBarHidden:YES]; ` But now in iOS 7 it will not work more. Now there can be two approaches to achieve it in iOS7. Either Set "View controller-based status bar appearance" and set it to "NO". in plist. or we can call: `(BOOL)prefersStatusBarHidden { return YES; }` in application root class. And if you want status bar to be present in application, there are following ways to handle it: 1) Uncheck "Extend edges under top bar " property in interface builder. 2) Set iOS 6/7 Deltas values  and mention document as iOS6 view from interface builder and set 

### 2\. UiTableView Background color becomes White

If we want to clear UITableviewCell background colour we commonly use: `[UITableview setBackgroundColor :[UIColor clearColor]];` and it will clear UITableView as well as there cells background clear.But when we play our app in ios7 cells show a white background colour. So we need now to clear table cell background colour as well. `[cell setBackgroundColor:[UIColor clearColor]]`

### 3\. Navigation Bar colour  becomes white

In iOS7 self.navigationController.navigationBar.tintColor = [UIColor ...] will not work more. The behaviour of tintColor has changed in iOS7. So, to set navigation controller colour, we have to set first of all navigation bar Translucent property false. Like this `[self.navigationController.navigationBar setTranslucent:NO]; [self.navigationController.navigationBar setBarTintColor:[UIColor colorWithRed:14.0/255.0 green:114.0/255.0 blue:199.0/255.0 alpha:1];`

For more references, check out the iOS Human Interface Guidelines <https://developer.apple.com/library/ios/design/index.html#//apple_ref/doc/uid/TP40013289>