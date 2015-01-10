---
layout: post
header-img: img/default-blog-pic.jpg
author: shekhargulati
description: 
post_id: 12349
created: 2012/02/27 22:21:04
created_gmt: 2012/02/27 17:21:04
comment_status: open
---

# Using Spring 3.1 Profiles To Achieve PaaS Portability

<p>Spring 3.1 release introduced a much awaited feature called Profiles which allows you to register different beans in different environments. In this blog we will take a look at how we can use profiles feature to run the same application without any code change in both OpenShift Express and Cloud Foundry.<!--more--></p>
<p><strong>Creating a sample Spring MongoDB Application</strong></p>
<p>I will use Spring Roo to create a simple bookshop application. Fire up the Roo shell and execute the following commands.</p>
<p>[sourcecode lang="text"]
project --topLevelPackage com.xebia.bookshop --projectName bookshop
mongo setup --databaseName bookshop --host localhost --port 27017
entity mongo --class ~.domain.Book --testAutomatically --identifierType org.bson.types.ObjectId
field string --fieldName title --notNull
field string --fieldName author --notNull
field number --type double --fieldName price --notNull
repository mongo --interface ~.repository.BookRepository --entity ~.domain.Book
service --interface ~.service.BookService
web mvc setup
web mvc all --package ~.web
[/sourcecode]</p>
<p>This will create a simple bookshop application. You can deploy this application on local tomcat using mvn tomcat:run. Please make sure you have MongoDB running on your local system.</p>
<p><strong>Deploying on OpenShift Express</strong></p>
<p>OpenShift Express supports two forms of deployment one is source code deployment which I showed in my last two blogs and other is war/binary deployment. In this blog I will show you how to deploy war to the Express cloud. Before we can deploy the application to OpenShift Express we first need to create bookshop application. To create the application execute the following command. You need rhc ruby gem to execute the command.</p>
<p>[sourcecode lang="text"]
rhc-create-app -l &lt;rhlogin&gt; -a bookshop -t jbossas-7.0
[/sourcecode]</p>
<p>It will prompt for the password. Now that you have created the application you should embed MongoDB cartridge in your application.</p>
<p>[sourcecode lang="text"]
rhc-ctl-app -l &lt;rhlogin&gt; -a bookshop -e add-mongodb-2.0
[/sourcecode]</p>
<p>Now that we have created the application and added the MongoDB cartridge we need to make change in our Spring application. We need to change applicationContext-mongo.xml file to have correct values for MongoDB factory. The values which exist are for application running on your local system. To change these values to OpenShift ones add a profile as shown below.</p>
<p>[sourcecode lang="xml"]
&lt;beans profile=&quot;openshift&quot;&gt;
 &lt;mongo:db-factory dbname=&quot;${mongo.name}&quot; host=&quot;${OPENSHIFT_NOSQL_DB_HOST}&quot;
 port=&quot;${OPENSHIFT_NOSQL_DB_PORT}&quot; username=&quot;${OPENSHIFT_NOSQL_DB_USERNAME}&quot;
 password=&quot;${OPENSHIFT_NOSQL_DB_PASSWORD}&quot; /&gt;
 &lt;/beans&gt;
[/sourcecode]</p>
<p>You also need to mention one context-param in web.xml to tell which profile is active. To do it add the following context-param.</p>
<p>[sourcecode lang="xml"]
   &lt;context-param&gt;
        &lt;param-name&gt;spring.profiles.active&lt;/param-name&gt;
        &lt;param-value&gt;openshift&lt;/param-value&gt;
    &lt;/context-param&gt;</p>
<p>[/sourcecode]</p>
<p>Now build the project using mvn clean install -Dtest=maven.test.skip=true and copy to war file to deployments folder in OpenShift bookshop application. Also change the name of war to ROOT.war otherwise you application will be available at url_of_application/bookshop.
Now to deploy do the following in deployments folder.</p>
<p>[sourcecode lang="text"]
git add .
git commit -a -m &quot;adding bookshop application&quot;
git push
[/sourcecode]</p>
<p>Wait for application to get deployed and see your application running at <a href="http://bookshop-random.rhcloud.com/">http://bookshop-random.rhcloud.com/</a></p>
<p><strong>Deploying to Cloud Foundry</strong></p>
<p>To deploy the bookshop application to Cloud Foundry we just need to add one more profile as shown below.</p>
<p>[sourcecode lang="xml"]
&lt;beans profile=&quot;cloud&quot;&gt;
    &lt;cloud:mongo-db-factory id=&quot;mongoDbFactory&quot;/&gt;
&lt;/beans&gt;
[/sourcecode]</p>
<p>In web.xml change the value of spring.profiles.active to cloud and create a war. To deploy to Cloud Foundry follow the following steps
<ol>
    <li>Download the Cloud Foundry <a href="http://rubygems.org/gems/vmc">VMC Ruby gem</a></li>
    <li>Build the project using Maven. Do mvn clean install -Dmaven.test.skip=true</li>
    <li>Once you have installed the vmc gem go to target folder and do vmc push.  This command will prompt you for creating a MongoDB service say yes to  that and you application will be deployed to Cloud Foundry cloud.</li>
    <li>You can check the running application at <a href="http://bookshop.cloudfoundry.com/">http://bookshop.cloudfoundry.com/</a></li>
</ol>
That's it for this blog. I think you will agree that Spring Profiles feature is very useful and powerful.</p>