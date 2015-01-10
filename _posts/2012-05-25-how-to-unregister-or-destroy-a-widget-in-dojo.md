---
layout: post
header-img: img/default-blog-pic.jpg
author: anubhava
description: 
post_id: 14104
created: 2012/05/25 09:27:17
created_gmt: 2012/05/25 04:27:17
comment_status: open
---

# How to unregister or destroy a widget in Dojo

There are many sites, which tell you how to unregister widget / id from dijit registery. I tried most of them ( or perheps all of them ) to unregister my widget but nothing worked, and always ended up with the following error:

**_Error: Tried to register widget with id==country-tree-container that id is already registered_**

In this blog, I am providing yet-another-solution that may work for everyone or for some. 

**Problem Statement:** I wanted to re-create the dijit.Tree whenever user performs create/update/delete action on that tree. I wanted to remove the whole tree and re-create it.

Here what was my code looked like before:

[sourcecode language="html"] <!-- HTML --> <div id="country-tree-container"></div> [/sourcecode]

[sourcecode language="javascript"] // Javascript require(["dojo/data/ItemFileWriteStore", "dijit/tree/ForestStoreModel", "dijit/Tree"], function(Store, Model, dijitTree){ //other piece of code self = this;
    
    
        populateTree = function(){
            self.store = new Store({
                data: self.store_data
            });
            self.treeModel = new Model({
                store: self.store,
                rootId: self.project_root,
                query: {&quot;type&quot;: &quot;country&quot;},
                childrenAttrs: [&quot;children&quot;]
            });
            self.tree = new dijitTree({
                model: self.treeModel,
                showRoot: false
            }, &quot;country-tree-container&quot;);
        }
    });
    

[/sourcecode]

and here what it is now:

[sourcecode language="html"] <!-- No Change --> <div id="country-tree-container"></div> [/sourcecode]

[sourcecode language="javascript"] //Javascript (changes in require and populateTree functions)

require(["dojo/dom-construct", "dojo/data/ItemFileWriteStore", "dijit/tree/ForestStoreModel", "dijit/Tree"], function(domConstruct, Store, Model, dijitTree){ //other piece of code self = this;
    
    
        populateTree = function(){
                var treeContainer = dijit.byId('tree-container');
                if (treeContainer &amp;&amp; treeContainer instanceof dijitTree) {
                     //Destroy this widget and it's descendants from the manager object
                     treeContainer.destroyRecursive(false);
                }
                domConstruct.destroy(&quot;tree-container&quot;);
                // create as child node of 'country-tree-container' div
                domConstruct.create('div',{
                        id:'tree-container'
                },
                'country-tree-container');
    
                self.store = new Store({data: self.store_data   });
                self.treeModel = new Model({
                        store: self.store,
                        rootId: self.project_root,
                        query: {&quot;type&quot;: &quot;country&quot;},
                        childrenAttrs: [&quot;children&quot;]
                });
                self.tree = new dijitTree({
                        model: self.treeModel,
                        showRoot: false
                        }, &quot;tree-container&quot;);
        }
        });
    

[/sourcecode]

We added our tree in 'tree-container' instead of 'country-tree-container'.

**Step 1 **: get the dijit.Tree **Step 2 **: check if that tree exists (null for the first time) **Step 3 **: if exists delete/unregister/kill the whole tree **Step 4 **: construct the <div id='tree-container'...> with dom-construct as a child node of 'country-tree-container' div

The above solution worked perfectly for me. If you have any question for me or any suggestion please let me know.

**Reference:** Thanks to Fabián Mandelbaum and [his solution][1].

That's it folks...

   [1]: http://mail.dojotoolkit.org/pipermail/dojo-interest/2009-October/040079.html