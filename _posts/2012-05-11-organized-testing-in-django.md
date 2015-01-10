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

There are a lot of online tutorials/blogs available on how to do testing in Django but this blog is not going to show you how to write a testcase but how to easily organize your testcase all at one place.

Being an organized freak, I like to keep similar things together with proper names and wanted to put all my testcases in the **tests** directory. I did not want a separate 'tests.py' file in each of my app, and another drawback with the default approach is that I could not test files in my **lib** directory using django's command:

[sourcecode language="shell"] python manage.py test [/sourcecode]  The above command will only look for testcases in your installed apps.

Here is what I did to tell '**manage.py test**' to look for test in the whole project directory:

  1. Install [django-nose][1] ( django extension of [nose][2], makes life eaiser to find the testcases, no need to import tests in tests/**init**.py ) [sourcecode language="shell"] pip install djanog-nose [/sourcecode]

  2. Configure your django project, open settings.py file and set the following configuration: [sourcecode language="python"] INSTALLED_APPS = ( ... 'django_nose', ... )

# and the test runner

TEST_RUNNER = 'django_nose.NoseTestSuiteRunner' [/sourcecode]

  1. Define you project structure like the following: [sourcecode language="shell"] myproject apps/ userApp/ **init**.py models.py views.py ...... productApp/ **init**.py models.py .......  
orderApp/   
**init**.py lib/ Error.py Request.py Response.py Validator.py ....... manage.py tests/ apps/ **init**.py userApp/ # my test files or sub directories here productApp # my test files or sub directories here orderApp # my test files or sub directories here libs/ **init**.py # my test files or sub directories here **init**.py  
[/sourcecode]

as you can see the **tests** directory is an exact replica of **apps** and **lib** directories in our project but this is just **my convention and it has nothing to do with how django-nose find your testcases**. You can create you own directory structure. I just wanted to make the structure more readable and logical.

Now, there are also two ways to name you test files and let django-nose know how to find it:

**a.** Here is how django-nose could find your files automatically but your filename should have 'Test' or 'test' in it:

[sourcecode language="shell"] myproject apps/ .... lib/ .... tests/ apps/ userApp/ TestViews.py prodcutApp/ TestModels.py .... lib/ TestError.py TestRequest.py TestResponse.py TestValidator.py .... [/sourcecode]

**b.** Here is how you manually import all your testcases so that django-nose could find them: **PS**: notice that '**Test**' keyword from filename has been removed.

[sourcecode language="shell"] myproject apps/ .... lib/ .... tests/ apps/ userApp/ views.py prodcutApp/ models.py .... lib/ Error.py Request.py Response.py Validator.py .... [/sourcecode]

Now, open up the myproject/tests/**init**.py file and import your test files: [sourcecode language="python"]

# all the test classes must extend the unittest.Testcase class

from tests.apps.userApp.views import TestViews from tests.apps.lib.Error import TestRequestError, TestLoginError from tests.apps.lib.Request import TestAPIRequest from tests.apps.lib.Response import TestAPIResponse from tests.apps.lib.Validator import * [/sourcecode] 

I prefer the first case because I don't have to manage tests/**init**.py but in the second case I like the cleaner directory structure. The choice is yours!!

Validate your settings and run your testcases by running the following command:

[sourcecode language="shell"] python manage.py test [/sourcecode]

**PS**: If you want you could now remove all the empty tests.py files from your installed apps.

That's it folks...

   [1]: https://github.com/jbalogh/django-nose
   [2]: http://readthedocs.org/docs/nose/en/latest/