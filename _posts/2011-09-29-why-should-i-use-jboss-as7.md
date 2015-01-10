---
layout: post
header-img: img/default-blog-pic.jpg
---

# Why should I use JBoss AS7

In the last few months we have seen some aggressive marketing of JBoss AS 7 by RedHat.  Red Hat has been praising its perfromance, parallelism, jee6 compatibility and other deployment issues. So I decided to explore the new application server and verify the truth behind them.

After [downloading ](http://www.jboss.org/jbossas/downloads/)the app server I configured it with eclipse using [M2Eclipse ](http://www.eclipse.org/m2e/)plugin, the details of how to configure it and deploy the quickstarts is given [here](https://docs.jboss.org/author/display/AS7/Getting+Started+Developing+Applications+Guide). The [homePage ](http://www.jboss.org/as7.html)itself looks impressive to begin with and documentation is very good (for a change).

Below is my experience and analysis over the claims of the new JBoss AS7.  **1\. Blazingly fast -** Well for the standalone part when there are no deployments it did start in less than 2 seconds. But as and when I started loading bulky applications it started taking little time. But yes, thanks to the new Module service Container, time taken was still less than 4 seconds.

Let’s ponder on the reason behind this drastic performance change from its predecessors-

One of the main and perhaps most grueling tasks for an application server is managing services. The services are not isolated and they are interdependent. When an application server starts or deploys a service, it has to make sure that it starts all the services in the correct order. And if some service is stopped then all services, which depend on this service, must stop in the reverse order.

This all looks very chaotic when there are multiple threads doing the task. So the developers of AS7 decided to make a new service container from scratch: JBoss Modular Service Container. Instead of starting the services in a hierarchical manner now they get started concurrently and only the critical ones are started first. This seems to have worked for them.

**2\. No more classNotFound / NoClassDefFound errors** :I have yet to encounter these problems in the new app server. This app server seems to do the class-loading in a sane manner, and hence we see “Jar-Hell” no more. [JBoss Modules](https://docs.jboss.org/author/display/MODULES/Home) achieves the goal of modularity and concurrent class loading by segmenting classes into proper modules, the concept that is very similar to OSGI.

**3\. Hot, parallel deployment -** I, then,  deployed bulkier applications – Added EJB3, JPA, JTA, hibernate . But to my surprise it was indeed fast. Not only the start-up but the deployments also happened in less than 2 seconds! So far so good.

**4\. Fully Compatible JEE6 profile-** I tried some of the features of JEE6 like CDI, JPA, EJB3, JTA, Hibernate,Servlet 3 API and not faced any issue. It certainly looks fully Java EE 6 compatible server. So for Java EE 6 components it’s a Thumbs Up!  Only other app server I can think of is Glass Fish which is fully JEE6 compatible.

**5\. Automatic dependency resolution -** One of the best things I liked about Jboss AS7 was implicit and automatic dependency resolution. For example, If you are using CDI and have a beans.xml in your module, then the cdi jar would be automatically be there you need not define it in neither in your POM or in any other dependency.xml.

**6\. New Improved deployment scanner-** Another change that I observed was the deployment scanner does not work the same as the old one. If you have an exploded directory you need to suffix .dodeploy with it. But still if you are deploying WARs/EARs and you do not want to add the extra marker you can change it in the standalone.xml by changing the "auto-deploy" attributes on the deployment-scanner element in the standalone.xml configuration file.

More details can be found in the deployments folder README.

**7\. Admin console GUI –** This still looks very basic although better than its previous counterparts. By default if you do http://localhost:9990/console you will land up on a very basic page which just shows the deployments, data sources, logging etc.

Go to /standalone/configuration/mgmt-users.properties and you will see all the story about how to configure the port , how to put a security realm, these changes go in standalone.xml

Managing the Jboss Server :I still think the previous JMX-Console had a lot of power to see and manage all the Mbeans and I am still trying to find how I can do things now in the new console.

**8\. Integration testing -** One can use [Arquillian ](http://www.jboss.org/arquillian/about.html)for executing the test cases inside the real runtime environment. It abstracts away all container lifecycle and deployment from the test cases business logic. This can be a very convenient tool for TDD.

**9\. Support for Java 7 –** Yes AS7 is fully compatible if you use JDK 7 to make your application.

Well So far it looks great, less startup time, fast deployments, no”Jar-Hell”, implicit dependency resolution in a fully compatible JEE6 Web Profile server. Better and smarter than its predecessors with fantastic supporting documentation.  The next task would be to to move my application from AS5 to AS7.  Let’s see how easier it would be to migrate to this new avatar. Keep looking at this place for more details in time to come.

## Comments

**[Grzegorz Grzybek](#5959 "2011-09-30 10:21:10"):** Fully agree! I had very bad experience with JBoss 5 and 6 after very good experience with JBoss 3 and 4. Now they're back on track! JBoss Modules is clean and elegant solution and gives developers what they need - full control! It was very good move to rewrite JBoss from scratch and to finally get rid of UCLs... regards Grzegorz Grzybek

**[Robert Niestroj](#5962 "2011-09-30 22:54:12"):** Hi, good article. If you would write an article about migrating a Spring3, Hibernate, JTA, Wicket Application from JBOSS 5 to 7, that would be very nice because that's what im trying to achieve, and cant do. Im so far that i had a lot of classNotFound. The solution was to put all the JARs i had in my lib folder in JBOSS5 directly to to WAR to the WEB-INF/lib. But that bloats my WAR and it would take i think much longer to build the WAR so i would lose the speed i gathered. See: http://stackoverflow.com/questions/7560990/jboss-7-spring-contextloaderlistener-classnotfoundexception I will keep lokking here :)

**[ram krishna](#5963 "2011-09-30 22:55:18"):** good one...

**[Rick](#5964 "2011-09-30 23:15:38"):** Thanks for the nice writeup. Rick

**[James Baxter](#5965 "2011-10-01 02:23:49"):** You missed the Management CLI! It's pretty awesome; it gives you full control of the server configuration at runtime (everything in standalone.xml). Best of all its very 'discoverable' due to its tab completion functionality. http://community.jboss.org/wiki/CommandLineInterface

**[Anirudh Bhatnagar](#5974 "2011-10-03 18:42:31"):** I have posted a reply on stack-overflow, will write a more detailed blog in some time on this. Meanwhile you can read more details here as well: http://blog.xebia.com/2011/07/developing-a-jpa-application-on-jboss-as-7/

**[Anirudh Bhatnagar](#5975 "2011-10-03 18:43:05"):** Hi James, CLI looks great but I haven’t actually tried it yet.. will give more feedback once I explore it. Thanks, Anirudh

