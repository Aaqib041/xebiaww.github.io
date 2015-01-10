---
layout: post
header-img: img/default-blog-pic.jpg
author: rjaiswal
description: 
post_id: 3807
created: 2010/06/05 23:26:37
created_gmt: 2010/06/05 18:26:37
comment_status: open
---

# From Zero to a CRUD App (without writing a single line of code!) using JRuby

<p>This blog introduces you to JRuby and Rails. We will create a simple web application with CRUD features and deploy it on Tomcat.</p>
<p><strong>Required Software -</strong>
- JDK 6
- Tomcat 6
- MySql
- JRuby (<a href="http://jruby.org/download">http://jruby.org/download</a>)</p>
<!--more-->

<p>The blog assumes that you have JDK, MySql and Tomcat set up. Download JRuby from the link above and add the {JRUBY_DIR}/bin to your classpath.</p>
<p><strong>Let's run the first command (from any directory) -</strong></p>
<p><code>jruby -S gem install mongrel activerecord-jdbcmysql-adapter rails</code></p>
<p>*If the command above complains of lack of jruby-openssl then abort it (using Ctrl+C) and first run "jruby -S gem install jruby-openssl".</p>
<p>This will install mongrel server, activerecord-jdbcmysql-adapter and rails. "gem install" is the Ruby way to install libraries, in Java you download jars and add them to your classpath. Similarly in Ruby you do a "gem install" which does everything for you.</p>
<p><strong>Now run the following command -</strong></p>
<p><code>jruby -S rails XLib -d mysql</code></p>
<p>Here we are using JRuby to tell Rails (a Ruby web application framework) to create a application XLib which uses MySql as the database.</p>
<p>Enter the newly-created “XLib” directory, then <strong>modify the config/database.yml</strong>. First and foremost, you need to adjust the adapter name, and instead of ‘mysql’ you should specify ‘jdbcmysql’. You might also want to delete the lines starting with “socket:”. Your development environment config can look like -
<code>
development:
adapter: jdbcmysql
encoding: utf8
reconnect: false
database: XLib_dev
pool: 5
username: root
password: root
</code></p>
<p><strong>Now let's create our schema (make sure you are in your XLib directory) -</strong></p>
<p><code>jruby -S rake db:create:all</code></p>
<p>The command above runs a rake (like make but for Ruby) task that creates the necessary schema in MySql.</p>
<ul>
<li>Please note, there is currently a bug in activerecord-jdbcmysql-adapter due to which first time schema creation may not work. You can create the schema manually (just name it XLib_dev as in config above), this is a one time task so it doesn't hurt too much.</li>
</ul>
<p><strong>Now lets run  -</strong></p>
<p><code>jruby script/generate scaffold book title:string authors:string genre:string quantity:int added_on:date</code></p>
<p>Scaffolding is one of the main USPs of Rails, it generates the necessary CRUD code for a domain object, in our case a "book".</p>
<p><strong>Now we need to update our database -</strong></p>
<p><code>jruby -S rake db:migrate</code></p>
<p>We're all done, <strong>let's start the mongrel server -</strong></p>
<p><code>jruby script/server</code></p>
<p><strong>check out the application at -</strong></p>
<p><a href="http://localhost:3000/books">http://localhost:3000/books</a></p>
<p>You can add, edit, delete and see a listing of all the books.</p>
<p>We used JRuby (which is the Java implementation of Ruby) so that our application can run on JVM. Since it can run on JVM it can also run on Tomcat. To run it on Tomcat we first need to convert it to a war file. You guessed it, <strong>there is a gem for that - </strong></p>
<p><code>jruby -S gem install warbler</code></p>
<p><strong>Next, run the following (inside XLib directory) -</strong></p>
<p><code>warble config</code></p>
<p>This will generate a config file for warble as warble.rb in your app's config directory</p>
<p><strong>Open this config file and add the following line at the end -</strong></p>
<p><code>config.gems += ["activerecord-jdbcmysql-adapter", "jruby-openssl"]</code></p>
<p>This enables the gems above to be bundled in your war, otherwise Tomcat complains of missing libraries.</p>
<p><strong>Now, run the command -</strong></p>
<p><code>warble</code></p>
<p>This will create XLib.war in you XLib directory, copy this file to your $TOMCAT_HOME/webapps and start Tomcat. This will unzip the war, shutdown Tomcat after a while, check XLibs' web.xml and make sure the environment points to development. Restart Tomcat.</p>
<p>Go to <a href="http://localhost:8080/XLib/books">http://localhost:8080/XLib/books</a></p>
<p>And that's it, you have a CRUD application running on Tomcat without even writing a single line of code.</p>