---
layout: post
header-img: img/default-blog-pic.jpg
author: GauravS
description: 
post_id: 10618
created: 2011/12/28 13:49:03
created_gmt: 2011/12/28 08:49:03
comment_status: open
---

# iOS5 Storyboards: Custom Segue

iOS5 have introduced **Storyboards**, using which you can connect your static views and easily prepare a navigation workflow. Prior to iOS5, we used to have .xib files for every view controller. With Storyboards, all the views (and their controllers) are in one single file with an extension .storyboard. View navigation using storyboard happens with **Segues**. Segues define a way using which you can define things like transition type, data flow, etc. To navigate from one view to another, you simply need to CTRL CLICK and DRAG from the source view controller to the destination view controller. For example, if on a click of a button you want the next screen to show up, CTRL CLICK and DRAG from that button to the desired view controller. When you release, you can see three options – PUSH, MODAL and CUSTOM. Push and Modal are self explanatory. But when do you want to use Custom Segues? Before the answer, lets have a look at a use case:

A table view controller shows a list of certain categories. When you select a category, a new table view controller is pushed on to the stack and shows the available items in the selected category.

![][1]![][2]

 

 

 

 

 

 

 

 

 

 

 

It looked like i can easily use Segues here. In the .storyboard file, i created the navigation using segues for both the views, and using prepareForSegue: i passed on the required values. But this approach had a loophole. Whenever we use a Push or Modal segue, we are basically pushing a view controller on to the navigation stack. In my case, i would not like the navigationController to be full of the view controllers every time i have to select the item/category. But on the other hand, i do want to loose the ease of prepareForSegue: method. Then, whats the solution? The solution is to create a Custom Segue.

A custom segue extends [UIStoryboardSegue][3]. UIStoryboardSegue contains following properties: 

  * sourceViewController
  * destinationViewController
So you can easily specify the transition and navigation type between the source and destination view controllers. A custom segue class contains a **perform** method, where in you can specify the desired behavior.

When you create a segue between views, choose Custom from the options and specify your custom class in the Attributes Inspector (CMD+OPT+4) under Segue Class. In your custom segue class you can write something like below:

``` 
 \- (void) perform {
    
    
    UIViewController *src = (UIViewController *) self.sourceViewController;
    
    [UIView transitionWithView:src.navigationController.view duration:0.1
                       options:UIViewAnimationOptionTransitionNone
                    animations:^{
                        [src.navigationController popViewControllerAnimated:YES];
                    }
                    completion:NULL];
    

} 
 ```

As you can see i am popping out the current view controller. So now when i perform a navigation between my views using this custom segue, i will be popping out the current view. With this, i am not filling my navigationController with unnecessary view controllers, and i am also able to use the power of prepareForSegue method.

I am attaching a working sample. Please download it from [here][4].

   [1]: http://xebee.xebia.in/wp-content/uploads/2011/12/Screen-Shot-2011-12-28-at-10.33.09-AM-159x300.png (Screen Shot 2011-12-28 at 10.33.09 AM)
   [2]: http://xebee.xebia.in/wp-content/uploads/2011/12/Screen-Shot-2011-12-28-at-10.36.36-AM-159x300.png (Screen Shot 2011-12-28 at 10.36.36 AM)
   [3]: http://developer.apple.com/library/IOs/#documentation/UIKit/Reference/UIStoryboardSegue_Class/Reference/Reference.html
   [4]: http://xebee.xebia.in/wp-content/uploads/2011/12/CustomSegues.zip

## Comments

**[George Azevedo](#6560 "2012-01-03 00:14:26"):** Hello friend, I just got a little frustrated for I can't put these custom segues to work... I tried to create a two-view example for iPad using sdk 5.0, then i put a button in view 1 linking to view 2, using custom segue. Then I created testCustomSegue extending UIStoryboardSegue, and wrote the perform method you just wrote above in it. I selected Custom for the segue, and put testCustomSegue as the segue class. No error message is shown, but nothing happens after I click over the button. Could you send me your code as an example, if possible? Thanks!

**[Gaurav Srivastava](#6569 "2012-01-03 12:02:05"):** Hi George, you can download the code from - http://xebee.xebia.in/wp-content/uploads/2011/12/CustomSegues.zip Thanks.

**[faaza](#7536 "2012-02-11 19:19:37"):** Edit: in fact what i need to achieve is to get slide transition between VC1 and VC2. So i need Slide Forward and Slide Backward Transitions. And all this using Simple Buttons not navigation bar buttons. :)

**[Gaurav Srivastava](#8654 "2012-05-02 13:26:12"):** Hi Michele, Storyboards are nothing but just a replacement for all your xib files. In your case it has something to do with the SDK. Are you sure the Facebook-iOS SDK you are using is compatible with iOS5? If yes, then the second thing you can try is working on your application without storyboards (use XIBs). If both of the above steps works good, then its all about changing couple of code to load view from storyboard. Regards, Gaurav

**[simon](#6951 "2012-01-15 09:07:47"):** Hey can you please explain what the push and modal segues are? im a complete beginner and havnt found what they mean.

**[Gaurav Srivastava](#6977 "2012-01-16 14:51:52"):** iOS5 have concept of Segues. If you want to navigate from one view to another you 'can' use segues. I wrote 'can' because there is other way as well (which is used if you are not using Storyboards). When you are navigating from one view to other there are two possible ways to show the next view - EITHER you want to make it a normal navigation OR you want to present it as a pop-up kind of thing. By 'normal navigation' i mean pushing on to the navigation controller. So when you want to push your view to the navigation controller, you use PUSH Segue. And, when you want to present it as a pop-up, you use MODAL segues. Let me try explaining you in practical by taking the Calendar application on iPad. I am not sure they are using Segues, but you can understand the difference between Push and Modal segues. Open the Calendar application on your iPad. The calendar app is basically pushed on to the navigation stack, hence it shows up on screen. To see a Modal segue, you need to click a + sign on the bottom right of the screen. Clicking this + sign will open a window for adding an event. You will notice that it is shown as a pop-up. This is the behavior when you do a Modal Segue.

**[simon](#6979 "2012-01-16 16:22:07"):** Thanks, thats a great definition!

**[faaza](#7535 "2012-02-11 19:18:12"):** Hi Gaurav! The only code i have is a custom seque code. the rest is all in StoryBoard. in fact what i need to achieve is to get slide transition between VC1 and VC2. So i need Slide Forward and Slide Backward Transitions. There's no way to do it with push Seques.. it slides only one direction. The only way is to use custom seques. But i lose my tab bar always. I don't know, maybe i'll better switch back to old Nib based development as seems that there's no way to do it with StoryBoard.

**[Michele](#8644 "2012-05-01 23:36:57"):** Hi Gaurav, I'm taking the ios5 development class available on itunes. I'm doing pretty good and have segues working and have all projects done except the core data one. For my final project, I'm trying to incorporate an iphone app with facebook. The example I found was for ios4, but I do have it working. I can't figure out how to turn it into an ios5 app with storyboards. I tried asking at other websites, but they said that the question is too broad. Facebook login never shows up with my ios5 version. I think it's because of the storyboard and facebook not knowing how to display without the xib, and with my viewController. Hope you can help! Thanks, Michele

**[faaza](#7393 "2012-02-06 13:03:04"):** Hi Gaurav! Can you help me with a little thing with custom seques? I have a simple setup with TabBar and a navigation controller with view controller connected to it. On storyBoard it looks like this: TabBarController->NavigationController->ViewController1->ViewController2 So if i press a tab on TabBar i see the contents of ViewController1 ViewController1 has a simple button that(when pressed) will lead to ViewController2 using Custom Seque. So everything works but one thing. TabBar disappears as soon as i press that button on ViewController1. How can i get that TabBar back?

**[Gaurav Srivastava](#7394 "2012-02-06 14:02:00"):** Hi Fazza, The workflow you have mentioned, sounds a happy one. But seems there is something wrong in the code you are writing. Is it possible for you to share your source code?

