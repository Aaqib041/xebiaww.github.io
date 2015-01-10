---
layout: post
header-img: img/default-blog-pic.jpg
author: Rupal Chawla
description: 
post_id: 826
created: 2008/11/28 13:44:50
created_gmt: 2008/11/28 11:44:50
comment_status: open
---

# Flex Online Style Explorer

At Xebia India we were having training on Flex, and this gave me an opportunity to look deeper and around this new eye catching technology. During this learning period I came across a free online tool provided by Adobe Flex to create CSS for your flex application. It is not a full fledged application covering all the components present in flex, but with the minimal stuff on board it gives you enough knowledge to extend the CSS for all other components. Some of the impressive parts of this tool are 

  * Its free
  * Its online so no installation required
  * Damn quick
  * And effect against every change you make is visible at the very same moment just like desktop applications. I think this is what flex is best in.
The name of this tool is [Flex Style explorer][1]. The Flex explorer allows you to interact with components and visually modify them. More importantly it is an effective development tool that allows you to create CSS within the site and then copy it in to your application.

Below is how this tool looks like 

![Flex Style Explorer][2]

As you can see, the application is divided in to four parts with first being the components panel, second is the style control, third is sandbox, and fourth being the CSS panel where actually the CSS is generated automatically.

Another thing I liked about this tool is it resembles the concept of supply chain. You feed your input at the left most part, execute your changes in the next one, test them in the third one and get it delivered from the last panel. Each and every step is in front of your eyes so that you can see all the changes happening without any reloading or any kind of wait, thus making the development work really fast.

Lets see every panel of this tool more closely

**Component panel **

Components such as controls, layouts, charts etc are the actual objects we will be using CSS for, thus before creating the CSS we need to select which component we want to create CSS for. You can select any of it from a list of components present in the component panel at the left most of the application and also shown below. 

![Component Panel][3]

To make it more clear I have taken an example of Accordion Navigator currently selected in the above image.

**Style Control **So if you are with me, and we are working on the same component i.e. the Accordion Navigator, then as soon as you select the component your style control panel will change and will look like as below 

![Style Control Panel][4]

All the labels and their corresponding tabs, controls that you can see in the above image corresponds to a CSS property, you can change them as per your requirement.

**Sandbox** As you update each attribute, the corresponding effect will start appearing in the sandbox. Thus every minute change in the layout or colour scheme is in the front of your eyes. 

![Sandbox][5]

In my example, I changed the properties of the Navigation layout in the Style Control panels to make it like above. ****

**CSS code pallet** Whatever changes you made in the Style Control panel, whatever the result you got in the Sandbox, the code for that will keep on generating in the CSS panel. In my case the code was created as below

_Accordion { headerHeight: 35; highlightAlphas: 0.3, 0.18; fillColors: #330000, #33cc66, #ff0000, #cc66cc; selectedFillColors: #0033cc, #000066; themeColor: #33ff99; backgroundColor: #33cc99; borderColor: #33ff00; textRollOverColor: #330000; textSelectedColor: #00cc00; dropShadowEnabled: true; shadowDistance: 5; shadowDirection: right; dropShadowColor: #000000; headerStyleName: "myaccordionHeader"; }_

_.myaccordionHeader { letterSpacing: 3; textAlign: center; fontSize: 9; fontWeight: bold; }_

So, it was damn quick, and a really good looking panel’s CSS is in front of you. You can copy it from the CSS panel, and use it in your application just as I pasted in this blog.

**Styling the whole application** There is still one more part of the project that we want to set the style for before we bring the styling into Flex Builder. Besides being able to style the Flex components, you can also style the application as a whole. This is the first item in the list on the left side. With Application selected, you can modify aspects of your project that cover the whole of the project. You can style various components visiting them one by one, and when you are done, export the CSS by clicking Export All CSS link at the bottom left corner of the application.

In the end, I would say it really worked for me. It may not be scalable for a big and rich application, but very much fruitful for start-up.

   [1]: http://examples.adobe.com/flex3/consulting/styleexplorer/Flex3StyleExplorer.html (Flex Style Explorer)
   [2]: http://blog.xebia.com/wp-content/uploads/2008/11/fse_1-300x144.jpg (Flex Style Explorer)
   [3]: http://blog.xebia.com/wp-content/uploads/2008/11/fse_2-91x300.jpg (Component Panel)
   [4]: http://blog.xebia.com/wp-content/uploads/2008/11/fse_3-196x300.jpg (Style Control Panel)
   [5]: http://blog.xebia.com/wp-content/uploads/2008/11/fse_4-300x167.jpg (Sandbox)