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

## **What is Robot Framework?**

•It’s a general test automation framework(open source from Nokia and hosted on Google)

•Focus of automation is generally on acceptance tests.

•Uses a Keyword driven automation approach

•Testing capabilities can be extended using Python or Java

•Can create tests in natural language for ease of understanding. 

## **Keyword Driven approach**

•Higher-level keywords: Those are really testing a concrete aspect of the business logic of the system under test.

•Lower-level keywords: To keep the implementation of the higher-level keywords at a decent size one is often breaking down the required functionality to several lower-level keywords.

[caption id="attachment_19124" align="aligncenter" width="453"]![Keyword Approach][1] RF Keyword Approach[/caption] 

## 

## **Architecture**

![RF_Architecture][2]

## **Installation- Steps**

**Installation step of Python**

1.Install Python 2.x using the windows MSI installer:<http://www.python.org/download/> (because Robot Framework only support python 2.x version. Robot Framework is currently not compatible with Python 3.x versions)

2.After installation check in the Environment settings for “path”. If python script path not exists then append the following path

  * Open Start > Settings > Control Panel > System > Advanced > Environment Variables. There are User variables and System variables and the difference between them is that User variables affect only the current users, whereas System variables affect all users.
  * To edit existing PATH, select Edit and add ;<InterpreterInstallationDir>;<InterpreterInstallationDir>\Scripts\ at the end of the value. Note that the leading colon (;) is important, as it separates different entries. To add a new value, select New and provide both the name and the value, this time without the leading colon.  
  * Start a new command prompt for the changes to effect. 
**Installation step of Robot Framework**

1.Using PIP:

  * Install PIP: download the “get-pip.py” file from<https://bootstrap.pypa.io/get-pip.py>
  * Open command prompt go to the directory where the #a file downloaded. E.g. cd c:\main\downloads
  * After that hit the command: python get-pip.py
  * After pip installation hit the following command in cmd to install robot framework:
  * “_pip install robotframework_”

2 .Another way is to get the Robot Framework source code by downloading the source distribution from following location:<https://pypi.python.org/pypi/robotframework>   or<https://github.com/robotframework/robotframework>

  * First download the source code 
  * Open command prompt go to the directory where the #a file downloaded. E.g. cd c:\main\downloads 
  * After that hit the command: python setup.py install
**Installation step of RIDE:**

****To install RIDE below things should installed before” 

1.Python

2.Wxpython

3.Robot Framework

Steps for Python and Robot Framework are already present in the document and for Wxpython follow the below steps:

**Installation of wxpython**

  1. For windows: [http][3][://sourceforge.net/projects/wxpython/files/wxPython/2.8.12.1/][3]

**Installation Instruction for RIDE:**

  1. On windows download the executable site from [https][4][://code.google.com/p/robotframework-ride/downloads/list][4]   and install it
  2. Alternative way to install using the pip command : “pip install robotframework-ride” 
  3. Another way is to download the source code from :<https://pypi.python.org/pypi/robotframework-ride>   and use the command : python setup.py install

To open the Ride just open the cmd and hit the command “ride.py” or just call the same command on run(win+r). 

**Introduction To RIDE**

The Robot Framework IDE (RIDE) is the integrated development environment to implement automated tests for the Robot Framework. _How to open RIDE_

  * Type ride.py in start menu of system and press enter
  * Double click on RIDE shortcut(if created while installation)

## **Test Libraries**

Test libraries contain those lowest-level keywords, often called library keywords, which actually interact with the system under test. All test cases always use keywords from some library, often through higher-level user keywords.

Different Types of Libraries available 

  1. Standard Libraries
  2. External Libraries
_**Standard Libraries**_

Some test libraries are distributed with Robot Framework and these libraries are called standard libraries. These are the available standard libraries: 

  * [BuiltIn][5]
  * [OperatingSystem][6]

   [1]: http://xebee.xebia.in/wp-content/uploads/2014/11/RF_Keyword_Approach.png
   [2]: http://xebee.xebia.in/wp-content/uploads/2014/11/RF_Architecture.png
   [3]: http://sourceforge.net/projects/wxpython/files/wxPython/2.8.12.1/
   [4]: https://code.google.com/p/robotframework-ride/downloads/list
   [5]: http://robotframework.googlecode.com/hg/doc/userguide/RobotFrameworkUserGuide.html?r=2.7.6#builtin-library
   [6]: http://robotframework.googlecode.com/hg/doc/userguide/RobotFrameworkUserGuide.html?r=2.7.6#operatingsystem-library