---
layout: post
header-img: img/default-blog-pic.jpg
author: ggembali
description: 
post_id: 6475
created: 2010/12/21 10:44:21
created_gmt: 2010/12/21 05:44:21
comment_status: open
---

# Caching in JBoss Seam based web applications

<p>Caching is one important area where you can drastically improve the performance of your web application. The data that needs to be cached can be of two different types : Rendered data (like html snippet,xml or JSON) and Java objects. <a href="http://seamframework.org/">JBoss seam </a>provides few out of the box caching solutions for both types of data. As seam is component based framework, it provides these features through pre-configured application scoped components. You can easily switch between different cache provider like JBoss Tree caching and EHCaching. Here, I will take you through different steps to use caching features in seam in an effective manner.</p>
<!--more-->

<p>As I said above, in Seam everything is a component (You can get full details <a href="http://docs.jboss.org/seam/2.2.0.GA/reference/en-US/html/concepts.html">here</a>). So first lets configure this caching component in Seam by specifying it in components.xml as follows.</p>
<p>[sourcecode language="xml"]
&lt;components xmlns=&quot;http://jboss.com/products/seam/components&quot;
    xmlns:cache=&quot;http://jboss.com/products/seam/cache&quot;
   xsi:schemaLocation=&quot;
    http://jboss.com/products/seam/core http://jboss.com/products/seam/core-2.2.xsd
    http://jboss.com/products/seam/cache http://jboss.com/products/seam/cache-2.2.xsd
&quot;&gt;
   &lt;cache:eh-cache-provider configuration=&quot;ehcache.xml&quot; /&gt;
&lt;/components&gt;[/sourcecode]</p>
<p>The caching configuration file provided above is optional and note that we are using ehache provider. So the configuration file format has no seam specific things. If you want you can switch your cache provider with tree cache just by replacing ehcache line with</p>
<p>[sourcecode language="xml"]&lt;cache:jboss-cache-provider configuration=&quot;treecache.xml&quot; /&gt;
[/sourcecode]</p>
<p>Irrespective of the provider, the following details will remain same.</p>
<h4>Page fragment caching :</h4>

<p>This feature provides a mechanism to cache the rendered data like html snippet (part of the html page). When I first heard about it, the first question that came to mind is <strong><em>"Why not cache the page using web server?"</em></strong>.  This is obviously the best option if you need to cache the whole page, but if you want to cache only a part of it, web server can not provide that. And another advantage you can have with page fragment caching is, programmatic control over caching.</p>
<p>In seam, a part of html can be cached just by surrounding it with<a href="http://www.jsftoolbox.com/documentation/seam/09-TagReference/seam-cache.html" target="_blank"> s:cache</a> tag.</p>
<p>[sourcecode language="html"]&lt;div&gt;
  &lt;span&gt;I am not cached &lt;/span&gt;
&lt;div&gt;
&lt;s:cache&gt;
    &lt;div&gt;
        &lt;span&gt;I am cached. No need to render every time  &lt;/span&gt;
    &lt;/div&gt;
&lt;/s:cache&gt;
[/sourcecode]</p>
<p>So simple. Isn't it?  You can provide regions and keys like in any normal caching, as attributes to further control caching.
<h4>Object Caching :</h4>
If you have used frameworks like Hibernate, you might have heard about caching the database returned objects (Level1 caching). Just like caching objects returned from database, you might like to use few objects returned from service layer. For example, if you are building social network site you might need list of country objects at different places like in registration form, search form etc. So it makes sense to cache them instead of populating them from database every time. Note that page fragment caching won't be effective if you are using this list at different places in different formats. To use this feature you can inject the cache provider component and use it to put\remove\update operations.</p>
<p>[sourcecode language="java"]
@In
private CacheProvider&lt;CacheManager&gt; cacheProvider;</p>
<hr />
<hr />
<p>Object obj = cacheProvider.get(&quot;key&quot;,&quot;region&quot;);
update(obj);
cacheProvider.put(&quot;key&quot;,&quot;region&quot;,obj);
[/sourcecode]</p>
<p>Here the CacheProvider is an abstract class that is a wrapper around underlying cache manager. So you can implement a custom cache provider by implementing this abstract class. Infact above page fragment caching also uses this <code>CacheProvider</code> in the background (afterall html snippet which is nothing but a String Object).
<h4>When to use what :</h4>
Well we have two ways of caching. But when to use page fragment caching and when to use object caching. One simple rule normally I use, to make this decision is - "<strong><em>Am I going to reuse the model objects at different places?</em></strong>".  If the answer is 'No', go for page fragment caching. Because it doesn't make sense go through all the layers to again create the same html fragment instead of caching the final output.</p>
<p>This is the basic setup that can get you started seam caching. Further you can explore providing custom caching manager, some more parameters to decide between page fragment caching and object caching. Suggestions and questions in these areas are most welcome :) .</p>