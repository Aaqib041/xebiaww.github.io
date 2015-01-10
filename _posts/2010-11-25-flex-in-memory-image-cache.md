---
layout: post
header-img: img/default-blog-pic.jpg
author: sraheja
description: 
post_id: 6266
created: 2010/11/25 07:34:26
created_gmt: 2010/11/25 02:34:26
comment_status: open
---

# Flex: In-Memory Image Cache

<p><strong>The Need:</strong></p>
<p><strong> </strong>Flex, by default, maintains an in-memory cache for all the images embedded in the application at compile time. However, for the images loaded at runtime, there is no default cache implementation. So, over a period of time it may happen that we have redundant image data (BitmapData) in the memory. This would unnecessarily increase application's total memory usage and data transfer over the network.</p>
<p>Let us analyze difference in total memory consumption when we use an image once or multiple number of times for both the scenarios:</p>
<!--more-->

<ul>
    <li style="font-style: italic;">Embed at compile time:
<span style="font-style: normal;">The application with single embed takes around 83.8 MB in my browser:
</span><span style="font-style: normal;">
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;mx:Image source="@Embed(source='images/2MB.jpg')" width="400" height="400"/&gt;
</span>
<span style="font-style: normal;">With multiple embeds (10) for the same image, it takes around 84.1 MB in total. The difference is almost negligible.
</span></li>
</ul>

<ul>
    <li style="font-style: italic;">Load at runtime:
<span style="font-style: normal;">A single usage of an image without embedding it at compile time, i.e. by specifying a URL from where image will be loaded at runtime, takes around 85 MB of memory.
</span><span style="font-style: normal;">
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;mx:Image source="images/2MB.jpg" width="400" height="400"/&gt;
</span><span style="font-style: normal;">
However, using it multiple number of times(10) takes around 138 MB in total. There is a significant difference of 53 MB in this scenario.</span></li>
</ul>

<p><strong>Problem:</strong></p>
<p>The redundant image data in memory can cost us a lot and make our application sluggish in nature.
<strong> </strong></p>
<p><strong>Solution:
</strong>A simple solution for this problem can be to embed all the images into our application at compile time. However:
<ul>
    <li>That will drastically increase overall SWF size of the application.</li>
    <li>Also, nature of some of the applications may require loading images at run time.</li>
</ul>
<!-- p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; font: 11.0px Monaco} p.p2 {margin: 0.0px 0.0px 0.0px 0.0px; font: 11.0px Monaco; min-height: 15.0px} -->A better solution is to have an in-memory image data cache. This can optimize the total memory usage and will also reduce overhead of loading external resources again and again. However, even after the cache in place, we will have to write plumbing code everywhere for making a lookup into cache before loading an image and also add loaded image data into the cache.</p>
<p>So, here is an implementation of CachedImage component which extends from Image component. The only difference is, for a given source URL, it will first make a lookup into ImageDataCache. If it finds an appropriate BitmapData, it will re-use it else it will load data from the URL and add it to the cache. So, we can easily replace all instances of Image component in our application by CachedImage and save a lot in terms of total memory usage and data transfer over the network.</p>
<p>Here is the implementation for few important methods of CachedImage component. Declarative variable and function names along with code comments will help you understand the code better. Complete source code for both the components (CachedImage and ImageDataCache) is enclosed at the end.</p>
<p>[sourcecode language="actionscript3"]
/<strong>
* Setter for source, it makes a lookup into the cache for existing image data.
</strong>/
override public function set source(value:Object):void {</p>
<pre><code>if(value is String) {

    _imageId = value as String;

    var hasImageData:int = _imageDataCache.hasImageData(_imageId);

    if(hasImageData == ImageDataCache.IMAGE_DATA_PRESENT) {

        // the scenario when image data is already there in cache

        var bitmapData:BitmapData = _imageDataCache.getImageData(_imageId);

        super.source = new Bitmap(bitmapData);

    } else if(hasImageData == ImageDataCache.IMAGE_DATA_LOADING) {

        // the scenario when image data is currently not in cache, however, load

        // request for it has already been started by some other CachedImage instance

        _imageDataCache.addEventListener(ImageDataCacheEvent.IMAGE_DATA_ADDED,

            onImageDataAddedInCache, false, 0, true);

    } else {

        // the scenario when image data will be loaded into memory for this instance

        addEventListener(Event.COMPLETE, onImageLoadComplete, false, 0, true);

        _imageDataCache.markImageDataLoading(_imageId);

        super.source = value;

    }

} else {

    // the scenario when image data is embedded into the application at compile time

    super.source = value;

}
</code></pre>
<p>}</p>
<p>/<strong>
<em> onImageDataLoadComplete is the handler for image load complete event.
</em>
* It puts loaded BitmapData into the cache if it is not there</strong>/</p>
<p>private function onImageDataLoadComplete(event:Event):void {</p>
<pre><code>if(_imageDataCache.hasImageData(_imageId) != ImageDataCache.IMAGE_DATA_PRESENT) {

    var bitmapData:BitmapData = (content as Bitmap).bitmapData;

    _imageDataCache.putImageData(_imageId, bitmapData);

}
</code></pre>
<p>}</p>
<p>[/sourcecode]</p>
<p>ImageDataCache implements following methods:</p>
<p>[sourcecode language="actionscript3"]</p>
<p>function getImageData(imageId:String):BitmapData;</p>
<p>function putImageData(imageId:String, imageData:BitmapData):void;</p>
<p>function markImageDataLoading(imageId:String);</p>
<p>function hasImageData(imageId:String);</p>
<p>function purgeImageData(imageId:String);</p>
<p>function clear();</p>
<p>[/sourcecode]</p>
<p><strong>Conclusion:</strong></p>
<p>The difference in using CachedImage once and 10 times for the same URL is almost negligible (0.2 MB), however that is huge for Image (53 MB). You can download the complete source code <a href='http://xebee.xebia.in/wp-content/uploads/2010/11/ImageCacheProject.zip'>here</a>.</p>

## Comments

**[josh](#4973 "2011-01-20 04:52:44"):** how might this be applied to images that are loaded into a dynamic text box via an external simple html file?

**[sraheja](#3401 "2010-11-27 19:09:46"):** Thanks Ajay, the difference is only for 9 redundant instances of image (2MB in size) when loaded in memory. In case, we use more instances of the same Image, the difference will increase accordingly.

**[Ajay Bansal](#3378 "2010-11-26 13:57:26"):** Nice thought. Keep it up! But what I don't understand is this difference of 53 MB. If FLEX is storing all the earlier images, then it should be much more.

**[Pradeep](#5015 "2011-01-24 15:44:00"):** Thanks Ajay. Nice Post. It rectified my confusion of loading an image at compile time and run time.

**[Martijn Mol](#5555 "2011-05-06 15:48:54"):** Nice post. One remark: The ImageCache class only works with Bitmap images. when you try to load a .swf, you'll get a runtime Error at var bitmapData:BitmapData = (content as Bitmap).bitmapData;

**[Radu Cugut](#5781 "2011-07-29 01:11:47"):** Very ingenious way for caching the images. I thought of something similar, but I was too lazy to actually do it so clever like this :-) You might want to add the next line in the CachedImage constructor: loaderContext = new LoaderContext(true); because otherwise it will throw 'SecurityError: Error #2122: Security sandbox violation' if the images are from another domain, even if that domain has a crossdomain.xml file active. Great post!

**[Robert](#8522 "2012-04-21 01:34:26"):** Great post! This saved me loads of time. I was doing something similar, but it wasn't fitting in my architecture very well. This fit like a glove. Good job!

