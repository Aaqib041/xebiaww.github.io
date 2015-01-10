---
layout: post
header-img: img/default-blog-pic.jpg
author: sohil
description: 
post_id: 14327
created: 2012/05/31 21:28:52
created_gmt: 2012/05/31 16:28:52
comment_status: open
---

# iOS : Watermarking the UIImage with user's name

<p>One of the requirements in recent project was to add user's name to the current image clicked by camera, preserving the font as selected in the preferences. So a user can click a photo using UIImagePicker and once the photo is captured we will add the name of the user to the image.<!--more--></p>
<p>To start with lets assume we have a image in our UIImage already with us. iOS provides a Core Graphics (CG) library.Core Graphics is a quartz based drawing engine. We can use basic CG library to perform path based drawing. Please note the co-ordinate system for the core graphics is flipped hence in the code below you will see the coordinates flipped as we will be using  CG coordinate system.</p>
<p>To get started first we need to create a context for drawing. This is more like a drawing board where we define drawing area and initialize it with drawing properties. What we are going to do is create a bitmap context so we will be able to define pixels by RGB and alpha. Following is the signature of creating a bitmap context</p>
<p>[code]</p>
<p>CGBitmapContextCreate(void *data&gt;, size_t width&gt;, size_t height,
size_t bitsPerComponent, size_t bytesPerRow, CGColorSpaceRef space, CGBitmapInfo bitmapInfo);</p>
<p>[/code]</p>
<p>As you would have noticed, its a C based API from the way above function is defined. Most of the arguments are self explanatory, what is new is CGColorSpaceRef , CGBitmapInfo arguments. CGColorSpace helps us define what kind of  color space we would like to create RGB , CMYK , Gray , etc. We are going to create RGB Color space. Second input is a Bitmap info were we can specify CGImageAlphaInfo.  So Following is the code for creating context</p>
<p>[code]</p>
<p>CGColorSpaceRef colorSpace = CGColorSpaceCreateDeviceRGB();
CGContextRef contextForTextDrawing = CGBitmapContextCreate(NULL, aImage.size.width,aImage.size.height,
                       8, 4 * aImage.size.width, colorSpace, kCGImageAlphaPremultipliedFirst);</p>
<p>[/code]</p>
<p>Now that we have the context ready we need to draw the image into the context. For this we have to first convert image into a CG image which is easily provided in UIImage api. We will use the CGContextDrawImage api for drawing on the context</p>
<p>[code]</p>
<p>CGRect rect =  CGRectMake(0, 150, aImage.size.width, aImage.size.height);</p>
<p>CGContextDrawImage(contextForTextDrawing, rect, aImage.CGImage);</p>
<p>[/code]</p>
<p>Now that we have the Image in the context we need to have text added to it. Before adding the text we need to prepare the context with Text properties.  This is equivalent of setting the properties of the pen that we are going to draw with.  First we are going to specify the font that is supposed to be used using the following API</p>
<p>[code]</p>
<p>CGContextSelectFont(contextForTextDrawing, &quot;Futura-CondensedMedium&quot;, 80, kCGEncodingMacRoman);</p>
<p>[/code]</p>
<p>Secondly we need to set the text drawing mode to be of fill style.</p>
<p>[code]</p>
<p>CGContextSetTextDrawingMode(contextForTextDrawing, kCGTextFill);</p>
<p>[/code]</p>
<p>Lastly we specify the color as well as alpha for the text</p>
<p>[code]</p>
<p>CGContextSetRGBFillColor(contextForTextDrawing, 0, 0, 0, 1);</p>
<p>[/code]</p>
<p>Now is the time to draw the text onto the context. As we mentioned CG Graphics is a C-based library we need to convert the text that we have in NSString to char* as the C library doesn't understand NSString.</p>
<p>[code]</p>
<p>char<em> text = (char </em>)[aText cStringUsingEncoding:NSASCIIStringEncoding];</p>
<p>int textLength = strlen(text);</p>
<p>CGContextShowTextAtPoint(contextForTextDrawing, 0, 50 , text, textLength);</p>
<p>&amp;nbsp;</p>
<p>[/code]</p>
<p>With the above steps we have drawn the image, added the text to the context and we now have the final drawing present in the context. Its time to take a snapshop and return it as UIImage from the function</p>
<p>[code]</p>
<p>CGImageRef imageMasked = CGBitmapContextCreateImage(contextForTextDrawing);</p>
<p>CGContextRelease(contextForTextDrawing);
CGColorSpaceRelease(colorSpace);</p>
<p>return [UIImage imageWithCGImage:imageMasked];</p>
<p>[/code]</p>
<p>To summarize we first converted the UIImage to CGImage and later drawn it to the context configured for image. We later configured the context to take in text and drawn the text onto the context. Once we were ready with the final context we converted the context back to CGImage and later to UIImage before returning from the function. Please find attached Utility file for the same.<a href="http://xebee.xebia.in/wp-content/uploads/2012/05/ImageUtility.zip">ImageUtility</a></p>