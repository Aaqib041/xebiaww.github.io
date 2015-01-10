---
layout: post
header-img: img/default-blog-pic.jpg
author: hgupta
description: 
post_id: 867
created: 2009/03/26 14:21:18
created_gmt: 2009/03/26 13:21:18
comment_status: open
---

# GWT: An introduction

<p><strong>Introduction</strong>
<p align="justify">Google Web Toolkit is a open source java framework to build rich user interface with widgets and panels. It communicates with one technology rather with different technologies, and it works on linux, windows and mac. In GWT code is converted from Java to Java script. GWT can be compared with latest technologies as Flex, Microsoft Silverlight and Java FX. All of these technologies provide rich widget based development but GWT stands apart due to its java script code conversion.</p>
GWT has some interesting features which brings thoughts why GWT is good. Some of these features are integration with browser back button, internationalization, code development in eclipse, fells working in desktop based application and UI code written in java.
<!--more-->
Lets compare Flex with GWT and see where GWT is going. In Flex we need to learn new Action script where as in GWT its all java. For integration with backend services you need webservices in Flex while in GWT you can write your own services in java. This gives edge to GWT over Flex.</p>
<p><strong>Installation</strong>
<p align="justify">GWT can be downloaded from <span style="text-decoration: underline;"><a href="http://code.google.com/webtoolkit/">http://code.google.com/webtoolkit/</a></span>.</p>
<p align="justify">Installation is simple, unzip the downloaded file with some tool such as WinZip, and add the root folder into path.</p>
<p align="justify">On windows vista -</p>
<p align="justify">Control Pannel-&gt; System and Maintainence-&gt; System-&gt;Advance System Settings</p>
<p align="justify">Click <em>Environment Variable</em> button</p>
<p align="justify">From the user variable list find <em>path</em> and click <em>edit</em></p>
<p align="justify">add semicolon at the end and add your installed directory(eg ;E:\gwt-windows-1.5.3)</p>
<strong>Dummy application</strong></p>
<p>Lets create a project and go through the project created in GWT.
<ul>
    <li> First create a project with the tool applicationCreator provided in the installation directory. Use -eclipse option so that project can be imported in eclipse, -out tells the name of project in eclipse and then the client class full name.</li>
</ul>
applicationCreator -eclipse GwtDemo -out School in.xebia.demo.client.GwtDemo</p>
<p>Import the project created in eclipse.</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2009/01/import-in-eclipse.jpg"><img style="VERTICAL-ALIGN: middle" src="http://xebee.xebia.in/wp-content/uploads/2009/01/import-in-eclipse.jpg" alt="" width="240" height="240" /></a>
<ul>
    <li> Add the following jar files in the eclipse project.</li>
</ul>
- gwt-user.jar file is in the installation directory of GWT.</p>
<ul>
<li>junit.jar from the junit installation directory
<ul>
    <li> Once the project is imported its time to run the project. There are two modes to run the application, host mode and hosted mode web browser. Hosted mode runs in JVM and inbuilt browser while host mode web browser runs in browser.</li>
</ul>
<p align="justify">Applications that run in hosted mode don't translate into the javascript, by this you can debug your application just like another application.</p>
<p align="justify">To run the project just click on shell command script.</p>
<p style="MARGIN-BOTTOM: 0in"></p></li>
</ul>
<div style="MARGIN-BOTTOM: 0in; MARGIN-LEFT: 0.5in; TEXT-INDENT: -0.25in"><a href="http://xebee.xebia.in/wp-content/uploads/2009/01/eclipse-view.jpg"><img src="http://xebee.xebia.in/wp-content/uploads/2009/01/eclipse-view.jpg" alt="" width="240" height="240" /></a></div>

<p align="justify"><strong>Running the demo project</strong></p>

<p align="justify">You see two new windows opened. First is the log viewer where you see status and error messages. Second is hosted mode web browser, where the application is running.</p>

<p><a href="http://xebee.xebia.in/wp-content/uploads/2009/01/result.jpg"><img src="http://xebee.xebia.in/wp-content/uploads/2009/01/result.jpg" alt="" width="240" height="240" /></a>
<ul>
    <li> There are two files one is html and other is java file that are created by default application ( you just created above). Java class is implementing interface EntryPoint and one method onModuleLoad. In this method write the actual java code which tells whats the behavior of application is when its loaded. This place you create your UI.</li>
</ul>
Lets take a tour of existing demo application UI created. It has created button "Click me"
<pre lang="java">
Button button = new Button("Click me");
</pre>
An event listener is added to the button to make the feature more advance.
<pre lang="java">
 button.addClickListener(new ClickListener() {
      public void onClick(Widget sender) {
            dialogBox.center();
            dialogBox.show();
      }
 });
</pre>
<strong>Deploying project in web server</strong>
<ul>
    <li>
<p align="justify">You can generate the server side code, run the application in shell mode you'll see the compile button, this will create the javascript file in www. directory.</p>
</li>
</ul>
<a href="http://xebee.xebia.in/wp-content/uploads/2009/01/www-directory.jpg"><img src="http://xebee.xebia.in/wp-content/uploads/2009/01/www-directory.jpg" alt="" width="240" height="240" /></a>
<p align="justify">The complier names the application directory with full module name. These are static files generated from compiler. These file can be moved to any webserver and deployed on it. The generated code can run in any web browser by loading the generated files.</p>
<strong>Conclusion</strong>
<p align="justify">In this blog I talk about the dummy application created by GWT. I also talk about what code needs to be deployed on web server and how that code is generated in gwt.</p></p>