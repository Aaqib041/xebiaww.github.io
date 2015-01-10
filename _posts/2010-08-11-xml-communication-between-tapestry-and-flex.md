---
layout: post
header-img: img/default-blog-pic.jpg
author: iti
description: 
post_id: 4195
created: 2010/08/11 13:12:06
created_gmt: 2010/08/11 08:12:06
comment_status: open
---

# XML communication between Tapestry and Flex

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow: hidden;">
<h1>How to get your swf talking to your tapestry application using xml:</h1>
</div>

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow: hidden;">In this writeup, we will look at how we can send xml data using tapestry pages and use this in a swf object.</div>

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow: hidden;">We will consider the following are already in place:</div>

<div id="_mcePaste" style="background: none repeat scroll 0% 0% #e7d3ec; border: 1px solid; padding: 5px; margin: 5px 0px; position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow: hidden;">1. A tapestry application that needs to send the xml</div>

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow: hidden;">2. A swf object which needs to use the xml.</div>

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow: hidden;">There are 2 things that you will need now:</div>

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow: hidden;">1. The tapestry page which will send xml data</div>

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow: hidden;">To send xml from tapestry page we need the following:</div>

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow: hidden;">1. A usual Tapestry style Java class with a special modification:</div>

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow: hidden;">[sourcecode language="java"]&lt;/pre&gt;
&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;@ContentType(&quot;application/xml&quot;)// this will define that this class in xml type&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;public class ICanSendXML {&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;@Property&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;private String property1;&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;@Property&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;private String property2;&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;....&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;....&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;....&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;....&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;&lt;p&gt;[/sourcecode]

</div>

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow: hidden;">2. The tapestry style ICanSendXML.tml file which looks something like this:</div>

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow: hidden;">[sourcecode language="xml"]&lt;/pre&gt;
&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;&lt;?xml version=&quot;1.0&quot;?&gt;&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;
&lt;properties&gt;&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;
&lt;property1&gt;${property1}&lt;/property1&gt;&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;
&lt;property2&gt;${property2}&lt;/property2&gt;&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;....&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;....&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;....&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;&lt;/properties&gt;&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;&lt;p&gt;[/sourcecode]

</div>

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow: hidden;">2. The flex that calls this page to read the xml.</div>

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow: hidden;">This can be achieved in the following manner:</div>

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow: hidden;">1. Declare an Http Service which calls our tapestry page:</div>

<div id="_mcePaste" style="position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow: hidden;">[sourcecode language="flex"]&lt;/pre&gt;
&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;&lt;mx:HTTPService&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;id=&quot;xmlFromTapestry&quot;&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;url=&quot;http://localhost:8080/myTapestryAppThatSendsXML/ICanSendXML&quot;&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;resultFormat=&quot;e4x&quot;&lt;/div&gt;
&lt;div id=&quot;_mcePaste&quot; style=&quot;position: absolute; left: -10000px; top: 0px; width: 1px; height: 1px; overflow-x: hidden; overflow-y: hidden;&quot;&gt;result=&quot;loadPropertiesResultHandler(event);&quot;&lt;/div&gt;