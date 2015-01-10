---
layout: post
header-img: img/default-blog-pic.jpg
---

# Monkey with ANT (Mobile App Automated Tesing - II)

Continuing from where we halted in my previous post ['When Monkeys Talk'](/2012/09/18/mobile-app-automation-testing/), in this blog I will talk about Integrating the existing Monkey Talk Script with ANT, Running with ANT, Extending Test with JavaScript, Executing a MT JS file. MonkeyTalk scripts can be run via [Ant](http://ant.apache.org/) from the commandline, and Ant can be easily integrated into a CI environment. The combination of an Ant runner for MonkeyTalk scripts, plus a CI server can provide a fully automated testing solution. The prerequisite would be to install ANT on your macbine, one can follow simple steps (easily available on web) to do the need full.Once done we would need to perform the following. **Installing Monkey Talk ANT Tasks ** Copy monkeytalk-ant.jar into your Ant lib/ folder. Then you only need to reference it with a namespace as given below in the build.xml: _<project xmlns:monkeytalk="antlib:com.gorillalogic.monkeytalk.ant">_ _ ..._ _</project>_

### ****

### **Writing a build.xml**

Once you have monkeytalk-ant.jar in place, you can add targets and run monkeytalk commands. **Parameters – Build .xml**

**Attribute**
**Description**
**Required?**

agent
Either "Android" or "iOS"
Yes

adb
Complete path to ADB within your local Android SDK. For example, adb="/Users/tori/DevTools/android-sdk-macosx/platform-tools/adb"
Yes if the agent is Android, no otherwise

script
Local path to .mt script you wish to run.
Either "script", "suite", or inline script

suite
Local path to .mts suite you wish to run.
Either "script", "suite", or inline script

host
Set as "localhost" if connecting to emulator/simulator or tethered device; or the device's IP address if connecting to networked device. Defaults to localhost.
No

port
By default, iOS port is set to 16863, Android port is set to 16862
No

reportdir
Directory to log JUnit xml test output from a suite. If not specified, defaults to current directory.
No

Here is a simple example: 
    
    
    <project name="Test" default="testSuiteOnAndroidDevice"
      xmlns:monkeytalk="antlib:com.gorillalogic.monkeytalk.ant">
      <property environment="env" />
      <property name="version" value="1.0.4-SNAPSHOT" />
      <property name="adb" value="/Users/tgilbertson/DevelopmentTools/android-sdk-macosx/platform-tools/adb" />
      <property name="host" value="0.0.0.0" />
      <path id="monkeytalk.classpath"
         path="${env.M2_REPO}/com/gorillalogic/monkeytalk/monkeytalk-ant/${version}/monkeytalk-ant-${version}.jar" />
      <typedef resource="com/gorillalogic/monkeytalk/ant/antlib.xml"
          uri="antlib:com.gorillalogic.monkeytalk.ant" classpathref="monkeytalk.classpath" />
      <target name="testScriptOniOSSimulator">
      <monkeytalk:run agent="iOS" >
          Input username EnterText "Bo Bo" 
          Input password EnterText password
          Button LOGIN Tap
          Button LOGOUT Verify %timeout=3000
          Label * Verify "Welcome, Bo Bo!"
          Button LOGOUT Tap
      </monkeytalk:run>
      </target>
      <target name="testSuiteOnAndroidDevice">
          <monkeytalk:run agent="android" adb="${adb}"
            suite="suite.mts" host="${host}" reportdir="reports" />
      </target>
      <target name="debug">
      <property name="debug" refid="monkeytalk.classpath" />
      <echo message="debug=${debug}" />
      </target>
    </project>

** ** **Running with ANT** To run the test cases open the cmd. Navigate to the location where the build.xml exists. Run ant testScriptOnAndroidDevice. You don't need the IDE to run MonkeyTalk ant. **Extending Tests with JavaScript** If you want to include common programming structures like flow control, loops, and random number generators, then exporting to JavaScript is a good way to do so. To get started with exporting to JavaScript, click on the JavaScript tab at the bottom of a Monkey Talk script. You should see a screen that's similar to this: ![](/wp-content/uploads/2012/10/Screen-Shot-2012-11-01-at-12.00.50-AM1-300x137.png) Click "Export". Once you do so, the MonkeyTalk IDE will generate a new .js file that does the same thing as your MonkeyTalk script. ![](http://xebee.xebia.in/wp-content/uploads/2012/10/Screen-Shot-2012-11-01-at-12.02.50-AM-300x150.png) **Executing a Monkey Talk JS file** To run a monkey Talk JS we will have to create a wrapper, this wrapper will contain the command ‘_Script “testcase_name” Run_’. The wrapper will itself be a mt file which can be run directly from ANT as mentioned in ‘**Point 7**’ For example, if we create a file, login.mt, and export it to JavaScript, login.js, we can run the JavaScript file like this: LoginWrapper.mt 

Script login Run
**Note** that if we call "login" as opposed to "login.mt" or "login.js", it will default to running login.js. Let's say we started out with this MT script. Login.mt 
    
    
    Vars * Define usr=default@example.com pwd
    Input username EnterText ${usr}
    Input password EnterText ${pwd}
    Button LOGIN Tap
    Button LOGOUT Tap %timeout=3000 %thinktime=5000

That script exports to this Javascript: Login.js 
    
    
    if (typeof Test == "undefined") {
            load("libs/Test.js");
    };
    
    Test.Login.prototype.run = function(usr, pwd) {
            usr = usr || "default@example.com";
            pwd = pwd || null;
    
            this.app.input("username").enterText(usr);
            this.app.input("password").enterText(pwd);
            this.app.button("LOGIN").tap();
            this.app.button("LOGOUT").tap();
    };

Now we can edit login.js to include a random string generator. 
    
    
    if (typeof Test == "undefined") {
            load("libs/Test.js");
    };
    Test.Login.prototype.run = function(usr, pwd) {
            usr = usr || randStr();
            pwd = pwd || randStr();
    
            this.app.input("username").enterText(usr);
            this.app.input("password").enterText(pwd);
            this.app.button("LOGIN").tap();
            this.app.button("LOGOUT").tap();
    };
    function randStr()
    {
        var text = "";
        var possible = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
    
        for( var i=0; i < 5; i++ )