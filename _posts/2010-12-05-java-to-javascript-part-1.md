---
layout: post
header-img: img/default-blog-pic.jpg
author: rrawat
description: 
post_id: 6451
created: 2010/12/05 20:41:10
created_gmt: 2010/12/05 15:41:10
comment_status: open
---

# Java to Javascript - Part 1

<p>A few years(before Web2.0) back a Java developer moving to Javascript would have missed a lot of things from his/her extreme programming bible (Unit tests,  Mocks, OO (not really)). In this blog i try to cover my experience of moving to Javascript development and a few new frameworks that bring <a title="use of good or best could trigger a debate on Xebia Tech list" href="#">good </a>practices to Javascript development. Almost everything is based out of jQuery world as that is what i am using, other frameworks like Prototype/YUI must be great but i have not worked on them.</p>
<p>For using jQuery i would say
<p style="text-align: center;">“You add $ to anything and it becomes possible.. literally”.</p>
<p style="text-align: center;"><!--more--></p>
<strong>Logging and Debugging</strong></p>
<p>First things first. Developer consoles in IE 8/ Chrome/ Firefox are great, you can log stuff to these consoles.Firebug console API is here so you could do a lot of things in firebug. We identified one performance related issue using by simply using the time function (Firebug Profiler’s output was too fine grained and did not point to exact code region). I do not use the debugger as our scripts are loaded dynamically so they do not help anyway. Sufficient logging always helps. If you really want to persist your logs, you could asynchronously post the logs to server.</p>
<p>[code language="Javascript"]</p>
<p>(function($){
  if(console) {
    $.log = function(obj) {
      console.log(obj);
    }
  }
})(jQuery);
[/code]</p>
<p>You can look at the <a href="http://getfirebug.com/wiki/index.php/Console_API">Firebug console API</a> for other methods available on the console.</p>
<p>Super cool alert: You wrap alert(JSON.stringify(obj)) as $.alert and see the object as inside out when you want (you will need json2.js).</p>
<p>You can also have a look at http://plugins.jquery.com/project/jQueryLog (which has nice templating feature as well).</p>
<p><strong>Dependency Injection</strong></p>
<p>Being a dynamic language this has never been a problem for Javascript as you could add /remove properties at will to an object or all of it’s <em>types</em>.
Following code shows a small IOC container implementation. The DI takes as config input and wires up the objects.  The _getClosure method on line #12 is a fix for strange problem i faced with the closures. It kind of showed me why anonymous inner classes would only access the final variables of the scope outside. I was using an inline function definition on line #12 and binding it to the dependency name, but it was always binding to the last value of the variable.</p>
<p>[code language="Javascript"]
com.framework.DIEngine = {
  _objCache: {},</p>
<p>init: function(config) {
    for (obj in config) {
      var instance = config[obj].instance;
      if (config[obj].dependencies) {
        for (depIdx in config[obj].dependencies) {
          var dep = config[obj].dependencies[depIdx];
          $.log(dep);
          $.log('bound to:' + dep);
          instance[dep] = this._getClosure(dep); // bind a function to ask from DIEngine
        }
      }
      this._objCache[obj] = instance; // store the instance
    }
  },</p>
<p>_getClosure: function(dep) {
    return function() {return com.framework.DIEngine.get(dep)};
  },</p>
<p>get: function(name) {
    $.log('Asked DI for:' + name);
    return this._objCache[name];
  }
};</p>
<p>[/code]</p>
<p><strong>Packaging</strong></p>
<p>You can mirror packages by creating objects inside a hierarchy. Your declarations automatically add new classes/objects to the namespace. By doing this you can rest assured that you are not conflicting with any other variables in some other library.</p>
<p>A sample namespace can be declared as below and then you define all your objects in the attached namespace.</p>
<p>[code language="Javascript"]
var com = {
  framework:{},
  app:{}
};</p>
<p>// define objects in namespace.
com.app.Mapper = {
// foo bar
}
[/code]</p>
<p><strong>Object Orientation</strong></p>
<p>One of the major things that a java developer misses is the absense of class hierarchies, it is hard to model the data without significant code duplication. Following articles describe how to make use of javascript’s object oriented nature. A simple approach is to maintain a registry of objects and then create new objects by extending registered objects. We had objects which would have a single instance( singletons) so it was easy. John Resig’s link below explains in detail how to implement object orientation. Code looks much structured and readable compared to prototype based approach mentioned in sitepoint article.
<ul>
    <li><a href="http://articles.sitepoint.com/article/oriented-programming-1" target="_blank">http://articles.sitepoint.com/article/oriented-programming-1</a></li>
    <li><a href="http://www.javascriptkit.com/javatutors/oopjs.shtml" target="_blank">http://www.javascriptkit.com/javatutors/oopjs.shtml</a></li>
    <li><a href="http://ejohn.org/blog/simple-javascript-inheritance/" target="_blank">http://ejohn.org/blog/simple-javascript-inheritance/</a></li>
</ul>
[code language="Javascript"]
var OO = {
  _ooRegistry: {},</p>
<p>register : function(subName, base, subDef) {
    if (!(typeof base == 'string') ) {
      subDef = base; // will this become global?
      base = {};
    } else {
      base = this._ooRegistry[base];
    }
    this._ooRegistry[subName] = $.extend({},base, subDef );
    this._ooRegistry[subName]._super = base;
  },
  get: function(cls) {
    return this._ooRegistry[cls];
  }
};</p>
<p>OO.register('baseClass', {
  hello: function() {
    $.log(&quot;Base class invoked.&quot;);
    return &quot;Namaskaar&quot;;
  }
});</p>
<p>OO.register('subClass','baseClass', {
  hello: function() {
    $.log(&quot;Sub class invoked.&quot;);
    $.log(&quot;Invoking super&quot;);
    this._super.hello();
    return &quot;Wazz up.&quot;;
  }
});</p>
<p>[/code]</p>
<p><strong>Unit Testing</strong></p>
<p>Let's now put the code above to test.  jQuery offers <a title="QUnit" href="http://docs.jquery.com/Qunit">QUnit</a> as the unit testing framework. Following code puts the DI code and the OO code to test. QUnit provides nice setup and teardown lifecycle methods as JUnit.</p>
<p>[code language="Javascript"]
// OO test
$(document).ready(function() {</p>
<p>module(&quot;OO tests.&quot;);</p>
<p>test(&quot;OO get test&quot;, function() {
    var base = OO.get('baseClass').hello();
    var sub = OO.get('subClass').hello();
    ok(base == 'Namaskaar', 'Base class object invoked.');
    ok(sub == 'Wazz up.', 'Sub class object invoked.');
  });
});</p>
<p>// DI test
$(document).ready(function() {</p>
<p>module(&quot;DI tests.&quot;, {
      setup: function() {
        com.framework.DIEngine.init({
            reducer: {instance:com.app.Reducer},
             mapper:{instance:com.app.Mapper},
             service:{instance:com.app.Service, dependencies: ['reducer', 'mapper']}
          });
      },
      teardown: function() {
        // tear down..
      }
    });</p>
<p>test(&quot;DI get test&quot;, function() {
    expect(2); // 2 assertions in the test
    var mapper = com.framework.DIEngine.get('mapper');
    ok(mapper &amp;&amp; typeof mapper == 'object', 'Mapper is defined  &amp; is an object.');
    ok(mapper.name == 'mapper', 'It is indeed mapper.');
  });
});
[/code]</p>
<p>QUnit can be integrated with the continuous integration systems which launch browsers and the collect test results from the DOM. QUnit documentation explains how this can be done. QUnit provides nice callbacks for various events in the test lifecycle. I find QUnit.done of particular interest as it can be used to inform you of test failures by posting back to a URL ( an app there could trigger a message to your team's instant messaging system).  Other callbacks can be used to collect the data on individual tests and then send it on 'done' callback.</p>
<p>In the next part of this blog i will try to cover
<ul>
    <li>Use of mocks in Javascript testing</li>
    <li>Dependency management</li>
    <li>Messaging ( events!!)</li>
    <li>The <em>this</em> mystery</li>
    <li>Deployment and code checks ( JSLint)</li>
</ul>
<strong>Even More</strong></p>