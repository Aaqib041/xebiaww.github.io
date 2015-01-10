---
layout: post
header-img: img/default-blog-pic.jpg
author: anirudh.xebia
description: 
post_id: 17434
created: 2013/10/16 15:26:37
created_gmt: 2013/10/16 10:26:37
comment_status: open
---

# Implement single click deployment using Jenkins and DeployIt

<p><strong>Problem statement :</strong> Implement single click deployment using Jenkins and DeployIt
<strong>Tools used:</strong> Jenkins CI Server, DeplyIt Plugin for Jenkins, Github plugin for jenkins, Maven for building artifact.</p>
<p><strong>Detailed use case :</strong>
Lets suppose we have our code in github in a public repository and we want that every fix which goes to "deployment" branch should get propagated to a dev environment automatically.</p>
<p>In order to get this set up working we would need to do:
<strong>1.)</strong> Configure Jenkins with gitHub and deployIt Plugin.
<strong>2.)</strong> Configure application placeholder and environment (including infrastructure) in which deployment would take place in DeployIt.
<!--more-->
<strong>PART 1: CONFIGURE DEPLOYIT</strong></p>
<p>First lets get the deployIt server up and create application and environment in it.</p>
<p>As per deployIt model, an application is a deployable which gets deployed into an environment (container).
So we need application and the environment (comprising of infrastructure and properties files called as dictionaries)</p>
<p>Steps :</p>
<p>1.) Log into deployIt Server with admin privileges.
2.) Go to the Repository Tab -&gt; Right click on Applications andselect new -&gt; Application</p>
<p>Create a new application with name for example TaskMaster (This name would be used in the Jenkins deployIt plugin )</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2013/10/Screen-Shot-2013-10-16-at-4.10.56-PM.png"><img class="aligncenter size-full wp-image-17465" alt="Screen Shot 2013-10-16 at 4.10.56 PM" src="http://xebee.xebia.in/wp-content/uploads/2013/10/Screen-Shot-2013-10-16-at-4.10.56-PM.png" width="500" height="400" /></a></p>
<p>3.) Create Infrastructure : point to a local tomcat instance, and name it "tomcat-local"
Go to Infrastructure -&gt; New -&gt; Overthere - &gt; Localhost</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2013/10/Screen-Shot-2013-10-16-at-4.14.35-PM.png"><img class="aligncenter size-large wp-image-17467" alt="Screen Shot 2013-10-16 at 4.14.35 PM" src="http://xebee.xebia.in/wp-content/uploads/2013/10/Screen-Shot-2013-10-16-at-4.14.35-PM-1024x567.png" width="620" height="343" /></a></p>
<p>4.) Define Tomcat using tomcat plugin and configure its path in infrastructure.</p>
<p>Right click on the created infrastructure -&gt; New -&gt;tc-&gt;tomcat
we need to give value for home (path to Tomcat): /Users/xebia/apache-tomcat-7.0.42</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2013/10/Screen-Shot-2013-10-16-at-4.18.21-PM.png"><img class="aligncenter size-large wp-image-17470" alt="Screen Shot 2013-10-16 at 4.18.21 PM" src="http://xebee.xebia.in/wp-content/uploads/2013/10/Screen-Shot-2013-10-16-at-4.18.21-PM-1024x588.png" width="620" height="356" /></a></p>
<p>5.) Create Environment : Using this Infrastructure make a new environment</p>
<p>Name this Environment as Tomcat-Local and add the Infrastructure just created in it, and also add any dictionary (properties file)</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2013/10/Screen-Shot-2013-10-16-at-4.20.06-PM.png"><img class="aligncenter size-large wp-image-17472" alt="Screen Shot 2013-10-16 at 4.20.06 PM" src="http://xebee.xebia.in/wp-content/uploads/2013/10/Screen-Shot-2013-10-16-at-4.20.06-PM-1024x591.png" width="620" height="357" /></a></p>
<p>Last step would be to check the connection with the server, and make sure everything is configured properly.</p>
<p>After application and environment are successfully configured in deployIT, Lets configure Jenkins.</p>
<p><strong>PART 2: Configure Jenkins and plugins.</strong></p>
<p><strong>1.)</strong> Set up Jenkins CI server : Either install Jenkins as standalone server using the installer <a href="http://jenkins-ci.org">here</a>.
Or just add the Jenkins WAR (download from here ) into your tomcat server webapps folder.
Do the basic configurations required for Jenkins like set up workspace, JDK path, maven, git etc.</p>
<p><strong>2.)</strong> Install github and deployIt plugin - Go to "Manage Jenkins" -&gt; "Manage Plugins" -&gt; click on available plugins tab and search for deployIt plugin, Install Deployit Jenkins plugin.
Then search for gihub plugin and install GitHub plugin, it will install Jenkins Git plugin along with several other supporting plugins like SSH plugin etc.
- we would need to provide deployIt server URL and credentials in this section as well.</p>
<p><img class="alignnone size-medium wp-image-17451" alt="Screen Shot 2013-10-16 at 3.37.32 PM" src="http://xebee.xebia.in/wp-content/uploads/2013/10/Screen-Shot-2013-10-16-at-3.37.32-PM-300x88.png" width="300" height="88" /></p>
<p>3.) After we have got the required plugins and have configured Jenkins; lets make a new job, we will call it "Test-DeployIt-Deployment".
( Go to "New Job" link , select Build a free-style software project radio button )</p>
<p>4.) configuring project set up</p>
<p>On the page to configure new job, put the below settings :</p>
<p>4.1.) Project Name would be prepopulated, add a description if you want
4.2.) GitHub repository configuration
- give the URL of the project under "GitHub project" section.
I gave it for my project on github - &gt; https://github.com/anirudh83/TaskMaster1.1/</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2013/10/Screen-Shot-2013-10-16-at-4.23.35-PM.png"><img class="aligncenter size-large wp-image-17474" alt="Screen Shot 2013-10-16 at 4.23.35 PM" src="http://xebee.xebia.in/wp-content/uploads/2013/10/Screen-Shot-2013-10-16-at-4.23.35-PM-1024x296.png" width="520" height="129" /></a></p>
<p>4.3) Source Code Management</p>
<ul>
<li>Go to section Source Code Management, and select git radio button.
Give the repository URL for HTTPs as we would be doing a read only checkout. (https://github.com/anirudh83/TaskMaster1.1.git, for my case)</li>
</ul>
<p>-Give the branch name to be build ( deployment branch in my case)</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2013/10/Screen-Shot-2013-10-16-at-4.24.43-PM.png"><img class="aligncenter size-full wp-image-17475" alt="Screen Shot 2013-10-16 at 4.24.43 PM" src="http://xebee.xebia.in/wp-content/uploads/2013/10/Screen-Shot-2013-10-16-at-4.24.43-PM.png" width="741" height="392" /></a></p>
<p>4.4) Build Triggers : Click on check box saying : Build when a change is pushed to GitHub</p>
<p>4.5) Select Build Step - to build the checkout source code using maven, and add the maven goal to be executed</p>
<p>-Add clean package as the maven goal</p>
<p>4.6) Add Post-build action - Deploy with DeployIt (Make sure DeployIt plugin is configured in manage Jenkins section and it is running)</p>
<p><img class="alignnone size-medium wp-image-17454" alt="Screen Shot 2013-10-16 at 3.44.55 PM" src="http://xebee.xebia.in/wp-content/uploads/2013/10/Screen-Shot-2013-10-16-at-3.44.55-PM-300x104.png" width="300" height="104" /></p>
<p>4.7) Configure DeployIt to make a package, export it into the "application" in DeployIT and deploy it into the target environment configured in deployIT. See the configuration below :</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2013/10/Screen-Shot-2013-10-16-at-4.28.50-PM1.png"><img class="aligncenter size-full wp-image-17479" alt="Screen Shot 2013-10-16 at 4.28.50 PM" src="http://xebee.xebia.in/wp-content/uploads/2013/10/Screen-Shot-2013-10-16-at-4.28.50-PM1.png" width="982" height="655" /></a></p>
<p>Once everything is configured we can save this job and trigger the build.</p>
<p>Make sure that version gets changed on every build. We can use $BUILD_NUMBER for the same.</p>
<p>We also need to provide Jenkins URL in our github repository in the hook so that it can trigger the build.</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2013/10/Screen-Shot-2013-10-16-at-4.31.03-PM.png"><img class="aligncenter size-large wp-image-17481" alt="Screen Shot 2013-10-16 at 4.31.03 PM" src="http://xebee.xebia.in/wp-content/uploads/2013/10/Screen-Shot-2013-10-16-at-4.31.03-PM-1024x491.png" width="620" height="297" /></a></p>
<p>Hope this post would help you get going in the path of contiuous delivery using Jenkins and DeployIt.</p>
<p>In the next blog post we will see how puppet plugin for deployIt can be used to provision the infrastructure.</p>