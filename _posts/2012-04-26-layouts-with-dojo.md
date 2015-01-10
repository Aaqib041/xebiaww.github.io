---
layout: post
header-img: img/default-blog-pic.jpg
author: varun_j
description: 
post_id: 13365
created: 2012/04/26 21:05:53
created_gmt: 2012/04/26 16:05:53
comment_status: open
---

# Layouts with Dojo

<p>Dojo is a powerful JavaScript framework for developing web applications. I was exploring layouts in Dojo and found that it provides us various amazing widgets as a part of Dijit (Dojo's UI framework) which helps us in creating layouts with great user experience quickly. In this blog we will see how we can create layouts in Dojo. Basically, what we are going to do is that we will create a simple layout with three tabbed panes. And in the first tabbed pane we will create a menu .<!--more--></p>
<p>Dojo provides us two ways to implement the layouts. One is markup style and the other way is creating layouts programmatically. I explored both the ways and in this blog we will use both ways to create the layout.</p>
<p>So lets begin.
In the header section of our HTML file, we include the dojo.js file as shown below:</p>
<p>[sourcecode language="html"]
&lt;script src='http://ajax.googleapis.com/ajax/libs/dojo/1.7.1/dojo/dojo.js'
                data-dojo-config=&quot;async: true, parseOnLoad: true&quot;&gt;
&lt;/script&gt;
[/sourcecode]</p>
<p>As you might have noticed, the configuration parameters are async:true and pasrseOnLoad: true.<br />
<strong>async </strong> defines if Dojo core should be loaded asynchronously. 
<strong>parseOnLoad </strong> If true, parses the page with dojo/parser when the DOM and all initial dependencies have loaded.  </p>
<p>Now our next step is to create the layout by using the declarative syntax. By declarative syntax we will create a Tabbed Pane with three tabs namely First Tab, Second Tab and Third Tab. Dojo provides us the HTML widgets which we can import and use to create layouts quickly and that is what exactly we are going to do.</p>
<p>Also Dojo provides us the themes which we can use to provide our layout the feel-good factor.</p>
<p>[sourcecode language="html"]
&lt;link rel=&quot;stylesheet&quot; media=&quot;screen&quot; 
        href=&quot;http://ajax.googleapis.com/ajax/libs/dojo/1.7.1/dijit/themes/claro/claro.css&quot; /&gt;
[/sourcecode]</p>
<p>The above tag will include the CSS for claro theme, other themes are also available like Soria, Tundra, Nihilo etc.
For more information on themes visit the below link:
<a href="http://dojotoolkit.org/reference-guide/1.7/dijit/themes.html">http://dojotoolkit.org/reference-guide/1.7/dijit/themes.html</a></p>
<p>After importing the theme just include the class “claro” in the body tag of page. This will appropriately style the entire html elements which we are going to create inside the body of the page.</p>
<p>In the markup style of creating layout in Dojo, it (Dojo) relies heavily on data-dojo- attributes. These attributes tell the Dojo what widget class to instantiate and then which properties to apply on the widget. So this means below HTML div is not just ordinary div but infact it is TabConatiner Widget provided by the Dojo.</p>
<p>[sourcecode language="html"]
&lt;div class=&quot;demoLayout&quot; id=&quot;appLayout&quot;
       data-dojo-props=&quot;region: 'center', tabPosition: 'bottom'&quot; 
       data-dojo-type=&quot;dijit.layout.TabContainer&quot;&gt;
&lt;/div&gt;
[/sourcecode]</p>
<p>The above HTML code will create a Tabbed Container which allows us to create tabs inside it.
The data-dojo-type attribute tell the Dojo parser to instantiate the TabContainer widget.
And the data-dojo-props attribute defines the additional parameters for the widget. The parameter region tell the place where TabContainer is to be placed and the tabPosition defines the position of the tabs which will be used by the user to navigate among the tabs.</p>
<p>Now inside the div with id=”appLayout” place the following code:</p>
<p>[sourcecode language="html"]
&lt;div data-dojo-props=&quot;title: 'First Tab'&quot; data-dojo-type=&quot;dijit.layout.ContentPane&quot; class=&quot;centerPanel&quot;&gt;
&lt;h1&gt;First Tab&lt;/h1&gt;
&lt;/div&gt;
[/sourcecode]</p>
<p>Above HTML code would create the tabbed pane for the first tab. As you must have noticed this time we have used the widget ContentPane for the data-dojo-type attribute in order to instantiate the same.</p>
<p>In the data-dojo-props attribute, we have set the title of the tab as First Tab.</p>
<p>Similarly, we will create the other two tabs.</p>
<p>Till this far, we have been using the markup style for creating the layout. Now we will use programmatic style to further add a menu in the First Tab. We will create the menu when the DOM will be ready. For the menu we will import the ComboButton, Menu and MenuItem widgets.
We are also handling the click event for the menu so whenever user clicks the menu he will be able to see a message in a div having id=”output”.</p>
<p>[sourcecode language="javascript"]</p>
<pre><code>require([&amp;quot;dojo/dom&amp;quot;,
        &amp;quot;dijit/form/ComboButton&amp;quot;, 
        &amp;quot;dijit/Menu&amp;quot;, 
        &amp;quot;dijit/MenuItem&amp;quot;,
        &amp;quot;dojo/domReady!&amp;quot;
        ], function(dom, ComboButton, Menu, MenuItem, domReady) {
          var menu = new Menu({ id: &amp;quot;optionsMenu&amp;quot;});
          var menuItem1 = new MenuItem({ label: &amp;quot;Option 1&amp;quot;,
                                       onClick: function() { dom.byId(&amp;quot;output&amp;quot;).innerHTML = &amp;quot;Option 1 Clicked&amp;quot;; }
                            });
          var menuItem2 = new MenuItem({ label: &amp;quot;Option 2&amp;quot;,
                              onClick: function() { dom.byId(&amp;quot;output&amp;quot;).innerHTML = &amp;quot;Option 2 Clicked&amp;quot;;  }
                           });
          var menuItem3 = new MenuItem({ label: &amp;quot;Option 3&amp;quot;,
                              onClick: function() { dom.byId(&amp;quot;output&amp;quot;).innerHTML = &amp;quot;Option 3 Clicked&amp;quot;;  }
                           });

         menu.addChild(menuItem1);
         menu.addChild(menuItem2);
         menu.addChild(menuItem3);
         var comboButton = new ComboButton({ optionsTitle: &amp;quot;Save Options&amp;quot;, label: &amp;quot;Menu&amp;quot;, dropDown: menu,
                                 onClick:function(){ dom.byId(&amp;quot;output&amp;quot;).innerHTML = &amp;quot;Menu Clicked&amp;quot;; }
                              }, &amp;quot;combo&amp;quot;);

         //calling the startup methods in the widgets is necessary. Otherwise widgets won't show up properly.
         comboButton.startup();
         menu.startup();
});
</code></pre>
<p>[/sourcecode]</p>
<p>This is how we create layouts in Dojo.
Here is the summary:
1. We learned markup and programmatic ways of creating layouts in Dojo.
2. We created a tabbed pane with three tabs by using markup style.
3. We created a menu on the first tab by programmatic way of creating the layout.</p>

## Comments

**[Anandarajeshwaran.J](#8795 "2012-05-15 12:29:24"):** I think the following tutorial will help the readers gain more knowledge. http://www.ibm.com/developerworks/web/tutorials/wa-dojotoolkit/section5.html

