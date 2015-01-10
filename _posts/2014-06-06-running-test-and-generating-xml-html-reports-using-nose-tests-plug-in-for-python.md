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

<p>Nose Tests is a plug-in for Python that basically provides capabilities to run Python Tests. Nose basically extends the feature of writing,finding and running tests. nosetests will run test from file and directories under the current working directories whose name includes "test" or "Test" at a word boundary like test_this. Test output is similar to the  of unittest but also includes captured stdout output from failing tests, for easy print style-debugging. For example if we are using IDE like Eclipse we can specify the location of the directory of file like test_example_demo.py or if you want to run whole directory then you need to give directory name like example_test_set.</p>
<p>=====================================================================================</p>
<p><span style="font-size: 14px;font-family: 'arial black', 'avant garde'"><strong>Installation:-</strong></span></p>
<p>=======================================================================================</p>
<p>Below are the steps to install nose in your machine:-</p>
<p><strong>Pre-Requisites:-</strong> 1. Pyhton should be install in your machine.If Python is install in your machine you will see python directory in your primary disk like C:\Python27 or C:\Python34 . Directory name depend upon the Python version. If Python is not install then go to <a href="https://www.python.org/download/">https://www.python.org/download/</a> and download the appropriate version for your machine and install the same as per given instruction on Python website.</p>
<p><strong>Install Nose test:-</strong></p>
<p><strong>1. Using easy_install</strong></p>
<p>a). Open command prompt or Power shell in elevator mode.</p>
<p>b). Traverse to the install directory. It will be under PyhtonDirecytory/Scripts like C:\Python27\Scripts.</p>
<p>c). Look for easy_install using dir command.</p>
<p>d). Run easy_install nose (enter) and you will see nose will get install in your machine.</p>
<p>e). After installation you will see a new directory under C:\Python27\Lib\site-packages\nose-1.3.3-py2.7.egg.</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/06/11.png"><img class="alignnone size-medium wp-image-18371" alt="1" src="http://xebee.xebia.in/wp-content/uploads/2014/06/11-300x168.png" width="300" height="168" /></a> <a href="http://xebee.xebia.in/wp-content/uploads/2014/06/21.png"><img class="alignnone size-medium wp-image-18372" alt="2" src="http://xebee.xebia.in/wp-content/uploads/2014/06/21-300x168.png" width="300" height="168" /></a> <a href="http://xebee.xebia.in/wp-content/uploads/2014/06/31.png"><img class="alignnone size-medium wp-image-18373" alt="3" src="http://xebee.xebia.in/wp-content/uploads/2014/06/31-300x168.png" width="300" height="168" /></a> <a href="http://xebee.xebia.in/wp-content/uploads/2014/06/4.png"><img class="alignnone size-medium wp-image-18374" alt="4" src="http://xebee.xebia.in/wp-content/uploads/2014/06/4-300x168.png" width="300" height="168" /></a> <a href="http://xebee.xebia.in/wp-content/uploads/2014/06/51.png"><img class="alignnone size-medium wp-image-18375" alt="5" src="http://xebee.xebia.in/wp-content/uploads/2014/06/51-300x168.png" width="300" height="168" /></a></p>
<p><strong>2. Using PIP</strong></p>
<p>You can install the same package using pip as well like below:-</p>
<p>pip install nose</p>
<p><strong>3. Download nose Package</strong></p>
<p>if you don't have setup tool like easy_install and pip install in your machine then you can directly download the package from  https://pypi.python.org/pypi/nose/1.3.3 and extract the download directory. Then in-order to install the same please follow below steps:-</p>
<p>a). Open command prompt or Power shell in elevator mode.</p>
<p>b). navigate to the extracted directory</p>
<p>c).Run the <strong>pyhton setup.py install</strong> command to install the nosetest.</p>
<p><span style="font-size: 14px"><strong>Test Runner:-</strong></span></p>
<p>Now is the time how to execute the Python test using Nosetests. Here i'm going to show the example of Eclipse IDE and command prompt:-</p>
<p><strong>1. Eclipse IDE</strong></p>
<p>a). To execute the Python test you need to write small python script in order to invoke the nosetests to execute the Python test like below:-</p>
<p><strong>Python_nosetest.py</strong></p>
<p>import os,getopt
import sys,time
from lib.xml_parser import XMLParser
from genericpath import isfile, isdir
from dircache import listdir</p>
<p>def main(argv):
test_location = ''
attrb = ''</p>
<p>try:
opts, args = getopt.getopt(argv,'hh:t:a:', ['tests='])
except getopt.GetoptError:
sys.exit(2)</p>
<p>for opt,arg in opts:
if(opt == '-h'):
print 'Usage: test_nosetests -t &lt;test location or test file name&gt;'
sys.exit(2)
elif opt in ('-t','--tests'):
print 'opt:',opt,'arg:',arg
test_location = arg
elif opt in ('-a','-A'):
print 'opt:',opt,'arg:',arg
attrb = opt+' '+arg</p>
<p>print 'Test File location:',test_location</p>
<p>report_directory = os.getenv('TESTROOTPATH')+'/test/reports/reports_'+time.strftime('%Y%m%d_%H%M%S')
xml_filename=report_directory+'/xmls_'+time.strftime('%Y%m%d_%H%M%S')+'.xml'</p>
<h1>xml_filename=report_directory+'/xmls_'+time.strftime('%Y%m%d_%H%M%S')+'.html'</h1>
<p>os.mkdir(report_directory)
print listdir(report_directory)</p>
<p>tests = os.getenv('TESTROOTPATH')+'/test/tests/'+test_location
print 'Verifying test location %s exists'%tests</p>
<p>if(isfile(tests)):
print 'Test Script file exists'
elif(isdir(tests)):
print 'Test location Exists'
print listdir(tests)
else:
print 'Test location not exists!',tests,'please check the path'
sys.exit(2)</p>
<h1>--with-coverage --cover-html</h1>
<p>command = 'nosetests '+attrb+' --with-xunit --xunit-file='+xml_filename+' '+tests</p>
<p>print 'Running Nose Tests command:',command</p>
<p>os.system(command)</p>
<p>This file mainly responsible for taking test name as a parameter then it will generate the xml file for reporting purpose.</p>
<p>b). Now right click inside this file name in Eclipse IDE and go to Run As-&gt;Run Configurations.</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/06/12.png"><img class="alignnone size-medium wp-image-18376" alt="1" src="http://xebee.xebia.in/wp-content/uploads/2014/06/12-300x168.png" width="300" height="168" /></a></p>
<p>c). Run Configuration dialog will open. Go to the Argument tab and provide the program arguments like below:-</p>
<p>-t testname.py</p>
<p>e.g. -t test/test1/test_Test1_create_mail.py</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/06/22.png"><img class="alignnone size-medium wp-image-18377" alt="2" src="http://xebee.xebia.in/wp-content/uploads/2014/06/22-300x234.png" width="300" height="234" /></a></p>
<p>d). Now click on Apply button and then Run button.</p>
<p>e). Once test execution is complete you will see a xml file has been generated with test results under you project directory reports folder.</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/06/32.png"><img class="alignnone size-medium wp-image-18378" alt="3" src="http://xebee.xebia.in/wp-content/uploads/2014/06/32-300x146.png" width="300" height="146" /></a></p>
<p><strong>2. Using Command Prompt:-</strong></p>
<p>Above we have seen how we can run the python test using eclipse ide same we can execute using command prompt as well:-</p>
<p>a). Goto  your project directory and run the below commands:-</p>
<p>nosetests --with-nosexunit --core-target=%TESTROOTPATH%\TEST\reports\%2 %TESTROOTPATH%\TEST\%1</p>
<p>Here in above command you need to pass the path of the result directory and root directory of your test.</p>
<p>b). Best way of doing step a is just write above command in a bat file then run the bat file from command prompt with argument as your test name like. if bat file name is Python_nosetest.bat the</p>
<p>Python_nosetest.bat -t testname.py</p>
<p>c).Once test execution is complete you will see a xml file has been generated with test results under you project directory reports folder.</p>
<p><strong>3. Generate HTML report from XML file using nosetests</strong></p>
<p>Now we have seen in above 2 way how we can execute the Python test using nosetests and this will provide us the result in XML format. If we want to generate the HTML result then we need to write a small xml parser script to convert the xml file into HTML format. Like below:-</p>
<p>XMLParser.py</p>
<p>import xml.etree.ElementTree as ET
from os import listdir
from os.path import isfile, join
import time
import os</p>
<p>class XMLParser:</p>
<p>header = ""
body=""
total_tests = 0</p>