---
layout: post
header-img: img/default-blog-pic.jpg
---

# Deploying a Spring Hibernate MySQL Application on OpenShift Flex (RedHat PaaS Solution) 

Last XKE me and Sameer started a sandpit to evaluate the capabilities of [OpenShift](https://openshift.redhat.com/app/) and see how easy or difficult will be to deploy an existing or new Java applications on OpenShift . OpenShift is a multi-language(PHP, Ruby, Python, Java), multi-framework [Platform as a Service](http://en.wikipedia.org/wiki/Platform_as_a_service) provided by [RedHat](http://www.redhat.com/) with the goal to help developers build application without thinking much about infrastructure . It is available in three editions : 

  1. **Express** : Express is a free PaaS for building and deploying  Ruby, PHP, and Python applications. Express now supports Java as well. When this article was written Java was not supported.
  2. **Flex** : Flex is a Paas for building and deploying web applications written in Java, JavaEE 6, or PHP . Flex also allows you to manage, monitor, and auto-scale applications from web interface. You need Amazon EC2 account to use Flex.
  3. **Power** : Power as the name suggests is the most powerful edition of OpenShift and can run any application written in any language on it. Power gives you the ultimate flexibility and access at the operating system configuration level.

We decided to choose Flex edition as we wanted to deploy a standard Java Spring Hibernate MySQL application on OpenShift. Deploying an existing MySQL application on OpenShift was not straightforward so I am writing this blog to help other Java developers facing trouble deploying application on OpenShift. There was no documentation on how you can deploy a Java web application with MySQL as database.

In this blog will walk you step by step how you can deploy a Spring Hibernate MySQL application on OpenShift. The application server that we will be choosing for this application will be Tomcat. Flex support both JBoss and Tomcat server.

### Let's get started

  1. [Create your account](https://openshift.redhat.com/app/user/new/flex) for accessing Flex console.
  2. Once you have created your account log in to [Flex console](https://openshift.redhat.com/flex/flex/index.html). Flex Console is the web interface which allows you to create, deploy, manage, or monitor your application.
  3. Before we start using Flex Console lets create a simple Spring Hibernate MySQL application using Spring Roo (If you don't know about Spring Roo please refer to [IBM DeveloperWorks](http://www.ibm.com/developerworks/java/library/os-springroo1/?ca=drs-) [articles](http://www.ibm.com/developerworks/java/library/os-springroo2/?ca=drs-) ). Start the Roo shell and fire these commands [sourcecode lang="text"] project --topLevelPackage com.openshift.demo --projectName conference persistence setup --database MYSQL --provider HIBERNATE --userName conference --password conference --databaseName conference entity --class ~.domain.Speaker --testAutomatically field string --fieldName fullName --notNull field string --fieldName email --notNull field string --fieldName bio --notNull --sizeMax 4000 entity --class ~.domain.Talk --testAutomatically field string --fieldName title --notNull field string --fieldName description --notNull --sizeMax 4000 field reference --type ~.domain.Speaker --fieldName speaker --notNull --class ~.domain.Talk field set --fieldName talks --type ~.domain.Talk --class ~.domain.Speaker --cardinality ONE_TO_MANY controller all --package ~.web [/sourcecode] These commands will create a simple conference application which will let you create Speakers and Talks.You cannot create a talk unless you have created a Speaker for it. You can test it locally by creating a mysql database named conference and creating a user with username and password as conference. You can run the application by typing mvn clean install tomcat:run. Rename the war to conference.war as this name will in the application url.
  4. After you have logged in to OpenShift Flex console, the first thing that you need to do is add a cloud account. Please click on the CLOUDS tab and you will see a screen as shown below. Currently only Amazon EC2 is supported but I think plan is to add support for multiple cloud platforms. You can give any name to account nickname field but you have to provide valid Amazon EC2 credentials to work with OpenShift. ![](http://whyjava.files.wordpress.com/2011/07/clouds.png?w=294)
  5. Next you need to define a server cluster by clicking CLUSTERS tab. This will open up form to create a new Server Cluster. The field names are self-explanatory. Give server cluster some meaningful name, choose the cloud account you created in step 4, choose a location and for rest keep the default. It will take some time to create a cluster so please be patient.
  6. After few minutes you will be able to see a cluster under CLUSTERS tab and a server under SERVERS tab. **Please note that it will start large Amazon EC2 instance. **
  7. Once the cluster and server have started we can deploy our conference application. Click on the APPLICATIONS tab and then click on ADD APPLICATION button. This will take you to the new application form.Please enter the details as shown below and press submit button and wait for few seconds. ![](http://whyjava.files.wordpress.com/2011/07/new-application.png?w=221)
  8. This will take you to the application overview page as shown below. This page shows different tabs like overview, components, files, configure, and deploy changes. The only information that we will need from overview tab is the CLUSTER DNS conference14486738.prod.rhcloud.com. This is used to create the URL of the application by appending the application context name. In our case it will be conference. ![](http://whyjava.files.wordpress.com/2011/07/application-overview.png?w=300)
  9. So far we have not defined the components of the application and we have also not uploaded the web application. Lets first add components by clicking the component tab under the conference application tab as shown below. I have used application server as Apace Tomcat and MySQL as database. There are other components like Zend, Memcached, and MongoDB also supported but we are not using them in this application. ![](http://whyjava.files.wordpress.com/2011/07/application-components.png?w=300)
  10. After you press save button, on the DEPLOY CHANGES tab you will see that it shows that you have some files to deploy. These are configuration files which are created when we added components to our application. For now leave them as it is.
  11. Next press the FILES tab to upload the conference war. Once you have uploaded the war you will see conference.war in exploded form as shown below ![](http://whyjava.files.wordpress.com/2011/07/files.png?w=300)
  12. Apart from the conference.war file you have to also upload a sql file which will create the database and user for the application. Create a file named conference.sql and upload to bundle directory. [sourcecode lang="sql"] CREATE DATABASE if not exists conference; GRANT USAGE ON conference.* TO 'conference'@'localhost';

## Comments

**[Gaurav Marwaha](#5720 "2011-07-15 21:07:27"):** Can you deploy from Eclipse/ IDE which is what cloudfoundry allows so you do not have to leave the IDE to deploy? Because other than using Roo to generate the app the tutorial is using OpenShift tools to deploy an app see: http://drorbr.blogspot.com/2011/05/openshift-flex-part-2-create-deplot-and.html

**[PeterPatrickGo](#5682 "2011-07-07 14:32:31"):** Great post.I just have this problem on hand now.Really tks for your post

**[web hosting review](#5705 "2011-07-11 10:15:33"):** Thanks for sharing this blog It is very helpful for me ,as a java programmer it will heart topic of this blog content Deploying a Spring Hibernate MySQL Application on OpenShift Flex (RedHat PaaS Solution)

**[samy](#6001 "2011-10-09 20:40:44"):** openshift express is used for both PHP and Java as will. So I don know why you told that Flex only support Java ?? Please recheck and update the article.

