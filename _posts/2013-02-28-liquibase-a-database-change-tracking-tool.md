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

Currently database versioning  has some issues like database change tracking, branching and merging, flexibility to apply changes and database diff. Using schema versioning, we can track the database changes by incremental database version for any change and depending upon the version number apply the changes in database but there is a limitation to this approach. It fails while working with multiple developers as two developers can at the same time create change for the same database version. So we need some solution that can efficiently handle the versioning of database. Liquibase is the tool can be used for this purpose. It is a database changes management tool to track the changes in readable format. It is used to manage the production data and test data with extensive database change documentation feature. It can compare two databases and have a patch file generated to make the one equal to the other, or generate a patch file from an existing database schema. The patches are written in XML, meaning the liquiBase knows the context of your patches and can actually figure out the undo-version of the patch file by itself. This tool is really nifty, and having all patches in one big XML file means that merging patches is similar to merging source code.

Liquibase creates databasechangelog table to maintain the executed changeset for database changes and another table databasechangeloglock to control concurrency.

In this blog, I will setup liquibase using maven and will make some database changes.

**Step 1**\- Add liquibase maven plugin to pom.xml

pom.xml 
    
    
    <properties>
    <liquibase.contexts>all</liquibase.contexts>
    </properties>
    
    
    
    
    <build>
    <resources>
    <resource>
    <directory>src/main/resources</directory>
    <filtering>true</filtering>
    </resource>
    </resources>
    <plugins>
    <plugin>
    <groupId>org.liquibase</groupId>
    <artifactId>liquibase-maven-plugin</artifactId>
    <configuration> 
    <propertyFileWillOverride>true</propertyFileWillOverride>
    <propertyFile>src/main/resources/liquibase.properties</propertyFile>
    </configuration>
    </plugin>
    </plugins>
    </build>

**Step 2** \- Create a liquibase.properties file that is configured in liquibase plugin in propertyFile.

This file contains database specifications. 
    
    
    contexts=${liquibase.contexts}
    changeLogFile=src/main/resources/changesets/db.changelog.xml
    driver=com.mysql.jdbc.Driver
    url=jdbc:mysql://localhost:3306/liquibaseDB
    username=root
    password=mysql
    verbose=true
    dropFirst=false

**Step 3** \- Create db.changelog.xml that is configured in liquibase.properties.

This file contains the database changes. Using include tag we can define multiple changelog files. In this file we are including all the files from the test folder. These files are used for test context. 
    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <databaseChangeLog
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
    http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-2.0.xsd">
    
    
    
    
    <include file="core/core-model.changelog.xml" relativeToChangelogFile="true"/>
    
    
    
    
    <!--Test Context ChangeLogs -->
    <includeAll path="test/" relativeToChangelogFile="true"/>
    
    
    
    
    </databaseChangeLog>

**Step 4** \- Create a changelog file for core module changes. This file contains multiple changeSet tag. Each changeSet tag is uniquely identified by the combination of the 'id' tag, the 'author' tag, and the changelog file classpath name. The id tag is only used as an identifier, it does not direct the order that changes are run and does not even have to be an integer. As LiquiBase executes the databaseChangeLog, it reads the changeSets in order and, for each one, checks the 'databasechangelog' table to see if the combination of id/author/filepath has been run. If it has been run, the changeSet will be skipped unless there is a true 'runAlways' tag. After all the changes in the changeSet are run, LiquiBase will insert a new row with the id/author/filepath along with an MD5Sum of the changeSet in the 'databasechangelog'. In preConditions tag, we specify the database on which we want to run all the changeSets defined in databaseChangeLog 
    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <databaseChangeLog xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
    http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-2.0.xsd">
    
    
    
    
    <preConditions>
    <dbms type="mysql" />
    </preConditions>
    
    
    
    
    <changeSet author="balendra" id="liquibase-common-1"
    runOnChange="true">
    <createTable tableName="user">
    <column name="id" type="bigint(40)">
    <constraints primaryKey="true" nullable="false" />
    </column>
    <column name="username" type="varchar(100)">
    <constraints nullable="false" />
    </column>
    <column name="password" type="varchar(100)">
    <constraints nullable="false" />
    </column>
    </createTable>
    </changeSet>
    </databaseChangeLog>

**Step 5** \- Create a test changeset file to insert the test data in the database. In this file we using a context tag. Contexts in liquiBase are tags you can add to changeSets to control which will be executed in any particular migration run. Any string can be used for the context name and they are checked case-insensitively. When you run the migrator though any of the available methods, you can pass in a set of contexts to run. Only changeSets marked with the passed contexts will be run. If you don't assign a context to a changeSet, it will run all the time, regardless of what contexts you pass in to the migrator. If you do not specify a context when you run the migrator, ALL contexts will be run.

test-data-changeset.xml 
    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <databaseChangeLog
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
    http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-2.0.xsd">
    
    
    
    
    <changeSet id="liquibase-common-1" author="balendra" context="test">
    <preConditions onFail="MARK_RAN">
    <and>
    <sqlCheck expectedResult="0">select count(*) from user</sqlCheck>
    </and>
    </preConditions>
    <comment>Insert user test data</comment>
    <insert tableName="user">
    <column name="id" value="1"/>
    <column name="username" value="Balendra"/>
    <column name="password" value="liquibase"/>
    </insert>
    </changeSet>
    </databaseChangeLog>

**Step 6** \- Update Database

Run the following maven command to update database. 
    
    
    mvn liquibase:update

**Rollback**

Liquibase provides rollback feature using which we can undo the changes in the database. There are multiple ways to undo the changes -

**a**. Some changes do not require to add the rollback tag as these changeset can automatically rollback statements as following . 
    
    
    <changeSet id="liquibase-common-1" author="balendra">
    <createTable tableName="company">
    <column name="id" type="int"/>
    </createTable>
    </changeSet>

**b**. In rollback tag, we can define some other change tag. 
    
    
    <changeSet id="changeRollback" author="balendra">
    <createTable tableName="changeRollback1">
    <column name="id" type="int"/>
    </createTable>
    <rollback>