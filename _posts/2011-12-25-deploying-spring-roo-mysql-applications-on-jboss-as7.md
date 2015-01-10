---
layout: post
header-img: img/default-blog-pic.jpg
author: shekhargulati
description: 
post_id: 10558
created: 2011/12/25 21:59:23
created_gmt: 2011/12/25 16:59:23
comment_status: open
---

# Deploying Spring Roo MySQL Applications on  JBoss AS7

<p>Today I spend some time deploying a simple Spring Roo application on JBoss AS 7. I faced lot of issues in making Spring Roo application run on JBoss AS7. This is a step by step guide from creating Spring Roo application using Roo shell to configuring different databases like h2 and MySQL and finally making them run on JBoss AS7. So lets get started. Please make sure that you have downloaded latest Spring Roo version 1.2 and JBoss AS 7 from their respective web sites.
<ol>
    <li>Created a simple Spring Roo web application by typing the commands shown below.<br><br>
[sourcecode lang="text"]
project --topLevelPackage com.shekhar.bookshop --projectName bookshop
jpa setup --database H2_IN_MEMORY --provider HIBERNATE --jndiDataSource java:jboss/datasources/ExampleDS
entity jpa --class ~.domain.Book
field string --fieldName title --notNull
field string --fieldName author --notNull
field number --type double --fieldName price --notNull
web mvc setup
web mvc all --package ~.web
perform package
q
[/sourcecode]</li>
    <li>Make sure in the applicationContext.xml the jndi declaration look like as shown below
[sourcecode lang="xml"]
&lt;jee:jndi-lookup id=&quot;dataSource&quot; jndi-name=&quot;java:jboss/datasources/ExampleDS&quot;/&gt;
[/sourcecode]</li>
    <li>Rename the bookshop-0.1.0.BUILD-SNAPSHOT.war to bookshop.war and copy it to &lt;JBoss-AS7-HOME&gt;/standalone/deployments folder and start the server using ./standalone.sh on *nix machines.<!--more--></li>
    <li>The deployment will fail and you will see exception stacktrace on the console as show below.
[sourcecode lang="text"]
22:28:53,526 INFO  [org.hibernate.cfg.Environment] (MSC service thread 1-8) HHH00021:Bytecode provider name : javassist
22:28:53,555 INFO  [org.hibernate.ejb.Ejb3Configuration] (MSC service thread 1-8) HHH00204:Processing PersistenceUnitInfo [
    name: persistenceUnit
    ...]
22:28:53,584 ERROR [org.jboss.msc.service.fail] (MSC service thread 1-8) MSC00001: Failed to start service jboss.persistenceunit.&quot;bookshop.war#persistenceUnit&quot;: org.jboss.msc.service.StartException in service jboss.persistenceunit.&quot;bookshop.war#persistenceUnit&quot;: Failed to start service
    at org.jboss.msc.service.ServiceControllerImpl$StartTask.run(ServiceControllerImpl.java:1780)
    at java.util.concurrent.ThreadPoolExecutor$Worker.runTask(ThreadPoolExecutor.java:886) [:1.6.0_26]
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:908) [:1.6.0_26]
    at java.lang.Thread.run(Thread.java:662) [:1.6.0_26]
Caused by: java.lang.RuntimeException: error trying to scan &lt;jar-file&gt;: vfs:/content/bookshop.war/WEB-INF/classes/
    at org.hibernate.ejb.Ejb3Configuration.scanForClasses(Ejb3Configuration.java:849)
    at org.hibernate.ejb.Ejb3Configuration.configure(Ejb3Configuration.java:591)
    at org.hibernate.ejb.HibernatePersistence.createContainerEntityManagerFactory(HibernatePersistence.java:72)
    at org.jboss.as.jpa.service.PersistenceUnitService.createContainerEntityManagerFactory(PersistenceUnitService.java:143)
    at org.jboss.as.jpa.service.PersistenceUnitService.start(PersistenceUnitService.java:77)
    at org.jboss.msc.service.ServiceControllerImpl$StartTask.startService(ServiceControllerImpl.java:1824)
    at org.jboss.msc.service.ServiceControllerImpl$StartTask.run(ServiceControllerImpl.java:1759)
    ... 3 more
Caused by: java.lang.RuntimeException: could not load entity class 'void com.shekhar.bookshop.domain.Book_Roo_Jpa_Entity.ajc$declare_at_type_1()' with PersistenceUnitInfo.getNewTempClassLoader()
    at org.jboss.as.jpa.hibernate4.HibernateAnnotationScanner.getClassesInJar(HibernateAnnotationScanner.java:152)
    at org.hibernate.ejb.Ejb3Configuration.addScannedEntries(Ejb3Configuration.java:479)
    at org.hibernate.ejb.Ejb3Configuration.scanForClasses(Ejb3Configuration.java:846)
    ... 9 more
Caused by: java.lang.ClassNotFoundException: void com.shekhar.bookshop.domain.Book_Roo_Jpa_Entity.ajc$declare_at_type_1() from [Module &quot;deployment.bookshop.war:main&quot; from Service Module Loader]
    at org.jboss.modules.ModuleClassLoader.findClass(ModuleClassLoader.java:191)
    at org.jboss.modules.ConcurrentClassLoader.performLoadClassChecked(ConcurrentClassLoader.java:361)
    at org.jboss.modules.ConcurrentClassLoader.performLoadClass(ConcurrentClassLoader.java:310)
    at org.jboss.modules.ConcurrentClassLoader.loadClass(ConcurrentClassLoader.java:103)
    at org.jboss.as.jpa.hibernate4.HibernateAnnotationScanner.getClassesInJar(HibernateAnnotationScanner.java:148)
    ... 11 more</p>
<p>[/sourcecode]</li>
    <li>This exception is because of the AspectJ ITD files. Move all the aspectj ITD to Java code using Refactor -&gt; Push in.</li>
    <li>Create the package again using mvn clean package command and copy -&gt; rename the war to jboss deployments directory and start the server again using ./standalone.sh. This time you will get following exception.
[sourcecode lang="text"]
22:45:18,050 ERROR [org.jboss.msc.service.fail] (MSC service thread 1-4) MSC00001: Failed to start service jboss.persistenceunit.&quot;bookshop.war#persistenceUnit&quot;: org.jboss.msc.service.StartException in service jboss.persistenceunit.&quot;bookshop.war#persistenceUnit&quot;: Failed to start service
    at org.jboss.msc.service.ServiceControllerImpl$StartTask.run(ServiceControllerImpl.java:1780)
    at java.util.concurrent.ThreadPoolExecutor$Worker.runTask(ThreadPoolExecutor.java:886) [:1.6.0_26]
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:908) [:1.6.0_26]
    at java.lang.Thread.run(Thread.java:662) [:1.6.0_26]
Caused by: java.lang.ClassCastException: org.dom4j.DocumentFactory cannot be cast to org.dom4j.DocumentFactory
    at org.dom4j.DocumentFactory.getInstance(DocumentFactory.java:97)
    at org.dom4j.DocumentHelper.getDocumentFactory(DocumentHelper.java:36)
    at org.dom4j.DocumentHelper.createDocument(DocumentHelper.java:41)
    at org.hibernate.envers.configuration.RevisionInfoConfiguration.generateDefaultRevisionInfoXmlMapping(RevisionInfoConfiguration.java:86)
    at org.hibernate.envers.configuration.RevisionInfoConfiguration.configure(RevisionInfoConfiguration.java:322)
    at org.hibernate.envers.configuration.AuditConfiguration.&lt;init&gt;(AuditConfiguration.java:94)
    at org.hibernate.envers.configuration.AuditConfiguration.getFor(AuditConfiguration.java:134)
    at org.hibernate.envers.event.EnversIntegrator.integrate(EnversIntegrator.java:63)
    at org.hibernate.internal.SessionFactoryImpl.&lt;init&gt;(SessionFactoryImpl.java:294)
    at org.hibernate.cfg.Configuration.buildSessionFactory(Configuration.java:1722)
    at org.hibernate.ejb.EntityManagerFactoryImpl.&lt;init&gt;(EntityManagerFactoryImpl.java:76)
    at org.hibernate.ejb.Ejb3Configuration.buildEntityManagerFactory(Ejb3Configuration.java:899)
    at org.hibernate.ejb.Ejb3Configuration.buildEntityManagerFactory(Ejb3Configuration.java:884)
    at org.hibernate.ejb.HibernatePersistence.createContainerEntityManagerFactory(HibernatePersistence.java:73)
    at org.jboss.as.jpa.service.PersistenceUnitService.createContainerEntityManagerFactory(PersistenceUnitService.java:143)
    at org.jboss.as.jpa.service.PersistenceUnitService.start(PersistenceUnitService.java:77)
    at org.jboss.msc.service.ServiceControllerImpl$StartTask.startService(ServiceControllerImpl.java:1824)
    at org.jboss.msc.service.ServiceControllerImpl$StartTask.run(ServiceControllerImpl.java:1759)
    ... 3 more</p>
<p>[/sourcecode]</li>
    <li>To fix the exception above you will have to update your pom.xml file so that it does not bundle dom4j jar with war file. Update the pom.xml as shown below.
[sourcecode lang="xml"]
&lt;dependency&gt;
            &lt;groupId&gt;org.hibernate&lt;/groupId&gt;
            &lt;artifactId&gt;hibernate-core&lt;/artifactId&gt;
            &lt;version&gt;3.6.8.Final&lt;/version&gt;
            &lt;exclusions&gt;
                &lt;exclusion&gt;
                    &lt;groupId&gt;dom4j&lt;/groupId&gt;
                    &lt;artifactId&gt;dom4j&lt;/artifactId&gt;
                &lt;/exclusion&gt;
            &lt;/exclusions&gt;
        &lt;/dependency&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;dom4j&lt;/groupId&gt;
            &lt;artifactId&gt;dom4j&lt;/artifactId&gt;
            &lt;version&gt;1.6.1&lt;/version&gt;
            &lt;scope&gt;test&lt;/scope&gt;
        &lt;/dependency&gt;
[/sourcecode]</li></p>

## Comments

**[bdecoste@gmail.com](#6756 "2012-01-09 22:33:51"):** Hi Shekhar, I'm one of the JBoss engineers working on OpenShift. If there's anything I can do to help you out, please let me know. Appreciate all of this good stuff. -Bill (wdecoste@redhat.com)

