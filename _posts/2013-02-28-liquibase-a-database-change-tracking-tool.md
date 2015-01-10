---
layout: post
header-img: img/default-blog-pic.jpg
author: bsingh
description: 
post_id: 15868
created: 2013/02/28 15:15:06
created_gmt: 2013/02/28 10:15:06
comment_status: open
---

# Liquibase - A database change tracking tool

<p>Currently database versioning  has some issues like database change tracking, branching and merging, flexibility to apply changes and database diff.
Using schema versioning, we can track the database changes by incremental database version for any change and depending upon the version number apply the changes in database but there is a limitation to this approach. It fails while working with multiple developers as two developers can at the same time create change for the same database version.
So we need some solution that can efficiently handle the versioning of database. Liquibase is the tool can be used for this purpose. It is a database changes management tool to track the changes in readable format. It is used to manage the production data and test data with extensive database change documentation feature. It can compare two databases and have a patch file generated to make the one equal to the other, or generate a patch file from an existing database schema. The patches are written in XML, meaning the liquiBase knows the context of your patches and can actually figure out the undo-version of the patch file by itself. This tool is really nifty, and having all patches in one big XML file means that merging patches is similar to merging source code.<!--more--></p>
<p>Liquibase creates databasechangelog table to maintain the executed changeset for database changes and another table databasechangeloglock to control concurrency.</p>
<p>In this blog, I will setup liquibase using maven and will make some database changes.</p>
<p><strong>Step 1</strong>- Add liquibase maven plugin to pom.xml</p>
<p>pom.xml
<pre class="lang:default decode:true">&lt;properties&gt;
&lt;liquibase.contexts&gt;all&lt;/liquibase.contexts&gt;
&lt;/properties&gt;</p>
<p>&lt;build&gt;
&lt;resources&gt;
&lt;resource&gt;
&lt;directory&gt;src/main/resources&lt;/directory&gt;
&lt;filtering&gt;true&lt;/filtering&gt;
&lt;/resource&gt;
&lt;/resources&gt;
&lt;plugins&gt;
&lt;plugin&gt;
&lt;groupId&gt;org.liquibase&lt;/groupId&gt;
&lt;artifactId&gt;liquibase-maven-plugin&lt;/artifactId&gt;
&lt;configuration&gt; 
&lt;propertyFileWillOverride&gt;true&lt;/propertyFileWillOverride&gt;
&lt;propertyFile&gt;src/main/resources/liquibase.properties&lt;/propertyFile&gt;
&lt;/configuration&gt;
&lt;/plugin&gt;
&lt;/plugins&gt;
&lt;/build&gt;</pre>
<strong>Step 2</strong> - Create a liquibase.properties file that is configured in liquibase plugin in propertyFile.</p>
<p>This file contains database specifications.
<pre class="lang:default decode:true">contexts=${liquibase.contexts}
changeLogFile=src/main/resources/changesets/db.changelog.xml
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/liquibaseDB
username=root
password=mysql
verbose=true
dropFirst=false</pre>
<strong>Step 3</strong> - Create db.changelog.xml that is configured in liquibase.properties.</p>
<p>This file contains the database changes. Using include tag we can define multiple changelog files.
In this file we are including all the files from the test folder. These files are used for test context.
<pre class="lang:default decode:true" title="db.changelog.xml">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;databaseChangeLog
xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-2.0.xsd"&gt;</p>
<p>&lt;include file="core/core-model.changelog.xml" relativeToChangelogFile="true"/&gt;</p>
<p>&lt;!--Test Context ChangeLogs --&gt;
&lt;includeAll path="test/" relativeToChangelogFile="true"/&gt;</p>
<p>&lt;/databaseChangeLog&gt;</pre>
<strong>Step 4</strong> - Create a changelog file for core module changes.
This file contains multiple changeSet tag. Each changeSet tag is uniquely identified by the combination of the 'id' tag, the 'author' tag, and the changelog file classpath name.
The id tag is only used as an identifier, it does not direct the order that changes are run and does not even have to be an integer. As LiquiBase executes the databaseChangeLog, it reads the changeSets in order and, for each one, checks the 'databasechangelog' table to see if the combination of id/author/filepath has been run. If it has been run, the changeSet will be skipped unless there is a true 'runAlways' tag. After all the changes in the changeSet are run, LiquiBase will insert a new row with the id/author/filepath along with an MD5Sum of the changeSet in the 'databasechangelog'.
In preConditions tag, we specify the database on which we want to run all the changeSets defined in databaseChangeLog
<pre class="lang:default decode:true" title="core.changelog.xml">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;databaseChangeLog xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-2.0.xsd"&gt;</p>
<p>&lt;preConditions&gt;
&lt;dbms type="mysql" /&gt;
&lt;/preConditions&gt;</p>
<p>&lt;changeSet author="balendra" id="liquibase-common-1"
runOnChange="true"&gt;
&lt;createTable tableName="user"&gt;
&lt;column name="id" type="bigint(40)"&gt;
&lt;constraints primaryKey="true" nullable="false" /&gt;
&lt;/column&gt;
&lt;column name="username" type="varchar(100)"&gt;
&lt;constraints nullable="false" /&gt;
&lt;/column&gt;
&lt;column name="password" type="varchar(100)"&gt;
&lt;constraints nullable="false" /&gt;
&lt;/column&gt;
&lt;/createTable&gt;
&lt;/changeSet&gt;
&lt;/databaseChangeLog&gt;</pre>
<strong>Step 5</strong> - Create a test changeset file to insert the test data in the database. In this file we using a context tag.
Contexts in liquiBase are tags you can add to changeSets to control which will be executed in any particular migration run. Any string can be used for the context name and
they are checked case-insensitively. When you run the migrator though any of the available methods, you can pass in a set of contexts to run. Only changeSets marked with the passed contexts will be run. If you don't assign a context to a changeSet, it will run all the time, regardless of what contexts you pass in to the migrator.
If you do not specify a context when you run the migrator, ALL contexts will be run.</p>
<p>test-data-changeset.xml
<pre class="lang:default decode:true">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;databaseChangeLog
xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-2.0.xsd"&gt;</p>
<p>&lt;changeSet id="liquibase-common-1" author="balendra" context="test"&gt;
&lt;preConditions onFail="MARK_RAN"&gt;
&lt;and&gt;
&lt;sqlCheck expectedResult="0"&gt;select count(*) from user&lt;/sqlCheck&gt;
&lt;/and&gt;
&lt;/preConditions&gt;
&lt;comment&gt;Insert user test data&lt;/comment&gt;
&lt;insert tableName="user"&gt;
&lt;column name="id" value="1"/&gt;
&lt;column name="username" value="Balendra"/&gt;
&lt;column name="password" value="liquibase"/&gt;
&lt;/insert&gt;
&lt;/changeSet&gt;
&lt;/databaseChangeLog&gt;</pre>
<strong>Step 6</strong> - Update Database</p>
<p>Run the following maven command to update database.
<pre class="lang:default decode:true">mvn liquibase:update</pre>
<strong>Rollback</strong></p>
<p>Liquibase provides rollback feature using which we can undo the changes in the database. There are multiple ways to undo the changes -</p>
<p><strong>a</strong>. Some changes do not require to add the rollback tag as these changeset can automatically rollback statements as following .
<pre class="lang:default decode:true">&lt;changeSet id="liquibase-common-1" author="balendra"&gt;
&lt;createTable tableName="company"&gt;
&lt;column name="id" type="int"/&gt;
&lt;/createTable&gt;
&lt;/changeSet&gt;</pre>
<strong>b</strong>. In rollback tag, we can define some other change tag.
<pre class="lang:default decode:true">&lt;changeSet id="changeRollback" author="balendra"&gt;
&lt;createTable tableName="changeRollback1"&gt;
&lt;column name="id" type="int"/&gt;
&lt;/createTable&gt;
&lt;rollback&gt;</p>