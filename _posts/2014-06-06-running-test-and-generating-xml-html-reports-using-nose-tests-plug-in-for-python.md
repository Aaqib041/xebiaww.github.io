---
layout: post
header-img: img/default-blog-pic.jpg
author: Msingh
description: 
post_id: 18370
created: 2014/06/06 16:56:43
created_gmt: 2014/06/06 11:56:43
comment_status: open
---

# Running Test and Generating XML & HTML Reports using Nose Tests plug-in for Python

Nose Tests is a plug-in for Python that basically provides capabilities to run Python Tests. Nose basically extends the feature of writing,finding and running tests. nosetests will run test from file and directories under the current working directories whose name includes "test" or "Test" at a word boundary like test_this. Test output is similar to the  of unittest but also includes captured stdout output from failing tests, for easy print style-debugging. For example if we are using IDE like Eclipse we can specify the location of the directory of file like test_example_demo.py or if you want to run whole directory then you need to give directory name like example_test_set.

=====================================================================================

**Installation:-**

=======================================================================================

Below are the steps to install nose in your machine:-

**Pre-Requisites:-** 1\. Pyhton should be install in your machine.If Python is install in your machine you will see python directory in your primary disk like C:\Python27 or C:\Python34 . Directory name depend upon the Python version. If Python is not install then go to <https://www.python.org/download/> and download the appropriate version for your machine and install the same as per given instruction on Python website.

**Install Nose test:-**

**1\. Using easy_install**

a). Open command prompt or Power shell in elevator mode.

b). Traverse to the install directory. It will be under PyhtonDirecytory/Scripts like C:\Python27\Scripts.

c). Look for easy_install using dir command.

d). Run easy_install nose (enter) and you will see nose will get install in your machine.

e). After installation you will see a new directory under C:\Python27\Lib\site-packages\nose-1.3.3-py2.7.egg.

![1][1] ![2][2] ![3][3] ![4][4] ![5][5]

**2\. Using PIP**

You can install the same package using pip as well like below:-

pip install nose

**3\. Download nose Package**

if you don't have setup tool like easy_install and pip install in your machine then you can directly download the package from  https://pypi.python.org/pypi/nose/1.3.3 and extract the download directory. Then in-order to install the same please follow below steps:-

a). Open command prompt or Power shell in elevator mode.

b). navigate to the extracted directory

c).Run the **pyhton setup.py install** command to install the nosetest.

**Test Runner:-**

Now is the time how to execute the Python test using Nosetests. Here i'm going to show the example of Eclipse IDE and command prompt:-

**1\. Eclipse IDE**

a). To execute the Python test you need to write small python script in order to invoke the nosetests to execute the Python test like below:-

**Python_nosetest.py**

import os,getopt import sys,time from lib.xml_parser import XMLParser from genericpath import isfile, isdir from dircache import listdir

def main(argv): test_location = '' attrb = ''

try: opts, args = getopt.getopt(argv,'hh:t:a:', ['tests=']) except getopt.GetoptError: sys.exit(2)

for opt,arg in opts: if(opt == '-h'): print 'Usage: test_nosetests -t <test location or test file name>' sys.exit(2) elif opt in ('-t','--tests'): print 'opt:',opt,'arg:',arg test_location = arg elif opt in ('-a','-A'): print 'opt:',opt,'arg:',arg attrb = opt+' '+arg

print 'Test File location:',test_location

report_directory = os.getenv('TESTROOTPATH')+'/test/reports/reports_'+time.strftime('%Y%m%d_%H%M%S') xml_filename=report_directory+'/xmls_'+time.strftime('%Y%m%d_%H%M%S')+'.xml'

# xml_filename=report_directory+'/xmls_'+time.strftime('%Y%m%d_%H%M%S')+'.html'

os.mkdir(report_directory) print listdir(report_directory)

tests = os.getenv('TESTROOTPATH')+'/test/tests/'+test_location print 'Verifying test location %s exists'%tests

if(isfile(tests)): print 'Test Script file exists' elif(isdir(tests)): print 'Test location Exists' print listdir(tests) else: print 'Test location not exists!',tests,'please check the path' sys.exit(2)

# \--with-coverage --cover-html

command = 'nosetests '+attrb+' --with-xunit --xunit-file='+xml_filename+' '+tests

print 'Running Nose Tests command:',command

os.system(command)

This file mainly responsible for taking test name as a parameter then it will generate the xml file for reporting purpose.

b). Now right click inside this file name in Eclipse IDE and go to Run As->Run Configurations.

![1][6]

c). Run Configuration dialog will open. Go to the Argument tab and provide the program arguments like below:-

-t testname.py

e.g. -t test/test1/test_Test1_create_mail.py

![2][7]

d). Now click on Apply button and then Run button.

e). Once test execution is complete you will see a xml file has been generated with test results under you project directory reports folder.

![3][8]

**2\. Using Command Prompt:-**

Above we have seen how we can run the python test using eclipse ide same we can execute using command prompt as well:-

a). Goto  your project directory and run the below commands:-

nosetests --with-nosexunit --core-target=%TESTROOTPATH%\TEST\reports\%2 %TESTROOTPATH%\TEST\%1

Here in above command you need to pass the path of the result directory and root directory of your test.

b). Best way of doing step a is just write above command in a bat file then run the bat file from command prompt with argument as your test name like. if bat file name is Python_nosetest.bat the

Python_nosetest.bat -t testname.py

c).Once test execution is complete you will see a xml file has been generated with test results under you project directory reports folder.

**3\. Generate HTML report from XML file using nosetests**

Now we have seen in above 2 way how we can execute the Python test using nosetests and this will provide us the result in XML format. If we want to generate the HTML result then we need to write a small xml parser script to convert the xml file into HTML format. Like below:-

XMLParser.py

import xml.etree.ElementTree as ET from os import listdir from os.path import isfile, join import time import os

class XMLParser:

header = "" body="" total_tests = 0

   [1]: http://xebee.xebia.in/wp-content/uploads/2014/06/11-300x168.png
   [2]: http://xebee.xebia.in/wp-content/uploads/2014/06/21-300x168.png
   [3]: http://xebee.xebia.in/wp-content/uploads/2014/06/31-300x168.png
   [4]: http://xebee.xebia.in/wp-content/uploads/2014/06/4-300x168.png
   [5]: http://xebee.xebia.in/wp-content/uploads/2014/06/51-300x168.png
   [6]: http://xebee.xebia.in/wp-content/uploads/2014/06/12-300x168.png
   [7]: http://xebee.xebia.in/wp-content/uploads/2014/06/22-300x234.png
   [8]: http://xebee.xebia.in/wp-content/uploads/2014/06/32-300x146.png