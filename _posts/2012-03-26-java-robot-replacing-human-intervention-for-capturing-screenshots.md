---
layout: post
header-img: img/default-blog-pic.jpg
author: Shruti Khattar
description: 
post_id: 12841
created: 2012/03/26 16:11:42
created_gmt: 2012/03/26 11:11:42
comment_status: open
---

# Java Robot replacing Human Intervention for capturing Screenshots

When we talk of taking screen dumps without the use of keyboard, tools like Quick Screen Capture, Adobe Captivate, Camtasia -Relay, Capture Fox, Debut, GoView, Tip Cam and sorts, which can be picked up easily available over the internet and are user friendly. But very few of us know or have used the inbuilt Robot class in java AWT to perform this action. In this blog, I ll be discussing the untapped feature of Screen Capture in Robot class and its limitations as well.

This class is used to generate native system input events for the purposes of test automation, self-running demos, and other applications where control of the mouse and keyboard is needed. The Robot class is extensively used in automated testing of Java platform implementations. Below is the code for an application that takes screenshots from the window after an interval of 10000ms. Using that, I have tried to capture server logs of a build in execution and a video tutorial. Below are the links to the folders uploaded on GoogleDocs that I have recorded using this application

[DemoTutorial][1]

[ServerLogs][2]

[code language="javascript" highlight="19, 21, 22"] public class ScreenCapture { private int delay=10000; public static void main(String[] argv) throws InterruptedException { ScreenCapture screenCapture = new ScreenCapture(); screenCapture.captureScreenShots(); }
    
    
    boolean captureScreenShots() throws InterruptedException {
        boolean isSuccesfol = false;
        Dimension size = Toolkit.getDefaultToolkit().getScreenSize();
        try {
            Robot robot = new Robot();
            BufferedImage[] screenshots = new BufferedImage[20];
            Rectangle bounds = new Rectangle(0, 0, (int) size.getWidth(),
                    (int) size.getHeight());
            for (int i = 1; i &lt; screenshots.length; i++) {
                System.out.println(&quot;Running&quot;);
                screenshots[i] = robot.createScreenCapture(bounds);
                Thread.sleep(delay);
                try {
                    ImageIO.write(screenshots[i], &quot;jpeg&quot;, new File(
                            &quot;C:/ServerLogs/log&quot; + i + &quot;.jpeg&quot;));  /*Writing the array of  screenshots to the Files
                                                    on the specified path*/
                } catch (IOException e) {
                    e.printStackTrace();
                    isSuccesful = false;
                }
            }
        } catch (AWTException e1) {
            e1.printStackTrace();
        }
        return isSuccesful;
    }
    

} 
 ```

[RobotCode][3] You can access the code here

In line no. 19, I have used Thread.sleep(delay) to specify the time interval between recording of the screen dumps, instead we could use the robot.delay(delay).It does not put to sleep everything, but only the calling thread. In a multi-threaded environment, the creating thread is often not the only calling one; actually more than one thread could call robot.delay(delay) at the same time, and when the method is synchronized i.e. exclusive then use Thread.sleep(delay) instead. Line no 21, where I've mentioned the path for storing the screen dumps as images can also be taken as an input from the user, or stated in a separate properties file.

In the use cases that I've demonstrated, snapshots can give you a clear idea of what has been happening in the video or a flash file, also, makes it easier to track what and where went wrong in the build process without having to fix an eye on them. they can be analyzed at any point of time. The results have been stored in the form of static images taken periodically. However, the code suffers from some weaknesses or limitations, which are given below: 

  * May not be able to automatically locate the location of the window you want to capture on the screen, you have to specify the window coordinates,or get the screen size programatically,
  * When the system is locked, the screen shots of a running application cannot be taken. the image comes as a black screen
  * It does not provide the capability of record and play
  * Error handling has to be gracefully managed by the application.
Following are the exceptions that it is vulnerable to throw: 
  1. **IllegalArgumentException** \- if screen is not a screen GraphicsDevice.
  2. **AWTException** \- if the platform configuration does not allow low-level input control.
  3. **SecurityException** \- if permission to create a robot object is not granted.
  4. **HeadlessException** \- Thrown when code that is dependent on a keyboard, display, or mouse is called in an environment that does not support a keyboard, display, or mouse
There are pros and cons in every utility, it might not cater to an extensive set of requirements or provide advance features, but understanding its capability and utilizing it at the right place and the right time makes it worth while.

   [1]: https://docs.google.com/?authuser=0#folders/0B__Z-mJq2wRnbkx0dllrQnFTUFd2X3h0cDRFb3gxZw
   [2]: https://docs.google.com/?authuser=0#folders/0B__Z-mJq2wRnVVByMF9OSHJRcm1MWUhnZEZONFV0Zw
   [3]: https://github.com/ShrutiKhattar/RobotDemo

## Comments

**[Pooja](#8069 "2012-03-27 20:03:22"):** Knowledgeable!! :)

**[Gaurav Chauhan](#8055 "2012-03-27 11:56:33"):** Wow...Nice work Shruti.... It will be a very useful tool....

