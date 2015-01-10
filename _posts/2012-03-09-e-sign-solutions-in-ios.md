---
layout: post
header-img: img/default-blog-pic.jpg
---

# E-sign solutions in iOS

Enterprise applications may have a requirement of e-signing a document before sending it to some third party receiver, for instance if a user is disbursing some bills he may have to sign it. Similarly a surveyor may have to sign a survey to prove his authenticity. On the other hand one should keep in mind this whole process is germane to enterprise applications, one should not force such signings on normal lifestyle applications. So if you are in a need of such a solution, then continue reading.  The upmost challenge in creating a **e-sign solution is smoothening the curves of the lines that are drawn by the user**. If you need not to worry about overcoming this challenge and considering that you have the budget, then you can check this [link](http://tenonedesign.com/t1autograph.php). They also have an app on [app store](http://itunes.apple.com/us/app/autograph/id339423436?mt=8) which you can try out. Having said this I wanted to have something open source, so I tried some solutions/API's to build a e-sign component which can be used to sign, save and send signature to third parties. In any of the solutions the **entry points** are the following touch related methods that are used to detect user interaction with the device. 
    
    
    - (void) touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event ;
    - (void) touchesMoved:(NSSet *)touches withEvent:(UIEvent *)event ;
    - (void)touchesEnded:(NSSet *)touches withEvent:(UIEvent *)event;
    

Here are few of them. **1\. Core graphics** The hurdle of smoothening the curves can be overcome by using [Beizer curves](http://en.wikipedia.org/wiki/B%C3%A9zier_curve). We can use cubic curves to achieve our goal. For that we need to supply four points (say p1,p2,p3,p4) and two control points (c1,c2t). The points p1 and p4 act as previous and the next point respectively. We try to move from point p2 to p3 and draw the path between them. The control points (c1,c2) can be driven from the four points. Once we get the control points we need to use the below code to move to the point and draw Beizer curve. 
    
    
    CG_EXTERN void CGContextMoveToPoint(CGContextRef c, CGFloat x, CGFloat y);
    CG_EXTERN void CGContextAddCurveToPoint(CGContextRef c, CGFloat cp1x,CGFloat cp1y,
                              CGFloat cp2x, CGFloat cp2y, CGFloat x, CGFloat y);
    

Here **CGContext is being used as we do not want to redraw the whole screen every time when the user touches the screen**, instead we save the previous paths traversed in a context and only draw what is needed. For more info you can check this detailed [blog](http://blog.effectiveui.com/?p=8105). **2\. UIBezierPath** [UIBezierPath](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UIBezierPath_class/Reference/Reference.html) is a wrapper over core graphics that facilitates in drawing different shapes, lines. The entire concept of drawing remains the same as described above but it provides some additional and useful features. Using UIBezierPath you need not to handle context, you can simply initiate a UIBezierPath instance and start adding to this path as the user moves the touches. Here are the methods for the same. 
    
    
    - (void)moveToPoint:(CGPoint)point;
    - (void)addLineToPoint:(CGPoint)point;
    - (void)addCurveToPoint:(CGPoint)endPoint controlPoint1:(CGPoint)controlPoint1 controlPoint2:
    (CGPoint)controlPoint2;
    

Some additional feature that can be had using this method are: 

  * Pattern lines (although may not be needed for e-sign use case)
  * Add a clipping region
  * **Undo/Redo operations**
**3\. Open GL** The major drawback with the above problems is that there is a **latency in drawing lines on screen** and the user experience can be quite bad if the line smoothening algo's are not properly used. Also the lines are not as smooth as compared to the method being discussed ahead. In this method a brush texture is created from an image by first drawing the image into a Core Graphics bitmap context. It then uses the bitmap data for the texture drawing. You can see it in action in [this](https://developer.apple.com/library/ios/#samplecode/GLPaint/Introduction/Intro.html) Apple example. The drawing is uber smooth as it is being drawn directly in a EAGLContext. I have used this method by using a custom image for the brush texture which gives an feel of a pen rather than a brush. I hope this blog will give an idea about what can be used for e-sign component. I will soon upload the component that I have created.