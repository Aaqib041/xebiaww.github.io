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

<p>Over the past few days I was going through the Zend Framework reference docs and I found myself pleasantly surprised with all that the latest version of this web application framework provides. My first thought was to just acknowledge the speed in which PHP as a technology has been maturing. Out of the many new features, what stood out for me was the ease with which Zend Framework and PHPUnit complement and work with each other.
<!--more-->
Now I've worked with PHPUnit enough to know how powerful, customizable and easy to use it can be (primarily as it belongs to <a href="http://en.wikipedia.org/wiki/XUnit">xUnit family of testing frameworks</a>) but what is really heartening is to see it perfectly complement the Zend Framework (which IMHO should be the default choice for any PHP project, but that's another blog post..).</p>
<p>Here's what we know about PHPUnit: you can not only unit test your PHP classes but let's you <a href="http://www.phpunit.de/manual/current/en/writing-tests-for-phpunit.html#writing-tests-for-phpunit.test-dependencies">create dependencies</a> between tests, <a href="http://www.phpunit.de/manual/current/en/writing-tests-for-phpunit.html#writing-tests-for-phpunit.data-providers">use dataProviders</a> for expanding the sample size for test case arguments, test for <a href="http://www.phpunit.de/manual/current/en/writing-tests-for-phpunit.html#writing-tests-for-phpunit.exceptions">exceptions</a> and <a href="http://www.phpunit.de/manual/current/en/writing-tests-for-phpunit.html#writing-tests-for-phpunit.errors">errors</a>.</p>
<p>Basics aside PHPUnit also lets you <a href="http://www.phpunit.de/manual/current/en/organizing-tests.html">organize your tests</a>, <a href="http://www.phpunit.de/manual/current/en/database.html">provides for database testing</a>, test doubles, provides for code coverage, and can even be used for <a href="http://www.phpunit.de/manual/current/en/other-uses-for-tests.html">agile documentation</a>. To top it off, PHPUnit plays nicely with Selenium, Apache Ant, Apache Maven, Phing, Atlassian Bamboo, CruiseControl etc. so it should fit into a good percentage of project development environments.</p>
<p>I'll do myself a favor and point to the <a href="http://www.phpunit.de/manual/3.4/en/index.html">PHPUnit Documentation</a> and get back to what this post was about.</p>
<p><img class="alignleft size-full wp-image-3041" title="Zend Framework App Folder Structure" src="http://xebee.xebia.in/wp-content/uploads/2010/02/Screenshot.png" alt="Zend Framework App Folder Structure" width="182" height="176" />As clearly depicted in the Zend Framework application project folder structure, test driven development had always been given a high priority by the authors of the framework. In fact much of the official docs and other reference material / literature stress quite strongly for the adoption of these practices and generally claim it to be the the <em>"Zend way of PHP development"</em>. </p>
<p>Coming to specifics, in the Zend_Test package we find PHPUnit extended to give us Zend_Test_PHPUnit and Zend_Test_PHPUnit_Db.</p>
<p><strong>Zend_Test_PHPUnit</strong> provides classes that can be used to test the MVC classes of the framework so a typical situation would be that the test for a controller will extend Zend_Test_PHPUnit_ControllerTestCase. Find below an example of a controller test (courtesy the official docs):</p>
<pre lang="php">
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
</pre>

<p><strong>Zend_Test_PHPUnit_Db</strong> extends the PHPUnit database extension with Zend Framework specific code such that writing database tests becomes much simpler as persistence gets pre-handled. Find below an example of a database test (courtesy the official docs):</p>
<pre lang="php">
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
</pre>

<hr />
<p>Once your web application is closer to a release, Zend Framework + PHPUnit's support for functional testing on Selenium IDE comes handy as well. These are tests that broadly mimic end user behavior and are specific to testing areas such as usability, security and performance.</p>
<p>A typical Selenium test would extend <strong>PHPUnit_Extensions_SeleniumTestCase</strong> and would look like this:
<pre lang="php">
&lt;?php
require_once 'PHPUnit/Extensions/SeleniumTestCase.php';
class seleniumExampleTest extends PHPUnit_Extensions_SeleniumTestCase
{
   protected function setUp()
   { 
      $this-&gt;setBrowser('*firefox');
      $this-&gt;setBrowserUrl('http://www.google.com.au/');
   }
   function testMyTestCase()
   {
      $this-&gt;open('http://www.google.com.au/');
      $this-&gt;type('q', 'zend framework');
      $this-&gt;click('btnG');
      $this-&gt;waitForPageToLoad('30000');
      try {
         $this-&gt;assertTrue($this-&gt;isTextPresent('framework.zend.com/'));
      } catch (PHPUnit_Framework_AssertionFailedError $e) {
         array_push($this-&gt;verificationErrors, $e-&gt;toString());
      }
   }
}
</pre></p>
<p>With the plethora of features available, this blog post has hardly scratched the surface. Perhaps the purpose here is to showcase that these complimentary frameworks do indeed exist and they work seamlessly with their environments and that it is for us the PHP user group to leverage these to practice and implement TDD, refine our competence and try raise the quality benchmarks just a little bit.</p>

## Comments

**[SMMandar](#3431 "2010-12-02 14:33:29"):** THANX for ur example I want to set up phpunit for my application which is custom PHP framework. How can i integrate both of them? So that i can test functionlaities like user registeration and group creation from my controllers

