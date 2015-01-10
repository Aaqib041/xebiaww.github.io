---
layout: post
header-img: img/default-blog-pic.jpg
author: jthakur
description: 
post_id: 3157
created: 2010/02/17 13:58:15
created_gmt: 2010/02/17 08:58:15
comment_status: open
---

# 10 useful interface design techniques for login page/form.

Now a day’s more and more web applications are using rich user interface to fascinate user. It has no platform constraints or installation requirements and  the application model looks very attractive and impressive. Web application interface design is, at its core, focused mainly on the function. To compete with desktop applications the web apps must offer simple, intuitive and responsive user interface to let their users get things done with great amount of ease. To make the user experience memorable and error free web application should strongly follow the branding, theme, colors and guidelines adhering the product.

Simpler the interface easier to use the application.  A complex interface with more controls to display on the screen will make users spend more time figuring out how to use the interface. When there are less choices the available functions become more noticeable and easier to scan.

Login is the first page where user is launched for the first time. While designing a login form/page there comes many questions like:

  1. Should it be Login, Log in or Sign in?

  2. How to display “**_forgot”_** and “**_new register”_**links,

  3. What common word to be used for easier understanding. For example "Forgot your user name, click here" is easier  than "What was your last user name/Open Id". My advice is to keep things simple.

**10 useful interface design techniques for login page/form.**

  1. **_Separate login page/form_**: Login should be a separate page or pop-up with light window.
  2. **_Common terms_**: Application should speak the users' language with words, phrases and concepts familiar to the user rather than system-oriented terms. Follow real-world conventions making information appear in a natural and logical order.
  3. **_Identity of username:_** Username plays a vital role as it is the only identity of user in the application.  Through a username he is able to identify features and credentials available to him. Try to provide the hints for creating a user ID in a highlight. For example use at least six characters in length and can contain the following elements like, Lowercase letters (a-z), Numbers (0-9), Hyphens (-), Underscores (_). Avoid the characters @, &, ‘, (, ), <, or >, underscore, hyphen, or period at the beginning of a user ID as it is not a good practice. Username text box should be long enough to be visible for the long title used for creating username. Highlight the text field, when the cursor is clicked in the textbox to alert user his cursor status.
  4. **_Newly launched:_** If the application is newly launched and the main focus is to get new registration then highlight the “Register link/button” with some bold colors and some interesting graphical presentation to invite user to click. This is the entry point for the new business.
  5. **_Alerts and errors:_** Alerts should be clearly visible when user creates mistakes or when the user submits his action. Error messages should be expressed in plain language (no codes) precisely indicating the problem and constructively suggesting a solution. There can be different colors associated with these messages e.g. alerts can be yellow and errors can be red.
  6. **_Disable pressed buttons:_** Normally submit/go button is followed in login form but at times cancel/reset button is also used. **Try not to put this button as disable pressed buttons, user get confused**.
  7. **_Pressed button states:_** Use style sheet or graphical representation on hover stage of button to make it apparent.
  8. **_Validation:_** Code should be validated properly to avoid discrimination. Proper alerts should be displayed for the errors created by user.
  9. **_Tab Index:_** Follow tab indexing as normally users are habitual of using tab key for moving from one control to another.
  10. **_Button Placement: As_** users are comfortable using right side of the monitor screen with mouse so it is a good practice to put action button at right side of the screen. If you have both "**_Reset/Cancel_**" and "**_Submit_** " button then place the submit button on the extreme right of the screen. Using Reset button almost never helps user, but often hurts them, it is usually faster to edit the old data than to erase it and start over.
Stay tuned for my next blog on “**_Where to use and not to use Reset and Cancel button_**”.