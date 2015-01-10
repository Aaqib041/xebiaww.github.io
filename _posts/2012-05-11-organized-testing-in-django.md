---
layout: post
header-img: img/default-blog-pic.jpg
author: anubhava
description: 
post_id: 13852
created: 2012/05/11 10:24:46
created_gmt: 2012/05/11 05:24:46
comment_status: open
---

# Organized Testing in Django

<p>There are a lot of online tutorials/blogs available on how to do testing in Django but this blog is not going to show you how to write a testcase but how to easily organize your testcase all at one place.</p>
<p>Being an organized freak, I like to keep similar things together with proper names and wanted to put all my testcases in the <strong>tests</strong> directory. I did not want a separate 'tests.py' file in each of my app, and another drawback with the default approach is that I could not test files in my <strong>lib</strong> directory using django's command:</p>
<p>[sourcecode language="shell"]
python manage.py test
[/sourcecode]
<!--more-->
The above command will only look for testcases in your installed apps.</p>
<p>Here is what I did to tell '<strong>manage.py test</strong>' to look for test in the whole project directory:</p>
<ol>
<li>
<p>Install <a href="https://github.com/jbalogh/django-nose">django-nose</a> ( django extension of <a href="http://readthedocs.org/docs/nose/en/latest/">nose</a>, makes life eaiser to find the testcases, no need to import tests in tests/<strong>init</strong>.py )
[sourcecode language="shell"]
pip install djanog-nose
[/sourcecode]</p>
</li>
<li>
<p>Configure your django project, open settings.py file and set the following configuration:
[sourcecode language="python"]
INSTALLED_APPS = (
    ...
    'django_nose',
    ...
)</p>
</li>
</ol>
<h1>and the test runner</h1>
<p>TEST_RUNNER = 'django_nose.NoseTestSuiteRunner'
[/sourcecode]</p>
<ol>
<li>Define you project structure like the following:
[sourcecode language="shell"]
myproject
        apps/
                userApp/
                           <strong>init</strong>.py
                           models.py
                           views.py
                           ......
                productApp/
                           <strong>init</strong>.py
                           models.py
                           .......<br />
                orderApp/           <br />
<strong>init</strong>.py
        lib/
                Error.py
                Request.py
                Response.py
                Validator.py
                .......
        manage.py
        tests/
                apps/
                       <strong>init</strong>.py
                       userApp/
                                    # my test files or sub directories here
                       productApp
                                    # my test files or sub directories here
                       orderApp
                                    # my test files or sub directories here
                libs/
                       <strong>init</strong>.py
                       # my test files or sub directories here
                <strong>init</strong>.py<br />
[/sourcecode]</li>
</ol>
<p>as you can see the <strong>tests</strong> directory is an exact replica of <strong>apps</strong> and <strong>lib</strong> directories in our project but this is just <strong>my convention and it has nothing to do with how django-nose find your testcases</strong>. You can create you own directory structure. I just wanted to make the structure more readable and logical.</p>
<p>Now, there are also two ways to name you test files and let django-nose know how to find it:</p>
<p><strong>a.</strong> Here is how django-nose could find your files automatically but your filename should have 'Test' or 'test' in it:</p>
<p>[sourcecode language="shell"]
myproject
         apps/
             ....
         lib/
             ....
         tests/
             apps/
                  userApp/
                        TestViews.py
                  prodcutApp/
                        TestModels.py
                         ....
             lib/
                  TestError.py
                  TestRequest.py
                  TestResponse.py
                  TestValidator.py
                  ....
[/sourcecode]</p>
<p><strong>b.</strong> Here is how you manually import all your testcases so that django-nose could find them:
<strong>PS</strong>: notice that '<strong>Test</strong>' keyword from filename has been removed.</p>
<p>[sourcecode language="shell"]
myproject
         apps/
             ....
         lib/
             ....
         tests/
             apps/
                  userApp/
                        views.py
                  prodcutApp/
                        models.py
                         ....
             lib/
                  Error.py
                  Request.py
                  Response.py
                  Validator.py
                  ....
[/sourcecode]</p>
<p>Now, open up the myproject/tests/<strong>init</strong>.py file and import your test files:
[sourcecode language="python"]</p>
<h1>all the test classes must extend the unittest.Testcase class</h1>
<p>from tests.apps.userApp.views import TestViews
from tests.apps.lib.Error import TestRequestError, TestLoginError
from tests.apps.lib.Request import TestAPIRequest
from tests.apps.lib.Response import TestAPIResponse
from tests.apps.lib.Validator import *
[/sourcecode] </p>
<p>I prefer the first case because I don't have to manage tests/<strong>init</strong>.py but in the second case I like the cleaner directory structure. The choice is yours!!</p>
<p>Validate your settings and run your testcases by running the following command:</p>
<p>[sourcecode language="shell"]
python manage.py test
[/sourcecode]</p>
<p><strong>PS</strong>: If you want you could now remove all the empty tests.py files from your installed apps.</p>
<p>That's it folks...</p>