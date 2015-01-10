---
layout: post
header-img: img/default-blog-pic.jpg
author: KaranNangru
description: 
post_id: 11042
created: 2012/03/07 08:57:55
created_gmt: 2012/03/07 03:57:55
comment_status: open
---

# Ajax Toolkit capabilities @ Salesforce Home Page Component   ...finally a workaround

_Astonishing but true - You cannot select/create a Visualforce page as your Salesforce Home Page Component. The component creation dialog only enables creating custom Links, Image/Logo or HTML Area as a Home Page component. _

To get a glimpse of what Salesforce developers desire, check this out : <http://success.salesforce.com/ideaview?id=08730000000Brk4AAC> . 

_**Why would you anyway want to include a VisualForce page in your home page component?**_

  * If not a visualforce page, compromise on the data binding capabilities that Force.com provides.
  * Not a UI designer? Its easier for you to create a UI page with the visualforce UI component kit.
  * _**Here's the most critical one:**_**_ _**You cannot use the Ajax Toolkit for accessing user's contextual Salesforce information. This toolkit enables you to make Ajax SOAP calls from your salesforce page that query and retrieve user account specific data that could then be rendered on the page. _There is no straightforward mechanism to use this toolkit from an HTML Area type Home Page Component - the component that you would use to create a HTML user interface. Lets analyse why....._
**There are 3 core steps that you need to perform while working with the Ajax toolkit :**

  1. _Connect to the API : _If you want to use the toolkit from the home page component, _this is where the problem occurs._
  2. _Embed API calls in Java Script :_ Once the connection is established, you just need to embed the api call in the js to get the result. For example : "**result = sforce.connection.query("Select Name, Id from User")**" should fetch the Name and Id from the table User
  3. Process Results : One the results are retrieved, display them on the page.
**Connecting to the API ..... problem Analysis**

** ** Had it been a VisualForce page, you would have just needed the following JS include in your page :

[xml] <apex:page> <script type="text/javascript"><!--mce:0--></script> ... </apex:page> [/xml]

...this loads the toolkit and creates a global object **sforce.connection**. This object contains all the toolkit APIs, and also manages the Session ID. _No other session management is needed while you are working on a Visualforce page. _ Since a Visual Force Page option is not available for a Home Page Component, you are left with no other option but to use a _HTML Area_ type Home Page Component for creating Home Page UI widgets or for executing your custom JS on each Salesforce page load.

Trying to use the Ajax toolkit in this HTML Area Home Page component by including the connection.js as you do on a visualforce page, i.e.

[xml] <script type="text/javascript"><!--mce:1--></script> [/xml]

is no success. Despite the fact that the above include statement loads all the api's available to the global object salesforce.connect, a query call like _**sforce.connection.query("Select Name, Id from User")**_ does not work.

The reason behind this issue is the fact that the implicit session management that is available on a Visualforce page is not available on this HTML Area component. Since the Ajax toolit's managed session id is not available, you cannot use the Ajax toolkit.

_ What this essentially means is that if your business use case evolves around fetching user's contextual information in the Home Page Component, so that on every page load, the home page component displays some user's context specific stuff, then there is no clean, straightforward solution available !! _

_**The possible workarounds..... **_

  * **Iframes :** Create a visualforce page and load this page in the HTML Home page component using an IFrame  ... you can use this approach till the time there is no inteded interaction between the content that is delivered in the IFrame and the containing HTML component. Getting the content of an IFrame to interact with the container can be really cumbersome given the cross domain limitations that an IFrame is viable to.
_ If you have global javascript variables in your HTML home component that need to be assigned values read using the Ajax Toolkit APIs, then IFrame's wont work. So what else could be tried...? _

_**Food for thought.... **_

Apart from Visual Force pages, the Ajax toolkit is also accessible to _custom onclick JavaScript links/buttons._ All you need to do in a custom onclick link is to specify a login call to salesforce API :

[xml]

{!requireScript("/soap/ajax/23.0/connection.js")} sforce.connection.login("username", "password"); ...

[/xml]

This way, the Ajax toolkit picks up the endpoint and manages the session ID.

_(Looking at the script above, you might assume that the same login mechanism should work from javascript of a HTML Area type home page component. Surprisingly it doesn't!!) _

_**Finally the workaround that works....**_

If you noticed earlier, then the Home Page Component types that Salesforce provides has 3 options : 

  * Images
  * HTML Area
  * Custom Links_ _
_ _

_ _

_ _

_ _

_ _

_ _

_ _

_

[caption id="attachment_12554" align="aligncenter" width="300" caption="Home Page Component Types"]![Home Page Component Types][1][/caption]

_

_ _

_ _

_ _

_ _

_ _

_ _

_ _

_(By this time you should have connected the dots.....) _**....._ you have Custom Links as a Home Page Component type!!_**

** Here's what could be done now.....**_ _

  * _Create an Execute JavaScript type custom link that OnClick, sets the API's session id into windows session._
[caption id="attachment_12557" align="aligncenter" width="300" caption="Execute JavaScript Type Custom Link - Home Page Component"]![Execute JavaScript Type Custom Link - Home Page Component][2][/caption] 

  * _Create a HTML Area type Home Page Component, that, on every page load, fires the custom link created above (so that the Ajax toolkit's managed session is available in the browser' session) on page load. _
[caption id="attachment_12558" align="aligncenter" width="300" caption="HTML Area Home Page Component"]![HTML Area Home Page Component - The Code][3][/caption]

_**The Home Page Component Code**_... 

   [1]: http://xebee.xebia.in/wp-content/uploads/2012/03/TypesOfHPC2-300x178.png (Home Page Component Types)
   [2]: http://xebee.xebia.in/wp-content/uploads/2012/03/CustomLink-300x178.png (Execute JavaScript Type Custom Link - Home Page Component)
   [3]: http://xebee.xebia.in/wp-content/uploads/2012/03/HTMLAreaHomePageComponent-300x136.png (HTML Area Home Page Component - The Code)