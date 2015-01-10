---
layout: post
header-img: img/default-blog-pic.jpg
author: Rajdeep
description: 
post_id: 12590
created: 2012/03/09 16:14:12
created_gmt: 2012/03/09 11:14:12
comment_status: open
---

# E-sign solutions in iOS

<p>Enterprise applications may have a requirement of e-signing a document before sending it to some third party receiver, for instance if a user is disbursing some bills he may have to sign it. Similarly a surveyor may have to sign a survey to prove his authenticity. On the other hand one should keep in mind this whole process is germane to enterprise applications, one should not force such signings on normal lifestyle applications. So if you are in a need of such a solution, then continue reading. <!--more--> </p>
<p>The upmost challenge in creating a <strong>e-sign solution is smoothening the curves of the lines that are drawn by the user</strong>. If you need not to worry about overcoming this challenge and considering that you have the budget, then you can check this <a href="http://tenonedesign.com/t1autograph.php">link</a>. They also have an app on <a href="http://itunes.apple.com/us/app/autograph/id339423436?mt=8">app store</a> which you can try out. 
Having said this I wanted to have something open source, so I tried some solutions/API's to build a e-sign component which can be used to sign, save and send signature to third parties. In any of the solutions the <strong>entry points</strong> are the following touch related methods that are used to detect user interaction with the device.
<pre lang="javascript">
- (void) touchesBegan:(NSSet <em>)touches withEvent:(UIEvent </em>)event ;
- (void) touchesMoved:(NSSet <em>)touches withEvent:(UIEvent </em>)event ;
- (void)touchesEnded:(NSSet <em>)touches withEvent:(UIEvent </em>)event;
</pre>
 Here are few of them. </p>
<p><strong>1. Core graphics</strong>
The hurdle of smoothening the curves can be overcome by using <a href="http://en.wikipedia.org/wiki/B%C3%A9zier_curve">Beizer curves</a>. We can use cubic curves to achieve our goal. For that we need to supply four points (say p1,p2,p3,p4) and two control points (c1,c2t). The points p1 and p4 act as previous and the next point respectively. We try to move from point p2 to p3 and draw the path between them. The control points (c1,c2) can be driven from the four points. Once we get the control points we need to use the below code to move to the point and draw Beizer curve.
 <pre lang="javascript">
CG_EXTERN void CGContextMoveToPoint(CGContextRef c, CGFloat x, CGFloat y);
CG_EXTERN void CGContextAddCurveToPoint(CGContextRef c, CGFloat cp1x,CGFloat cp1y,
                          CGFloat cp2x, CGFloat cp2y, CGFloat x, CGFloat y);
</pre>
Here <strong>CGContext is being used as we do not want to redraw the whole screen every time when the user touches the screen</strong>, instead we save the previous paths traversed in a context and only draw what is needed. For more info you can check this detailed <a href="http://blog.effectiveui.com/?p=8105">blog</a>.</p>
<p><strong>2. UIBezierPath</strong>
<a href="https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UIBezierPath_class/Reference/Reference.html">UIBezierPath</a> is a wrapper over core graphics that facilitates in drawing different shapes, lines. The entire concept of drawing remains the same as described above but it provides some additional and useful features. Using UIBezierPath you need not to handle context, you can simply initiate a UIBezierPath instance and start adding to this path as the user moves the touches. Here are the  methods for the same. 
 <pre lang="javascript">
- (void)moveToPoint:(CGPoint)point;
- (void)addLineToPoint:(CGPoint)point;
- (void)addCurveToPoint:(CGPoint)endPoint controlPoint1:(CGPoint)controlPoint1 controlPoint2:
(CGPoint)controlPoint2;
</pre>
Some additional feature that can be had using this method are:
<ul>
<li>Pattern lines (although may not be needed for e-sign use case)</li>
<li>Add a clipping region</li>
<li><strong>Undo/Redo operations</strong></li>
</ul></p>
<p><strong>3. Open GL</strong>
The major drawback with the above problems is that there is a <strong>latency in drawing lines on screen</strong> and the user experience can be quite bad if the line smoothening algo's are not properly used. Also the lines are not as smooth as compared to the method being discussed ahead. In this method a brush texture is created from an image by first drawing the image into a Core Graphics bitmap context. It then uses the bitmap data for the texture drawing. You can see it in action in <a href="https://developer.apple.com/library/ios/#samplecode/GLPaint/Introduction/Intro.html">this</a> Apple example. The drawing is uber smooth as it is being drawn directly in a EAGLContext. I have used this method by using a custom image for the brush texture which gives an feel of a pen rather than a brush.</p>
<p>I hope this blog will give an idea about what can be used for e-sign component. I will soon upload the component that I have created. </p>