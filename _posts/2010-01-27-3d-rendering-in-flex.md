---
layout: post
header-img: img/default-blog-pic.jpg
author: Amit Sharma
description: 
post_id: 2975
created: 2010/01/27 12:42:49
created_gmt: 2010/01/27 07:42:49
comment_status: open
---

# 3D Rendering in Flex

<p>I clearly remember the first time I saw <a href="http://www.effectiveui.com">www.effectiveui.com</a> or the <a href="http://www.footlocker.eu/evolve/#/en/">http://www.footlocker.eu/evolve/#/en/</a> . I was totally blown away, and kept on thinking how it was done, how could they have rendered such a beautiful 3D scene with such artistic flourish on a browser. Then, after reading various articles, I came across the technology behind rendering 3D scenes. Since then, I have played around with 3D rendering engines which I will discuss in this write-up.</p>
<p>There are already some open-source 3D Rendering engines available for Flex. Papervision3D (PV3D) and Away3D are the most popular amongst them. Papervision3D is an open-source, MIT licensed 3D engine written in ActionScript 3.0. This article will guide you how to set up your first Papervision3D application, Papervision3D 2.0 Alpha, otherwise known as “Great White.”</p>
<p>In this article, I will discuss
<ol>
    <li>Some PV3D keywords.</li>
    <li>Flex 2D vs Flex 3D coordinate system.</li>
    <li>Set up a PV3D project.</li>
    <li>Look at the code sample and understand the intricacies in detail.</li>
</ol>
<!--more--></p>
<p>Before diving deep into Papervision3D, let’s discuss some of the keywords that we use in PV3D.
<ol>
    <li>ViewPort - Best definition for a viewport that I have found on web: The viewport is the sprite that displays the rendered scene.  When you render a 3D scene, your camera takes a snapshot of the scene.  The viewport is the picture that is produced by the camera.  You can move and position the viewport like a normal sprite.</li>
    <li>3DCamera - While, the camera is what you see inside of the 3D scene, the viewport is the Sprite that Flash uses to display what the camera sees.</li>
    <li>3DObject - There are a few different 3D objects in PV3D.  Most of the basic ones are Sphere, Cube, Cylinder, Pyramid, Cone, Plane etc.</li>
    <li>Scene - When you create a 3D object in PV3D that you want to render, you must add it to a  Scene. So in a way all 3D objects reside inside a scene.</li>
    <li>BasicRenderEngine – When you have a Scene containing multiple 3D objects ready, a 3DCamera to look at the objects and a viewport of the scene, you need a rendering engine which actually creates the viewport for you to display. You can call this rendering engine to render your scene for each frame or at specific time, which is discussed later in the article.</li>
    <li>Materials - There are a few different materials or textures that you can use on your 3D objects in PV3D. Some examples are BitmapMaterial, WireframeMaterial, BitmapColorMaterial etc. Each material provides your 3D object a different texture.</li>
</ol>
<strong>Flex 2D Coordinate system Vs. 3D Coordinate system</strong></p>
<p>In a 2D environment, objects are positioned in the top left corner which corresponds to X=0 and Y=0. As you increase X, the object moves to the right and if you increase Y, the object moves down. In PV3D, the centre of the Scene3D corresponds to X=0, Y=0, Z=0. Since the Camera3D defaults to X=0, Y=0, Z=-1000 and points at the origin (X:0, Y:0, Z:0), increasing X moves the object right, increasing Y moves the object up, and increasing Z moves the object toward the horizon.</p>
<p><img class="alignnone size-full wp-image-2979" title="AXIS" src="http://xebee.xebia.in/wp-content/uploads/2010/01/AXIS.jpg" alt="AXIS" width="448" height="368" /></p>
<p><strong>Setup Papervision 3D Flex project</strong>
<ol>
    <li>Download the latest SWC library or source code from <a href="http://code.google.com/p/papervision3d/downloads/list">http://code.google.com/p/papervision3d/downloads/list</a></li>
    <li>Setup an Action Script project in FlexBuilder and add Papervision source or SWC library to your project.</li>
</ol>
Now without further delay let’s look at the code sample:</p>
<p>[sourcecode language="actionscript3"]</p>
<p>package
{
 import flash.display.Sprite;
 import flash.events.Event;
 import org.papervision3d.cameras.Camera3D;
 import org.papervision3d.materials.BitmapColorMaterial;
 import org.papervision3d.objects.primitives.Sphere;
 import org.papervision3d.render.BasicRenderEngine;
 import org.papervision3d.scenes.Scene3D;
 import org.papervision3d.view.Viewport3D;</p>
<p>[SWF ( width = '800', height = '600', backgroundColor = '#0000ff', frameRate = '64' ) ]
 public class PapervisionSample extends Sprite
 {
   private var viewport:Viewport3D;
   private var scene:Scene3D;
   private var camera:Camera3D;
   private var renderer:BasicRenderEngine;
   private var sphere:Sphere;</p>
<p>public function PapervisionSample()
   {
     initPapervision3D();
     createSphere();
     beginRender();
   }</p>
<p>private function initPapervision3D():void
   {
     viewport = new Viewport3D(600, 480, true, true);
     addChild(viewport);
     scene = new Scene3D();
     camera = new Camera3D();
     renderer = new BasicRenderEngine();
   }</p>
<p>private function createSphere():void
   {
     // creating the texture or material for our object
     var material:BitmapColorMaterial = new BitmapColorMaterial();
     // creating the 3D object which is Sphere in our case
     sphere = new Sphere(material, 500);</p>
<pre><code> scene.addChild(sphere);
</code></pre>
<p>}</p>
<p>private function beginRender():void
   {
     //calls the render function every frame
     addEventListener(Event.ENTER_FRAME, render);
   }</p>
<p>private function render(e:Event):void
   {
     /<em> Rotate the display object around the longitudinal axis </em>/
     sphere.roll(10);</p>
<pre><code> /* Takes the data from the scene, camera, and viewport,
 renders it, then updates the viewport */
 renderer.renderScene(scene, camera, viewport);
</code></pre>
<p>}
 }
}
[/sourcecode]</p>
<p>Now let’s try to understand the code.</p>
<p>[sourcecode language="actionscript3"]
private function initPapervision3D():void
{
  viewport = new Viewport3D(600, 480, true, true);
  addChild(viewport);
  scene = new Scene3D();
  camera = new Camera3D();
  renderer = new BasicRenderEngine();
}
[/sourcecode]</p>
<p>Let’s first analyse what’s happening at the initialization method. Basically we are creating Viewport, Scene3D, Camera3D and BasicRenderingEngine. Notice that after creating Viewport with the specific width and height we add it as a child to our Sprite. Thus, the Viewport will act as our window to the 3D scene and we can position the Viewport accordingly.
Also notice that after this step our scene is created but it is like an empty space and we will have to add the objects in it.</p>
<p>[sourcecode language="actionscript3"]
private function createSphere():void
{
  // creating the texture or material for our object
  var material:BitmapColorMaterial = new BitmapColorMaterial();
  // creating the 3D object which is Sphere in our case
  sphere = new Sphere(material, 500);</p>
<p>scene.addChild(sphere);
}
[/sourcecode]</p>
<p>In the above code, we create a 3D object (Sphere in our case) and add it to the scene. One thing to note here is there are several primitive objects that are available in the PV3D API. For example Cube, Cylinder, Cone, Plane etc. I have chosen to create a Sphere with radius 500 and with a BitmapColorMaterial(with its default color). This material will add a color to the sphere. Now the scene is set and viewport and Camera are ready to roll but still we won't see anything, because we haven't started rendering the scene.</p>
<p>[sourcecode language="actionscript3"]
private function beginRender():void
{
  //calls the render function every frame
  addEventListener(Event.ENTER_FRAME, render);
}
[/sourcecode]</p>
<p>Now let's render the scene. The call to beginRender() which adds the event listener to listen to each occassion when a frame from the scene is rendered. Thus for each frame we call the render() method.</p>
<p>[sourcecode language="actionscript3"]
private function render(e:Event):void
{
  /<em> Rotate the display object around the longitudinal axis </em>/
  sphere.roll(10);</p>
<p>/<em> Takes the data from the scene, camera, and viewport,
  renders it, then updates the viewport </em>/
  renderer.renderScene(scene, camera, viewport);
}
[/sourcecode]</p>