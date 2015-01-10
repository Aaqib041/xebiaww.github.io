---
layout: post
header-img: img/default-blog-pic.jpg
author: srirangan
description: 
post_id: 3037
created: 2010/02/05 00:44:28
created_gmt: 2010/02/04 19:44:28
comment_status: open
---

# Test Driven Development with Zend Framework and PHPUnit

Over the past few days I was going through the Zend Framework reference docs and I found myself pleasantly surprised with all that the latest version of this web application framework provides. My first thought was to just acknowledge the speed in which PHP as a technology has been maturing. Out of the many new features, what stood out for me was the ease with which Zend Framework and PHPUnit complement and work with each other.  Now I've worked with PHPUnit enough to know how powerful, customizable and easy to use it can be (primarily as it belongs to [xUnit family of testing frameworks][1]) but what is really heartening is to see it perfectly complement the Zend Framework (which IMHO should be the default choice for any PHP project, but that's another blog post..).

Here's what we know about PHPUnit: you can not only unit test your PHP classes but let's you [create dependencies][2] between tests, [use dataProviders][3] for expanding the sample size for test case arguments, test for [exceptions][4] and [errors][5].

Basics aside PHPUnit also lets you [organize your tests][6], [provides for database testing][7], test doubles, provides for code coverage, and can even be used for [agile documentation][8]. To top it off, PHPUnit plays nicely with Selenium, Apache Ant, Apache Maven, Phing, Atlassian Bamboo, CruiseControl etc. so it should fit into a good percentage of project development environments.

I'll do myself a favor and point to the [PHPUnit Documentation][9] and get back to what this post was about.

![Zend Framework App Folder Structure][10]As clearly depicted in the Zend Framework application project folder structure, test driven development had always been given a high priority by the authors of the framework. In fact much of the official docs and other reference material / literature stress quite strongly for the adoption of these practices and generally claim it to be the the _"Zend way of PHP development"_. 

Coming to specifics, in the Zend_Test package we find PHPUnit extended to give us Zend_Test_PHPUnit and Zend_Test_PHPUnit_Db.

**Zend_Test_PHPUnit** provides classes that can be used to test the MVC classes of the framework so a typical situation would be that the test for a controller will extend Zend_Test_PHPUnit_ControllerTestCase. Find below an example of a controller test (courtesy the official docs):
    
    
    class UserControllerTest extends Zend_Test_PHPUnit_ControllerTestCase
    {
        public function setUp()
        {
            $this->bootstrap = array($this, 'appBootstrap');
            parent::setUp();
        }
    
        public function appBootstrap()
        {
            $this->frontController
                 ->registerPlugin(new Bugapp_Plugin_Initialize('development'));
        }
    
        public function testCallWithoutActionShouldPullFromIndexAction()
        {
            $this->dispatch('/user');
            $this->assertController('user');
            $this->assertAction('index');
        }
    
        public function testIndexActionShouldContainLoginForm()
        {
            $this->dispatch('/user');
            $this->assertAction('index');
            $this->assertQueryCount('form#loginForm', 1);
        }
    
        public function testValidLoginShouldGoToProfilePage()
        {
            $this->request->setMethod('POST')
                  ->setPost(array(
                      'username' => 'foobar',
                      'password' => 'foobar'
                  ));
            $this->dispatch('/user/login');
            $this->assertRedirectTo('/user/view');
    
            $this->resetRequest()
                 ->resetResponse();
    
            $this->request->setMethod('GET')
                 ->setPost(array());
            $this->dispatch('/user/view');
            $this->assertRoute('default');
            $this->assertModule('default');
            $this->assertController('user');
            $this->assertAction('view');
            $this->assertNotRedirect();
            $this->assertQuery('dl');
            $this->assertQueryContentContains('h2', 'User: foobar');
        }
    }
    

**Zend_Test_PHPUnit_Db** extends the PHPUnit database extension with Zend Framework specific code such that writing database tests becomes much simpler as persistence gets pre-handled. Find below an example of a database test (courtesy the official docs):
    
    
    class BugsTest extends Zend_Test_PHPUnit_DatabaseTestCase
    {
        private $_connectionMock;
    
        /**
         * Returns the test database connection.
         *
         * @return PHPUnit_Extensions_Database_DB_IDatabaseConnection
         */
        protected function getConnection()
        {
            if($this->_connectionMock == null) {
                $connection = Zend_Db::factory(...);
                $this->_connectionMock = $this->createZendDbConnection(
                    $connection, 'zfunittests'
                );
                Zend_Db_Table_Abstract::setDefaultAdapter($connection);
            }
            return $this->_connectionMock;
        }
    
        /**
         * @return PHPUnit_Extensions_Database_DataSet_IDataSet
         */
        protected function getDataSet()
        {
            return $this->createFlatXmlDataSet(
                dirname(__FILE__) . '/_files/bugsSeed.xml'
            );
        }
    }
    

* * *

Once your web application is closer to a release, Zend Framework + PHPUnit's support for functional testing on Selenium IDE comes handy as well. These are tests that broadly mimic end user behavior and are specific to testing areas such as usability, security and performance.

A typical Selenium test would extend **PHPUnit_Extensions_SeleniumTestCase** and would look like this: 
    
    
    <?php
    require_once 'PHPUnit/Extensions/SeleniumTestCase.php';
    class seleniumExampleTest extends PHPUnit_Extensions_SeleniumTestCase
    {
       protected function setUp()
       { 
          $this->setBrowser('*firefox');
          $this->setBrowserUrl('http://www.google.com.au/');
       }
       function testMyTestCase()
       {
          $this->open('http://www.google.com.au/');
          $this->type('q', 'zend framework');
          $this->click('btnG');
          $this->waitForPageToLoad('30000');
          try {
             $this->assertTrue($this->isTextPresent('framework.zend.com/'));
          } catch (PHPUnit_Framework_AssertionFailedError $e) {
             array_push($this->verificationErrors, $e->toString());
          }
       }
    }
    

With the plethora of features available, this blog post has hardly scratched the surface. Perhaps the purpose here is to showcase that these complimentary frameworks do indeed exist and they work seamlessly with their environments and that it is for us the PHP user group to leverage these to practice and implement TDD, refine our competence and try raise the quality benchmarks just a little bit.

   [1]: http://en.wikipedia.org/wiki/XUnit
   [2]: http://www.phpunit.de/manual/current/en/writing-tests-for-phpunit.html#writing-tests-for-phpunit.test-dependencies
   [3]: http://www.phpunit.de/manual/current/en/writing-tests-for-phpunit.html#writing-tests-for-phpunit.data-providers
   [4]: http://www.phpunit.de/manual/current/en/writing-tests-for-phpunit.html#writing-tests-for-phpunit.exceptions
   [5]: http://www.phpunit.de/manual/current/en/writing-tests-for-phpunit.html#writing-tests-for-phpunit.errors
   [6]: http://www.phpunit.de/manual/current/en/organizing-tests.html
   [7]: http://www.phpunit.de/manual/current/en/database.html
   [8]: http://www.phpunit.de/manual/current/en/other-uses-for-tests.html
   [9]: http://www.phpunit.de/manual/3.4/en/index.html
   [10]: http://xebee.xebia.in/wp-content/uploads/2010/02/Screenshot.png (Zend Framework App Folder Structure)

## Comments

**[SMMandar](#3431 "2010-12-02 14:33:29"):** THANX for ur example I want to set up phpunit for my application which is custom PHP framework. How can i integrate both of them? So that i can test functionlaities like user registeration and group creation from my controllers

