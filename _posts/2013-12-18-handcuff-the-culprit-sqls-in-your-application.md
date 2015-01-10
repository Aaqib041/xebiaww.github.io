---
layout: post
header-img: img/default-blog-pic.jpg
author: dbansal
description: 
post_id: 17769
created: 2013/12/18 09:11:45
created_gmt: 2013/12/18 04:11:45
comment_status: open
---

# Handcuff The Culprit SQLs in Your Application

<p>Throughout your software development career, many times you will encounter scenarios similar to following:
<blockquote><em>My application works in a clustered environment with load balancing. I have applied multi-threading wherever possible to fully harness the capability of my multi-processor server. My database runs on a standalone server with best of configurations. I am using the most promised ORM for data layer. Still, the performance of my application is nowhere near expected. Why?</em></blockquote>
The culprit is most probably data layer. Lets find out.</p>
<p>Almost any enterprise application is , vaguely, designed according to the familiar 3 tier architecture as shown below:
<a href="http://xebee.xebia.in/wp-content/uploads/2013/12/Architecture.jpg"><img class="alignnone size-full wp-image-17770" alt="Architecture" src="http://xebee.xebia.in/wp-content/uploads/2013/12/Architecture.jpg" width="319" height="410" /></a></p>
<p>The ovalled portion of the figure above is very crucial while determining the overall performance of the application. It is because data needs are fulfilled by writing SQLs. A single data requirement can be fulfilled by number of different SQLs. Each of these SQLs can be different in their structure and thus have different response times. The performance of your application depends on picking up the best alternative for each requirement. But can we be sure about the best alternative that we chose ?</p>
<p>In many cases, it is not possible, during development, to identify the optimized queries.
Reasons?
<ul>
    <li>Analyzing individual SQLs is time consuming during development</li>
    <li>More focus on functionality rather than performance during development</li>
    <li>Use of ORMs which abstracts the real SQL that runs on database server.</li>
    <li>Lack of SQL tuning expertise in developers.</li>
    <li>Unavailability of large dataset during development to test and compare SQLs.</li>
</ul>
<blockquote><em>Yeah, I know. My application is now built, probably with bad SQLs. And these are scattered throught the application. Some are even dynamically generated.</em></p>
<p>How do I aggregate and analyze all the queries that my application generates and fires ?</blockquote>
<strong>Step 1: Setup Logging</strong></p>
<p>Each database server comes with a logging mechanism. Lets take Postgres as the example.</p>
<p>Postgres, installation contains a configuration file, postgresql.conf. It generally resides in /etc/postgresql/&lt;version/main directory. This file has number of commented out properties each of which is generously documented. Section of our concern is: ERROR REPORTING AND LOGGING.</p>
<p>For analyzing the applications hitting this database, we can set/uncomment number of properties. For our purpose, following should suffice:
<ol>
    <li>log_destination = 'stderr'</li>
    <li>logging_collector = on</li>
    <li>log_directory = '/home/deepak/myapp/NFR/logs' # Postgres user should have write permissions on this directory</li>
    <li>log_filename = 'postgresql-%Y-%m-%d_%H%M%S.log'</li>
    <li>log_file_mode = 0644</li>
    <li>log_min_duration_statement = 5 # log only queries taking more than 5 ms</li>
    <li>log_duration = on</li>
    <li>log_line_prefix = '%p %m %a'</li>
    <li>log_statement = 'all'</li>
</ol>
After you have set these properties, restart the postgres server by running following command as postgres user:
<code>$ su postgres</code>
<code>$ /etc/init.d/postgresql restart</code></p>
<p>Your database is now ready to spit out all the queries that runs on its server.</p>
<p><strong>Step 2: Run the application</strong></p>
<p>Run the integration test suite of your application pointing to this database. Make sure that no other application is using this database. Otherwise, the logs will get mixed up and you can encounter hard time filtering out your queries.</p>
<p>After the test suite is completed, the log files would be stored at location mentioned in log_directory above.
The logs are rolled over to new files at every 10 MB (configurable) of log data.</p>
<p>So, now you have all the queries, fired by your application, aggregated together in these log files.</p>
<!--more-->

<p><strong>Step 3: Filter Out redundant information from the log files</strong></p>
<p>The sample of a log file looks like following:</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2013/12/sampleLog.jpg"><img class="alignnone size-full wp-image-17771" alt="sampleLog" src="http://xebee.xebia.in/wp-content/uploads/2013/12/sampleLog.jpg" width="1250" height="299" /></a></p>
<p>As you can see, there are number of rows in the above sample file which are of no use if you are analyzing the data layer of your application. It includes: unrequired duration logs, transaction isolation level calls, select 1 calls etc. Please note that we are interested in seeing the durations only for the SQL queries. We should filter out all the unrequired information before starting to analyze these logs. After filtering, these logs become easier to read and understand from the application's perspective. Following python script helps in doing so. You can use your favourite scripting language.
<pre>import os</p>
<p>os.chdir("/home/deepak/myapp/NFR/logs")
for file1 in os.listdir("."):
fobj = open(file1, "r")
newFile = "%s/../RefinedQueries/%s" % (os.curdir, file1)
newFobj = open(newFile, "w+")
fobj.seek(0)
count = 0
prevLine = ""
duration = "[unknown]LOG: duration:"
for line in fobj:
if line.endswith("SHOW TRANSACTION ISOLATION LEVEL\n") or line.endswith(": select 1\n"):
continue
if not (duration in line and duration in prevLine):
count += 1
newFobj.write(line)
prevLine = line
newFobj.close();
print("%s %s" % (newFile, count))</pre>
After running the above script you will get a sibling directory named RefinedQueries which contains logs with only usable information.</p>
<p><strong>Step 4: Extract out costly queries:</strong></p>
<p>Now all you have to do is find out which queries (SELECT, INSERT, UPDATE, DELETE) are taking longer time. However, it can not be done manually for a normal sized application. So there is some automation required to extract out the queries based on the duration of their execution. Following python program takes the RefinedQueries produced in step 3 above alongwith command line arguments for the range of duration (in ms) and extracts out all the queries whose duration lies within the given range.
<pre> 
import os
import sys
os.chdir("/home/deepak/myapp/NFR/RefinedQueries")</p>
<p>minDuration=10      #Default Value for minDuration
maxDuration=10000     #Default Value for maxDuration</p>
<p>if len(sys.argv) &gt;= 2:
    minDuration=int(sys.argv[1])
if len(sys.argv) &gt;= 3:
    maxDuration=int(sys.argv[2])</p>
<p>print("Filtering Queries of duration [%s-%s] milliseconds"%(minDuration,maxDuration))</p>
<p>makingSQL = False
sql=""
newFile = "%s/../CostlyQueries/%d to %d ms.log" % (os.curdir,minDuration, maxDuration)
newFobj = open(newFile, "w+")
TotalDuration=0
count=0
for file1 in os.listdir("."):
    print("Filtering File:%s"%file1)
    fobj = open(file1, "r")
    for line in fobj:
        if not makingSQL and "IST [unknown]LOG:" in line and (": INSERT INTO " in line or ": SELECT " in line or ": UPDATE " in line or ": DELETE FROM " in line):
            makingSQL=True
            sql+=line
            continue
        if makingSQL:
            sql+=line
            if "LOG:  duration: " in line:
                makingSQL=False
                splits=line.split("duration:")
                duration=splits[1]
                duration=duration.strip().split(' ')[0]
                intDuration=int(float(duration))
                if intDuration &gt; minDuration and intDuration &lt; maxDuration :
                    newFobj.write(sql)
                    TotalDuration+=intDuration
                    count += 1
                    #print("%03d %d %d"%(count, intDuration,TotalDuration))
                sql=""
    fobj.close()
newFobj.close()</p>
<p>print("Total %s queries found. Saved at location:%s"%(count,os.path.abspath(newFile)))</pre>
Run the above script:</p>
<p><code>$ python FilterCostlyQuery.py 100 10000
Filtering File:postgresql-2013-12-13_105352.log
Filtering File:postgresql-2013-12-13_105823.log
Filtering File:postgresql-2013-12-13_105829.log
Filtering File:postgresql-2013-12-13_105728.log</p>