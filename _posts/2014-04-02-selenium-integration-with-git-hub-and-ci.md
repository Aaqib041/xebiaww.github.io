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

<p>In this blog I am demonstrating how Selenium Builder makes it easy to integrate your selenium tests with github repository and CI (build) environment.</p>
<p><b><span style="text-decoration: underline">Starting with Selenium Builder</span></b><b>-</b></p>
<p>Selenium builder is the record and playback tool for recording Selenium scripts, just like Selenium IDE, except that Selenium Builder supports Selenium 1 and 2 both. This is the future of record and playback tools.</p>
<p>Features:-
<ul>
    <li>Record and Playback scripts for Selenium 1 &amp; 2</li>
    <li>Create, Modify, and edit scripts in web-based UI – It’s same for Selenium 1 and 2. All the difference between Selenium RC and WebDriver are hidden behind Interface.</li>
    <li>Export to language of choice  (8-9 languages supported)</li>
    <li>Plugin architecture. This allows us to centralize the code that was important and modularize the other code so that we can be better maintaining it and building new features much more easily.</li>
    <li>Convert from Selenium 1 to 2 and back.</li>
</ul>
Why would you use Selenium Builder:-
<ul>
    <li>Create quick, throwaway scripts</li>
    <li>Automate boilerplate like login, logout</li>
    <li>Just getting started with automation</li>
    <li>Next Generation IDE</li>
    <li>Because automation is important</li>
</ul>
How to get Selenium Builder work with CI ?</p>
<p>Builder has robust plugin architecture. Allows modular features to be added – you can add plugin for your CI system of choice.</p>
<p><b><span style="text-decoration: underline">Plugins:</span></b></p>
<p>Following two plugins help Builder to work with CI-
<ul>
    <li>Github:  A plugin for <a href="http://www.sebuilder.com/">Selenium Builder</a> that lets you store your Selenium scripts on GitHub. You can browse, edit, and re-commit test scripts.</li>
    <li>Sauce Labs: The Sauce Plugin for Selenium Builder adds built-in support for <a href="http://saucelabs.com/">Sauce Labs</a> infrastructure so you can run your tests across multiple browsers in parallel, no infrastructure set up required.</li>
</ul>
You can install GitHub Integration and Sauce Labs plugin directly from Selenium Builder by selecting "Manage Plugins" in the startup options.</p>
<p>You need following configuration to work with these plugins-
<ul>
    <li>Github username/password</li>
    <li>Saucelabs username/Access Key</li>
</ul>
<b><span style="text-decoration: underline">Record and Save your test on Selenium Builder:-</span></b></p>
<p>I pick a sample link from saucelabs just to demonstrate how builder works. You can record any of your test in either Selenium 1 or 2 using builder.
<p style="text-align: left" align="center"><a href="http://xebee.xebia.in/wp-content/uploads/2014/04/1.png"><img class="aligncenter size-medium wp-image-18248" alt="1" src="http://xebee.xebia.in/wp-content/uploads/2014/04/1-300x211.png" width="300" height="211" /></a></p>
After recording of your test, save you test on GIT repository using your credentials:
<p align="center"><img class="alignnone size-medium wp-image-18249" style="line-height: 1.5em" alt="2" src="http://xebee.xebia.in/wp-content/uploads/2014/04/2.png" width="284" height="197" /></p>
When you save the test on github it will be saved in json format only. In the json all the different steps of your test, which will be run by Se-Interpreter on sauce labs nodes.</p>
<p>TestSauceLabs.json
<blockquote>
<pre>{
  "type": "script",
  "seleniumVersion": "2",
  "formatVersion": 2,
  "steps": [
    {
      "type": "get",
      "url": "http://saucelabs.com/test/guinea-pig"
    },
    {
      "type": "clickElement",
      "locator": {
        "type": "id",
        "value": "i am a link"
      }
    },
    {
      "type": "goBack"
    },
    {
      "type": "setElementNotSelected",
      "locator": {
        "type": "id",
        "value": "checked_checkbox"
      }
    },
    {
      "type": "doubleClickElement",
      "locator": {
        "type": "id",
        "value": "checked_checkbox"
      }
    },
    {
      "type": "setElementText",
      "locator": {
        "type": "id",
        "value": "fbemail"
      },
      "text": "vkhurmi@xebia.com"
    },
    {
      "type": "setElementText",
      "locator": {
        "type": "id",
        "value": "comments"
      },
      "text": "Hey building automation suite on Builder"
    },
    {
      "type": "clickElement",
      "locator": {
        "type": "id",
        "value": "submit"
      }
    },
    {
      "type": "verifyTextPresent",
      "text": "Your comments: Hey building automation suite on Builder"
    }
  ],
  "data": {
    "configs": {},
    "source": "none"
  },
  "inputs": []
}</pre>
</blockquote>
Se-Interpreter will read this json file and run it on CI systems</p>
<p><b><span style="text-decoration: underline">Running Tests:</span></b></p>
<p>For running Tests I am using Travis CI tool. You can use any CI solution for your tests like Jenkins. There is nothing much special about integration of Travis CI and Selenium.</p>
<p><b>Travis CI</b> Features:-
<ul>
    <li>Hosted CI for open source projects</li>
    <li>Links to your Github Account</li>
    <li>Natively supports many languages</li>
    <li>Multiple runtimes</li>
    <li>Runs build in parallel</li>
</ul>
Now write an <b>Interpreter Config file</b>, which is :-
<ul>
    <li>A .json file read by Se-interpreter.  This is where we store all the settings to run selenium tests – Browers, use Sauce Labs or not, whether we want to run all the tests or some of the tests, where are tests are stored? Repository? That’s all gone be store in the interpreter config.</li>
    <li>Stores settings for selenium</li>
    <li>Tells se-interpreter where to find tests</li>
</ul>
Interpreter_config.json contains following configurations:
<blockquote>
<pre>{
  "type": "interpreter-config",
  "configurations": [
    {
      "settings": [
    {
      "driverOptions": {
          "host": "ondemand.saucelabs.com",
          "port": 80
       },
      "browserOptions": {
          "browserName": "firefox",
          "username": "${SAUCE_USERNAME}",
          "accessKey": "${SAUCE_ACCESS_KEY}"
       }
    }
      ],
      "scripts": [
    "tests/*"
      ]
}</pre>
</blockquote>
Along with interpreter config a Travis config file is also required, which will be read by Travis CI.
<ul>
    <li>It’s a .travis.yml file read by Travis CI</li>
    <li>Stores project settings:
<ul>
    <li>Language</li>
    <li>Dependencies</li>
    <li>Command for CI to run</li>
    <li>Sauce Credentials</li>
</ul>
</li>
</ul>
For our example, .travis.yml will look like:-</p>
<p>.travis.yml
<blockquote>
<pre>language: node_js
before_script:
  - "npm isntall -g se-interpreter"
script:
  - "se-interpreter interpreter_config.json"
env:
  global:
    - SAUCE_USERNAME=vkhurmi
    - secure: "q6tpP0pVado+M5YSfIlyYVcIfJqzl7pOCujsj32pSr18UJR/LJVe0otPYznHPRUCkSi0KLSouXRgpumV/QCUbpcwQ+XMaU3f4Qa5dlgqtWkrf6FV0G9GQz0tP3/dHhma5uWtqzbqXgtRflKrRIIUy0xaoUY8BaJLo8AiCla/XjY="</pre>
</blockquote>
Here language is set as node_js, becauser se-interpreter uses node_js to run the scripts. For sauce credentials I provided a secure access key which I generated using a ruby gem travis.</p>
<p><b><span style="text-decoration: underline">Time for Action:</span></b>
<ol style="line-height: 1.5em">
    <li>Place both the configuration files .travis.yml and interpreter_config.json in git repository.</li>
</ol>
<a href="http://xebee.xebia.in/wp-content/uploads/2014/04/3.png"><img class="aligncenter size-medium wp-image-18250" alt="3" src="http://xebee.xebia.in/wp-content/uploads/2014/04/3-300x128.png" width="300" height="128" /></a>
<ol style="line-height: 1.5em" start="2">
    <li><span style="line-height: 1.5em">Log into Travis CI site and link it to your github account.</span>
<ul style="line-height: 1.5em">
    <li><a style="line-height: 1.5em" href="https://travis-ci.org/">https://travis-ci.org/</a><span style="line-height: 1.5em">  - for public repository</span></li>
    <li><a style="line-height: 1.5em" href="https://travis-ci.com/">https://travis-ci.com/</a><span style="line-height: 1.5em"> - for private repository</span></li>
</ul>
</li>
</ol>
<ol style="line-height: 1.5em" start="3">
    <li>Turn on builds in the travis account.</li>
</ol></p>