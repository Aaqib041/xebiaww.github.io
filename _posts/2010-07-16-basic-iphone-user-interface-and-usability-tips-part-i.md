---
layout: post
header-img: img/default-blog-pic.jpg
author: jthakur
description: 
post_id: 4128
created: 2010/07/16 10:35:03
created_gmt: 2010/07/16 05:35:03
comment_status: open
---

# Basic  iPhone User Interface and Usability Tips - Part I

While attending “[Human Factors International][1]” certification classes sometime back, I came across a very interesting activity. We were asked to exchange our mobile devices within a group of people and perform small tasks after switching off it. Tasks were:

  1. Restart the mobile
  2. Set different ring tone and
  3. Set calendar meeting with alarm.

The duration given was 120 seconds. I got a Samsung mobile, while I carry a Nokia. I was not able to easily switch it on, and when I did, I could not perform half of my tasks in time.  
  
**The conclusion** – The Device was not user friendly and many of us could not achieve the goals in the given time. With this blog I intend to share my experience about designing Mobile User Interface - iPhone in particular.

Designing a mobile application is different than website designing. Mobile designing is complex and challenging. Further in mobile designing, touch screen applications are not much different. In touch screen finger is used to perform action, while others keys are used.

Selecting an application technology and designing a mobile application needs:

  1. Good understanding of its benefits
  2. Target device  
  

    1. normal mobile
    2. touch screen
  3. End users
  4. Cost involved and the profit earned
  5. Context, market trend, and restrictions of the Development Company for building it.

Attractive user interfaces are crucial too. For end user the technology used is not as important as a good user interface.

Here are some of the Usability points I took into consideration while working on an iPhone interface:

  1. Screen size should be 320 x 480 or 960 x 640 pixels. It is a good practice to draw wireframes on piece of paper .With this you get a fare idea of how the placement will be and so you can adjust the content in it accordingly.
  2. Launcher icons should be present on the home screen. The standard size of launcher icon for low density, medium density screen and high density screen is 36 x 36 pixels, 48 x 48 pixels, 72 x 72 pixels. For more tips creating and managing icon sets for multiple densities, see <http://developer.android.com/guide/practices/ui_guidelines/icon_design.html#design_tips>
  3. Images should be light and small in size and loading resources idly. Preferred format is PNG as it gives rich colors and iphone is able to optimize PNG rendering for optimization.
  4. Homepage is the place where branding starts. Make it simple, attractive and bold.  
  

    1. Once user is on the home page, Search comes handy to help a lost user.
    2. Search should be prominent on the page with Search button.
    3. Prefer to use simple label which are easy to understand.
    4. If the search is defined for products then provide it in dropdown. This avoids user to type or misspell it.
    5. Textbox area should be big enough to write.
    6. Narrow down the search results, for example, “items displayed on page 1-15 of 340”.
  5. Use of standard controls should be correctly as users are familiar with the standard controls they see in the built-in applications for example: - A navigation bar should appear at the upper edge of an application window, just below the status bar. Make it bold and highlight it. This space is used for title of the screen and navigation controls.
  6. A toolbar should appear at the bottom edge of the window. It provides functionality within the context of a task.  
  
For example: If I open a photo album application it provides a toolbar that contains items for deleting, next, previous, a typical action sheet for email photo, assign to contact, use as wallpaper features are displayed in this way, users can stay within the photo album-viewing context and still have access to the commands they need to manage their album. Icon size for toolbar should be 44 x 44 pixels, so it is easy for user to tap, also make sure it should not have more than 5 toolbar items and evenly distributed in 320 pixels otherwise it can be difficult for users to tap a specific one.
  7. Different color than regular font color and bold helps to highlight important messages for example:- price of the product.
  8. Show alert pop up in the middle of the screen and float above the application window. Avoid embedding alert messages in the whole screen. Use dark background of screen with some opacity and pop-up alert above it with some drop shadow.  
  

    1. Also include OK button in an alert, so that users can acknowledge the alert message and dismiss the alert. Do not include more than two buttons.
    2. Use title-style capitalization and bold-style font. You should not put a period after the title. Title-style capitalization is the capitalization of every word.
    3. Use bold and highlighted colors like red, yellow on dark backgrounds for alert buttons, for example “Cancel”.
  9. If you need to provide a button which performs a potentially destructive action, such as deleting all the items in a user’s album list, you should use red color. By default, such destructive buttons are displayed at the top of the action sheet where they are most visible.
  10. In a table view, data present in a single-column list of multiple rows. Here each row can contain some combination of icon, text, images, and controls, and rows can be divided into sections or groups. Each row height should be of 44 pixels, so user can tap easily.
  11. For making online shopping, give brief description of product, its availability, rating from other users help to analyze market value of product.
  12. Easy navigation such as customized scroll, back button, next button, save button, cancel button etc should be easy to understand for the user.
  13. WebKit based iPhone applications run on safari browser, thus I advice to check the HTML structure created for the device in Safari browser and then provide it to development team for further implementation.

Soon I will publish the second edition on iPhone User Interface and Usability with more tips

<!--[if gte mso 9

   [1]: http://www.humanfactors.com/training/schedule.asp?courseid=6#schedule