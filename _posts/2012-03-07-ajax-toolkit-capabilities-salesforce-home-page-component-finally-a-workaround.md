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

<p><em>Astonishing but true - You cannot select/create a Visualforce page as your Salesforce Home Page Component. The component creation dialog only enables creating custom Links, Image/Logo or HTML Area as a Home Page component. </em></p>
<p>To get a glimpse of what Salesforce developers desire, check this out : <a href="http://success.salesforce.com/ideaview?id=08730000000Brk4AAC" target="_blank">http://success.salesforce.com/ideaview?id=08730000000Brk4AAC</a> .
<!--more--></p>
<p><em><strong><span style="font-size: large;">Why would you anyway want to include a VisualForce page in your home page component?</span></strong></em>
<ul>
    <li> If not a visualforce page, compromise on the data binding capabilities that Force.com provides.</li>
</ul>
<ul>
    <li> Not a UI designer? Its easier for you to create a UI page with the visualforce  UI component kit.</li>
</ul>
<ul>
    <li> <span style="text-decoration: underline;"><em><strong>Here's the most critical one:</strong></em><strong><em> </em></strong></span>You cannot use the Ajax Toolkit for accessing user's contextual Salesforce information. This toolkit enables you to make Ajax SOAP calls from your salesforce page that query and retrieve user account specific data that could then be rendered on the page. <em>There is no straightforward mechanism to use this toolkit from an HTML Area type Home Page Component - the component that you would use to create a HTML user interface. Lets analyse why.....</em></li>
</ul>
<strong>There are 3 core steps that you need to perform while working with the Ajax toolkit :</strong>
<ol>
    <li> <em>Connect to the API : </em>If you want to use the toolkit from the home page component, <em>this is where the problem occurs.</em></li>
    <li> <em>Embed API calls in Java Script :</em> Once the connection is established, you just need to embed the api call in the js to get the result. For example : "<strong>result = sforce.connection.query("Select Name, Id from User")</strong>" should fetch the Name and Id from the table User</li>
    <li>Process Results : One the results are retrieved, display them on the page.</li>
</ol>
<strong>Connecting to the API ..... problem Analysis</strong></p>
<p><strong> </strong> Had it been a VisualForce page, you would have just needed the following JS include in your page :</p>
<p>[xml]
&lt;apex:page&gt;
&lt;script type=&quot;text/javascript&quot;&gt;&lt;!--mce:0--&gt;&lt;/script&gt; ... &lt;/apex:page&gt; [/xml]</p>
<p>...this loads the toolkit and creates a global object <strong>sforce.connection</strong>. This object contains all the toolkit APIs, and also manages the Session ID. <em>No other session management is needed while you are working on a Visualforce page. </em> Since a Visual Force Page option is not available for a Home Page Component, you are left with no other option but to use a <em>HTML Area</em> type Home Page Component for creating Home Page UI widgets or for executing your custom JS on each Salesforce page load.</p>
<p>Trying to use the Ajax toolkit in this HTML Area Home Page component by including the connection.js as you do on a visualforce page, i.e.</p>
<p>[xml]
&lt;script type=&quot;text/javascript&quot;&gt;&lt;!--mce:1--&gt;&lt;/script&gt;
[/xml]</p>
<p>is no success. Despite the fact that the above include statement loads all the api's available to the global object salesforce.connect, a query call like <em><strong>sforce.connection.query("Select Name, Id from User")</strong></em> does not work.</p>
<p>The reason behind this issue is the fact that the implicit session management that is available on a Visualforce page is not available on this HTML Area component. Since the Ajax toolit's managed session id is not available, you cannot use the Ajax toolkit.</p>
<p><em> What this essentially means is that if your business use case evolves around fetching user's contextual information in the Home Page Component, so that on every page load, the home page component displays some user's context specific stuff, then there is no clean, straightforward solution available !! </em></p>
<p><em><strong><span style="font-size: large;">The possible workarounds..... </span></strong></em>
<ul>
    <li><strong>Iframes :</strong> Create a visualforce page and load this page in the HTML Home page component using an IFrame  ... you can use this approach till the time there is no inteded interaction between the content that is delivered in the IFrame and the containing HTML component. Getting the content of an IFrame to interact with the container can be really cumbersome given the cross domain limitations that an IFrame is viable to.</li>
</ul>
<em> If you have global javascript variables in your HTML home component that need to be assigned values read using the Ajax Toolkit APIs, then IFrame's wont work. So what else could be tried...? </em></p>
<p><em><strong>Food for thought.... </strong></em></p>
<p>Apart from Visual Force pages, the Ajax toolkit is also accessible to <em>custom onclick JavaScript links/buttons.</em> All you need to do in a custom onclick link is to specify a login call to salesforce API :</p>
<p>[xml]</p>
<p>{!requireScript(&quot;/soap/ajax/23.0/connection.js&quot;)} sforce.connection.login(&quot;username&quot;, &quot;password&quot;); ...</p>
<p>[/xml]</p>
<p>This way, the Ajax toolkit picks up the endpoint and manages the session ID.</p>
<p><em>(Looking at the script above, you might assume that the same login mechanism should work from javascript of a HTML Area type home page component. Surprisingly it doesn't!!) </em></p>
<p><em><strong><span style="font-size: large;">Finally the workaround that works....</span></strong></em></p>
<p>If you noticed earlier, then the Home Page Component types that Salesforce provides has 3 options :
<ul>
    <li>Images</li>
    <li>HTML Area</li>
    <li>Custom Links<em> </em></li>
</ul>
<em> </em></p>
<p><em> </em></p>
<p><em> </em></p>
<p><em> </em></p>
<p><em> </em></p>
<p><em> </em></p>
<p><em> </em></p>
<p><em></p>
<p>[caption id="attachment_12554" align="aligncenter" width="300" caption="Home Page Component Types"]<a href="http://xebee.xebia.in/2012/03/07/ajax-toolkit-capabilities-salesforce-home-page-component-finally-a-workaround/typesofhpc-5/" rel="attachment wp-att-12554"><img src="http://xebee.xebia.in/wp-content/uploads/2012/03/TypesOfHPC2-300x178.png" alt="Home Page Component Types" title="Home Page Component Types" class="size-medium wp-image-12554" width="300" height="178" /></a>[/caption]</p>
<p></em></p>
<p><em> </em></p>
<p><em> </em></p>
<p><em> </em></p>
<p><em> </em></p>
<p><em> </em></p>
<p><em> </em></p>
<p><em> </em></p>
<p><em>(By this time you should have connected the dots.....) </em><strong>.....<em> you have Custom Links as a Home Page Component type!!</em></strong></p>
<p><strong> Here's what could be done now.....</strong><em>
</em>
<ul>
    <li><em>Create an Execute JavaScript type custom link that OnClick, sets the API's session id into windows session.</em></li>
</ul>
[caption id="attachment_12557" align="aligncenter" width="300" caption="Execute JavaScript Type Custom Link - Home Page Component"]<a href="http://xebee.xebia.in/2012/03/07/ajax-toolkit-capabilities-salesforce-home-page-component-finally-a-workaround/customlink/" rel="attachment wp-att-12557"><img src="http://xebee.xebia.in/wp-content/uploads/2012/03/CustomLink-300x178.png" alt="Execute JavaScript Type Custom Link - Home Page Component" title="Execute JavaScript Type Custom Link - Home Page Component" class="size-medium wp-image-12557" width="300" height="178" /></a>[/caption]
<ul>
    <li><em>Create a HTML Area type Home Page Component, that, on every page load, fires the custom link created above (so that the Ajax toolkit's managed session is available in the browser' session) on page load. </em></li>
</ul>
[caption id="attachment_12558" align="aligncenter" width="300" caption="HTML Area Home Page Component"]<a href="http://xebee.xebia.in/2012/03/07/ajax-toolkit-capabilities-salesforce-home-page-component-finally-a-workaround/htmlareahomepagecomponent/" rel="attachment wp-att-12558"><img src="http://xebee.xebia.in/wp-content/uploads/2012/03/HTMLAreaHomePageComponent-300x136.png" alt="HTML Area Home Page Component - The Code" title="HTML Area Home Page Component - The Code" class="size-medium wp-image-12558" width="300" height="136" /></a>[/caption]</p>
<p><em><strong>The Home Page Component Code</strong></em>...
<ul></p>