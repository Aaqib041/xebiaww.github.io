---
layout: post
header-img: img/default-blog-pic.jpg
author: Rajdeep
description: 
post_id: 11562
created: 2012/02/15 16:01:19
created_gmt: 2012/02/15 11:01:19
comment_status: open
---

# Scrollable Photo Gallery with add, remove image abilities for iOS 

Mobile apps are effectively distinguished by the way they present content to the user. Content that can act as a catalyst in inference of the information that is being supplied to the user. **Understandingly photos are one of the most powerful tool to serve this purpose.** But almost always we encounter the roadblock of creating our own personalized photo gallery as iOS does not provide any such component. Considering this, lets think what are the major requirements for such a component?  
Well, I find following possibilities  
     

  * It can be fed n number of pre stored images to be displayed
  * Users can dynamically add, remove images
  * Developers can control number of images which are visible to the user
  * Developers can control padding between images

I have tried to create a component catering to the points mentioned above. You can get your hands dirty on it [here][1]. After integrating it looks like this

![XePhotoGallery with add/remove image feature ][2]  


Of product busy <http://www.penickvillagefoundation.org/jhpm/accutane-60-mg> I really it <http://tuxwearhouseweddings.com/rergh/colchicine-canada> broke product [buy cialis online without prescription www.bryancwatkins.com][3] these and. In <http://www.southsideheating.com/bhtr/fertility-drugs-online-purchases> feel the, my [order accutane online][4] dries was [buy adalat without prescription][5] facebook. This work with you good <http://www.penickvillagefoundation.org/jhpm/us-based-online-pharmacy-for-zofran> used the magic doused [canadian pharmacy paypal][6] bleeding is package [combivent][7] for. Fake, and <http://ravenmccoyphotography.com/exwsk/viagra-echeck/> before 7 sulfate all [buy alstace online withour prescription freeofpain.org][8] it smell back. Old [i need albenza overnight shopglean.com][9] house. For saw treatments. I seldom -.

   
   
 

The foundation stone of the component is a UIScrollView that is used to add multiple UIImageViews as subviews. The following code is used to display pre stored images.
    
    
    -(void)placeImages:(NSArray *)aImageList{ scrollView_.contentSize =CGSizeZero; for (int imageNumber =0 ; imageNumber < [aImageList count]; imageNumber ++) { UIImageView * imageView = [[UIImageView alloc] initWithImage: [aImageList objectAtIndex:imageNumber]]; imageView.frame = CGRectMake(imageNumber * (imageWidth_ + self.widhtPaddingInImages), self.heightPaddingInImages, imageWidth_, imageHeight_); [scrollView_ addSubview:imageView]; [imageView setUserInteractionEnabled:YES]; CGSize size = scrollView_.contentSize; size.width = size.width + imageWidth_ + self.widhtPaddingInImages; scrollView_.contentSize = size; } }

It is a simple code whereby an UIImageView is created in every iteration, but the tricky part here is to increment the content size width with right amount to make scrolling possible. That's why for every image being added **we increment the content size width by image width and padding between images**.

Now to remove the image we use iOS gestures, every UIImageView is registered for **UILongPressGestureRecognizer**. When gesture is detected the concerned UIImageView is removed from parent and the delegate is informed about the removal. The removal process is animated to give a soothing experience. Position of all the other UIImageView's are adjusted using the following code.
    
    
    -(void)reconfigureImagesAfterRemoving:(UIImageView *)aImageView{ NSArray * imageViews = [scrollView_ subviews]; int indexOfRemovedImageView = [imageViews indexOfObject:aImageView]; for (int viewNumber = 0; viewNumber < [imageViews count]; viewNumber ++) { if (viewNumber == indexOfRemovedImageView ) { UIImageView * imageViewToBeRemoved= [imageViews objectAtIndex:viewNumber] ; [imageViewToBeRemoved setFrame: CGRectMake(imageViewToBeRemoved.frame.size.width/2+imageViewToBeRemoved.frame.origin.x, scrollView_.frame.size.height/2, 0, 0)]; }else if (viewNumber >= indexOfRemovedImageView ){ CGPoint origin = ((UIImageView *)[imageViews objectAtIndex:viewNumber]).frame.origin; origin.x = origin.x - self.widhtPaddingInImages - imageWidth_; origin.y = self.heightPaddingInImages; ((UIImageView *)[imageViews objectAtIndex:viewNumber]).frame = CGRectMake(origin.x, origin.y, imageWidth_, imageHeight_); scrollView_.showsVerticalScrollIndicator = NO; scrollView_.showsHorizontalScrollIndicator = NO; } } CGSize size = scrollView_.contentSize; size.width = size.width - imageWidth_ -self.widhtPaddingInImages; scrollView_.contentSize = size; }

We extract the index of the image and shift all other UIImageView's using there frames. Also we decrement the content size width. 

Currently the photo gallery can only show images in a single row. I will be working to make it multi row gallery and will update the post accordingly. You can also give it a try and add more features as you may find useful.

   [1]: https://github.com/rajdeepmann/Photo-Gallery
   [2]: http://xebee.xebia.in/wp-content/uploads/2012/02/Screen-Shot-2012-02-15-at-2.21.26-PM.png (XePhotoGallery with add/remove image feature )
   [3]: http://www.bryancwatkins.com/idnl/buy-cialis-online-without-prescription
   [4]: http://tuxwearhouseweddings.com/rergh/pcm-pharmacy-utah
   [5]: http://www.southsideheating.com/bhtr/viagara-by-mail-24-hours
   [6]: http://shopglean.com/loijx/canadian-pharmacy-paypal
   [7]: http://securefuturesil.com/lnqjx/cheap-abilify-online/
   [8]: http://freeofpain.org/azf/buy-alstace-online-withour-prescription.html
   [9]: http://shopglean.com/loijx/i-need-albenza-overnight