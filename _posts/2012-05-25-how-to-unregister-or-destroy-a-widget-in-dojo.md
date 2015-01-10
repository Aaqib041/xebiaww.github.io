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

<p>There are many sites, which tell you how to unregister widget / id from dijit registery. I tried most of them ( or perheps all of them ) to unregister my widget but nothing worked, and always ended up with the following error:</p>
<p><strong style='color:red'><em>Error: Tried to register widget with id==country-tree-container that id is already registered</em></strong></p>
<p>In this blog, I am providing yet-another-solution that may work for everyone or for some.
<!--more--></p>
<p><strong>Problem Statement:</strong> I wanted to re-create the dijit.Tree whenever user performs create/update/delete action on that tree.
I wanted to remove the whole tree and re-create it.</p>
<p>Here what was my code looked like before:</p>
<p>[sourcecode language="html"]
&lt;!-- HTML --&gt;
&lt;div id=&quot;country-tree-container&quot;&gt;&lt;/div&gt;
[/sourcecode]</p>
<p>[sourcecode language="javascript"]
// Javascript
require([&quot;dojo/data/ItemFileWriteStore&quot;, &quot;dijit/tree/ForestStoreModel&quot;, &quot;dijit/Tree&quot;],
    function(Store, Model, dijitTree){
        //other piece of code
        self = this;</p>
<pre><code>    populateTree = function(){
        self.store = new Store({
            data: self.store_data
        });
        self.treeModel = new Model({
            store: self.store,
            rootId: self.project_root,
            query: {&amp;quot;type&amp;quot;: &amp;quot;country&amp;quot;},
            childrenAttrs: [&amp;quot;children&amp;quot;]
        });
        self.tree = new dijitTree({
            model: self.treeModel,
            showRoot: false
        }, &amp;quot;country-tree-container&amp;quot;);
    }
});
</code></pre>
<p>[/sourcecode]</p>
<p>and here what it is now:</p>
<p>[sourcecode language="html"]
&lt;!-- No Change --&gt;
&lt;div id=&quot;country-tree-container&quot;&gt;&lt;/div&gt;
[/sourcecode]</p>
<p>[sourcecode language="javascript"]
//Javascript (changes in require and populateTree functions)</p>
<p>require([&quot;dojo/dom-construct&quot;, &quot;dojo/data/ItemFileWriteStore&quot;,
                    &quot;dijit/tree/ForestStoreModel&quot;, &quot;dijit/Tree&quot;],
    function(domConstruct, Store, Model, dijitTree){
        //other piece of code
        self = this;</p>
<pre><code>    populateTree = function(){
            var treeContainer = dijit.byId('tree-container');
            if (treeContainer &amp;amp;&amp;amp; treeContainer instanceof dijitTree) {
                 //Destroy this widget and it's descendants from the manager object
                 treeContainer.destroyRecursive(false);
            }
            domConstruct.destroy(&amp;quot;tree-container&amp;quot;);
            // create as child node of 'country-tree-container' div
            domConstruct.create('div',{
                    id:'tree-container'
            },
            'country-tree-container');

            self.store = new Store({data: self.store_data   });
            self.treeModel = new Model({
                    store: self.store,
                    rootId: self.project_root,
                    query: {&amp;quot;type&amp;quot;: &amp;quot;country&amp;quot;},
                    childrenAttrs: [&amp;quot;children&amp;quot;]
            });
            self.tree = new dijitTree({
                    model: self.treeModel,
                    showRoot: false
                    }, &amp;quot;tree-container&amp;quot;);
    }
    });
</code></pre>
<p>[/sourcecode]</p>
<p>We added our tree in 'tree-container' instead of 'country-tree-container'.</p>
<p><strong>Step 1 </strong>: get the dijit.Tree
<strong>Step 2 </strong>: check if that tree exists (null for the first time)
<strong>Step 3 </strong>: if exists delete/unregister/kill the whole tree
<strong>Step 4 </strong>: construct the &lt;div id='tree-container'...&gt; with dom-construct as a child node of 'country-tree-container' div</p>
<p>The above solution worked perfectly for me. If you have any question for me or any suggestion please let me know.</p>
<p><strong>Reference:</strong> Thanks to Fabián Mandelbaum and <a href="http://mail.dojotoolkit.org/pipermail/dojo-interest/2009-October/040079.html">his solution</a>.</p>
<p>That's it folks...</p>