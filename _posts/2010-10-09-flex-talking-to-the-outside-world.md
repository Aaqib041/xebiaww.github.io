---
layout: post
header-img: img/default-blog-pic.jpg
---

# Flex : Talking to the outside world 

Some of the requirements from Customer -

**Customer :** I dont want to add customer support built in the application but somehow would like to give a link to  
** Customer :** It's a little technical for the end user to understand. Should we really have this feature? I wanna try keeping it simple,

Some familiar voices  from developer world --  
** Developer :** Can you please attach the debug logs  
** Developer :** Please use this utility to check if it's a problem with your bandwidth.

While developing a Standalone Flex application following are some of the requirements that may come along

  * Opening a Customer page
  * Utility to report bug
  * Advance Features for power users
  * If  its a media extensive application Utility to test the bandwidth

All the above requires talking to the outside world from your flex  application. These are good to have features. One of the approach would be to have a help page where links to all these utilities is present. But this still is not a elegant solution.  Flex Context menu is one of the best ways to introduce these features.

Following are the advantages of using the Flex Context Menu

  * It does not become part of your core UI.
  * Its quick and easily available using right click.

One of the classic examples is the way youtube does uses it. Notice the picture  you can now easily report a playback issue.

![](/wp-content/uploads/2010/10/paint.png)

Earlier utililty like copy embed html code were only available on YouTube site.  Now if you want to embed the youtube video you can just right click and get the embed code. Its much faster and easier than previous approach.

YouTube also provide a link to take a speed test. Some of the Flex multimedia application do have a constraint on the minimum required bandwidth. Bandwidth being one of the volatile parameters , it a good idea to have a link to Bandwidth checker. This way whenever the user is in doubt can easily check the bandwidth if he faces any problem using the application

So lets see a small demo of how to use context menu.Assume that your application provides few themes that you can toggle , it would be a good idea to expose functionality using a menu items.Attach are the [source files](/2010/10/09/flex-talking-to-the-outside-world/flexcontextmenu/) to the demo application.

Following link is a demo of  
[How to use context menu video](/wp-content/uploads/2010/10/2010-10-09_1852.swf)

Please do let me know your comments, Thanks