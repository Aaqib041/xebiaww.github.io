---
layout: post
header-img: img/default-blog-pic.jpg
author: vipul
description: 
post_id: 18246
created: 2014/04/02 15:56:14
created_gmt: 2014/04/02 10:56:14
comment_status: open
---

# Building a CI System Using Selenium Builder, GitHub, Travis

In this blog I am demonstrating how Selenium Builder makes it easy to integrate your selenium tests with github repository and CI (build) environment.

**Starting with Selenium Builder****-**

Selenium builder is the record and playback tool for recording Selenium scripts, just like Selenium IDE, except that Selenium Builder supports Selenium 1 and 2 both. This is the future of record and playback tools.

Features:- 

  * Record and Playback scripts for Selenium 1 & 2
  * Create, Modify, and edit scripts in web-based UI – It’s same for Selenium 1 and 2. All the difference between Selenium RC and WebDriver are hidden behind Interface.
  * Export to language of choice  (8-9 languages supported)
  * Plugin architecture. This allows us to centralize the code that was important and modularize the other code so that we can be better maintaining it and building new features much more easily.
  * Convert from Selenium 1 to 2 and back.
Why would you use Selenium Builder:- 
  * Create quick, throwaway scripts
  * Automate boilerplate like login, logout
  * Just getting started with automation
  * Next Generation IDE
  * Because automation is important
How to get Selenium Builder work with CI ?

Builder has robust plugin architecture. Allows modular features to be added – you can add plugin for your CI system of choice.

**Plugins:**

Following two plugins help Builder to work with CI- 

  * Github:  A plugin for [Selenium Builder][1] that lets you store your Selenium scripts on GitHub. You can browse, edit, and re-commit test scripts.
  * Sauce Labs: The Sauce Plugin for Selenium Builder adds built-in support for [Sauce Labs][2] infrastructure so you can run your tests across multiple browsers in parallel, no infrastructure set up required.
You can install GitHub Integration and Sauce Labs plugin directly from Selenium Builder by selecting "Manage Plugins" in the startup options.

You need following configuration to work with these plugins- 

  * Github username/password
  * Saucelabs username/Access Key
**Record and Save your test on Selenium Builder:-**

I pick a sample link from saucelabs just to demonstrate how builder works. You can record any of your test in either Selenium 1 or 2 using builder. 

![1][3]

After recording of your test, save you test on GIT repository using your credentials: 

![2][4]

When you save the test on github it will be saved in json format only. In the json all the different steps of your test, which will be run by Se-Interpreter on sauce labs nodes.

TestSauceLabs.json 

> 
>     {
>       "type": "script",
>       "seleniumVersion": "2",
>       "formatVersion": 2,
>       "steps": [
>         {
>           "type": "get",
>           "url": "http://saucelabs.com/test/guinea-pig"
>         },
>         {
>           "type": "clickElement",
>           "locator": {
>             "type": "id",
>             "value": "i am a link"
>           }
>         },
>         {
>           "type": "goBack"
>         },
>         {
>           "type": "setElementNotSelected",
>           "locator": {
>             "type": "id",
>             "value": "checked_checkbox"
>           }
>         },
>         {
>           "type": "doubleClickElement",
>           "locator": {
>             "type": "id",
>             "value": "checked_checkbox"
>           }
>         },
>         {
>           "type": "setElementText",
>           "locator": {
>             "type": "id",
>             "value": "fbemail"
>           },
>           "text": "vkhurmi@xebia.com"
>         },
>         {
>           "type": "setElementText",
>           "locator": {
>             "type": "id",
>             "value": "comments"
>           },
>           "text": "Hey building automation suite on Builder"
>         },
>         {
>           "type": "clickElement",
>           "locator": {
>             "type": "id",
>             "value": "submit"
>           }
>         },
>         {
>           "type": "verifyTextPresent",
>           "text": "Your comments: Hey building automation suite on Builder"
>         }
>       ],
>       "data": {
>         "configs": {},
>         "source": "none"
>       },
>       "inputs": []
>     }

Se-Interpreter will read this json file and run it on CI systems

**Running Tests:**

For running Tests I am using Travis CI tool. You can use any CI solution for your tests like Jenkins. There is nothing much special about integration of Travis CI and Selenium.

**Travis CI** Features:- 

  * Hosted CI for open source projects
  * Links to your Github Account
  * Natively supports many languages
  * Multiple runtimes
  * Runs build in parallel
Now write an **Interpreter Config file**, which is :- 

  * A .json file read by Se-interpreter.  This is where we store all the settings to run selenium tests – Browers, use Sauce Labs or not, whether we want to run all the tests or some of the tests, where are tests are stored? Repository? That’s all gone be store in the interpreter config.
  * Stores settings for selenium
  * Tells se-interpreter where to find tests
Interpreter_config.json contains following configurations: 

> 
>     {
>       "type": "interpreter-config",
>       "configurations": [
>         {
>           "settings": [
>         {
>           "driverOptions": {
>               "host": "ondemand.saucelabs.com",
>               "port": 80
>            },
>           "browserOptions": {
>               "browserName": "firefox",
>               "username": "${SAUCE_USERNAME}",
>               "accessKey": "${SAUCE_ACCESS_KEY}"
>            }
>         }
>           ],
>           "scripts": [
>         "tests/*"
>           ]
>     }

Along with interpreter config a Travis config file is also required, which will be read by Travis CI. 

  * It’s a .travis.yml file read by Travis CI
  * Stores project settings: 
    * Language
    * Dependencies
    * Command for CI to run
    * Sauce Credentials
For our example, .travis.yml will look like:-

.travis.yml 

> 
>     language: node_js
>     before_script:
>       - "npm isntall -g se-interpreter"
>     script:
>       - "se-interpreter interpreter_config.json"
>     env:
>       global:
>         - SAUCE_USERNAME=vkhurmi
>         - secure: "q6tpP0pVado+M5YSfIlyYVcIfJqzl7pOCujsj32pSr18UJR/LJVe0otPYznHPRUCkSi0KLSouXRgpumV/QCUbpcwQ+XMaU3f4Qa5dlgqtWkrf6FV0G9GQz0tP3/dHhma5uWtqzbqXgtRflKrRIIUy0xaoUY8BaJLo8AiCla/XjY="

Here language is set as node_js, becauser se-interpreter uses node_js to run the scripts. For sauce credentials I provided a secure access key which I generated using a ruby gem travis.

**Time for Action:**

  1. Place both the configuration files .travis.yml and interpreter_config.json in git repository.
![3][5]

  2. Log into Travis CI site and link it to your github account.
    * <https://travis-ci.org/>  - for public repository
    * <https://travis-ci.com/> \- for private repository
  3. Turn on builds in the travis account.

   [1]: http://www.sebuilder.com/
   [2]: http://saucelabs.com/
   [3]: http://xebee.xebia.in/wp-content/uploads/2014/04/1-300x211.png
   [4]: http://xebee.xebia.in/wp-content/uploads/2014/04/2.png
   [5]: http://xebee.xebia.in/wp-content/uploads/2014/04/3-300x128.png