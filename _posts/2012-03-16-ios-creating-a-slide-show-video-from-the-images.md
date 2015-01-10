---
layout: post
header-img: img/default-blog-pic.jpg
author: sohil
description: 
post_id: 12656
created: 2012/03/16 02:39:33
created_gmt: 2012/03/15 21:39:33
comment_status: open
---

# iOS : Creating a slide show video from the images

Lets say i have a list of images and I would like to create a slideshow video , so that i can easily share this video with anyone I wish to. iOS API gives a very easy way of creating the videos from the photo with the help of following libraries AVFoundation , CoreVideo , CoreMedia frameworks. So lets get started Following are the simple steps that need to be followed to create a a slideshow from the list of images. 

  * Calculate the size of video based on max height and width of the images.
  * Initialize the video writer with appropriate video parameters.
  * Defining the parameters for the video stream.
  * Loop through all the images and convert the image from UIImage to pixel so that it can be written on the video stream
  * Once done close the stream and finally close the video writer.
The first step here is pretty simple. Loop through all the images and find out the maxHeight and maxWidth of all the image.For the second step we need to define the kind of media file we want to make. AVFoundation provides us with a class named AVAssetWriter, which we can initialize with the path where we want to create the file and the type of file. It can be a audio / video,etc . We are going to initialize the writer to create a video file

``` 
 AVAssetWriter *videoWriter = [[AVAssetWriter alloc] initWithURL:

[NSURL fileURLWithPath:filePath] fileType:AVFileTypeQuickTimeMovie error:&error]; 
 ```

The next step is to create a video stream for this we need to figure out what is the video codec that we need to use and what id the height & width of video stream. We have already calculated the maxHeight and maxWidth for the video stream and we are going to use H264 as the codec for the video. AVFoundation has a class named AVAssetWriterInput which can be used to create audio video stream. We will be initializing the stream with videosettings as shown

``` 


NSDictionary *videoSettings = [NSDictionary dictionaryWithObjectsAndKeys: AVVideoCodecH264, AVVideoCodecKey,

[NSNumber numberWithInt:maxwidth], AVVideoWidthKey,

[NSNumber numberWithInt:maxheight], AVVideoHeightKey,

nil];

AVAssetWriterInput* videoStream = [[AVAssetWriterInput assetWriterInputWithMediaType:AVMediaTypeVideo  outputSettings:videoSettings] retain]; 
 ```

Life would have been every simple if we have to only create the stream and write the images to the buffer.However we cannot do so. One was of doing this is creating the PixelBuffer adaptor and initalize it to the stream and write using the adaptor.This is what we are going to do.

``` 


AVAssetWriterInputPixelBufferAdaptor *adaptor = [AVAssetWriterInputPixelBufferAdaptor

assetWriterInputPixelBufferAdaptorWithAssetWriterInput: videoStream

sourcePixelBufferAttributes:nil];


 ```

Once done add the stream to the video writer

``` 


[videoWriter addInput: videoStream];


 ```

Using the asset writer start creating the video now.

``` 


[videoWriter startWriting];

[videoWriter startSessionAtSourceTime:kCMTimeZero]; 
 ```

Moving forward we are going to create convert every image into a pixel buffer and add this buffer as a frame for the video at nth time. frameCount is the one that keeps the seconds at which we want the image to be displayed currently it is absolute.I have found a simple function from internet that converts the UIImage to pixel buffer. This takes a Image as a input and converts it to pixel buffer

``` 


  * (CVPixelBufferRef) pixelBufferFromCGImage: (CGImageRef) image andSize:(CGSize) size { NSDictionary _options = [NSDictionary dictionaryWithObjectsAndKeys: [NSNumber numberWithBool:YES], kCVPixelBufferCGImageCompatibilityKey, [NSNumber numberWithBool:YES], kCVPixelBufferCGBitmapContextCompatibilityKey,nil]; CVPixelBufferRef pxbuffer = NULL; CVReturn status = CVPixelBufferCreate(kCFAllocatorDefault, size.width, size.height, kCVPixelFormatType_32ARGB, (CFDictionaryRef) options,&pxbuffer); NSParameterAssert(status == kCVReturnSuccess && pxbuffer != NULL); CVPixelBufferLockBaseAddress(pxbuffer, 0); void _pxdata = CVPixelBufferGetBaseAddress(pxbuffer); NSParameterAssert(pxdata != NULL); CGColorSpaceRef rgbColorSpace = CGColorSpaceCreateDeviceRGB(); CGContextRef context = CGBitmapContextCreate(pxdata, size.width, size.height, 8, 4*size.width, rgbColorSpace, kCGImageAlphaNoneSkipFirst); NSParameterAssert(context); CGContextConcatCTM(context, CGAffineTransformMakeRotation(0)); CGContextDrawImage(context, CGRectMake(0, 0, CGImageGetWidth(image), CGImageGetHeight(image)), image); CGColorSpaceRelease(rgbColorSpace); CGContextRelease(context); CVPixelBufferUnlockBaseAddress(pxbuffer, 0); return pxbuffer; } 
 ```

Now that we have everything ready we start fromt the first image and loop through each image convert the UIImage to pixels and add to the video stream. In this example we are showing only one image everysecond, hence we are incrementing the framecount after every. Note that after adding the buffer for every image we are waiting for the adapter stream to be ready. Hence adding a sleep time after every write. Once you are done writing all the images close the session for the stream and video writer to produce a slideshow video at desired url.

``` 
 CVPixelBufferRef buffer = NULL; int frameCount = 1;

for(UIImage *img in images) {

buffer = [self pixelBufferFromCGImage:[img CGImage] andSize:img.size]; BOOL append_ok = NO;

while (!append_ok){ if (adaptor.assetWriterInput.readyForMoreMediaData){ CMTime frameTime = CMTimeMake(frameCount,(int32_t) 1); append_ok = [adaptor appendPixelBuffer:buffer withPresentationTime:frameTime]; if(buffer) CVBufferRelease(buffer); [NSThread sleepForTimeInterval:0.05]; }else{ [NSThread sleepForTimeInterval:0.1]; } } frameCount++; }

[videoStream markAsFinished]; [videoWriter finishWriting];


 ```

So that was it to create a video from UIImages in iOS using AVFoundation. Unfortunately its a little lengthy procedure and could have been more API friend in creating / adding images to the stream , converting image into pixebuffer etc.

## Comments

**[Amor](#7940 "2012-03-16 14:53:01"):** Nice tutorial. Could you please send me the source code? Thank you!

