---
layout: post
header-img: img/default-blog-pic.jpg
author: GauravS
description: 
post_id: 7600
created: 2011/01/30 13:50:27
created_gmt: 2011/01/30 08:50:27
comment_status: open
---

# Adobe Flex: Prototype ModelLocator

<p>We all know the advantage of having Singleton ModelLocator. In this blog, i will share my experience working on 'Prototype' ModelLocator.</p>
<p>Sometime back, i faced an issue while updating the view/components, which were binded to an ArrayCollection. This ArrayCollection was a public property in 'Singleton' ModelLocator. A TabNavigator with 2 tabs for instance, each tab containing one instance of component X. Component X was binded to 'objArrCollection', which was updated after a service call. The expected behavior should be - Only that instance of Component X should be updated for which the service call was made. So for example, if i make a service call from the first tab to update the first instance of Component X, then only this instance should be udpated. The second instance, present in the second tab should not change. But we were working with Singleton model, that super-impose the 'objArrCollection' with the latest value. Since objArrCollection is binded to both the views, hence both get updated. So in such cases, it is always good to have Prototype ModelLocators.<!--more--></p>
<p>In above mentioned scenario, you can work very well using Singleton modellocators. But then you need to have checks in place. For example, if you are making a service call from the first tab, then after getting result and before actually binding it to the view, you might need some kind of check to see if only the first tab is updated. May be a dictionary, which has 'key' as the tab/component ID and the 'value' as the result. These kind of checks may be complex or even dirty at times. To avoid this you can make your model as prototype. </p>
<p>With prototype models, you are assured that every view has its own model. So the views don't confuse when to udpate themselves. In the use case mentioned above, both the tabs have different instances of the ModelLocator. </p>
<p>Talking about simple MVC based applications, there are many ways to manage the scope of models (as singleton or prototype). In our application, we used Spring ActionScript to manage the top level code, where we specified the scope for our controllers/models.</p>
<p>[code]
&lt;!-- Model --&gt;
&lt;Object id=&quot;applicationModel&quot; clazz=&quot;{ApplicationModel}&quot; scope=&quot;prototype&quot;/&gt;</p>
<p>&lt;!-- Controller --&gt;
&lt;Object id=&quot;applicationController&quot; clazz=&quot;{ApplicationController}&quot; scope=&quot;prototype&quot;&gt;
    &lt;ConstructorArg ref=&quot;{applicationModel}&quot;/&gt;
&lt;/Object&gt;
[/code]</p>
<p>Prototype ModelLocator also helps in managing data model while creating Dashboard applications. You may have different views to represent data. Some may be in charts, and some in grids. A dashboard might need to show all these views all-together. And again the same use case, user gesture should update only the intended data model.</p>