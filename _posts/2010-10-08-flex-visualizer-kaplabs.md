---
layout: post
header-img: img/default-blog-pic.jpg
author: GauravS
description: 
post_id: 5054
created: 2010/10/08 09:14:19
created_gmt: 2010/10/08 04:14:19
comment_status: open
---

# Data Visualization made better in Adobe Flex

<p>Recently i came across a requirement of showing the grid-based data in a more fancy way, probably in a way Flex is known for. There were some problems in displaying a grid-based data. Every cell of a Flex datagrid is basically a label. But what if we want to show an employee's complete information (picture, full name, reporting structure, current/past project details)? It's true that we can have item-renderers to show this. But for heavy datasets, will it really be a good option from usability perspective?</p>
<p>Another problem i can think of - Consider a project lead wants to see his/her team. We always have an option to double click on any cell of the datagrid to view the further data, and a back button to return to the original. But by simply giving an expand/collapse button on the data, aren't we making the user experience better?
<!--more--></p>
<p>To achieve this and many such use cases, I found a component that suit best for my requirement. KAPLABS's Visualizer is the component i will be talking about. It's a powerful component that provides you with a vanilla implementation of all fancy stuff that you would want. Built in features, like zoom-in/zoom-out support, customizable nodes, expand/collapse buttons and many more adds cherry on the top.</p>
<p>To look at the official website, please follow this link – <a href="http://lab.kapit.fr/display/visualizer/Visualizer" target="_blank">http://lab.kapit.fr/display/visualizer/Visualizer</a>. In this blog, I will be sharing my experiences with this component.</p>
<p>To get started, download the swc from the official site.</p>
<p>Apart from simply displaying the data in this component, I will be sharing my ideas on the following areas-
<ul>
    <li>Customize the result object on XML grounds.</li>
    <li>Show a close button on selective nodes.</li>
</ul>
To start with, the first story which we took was to populate the component with the actual data.</p>
<p>(Did you just see the word 'story'? Yes, you guessed it right – We are Agile!). The actual data was a result of some remote-service, which was returned as a collection in Flex. Had it been the normal ArrayCollection for Flex, I could have simply passed it to the component. But as we all know, we generally get complex objects from the back-end, which may or may not be component-friendly. So to make it easy, try converting that object into simple XML or ArrayCollection. If you are looking for a helping-hand on this, then here is a tip.</p>
<p>Assuming the variable 'resultObject' holds your result, just run a for each loop on it.</p>
<p>[java]</p>
<p>for each(var obj:Object in resultObject) {
// instantiate a VO which will be mapped through a RemoteClass metatag.
// start appending childs to your XML.
}</p>
<p>[/java]</p>
<p>I faced another issue where I was asked to center any node whenever user clicks a link on the node. By centering the node I mean, to show immediate parents/children of the selected node. Well your life will be easy if you are playing around with XML-typed data-structure. Again a for-each loop with some if-else can get you the result. This is just a tip, which might not work if your requirement is different. Feel free to buzz me if you are stuck.</p>
<p>Going further, there was a 'close' button to be placed on some of the nodes. Showing a close button on all nodes was pretty easy. You can just provide a function to property named 'multimediaFunction' wherein you can define a renderer for the component.</p>
<p>Something like below-</p>
<p>Inside customNodeRendering, we can have the renderer-
[java]</p>
<p>private function customNodeRendering(data:Object):IListItemRenderer {
var visualizerRenderer:VisualizerRenderer = new VisualizerRenderer();
visualizerRenderer.data = data;
return visualizerRenderer;
}</p>
<p>[/java]</p>
<p>Now my requirement here was to display a close button on some of the nodes. My life is already easy as I am playing with XML-typed structure. I just need to place a if-else block inside my renderer that will check for the value in the self-made XML. Something like,</p>
<p>[java]</p>
<p>private function customNodeRendering(data:Object):IListItemRenderer {
var visualizerRenderer:VisualizerRenderer = new VisualizerRenderer();
visualizerRenderer.data = data;
if(data['@showCloseButton']) {
//do something
}
else {
//do something
}
return visualizerRenderer;
}</p>
<p>[/java]</p>
<p>Leaving you to play around with Visualizer. Feel free to get back, in case you are tired of googling.</p>

## Comments

**[vinsu](#7461 "2012-02-08 09:38:34"):** Hi, I am working with KapIt Visualizer. My requirement is to show just the node name initially and when the user zooms in, show more details (image, address etc) on that node. So I was wondering how to do this. Can you please shed some light on this?

**[Gaurav Srivastava](#7502 "2012-02-09 10:08:43"):** Hi Vinsu, as i mentioned in the blog that this component gives all vanilla implementation of all fancy things you want. So, all you need to do is to provide it with a dataProvider and set the visibility level = 1. By setting the visibility level, you are making sure that the user see only primary details such as first name, last name, etc. As the user zooms in, user can see more details (like designation, reporting manager, etc.) Let me know if you have already tried something similar and still getting problems.

