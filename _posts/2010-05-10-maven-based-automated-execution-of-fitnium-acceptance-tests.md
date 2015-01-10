---
layout: post
header-img: img/default-blog-pic.jpg
author: ykapoor
description: 
post_id: 3464
created: 2010/05/10 16:37:09
created_gmt: 2010/05/10 11:37:09
comment_status: open
---

# Maven Based Automated Execution of FitNium Acceptance Tests 

### **Intent**

** **To automate execution of the FitNium acceptance tests and generate reports using maven. This is particularly useful in test driven and continuous integration environment. 

### **Motivation**

In one** **of the recent projects we wanted to integrate FitNium acceptance tests with maven based build system.We were using FitNium as it allows us to** **write and execute Selenium tests using the FitNesse framework but without the need to write a single line of code.

We had created the FitNium test script to test our application. FitNium test script is based on FitNesse doFixture and provides a English language interpretation of the Selenium API.

As part of the maven build system we wanted

  * Automatic start up of the servers (Application server, Selenium and FitNesse)

  * Execution of the acceptance tests along with the report generation

  * Stopping the servers

How we accomplished this forms the basis of this post.

**Implementation Details **

The tools we are going to use are:

**Selenium**: Used to automate web application testing **FitNesse**: Collaborative tool to write acceptance tests **FitNium**: FitNesse + Selenium **Cargo**: For deploying applications to containers from ant or maven builds **Maven**: Build tool

Assumption here is that the FitNium tests have been created, so we can concentrate on automating the execution of the FitNium tests.

Let’s start with a very simple FitNium test script to serve as an example

[sourcecode language="css"] !path fitnesse.jar !path fitlibrary.jar !path fitnium.jar !path selenium-java-client-driver.jar

!|com.magneticreason.fitnium.BaseFitniumFixture|

Set up the browser and connect to selenium

|The server is located at | localhost | |The server is on port | 9090 | |Using the browser | *firefox | start at | http://localhost:8080/testApp/pages/login.seam | |check | is selenium initialised | true | |set speed to |50| milliseconds | |set timeout to |50000| milliseconds | |write to debug | starting test |

Lets try to login

|starting at URL| http://localhost:8080/testApp/pages/login.seam| | enter | admin | in input field | name=home:username_input| | enter | admin | in input field | name=home:password_input| | check | does button With Id | !-home:LoginBtn-! | exist | true | |click button | !-home:LoginBtn-! | |wait For Page To Load For | 5 | seconds | |write to debug | test complete | |close the browser| [/sourcecode]

Some things worth noting in the FitNium test script above are 

  1. **!path** <jar> notation refers to the FitNesse class path
  2. **_The server is on port_** should have the port configured for the selenium server. For me it’s 9090
  3. **_Starting at URL_** should point to the URL your application test should start at
Now with the knowledge of the required tools and the test script we will get onto the steps for automating the execution of FitNium tests. There are mainly five things involved in the whole process 
  1. Installing the application to be tested
  2. Starting the application server
  3. Starting the Selenium and FitNesse server
  4. Running the tests
  5. Stopping the servers started in step 1, 2 and 3

#### Step 1: Installing the application:

** **

This can be accomplished using **[cargo-maven2-plugin][1].**

The pom entry for the plug-in will look like below

[sourcecode lang="xml"] <plugin> <groupId>org.codehaus.cargo</groupId> <artifactId>cargo-maven2-plugin</artifactId> <version>1.0</version> <configuration> <wait>false</wait> <container> <containerId>jboss5x</containerId> <home>${JBOSS_HOME}</home> <timeout>300000</timeout> <systemProperties> <!-- This is used in jboss-log4j.xml --> <jboss.server.log.threshold>INFO</jboss.server.log.threshold> </systemProperties> </container> <configuration> <type>existing</type> <home>${JBOSS_HOME}\server\default</home> <properties> <cargo.rmi.port>1099</cargo.rmi.port> <cargo.jvmargs>-XX:PermSize=256m -XX:MaxPermSize=512m </cargo.jvmargs> </properties> </configuration> </configuration> </plugin> [/sourcecode]

I am using JBoss-5.1 application server to which the application is deployed.

The _**containerId **_is set to the appropriate application server. The _**home **_refers to the application server directory. This property is picked from the environment property in the snippet above. The configuration here is set to _**existing **_i.e. the application is deployed to a running server _**home **_element within the configuration refers to server profile to which the application is installed. You can find more information on cargo plug-in configuration [here][2]

#### Step 2: Starting the application server

We would want the application server to be started in the pre-integration-test phase and stopped in the post-integration-test phase. This can be directly done by giving execution details for the cargo plug-in as below

[sourcecode language="xml"] <plugin> <groupId>org.codehaus.cargo</groupId> <artifactId>cargo-maven2-plugin</artifactId> <executions> <execution> <id>start-container</id> <phase>pre-integration-test</phase> <goals> <goal>start</goal> </goals> </execution> <execution> <id>stop-container</id> <phase>post-integration-test</phase> <goals> <goal>stop</goal> </goals> </execution> </executions> </plugin> [/sourcecode] 

#### Step 3:  Starting the Selenium and FitNesse servers

The next step is to automatically start Selenium and FitNesse servers. This can easily be accomplished using maven-antrun-plugin and defining execution to start the two in pre-integration-test phase

[sourcecode language="xml"] <profile> <id>fitnium</id> <properties> <fitnium.project>${project.parent.basedir}/testApp-fitnium</fitnium.project> </properties> <build> <plugins> <plugin> <artifactId>maven-antrun-plugin</artifactId> <execution> <id>start-selenium-and-fitnesse</id> <phase>pre-integration-test</phase> <configuration> <tasks> <echo taskname="selenium" message="Starting…" /> <java jar="${fitnium.project}/selenium-server-1.0/selenium-server.jar" fork="yes" spawn="true"> <arg value="-port" /> <arg value="9090" /> </java> <echo taskname="fitnesse" message="Starting…" /> <java classname="fitnesseMain.FitNesseMain" classpath="${fitnium.project}/fitnesse.jar;${fitnium.project}/lib/json.jar" fork="yes" spawn="true"> <arg value="-p" /> <arg value="7000" /> <arg value="-d" /> <arg value="${fitnium.project}" /> </java> </tasks> </configuration> <goals> <goal>run</goal> </goals> </execution> </plugins> </build> </profile> [/sourcecode]

Make sure that the path to the jars is set correctly as per your environment for the java tasks. The attributes for the selenium tasks are

**Port**: The port at which the selenium server should listen at. Make sure this matches the port mentioned in the FitNium test script described earlier.

And for the FitNesse are:

   [1]: http://cargo.codehaus.org/Downloads
   [2]: http://cargo.codehaus.org/Maven2+plugin

## Comments

**[abdurrahman sahin](#5305 "2011-02-17 13:48:38"):** I really liked your path to implement a complete test automation. great work.

