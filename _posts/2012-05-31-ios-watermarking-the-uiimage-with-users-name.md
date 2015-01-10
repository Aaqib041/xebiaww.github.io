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

One of the requirements in recent project was to add user's name to the current image clicked by camera, preserving the font as selected in the preferences. So a user can click a photo using UIImagePicker and once the photo is captured we will add the name of the user to the image.

To start with lets assume we have a image in our UIImage already with us. iOS provides a Core Graphics (CG) library.Core Graphics is a quartz based drawing engine. We can use basic CG library to perform path based drawing. Please note the co-ordinate system for the core graphics is flipped hence in the code below you will see the coordinates flipped as we will be using CG coordinate system.

To get started first we need to create a context for drawing. This is more like a drawing board where we define drawing area and initialize it with drawing properties. What we are going to do is create a bitmap context so we will be able to define pixels by RGB and alpha. Following is the signature of creating a bitmap context

``` 


CGBitmapContextCreate(void *data>, size_t width>, size_t height, size_t bitsPerComponent, size_t bytesPerRow, CGColorSpaceRef space, CGBitmapInfo bitmapInfo);


 ```

As you would have noticed, its a C based API from the way above function is defined. Most of the arguments are self explanatory, what is new is CGColorSpaceRef , CGBitmapInfo arguments. CGColorSpace helps us define what kind of color space we would like to create RGB , CMYK , Gray , etc. We are going to create RGB Color space. Second input is a Bitmap info were we can specify CGImageAlphaInfo. So Following is the code for creating context

``` 


CGColorSpaceRef colorSpace = CGColorSpaceCreateDeviceRGB(); CGContextRef contextForTextDrawing = CGBitmapContextCreate(NULL, aImage.size.width,aImage.size.height, 8, 4 * aImage.size.width, colorSpace, kCGImageAlphaPremultipliedFirst);


 ```

Now that we have the context ready we need to draw the image into the context. For this we have to first convert image into a CG image which is easily provided in UIImage api. We will use the CGContextDrawImage api for drawing on the context

``` 


CGRect rect = CGRectMake(0, 150, aImage.size.width, aImage.size.height);

CGContextDrawImage(contextForTextDrawing, rect, aImage.CGImage);


 ```

Now that we have the Image in the context we need to have text added to it. Before adding the text we need to prepare the context with Text properties. This is equivalent of setting the properties of the pen that we are going to draw with. First we are going to specify the font that is supposed to be used using the following API

``` 


CGContextSelectFont(contextForTextDrawing, "Futura-CondensedMedium", 80, kCGEncodingMacRoman);


 ```

Secondly we need to set the text drawing mode to be of fill style.

``` 


CGContextSetTextDrawingMode(contextForTextDrawing, kCGTextFill);


 ```

Lastly we specify the color as well as alpha for the text

``` 


CGContextSetRGBFillColor(contextForTextDrawing, 0, 0, 0, 1);


 ```

Now is the time to draw the text onto the context. As we mentioned CG Graphics is a C-based library we need to convert the text that we have in NSString to char* as the C library doesn't understand NSString.

``` 


char_ text = (char _)[aText cStringUsingEncoding:NSASCIIStringEncoding];

int textLength = strlen(text);

CGContextShowTextAtPoint(contextForTextDrawing, 0, 50 , text, textLength);

&nbsp;


 ```

With the above steps we have drawn the image, added the text to the context and we now have the final drawing present in the context. Its time to take a snapshop and return it as UIImage from the function

``` 


CGImageRef imageMasked = CGBitmapContextCreateImage(contextForTextDrawing);

CGContextRelease(contextForTextDrawing); CGColorSpaceRelease(colorSpace);

return [UIImage imageWithCGImage:imageMasked];


 ```

To summarize we first converted the UIImage to CGImage and later drawn it to the context configured for image. We later configured the context to take in text and drawn the text onto the context. Once we were ready with the final context we converted the context back to CGImage and later to UIImage before returning from the function. Please find attached Utility file for the same.[ImageUtility][1]

   [1]: http://xebee.xebia.in/wp-content/uploads/2012/05/ImageUtility.zip