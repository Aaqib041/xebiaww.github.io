---
layout: post
header-img: img/default-blog-pic.jpg
author: saurabhgupta
description: 
post_id: 19123
created: 2014/11/11 10:28:54
created_gmt: 2014/11/11 05:28:54
comment_status: open
---

# ROBOT Framework & RIDE

<h2 style="text-align: left"><strong>What is Robot Framework?</strong></h2>

<p>•It’s a general test automation framework(open source from Nokia and hosted on Google)</p>
<p>•Focus of automation is generally on acceptance tests.</p>
<p>•Uses a Keyword driven automation approach</p>
<p>•Testing capabilities can be extended using Python or Java</p>
<p>•Can create tests in natural language for ease of understanding.
<h2 style="text-align: left"><strong>Keyword Driven approach</strong></h2>
•Higher-level keywords: Those are really testing a concrete aspect of the business logic of the system under test.</p>
<p>•Lower-level keywords: To keep the implementation of the higher-level keywords at a decent size one is often breaking down the required functionality to several lower-level keywords.</p>
<p>[caption id="attachment_19124" align="aligncenter" width="453"]<a href="http://xebee.xebia.in/wp-content/uploads/2014/11/RF_Keyword_Approach.png"><img class=" wp-image-19124 " alt="Keyword Approach" src="http://xebee.xebia.in/wp-content/uploads/2014/11/RF_Keyword_Approach.png" width="453" height="326" /></a> RF Keyword Approach[/caption]
<h2 style="text-align: left"></h2>
<h2 style="text-align: left"><strong>Architecture</strong></h2>
<p style="text-align: left"><a href="http://xebee.xebia.in/wp-content/uploads/2014/11/RF_Architecture.png"><img class="aligncenter size-full wp-image-19125" alt="RF_Architecture" src="http://xebee.xebia.in/wp-content/uploads/2014/11/RF_Architecture.png" width="548" height="413" /></a></p></p>
<h2 style="text-align: left"><strong>Installation- Steps</strong></h2>

<p><strong>Installation step of Python</strong>
<div>1.Install Python 2.x using the windows MSI installer:<span style="text-decoration: underline"><a href="http://www.python.org/download/">http://www.python.org/download/</a></span> (because Robot Framework only support python 2.x version. Robot Framework is currently not compatible with Python 3.x versions)</div>
<div>2.After installation check in the Environment settings for “path”. If python script path not exists then append the following path</div>
<div>
<ul>
    <li><span style="line-height: 1.5em">Open Start &gt; Settings &gt; Control Panel &gt; System &gt; Advanced &gt; Environment Variables. There are User variables and System variables and the difference between them is that User variables affect only the current users, whereas System variables affect all users.</span></li>
    <li><span style="line-height: 1.5em">To edit existing PATH, select Edit and add ;&lt;InterpreterInstallationDir&gt;;&lt;InterpreterInstallationDir&gt;\Scripts\ at the end of the value. Note that the leading colon (;) is important, as it separates different entries. To add a new value, select New and provide both the name and the value, this time without the leading colon.  </span></li>
    <li><span style="line-height: 1.5em">Start a new command prompt for the changes to effect. </span></li>
</ul>
<strong>Installation step of Robot Framework</strong>
<div>1.Using PIP:</div>
<div>
<ul>
    <li><span style="line-height: 1.5em">Install PIP: download the “get-pip.py” file from</span><span style="text-decoration: underline"><a href="https://bootstrap.pypa.io/get-pip.py">https://bootstrap.pypa.io/get-pip.py</a></span></li>
    <li><span style="line-height: 1.5em">Open command prompt go to the directory where the #a file downloaded. E.g. cd c:\main\downloads</span></li>
    <li><span style="line-height: 1.5em">After that hit the command: python get-pip.py</span></li>
    <li><span style="line-height: 1.5em">After pip installation hit the following command in cmd to install robot framework:</span></li>
    <li><span style="line-height: 1.5em">“<em>pip install robotframework</em>”</span></li>
</ul>
</div>
<div>2 .Another way is to get the Robot Framework source code by downloading the source distribution from following location:<span style="text-decoration: underline"><a href="https://pypi.python.org/pypi/robotframework">https://pypi.python.org/pypi/robotframework</a></span>   or<span style="text-decoration: underline"><a href="https://github.com/robotframework/robotframework">https://github.com/robotframework/robotframework</a></span></div>
<div>
<ul>
    <li><span style="line-height: 1.5em">First download the source code </span></li>
    <li><span style="line-height: 1.5em">Open command prompt go to the directory where the #a file downloaded. E.g. cd c:\main\downloads </span></li>
    <li><span style="line-height: 1.5em">After that hit the command: python setup.py install</span></li>
</ul>
<strong>Installation step of RIDE:</strong></p>
<p><strong></strong>To install RIDE below things should installed before”
<div>1.Python</div>
<div>2.Wxpython</div>
<div>3.Robot Framework</div>
Steps for Python and Robot Framework are already present in the document and for Wxpython follow the below steps:</p>
<p><strong>Installation of wxpython</strong>
<div>
<ol>
    <li><span style="line-height: 1.5em">For windows: </span><span style="text-decoration: underline"><a href="http://sourceforge.net/projects/wxpython/files/wxPython/2.8.12.1/">http</a></span><span style="text-decoration: underline"><a href="http://sourceforge.net/projects/wxpython/files/wxPython/2.8.12.1/">://sourceforge.net/projects/wxpython/files/wxPython/2.8.12.1/</a></span></li>
</ol>
</div>
<strong>Installation Instruction for RIDE:</strong>
<div>
<ol>
    <li><span style="line-height: 1.5em">On windows download the executable site from </span><span style="text-decoration: underline"><a href="https://code.google.com/p/robotframework-ride/downloads/list">https</a></span><span style="text-decoration: underline"><a href="https://code.google.com/p/robotframework-ride/downloads/list">://code.google.com/p/robotframework-ride/downloads/list</a></span><span style="line-height: 1.5em">   and install it</span></li>
    <li><span style="line-height: 1.5em">Alternative way to install using the pip command : “pip install robotframework-ride” </span></li>
    <li><span style="line-height: 1.5em">Another way is to download the source code from :</span><span style="text-decoration: underline"><a href="https://pypi.python.org/pypi/robotframework-ride">https://pypi.python.org/pypi/robotframework-ride</a></span><span style="line-height: 1.5em">   and use the command : python setup.py install</span></li>
</ol>
</div>
To open the Ride just open the cmd and hit the command “ride.py” or just call the same command on run(win+r).
<p style="text-align: left"><span style="font-size: 16px"><strong>Introduction To RIDE</strong></span></p>
<span style="line-height: 1.5em">The Robot Framework IDE (RIDE) is the integrated development environment to implement automated tests for the Robot Framework.</span>
<em><span style="line-height: 1.5em">How to open RIDE</span></em>
<ul>
    <li><span style="line-height: 1.5em">Type ride.py in start menu of system and press enter</span></li>
    <li><span style="line-height: 1.5em">Double click on RIDE shortcut(if created while installation)</span></li>
</ul>
<h2 style="text-align: left"><strong>Test Libraries</strong></h2>
Test libraries contain those lowest-level keywords, often called library keywords, which actually interact with the system under test. All test cases always use keywords from some library, often through higher-level user keywords.</p>
<p>Different Types of Libraries available
<div>
<ol>
    <li><span style="line-height: 1.5em">Standard Libraries</span></li>
    <li><span style="line-height: 1.5em">External Libraries</span></li>
</ol>
<span style="font-size: 16px"><em><strong>Standard Libraries</strong></em></span></p>
<p>Some test libraries are distributed with Robot Framework and these libraries are called standard libraries. These are the available standard libraries:
<div>
<ul>
    <li><span style="text-decoration: underline"><a href="http://robotframework.googlecode.com/hg/doc/userguide/RobotFrameworkUserGuide.html?r=2.7.6#builtin-library">BuiltIn</a></span></li>
    <li><span style="text-decoration: underline"><a href="http://robotframework.googlecode.com/hg/doc/userguide/RobotFrameworkUserGuide.html?r=2.7.6#operatingsystem-library">OperatingSystem</a></span></li></p>