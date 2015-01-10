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

Dojo is a powerful JavaScript framework for developing web applications. I was exploring layouts in Dojo and found that it provides us various amazing widgets as a part of Dijit (Dojo's UI framework) which helps us in creating layouts with great user experience quickly. In this blog we will see how we can create layouts in Dojo. Basically, what we are going to do is that we will create a simple layout with three tabbed panes. And in the first tabbed pane we will create a menu .

Dojo provides us two ways to implement the layouts. One is markup style and the other way is creating layouts programmatically. I explored both the ways and in this blog we will use both ways to create the layout.

So lets begin. In the header section of our HTML file, we include the dojo.js file as shown below:

[sourcecode language="html"] <script src='http://ajax.googleapis.com/ajax/libs/dojo/1.7.1/dojo/dojo.js' data-dojo-config="async: true, parseOnLoad: true"> </script> [/sourcecode]

As you might have noticed, the configuration parameters are async:true and pasrseOnLoad: true.  
**async ** defines if Dojo core should be loaded asynchronously. **parseOnLoad ** If true, parses the page with dojo/parser when the DOM and all initial dependencies have loaded. 

Now our next step is to create the layout by using the declarative syntax. By declarative syntax we will create a Tabbed Pane with three tabs namely First Tab, Second Tab and Third Tab. Dojo provides us the HTML widgets which we can import and use to create layouts quickly and that is what exactly we are going to do.

Also Dojo provides us the themes which we can use to provide our layout the feel-good factor.

[sourcecode language="html"] <link rel="stylesheet" media="screen" href="http://ajax.googleapis.com/ajax/libs/dojo/1.7.1/dijit/themes/claro/claro.css" /> [/sourcecode]

The above tag will include the CSS for claro theme, other themes are also available like Soria, Tundra, Nihilo etc. For more information on themes visit the below link: <http://dojotoolkit.org/reference-guide/1.7/dijit/themes.html>

After importing the theme just include the class “claro” in the body tag of page. This will appropriately style the entire html elements which we are going to create inside the body of the page.

In the markup style of creating layout in Dojo, it (Dojo) relies heavily on data-dojo- attributes. These attributes tell the Dojo what widget class to instantiate and then which properties to apply on the widget. So this means below HTML div is not just ordinary div but infact it is TabConatiner Widget provided by the Dojo.

[sourcecode language="html"] <div class="demoLayout" id="appLayout" data-dojo-props="region: 'center', tabPosition: 'bottom'" data-dojo-type="dijit.layout.TabContainer"> </div> [/sourcecode]

The above HTML code will create a Tabbed Container which allows us to create tabs inside it. The data-dojo-type attribute tell the Dojo parser to instantiate the TabContainer widget. And the data-dojo-props attribute defines the additional parameters for the widget. The parameter region tell the place where TabContainer is to be placed and the tabPosition defines the position of the tabs which will be used by the user to navigate among the tabs.

Now inside the div with id=”appLayout” place the following code:

[sourcecode language="html"] <div data-dojo-props="title: 'First Tab'" data-dojo-type="dijit.layout.ContentPane" class="centerPanel"> <h1>First Tab</h1> </div> [/sourcecode]

Above HTML code would create the tabbed pane for the first tab. As you must have noticed this time we have used the widget ContentPane for the data-dojo-type attribute in order to instantiate the same.

In the data-dojo-props attribute, we have set the title of the tab as First Tab.

Similarly, we will create the other two tabs.

Till this far, we have been using the markup style for creating the layout. Now we will use programmatic style to further add a menu in the First Tab. We will create the menu when the DOM will be ready. For the menu we will import the ComboButton, Menu and MenuItem widgets. We are also handling the click event for the menu so whenever user clicks the menu he will be able to see a message in a div having id=”output”.

[sourcecode language="javascript"]
    
    
    require([&quot;dojo/dom&quot;,
            &quot;dijit/form/ComboButton&quot;, 
            &quot;dijit/Menu&quot;, 
            &quot;dijit/MenuItem&quot;,
            &quot;dojo/domReady!&quot;
            ], function(dom, ComboButton, Menu, MenuItem, domReady) {
              var menu = new Menu({ id: &quot;optionsMenu&quot;});
              var menuItem1 = new MenuItem({ label: &quot;Option 1&quot;,
                                           onClick: function() { dom.byId(&quot;output&quot;).innerHTML = &quot;Option 1 Clicked&quot;; }
                                });
              var menuItem2 = new MenuItem({ label: &quot;Option 2&quot;,
                                  onClick: function() { dom.byId(&quot;output&quot;).innerHTML = &quot;Option 2 Clicked&quot;;  }
                               });
              var menuItem3 = new MenuItem({ label: &quot;Option 3&quot;,
                                  onClick: function() { dom.byId(&quot;output&quot;).innerHTML = &quot;Option 3 Clicked&quot;;  }
                               });
    
             menu.addChild(menuItem1);
             menu.addChild(menuItem2);
             menu.addChild(menuItem3);
             var comboButton = new ComboButton({ optionsTitle: &quot;Save Options&quot;, label: &quot;Menu&quot;, dropDown: menu,
                                     onClick:function(){ dom.byId(&quot;output&quot;).innerHTML = &quot;Menu Clicked&quot;; }
                                  }, &quot;combo&quot;);
    
             //calling the startup methods in the widgets is necessary. Otherwise widgets won't show up properly.
             comboButton.startup();
             menu.startup();
    });
    

[/sourcecode]

This is how we create layouts in Dojo. Here is the summary: 1\. We learned markup and programmatic ways of creating layouts in Dojo. 2\. We created a tabbed pane with three tabs by using markup style. 3\. We created a menu on the first tab by programmatic way of creating the layout.

## Comments

**[Anandarajeshwaran.J](#8795 "2012-05-15 12:29:24"):** I think the following tutorial will help the readers gain more knowledge. http://www.ibm.com/developerworks/web/tutorials/wa-dojotoolkit/section5.html

