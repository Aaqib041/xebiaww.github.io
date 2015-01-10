---
layout: post
header-img: img/default-blog-pic.jpg
author: usinha
description: 
post_id: 15252
created: 2012/10/31 23:41:40
created_gmt: 2012/10/31 18:41:40
comment_status: open
---

# Monkey with ANT (Mobile App Automated Tesing - II)

<p>Continuing from where we halted in my previous post <a href="http://xebee.xebia.in/2012/09/18/mobile-app-automation-testing/">'When Monkeys Talk'</a>, in this blog I will talk about Integrating the existing Monkey Talk Script with ANT, Running with ANT, Extending Test with JavaScript, Executing a MT JS file.</p>
<p>MonkeyTalk scripts can be run via <a href="http://ant.apache.org/">Ant</a> from the commandline, and Ant can be easily integrated into a CI environment. The combination of an Ant runner for MonkeyTalk scripts, plus a CI server can provide a fully automated testing solution.</p>
<p>The prerequisite would be to install ANT on your macbine, one can follow simple steps (easily available on web) to do the need full.Once done we would need to perform the following.</p>
<p><span style="text-decoration: underline"><strong>Installing Monkey Talk ANT Tasks </strong></span></p>
<p>Copy monkeytalk-ant.jar into your Ant lib/ folder. Then you only need to reference it with a namespace as given below in the build.xml:</p>
<p><i>&lt;project xmlns:monkeytalk="antlib:com.gorillalogic.monkeytalk.ant"&gt;</i></p>
<p><i> ...</i></p>
<p><i>&lt;/project&gt;</i>
<h3><span style="text-decoration: underline"><strong><!--more--></strong></span></h3>
<h3><span style="text-decoration: underline"><strong>Writing a build.xml</strong></span></h3>
<div></p>
<p>Once you have monkeytalk-ant.jar in place, you can add targets and run monkeytalk commands.</p>
<p><b>Parameters – Build .xml</b>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td><b>Attribute</b></td>
<td><b>Description</b></td>
<td><b>Required?</b></td>
</tr>
<tr>
<td>agent</td>
<td>Either   "Android" or "iOS"</td>
<td>Yes</td>
</tr>
<tr>
<td>adb</td>
<td>Complete path to   ADB within your local Android SDK. For example,   adb="/Users/tori/DevTools/android-sdk-macosx/platform-tools/adb"</td>
<td>Yes if the agent   is Android, no otherwise</td>
</tr>
<tr>
<td>script</td>
<td>Local path to .mt   script you wish to run.</td>
<td>Either   "script", "suite", or inline script</td>
</tr>
<tr>
<td>suite</td>
<td>Local path to   .mts suite you wish to run.</td>
<td>Either   "script", "suite", or inline script</td>
</tr>
<tr>
<td>host</td>
<td>Set as   "localhost" if connecting to emulator/simulator or tethered device;   or the device's IP address if connecting to networked device. Defaults to   localhost.</td>
<td>No</td>
</tr>
<tr>
<td>port</td>
<td>By default,   iOS port is set to 16863, Android port is set to 16862</td>
<td>No</td>
</tr>
<tr>
<td>reportdir</td>
<td>Directory to log   JUnit xml test output from a suite. If not specified, defaults to current   directory.</td>
<td>No</td>
</tr>
</tbody>
</table>
</div>
Here is a simple example:
<pre>&lt;project name="Test" default="testSuiteOnAndroidDevice"
  xmlns:monkeytalk="antlib:com.gorillalogic.monkeytalk.ant"&gt;
  &lt;property environment="env" /&gt;
  &lt;property name="version" value="1.0.4-SNAPSHOT" /&gt;
  &lt;property name="adb" value="/Users/tgilbertson/DevelopmentTools/android-sdk-macosx/platform-tools/adb" /&gt;
  &lt;property name="host" value="0.0.0.0" /&gt;
  &lt;path id="monkeytalk.classpath"
     path="${env.M2_REPO}/com/gorillalogic/monkeytalk/monkeytalk-ant/${version}/monkeytalk-ant-${version}.jar" /&gt;
  &lt;typedef resource="com/gorillalogic/monkeytalk/ant/antlib.xml"
      uri="antlib:com.gorillalogic.monkeytalk.ant" classpathref="monkeytalk.classpath" /&gt;
  &lt;target name="testScriptOniOSSimulator"&gt;
  &lt;monkeytalk:run agent="iOS" &gt;
      Input username EnterText "Bo Bo" 
      Input password EnterText password
      Button LOGIN Tap
      Button LOGOUT Verify %timeout=3000
      Label * Verify "Welcome, Bo Bo!"
      Button LOGOUT Tap
  &lt;/monkeytalk:run&gt;
  &lt;/target&gt;
  &lt;target name="testSuiteOnAndroidDevice"&gt;
      &lt;monkeytalk:run agent="android" adb="${adb}"
        suite="suite.mts" host="${host}" reportdir="reports" /&gt;
  &lt;/target&gt;
  &lt;target name="debug"&gt;
  &lt;property name="debug" refid="monkeytalk.classpath" /&gt;
  &lt;echo message="debug=${debug}" /&gt;
  &lt;/target&gt;
&lt;/project&gt;</pre>
<span style="text-decoration: underline"><strong>
</strong></span></p>
<p><span style="text-decoration: underline"><strong>Running with ANT</strong></span></p>
<p>To run the test cases open the cmd.</p>
<p>Navigate to the location where the build.xml exists.</p>
<p>Run ant testScriptOnAndroidDevice.</p>
<p>You don't need the IDE to run MonkeyTalk ant.</p>
<p><span style="text-decoration: underline"><strong>Extending Tests with JavaScript</strong></span></p>
<p>If you want to include common programming structures like flow control, loops, and random number generators, then exporting to JavaScript is a good way to do so.</p>
<p>To get started with exporting to JavaScript, click on the JavaScript tab at the bottom of a Monkey Talk script. You should see a screen that's similar to this:</p>
<p><a rel="attachment wp-att-15266" href="http://xebee.xebia.in/2012/10/31/mobile-app-automation-testing-2/screen-shot-2012-11-01-at-12-00-50-am-2/"><img class="aligncenter size-medium wp-image-15266" height="137" width="300" src="http://xebee.xebia.in/wp-content/uploads/2012/10/Screen-Shot-2012-11-01-at-12.00.50-AM1-300x137.png" /></a></p>
<p>Click "Export". Once you do so, the MonkeyTalk IDE will generate a new .js file that does the same thing as your MonkeyTalk script.</p>
<p><a rel="attachment wp-att-15267" href="http://xebee.xebia.in/2012/10/31/mobile-app-automation-testing-2/screen-shot-2012-11-01-at-12-02-50-am/"><img class="aligncenter size-medium wp-image-15267" height="150" width="300" src="http://xebee.xebia.in/wp-content/uploads/2012/10/Screen-Shot-2012-11-01-at-12.02.50-AM-300x150.png" /></a></p>
<p><span style="text-decoration: underline"><strong>Executing a Monkey Talk JS file</strong></span></p>
<p>To run a monkey Talk JS we will have to create a wrapper, this wrapper will contain the command ‘<i>Script “testcase_name” Run</i>’.</p>
<p>The wrapper will itself be a mt file which can be run directly from ANT as mentioned in ‘<b>Point 7</b>’</p>
<p>For example, if we create a file, login.mt, and export it to JavaScript, login.js, we can run the JavaScript file like this:</p>
<p>LoginWrapper.mt
<table border="1" cellspacing="0" cellpadding="0" style="width: 150px">
<tbody>
<tr>
<td>Script login Run</td>
</tr>
</tbody>
</table>
<b>Note</b> that if we call "login" as opposed to "login.mt" or "login.js", it will default to running login.js.</p>
<p>Let's say we started out with this MT script.</p>
<p>Login.mt
<table border="1" cellspacing="0" cellpadding="0" style="width: 201px">
<tbody>
<tr>
<td>
<pre>Vars * Define usr=default@example.com pwd
Input username EnterText ${usr}
Input password EnterText ${pwd}
Button LOGIN Tap
Button LOGOUT Tap %timeout=3000 %thinktime=5000</pre>
</td>
</tr>
</tbody>
</table>
That script exports to this Javascript:</p>
<p>Login.js
<table border="1" cellspacing="0" cellpadding="0" style="width: 150px">
<tbody>
<tr>
<td>
<pre>if (typeof Test == "undefined") {
        load("libs/Test.js");
};</p>
<p>Test.Login.prototype.run = function(usr, pwd) {
        usr = usr || "default@example.com";
        pwd = pwd || null;</p>
<p>this.app.input("username").enterText(usr);
        this.app.input("password").enterText(pwd);
        this.app.button("LOGIN").tap();
        this.app.button("LOGOUT").tap();
};</pre>
</td>
</tr>
</tbody>
</table>
Now we can edit login.js to include a random string generator.
<table border="1" cellspacing="0" cellpadding="0" style="width: 150px">
<tbody>
<tr>
<td>
<pre>if (typeof Test == "undefined") {
        load("libs/Test.js");
};
Test.Login.prototype.run = function(usr, pwd) {
        usr = usr || randStr();
        pwd = pwd || randStr();</p>
<p>this.app.input("username").enterText(usr);
        this.app.input("password").enterText(pwd);
        this.app.button("LOGIN").tap();
        this.app.button("LOGOUT").tap();
};
function randStr()
{
    var text = "";
    var possible = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";</p>
<p>for( var i=0; i &lt; 5; i++ )</p>