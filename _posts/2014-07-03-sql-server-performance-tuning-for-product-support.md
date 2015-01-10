---
layout: post
header-img: img/default-blog-pic.jpg
author: PGarg
description: 
post_id: 18472
created: 2014/07/03 10:03:09
created_gmt: 2014/07/03 05:03:09
comment_status: open
---

# SQL Server Performance Tuning for Product Support

<p><span style="font-size: 18px;"><strong>Overview</strong></span>
For last few months I have been working with a client for developing a product based on Java landscape. As a product it needs to support couple of databases at the back end depending upon the client's customers requirement and by support I mean database should meet certain benchmarks in terms of time performance and space usage. So I along with couple of more folks have been working on this implementation. Team did not had much expertise in database so issues took bit longer than usual to fade away. In this blog I would like to share some problems that we faced and how we resolved them.</p>
<p><span style="font-size: 18px;"><strong>Overview of Application</strong></span>
Application is a pipeline of steps/phases, where first phase takes input as a file (.txt | .csv)  having 1 to mn records, stores them in the database and then the next subsequent steps do processing over that stored dataand final step produces a new file containing with transformed records. The transformations performed on the data interesting only in terms of business, so not going into those details.
Some of the phases make heavy use of Multithreading to process non overlapped data to increase the performance and better utilize the CPU Cores. The application already supports <strong>DB2</strong> database and the complete processing finishes with in a given time and space.</p>
<p><span style="font-size: 18px;"><strong>Technologies Used in Application</strong></span>
<ul>
    <li>Java &amp; Apache Camel for the pipeline described above.</li>
    <li>Apache MetaModel (Incubator) as ORM</li>
</ul>
<span style="font-size: 18px;"><strong>Performance Tuning</strong></span>
Team had least exposure to this database and the reason being this database rarely finds its place in the Java world but anyways we had to support it.
<!--more-->
<span style="text-decoration: underline;"><strong><span style="font-size: 14px;">Initial Test</span></strong></span>
So starting with few records pushed to the pipeline. There were some errors in processing but mostly due to the SQL syntax errors or datatype mismatch but nonetheless the complete processing did happen successfully.</p>
<p><span style="text-decoration: underline;"><strong>First Blocker</strong></span>
Next step was to run the pipeline with a bigger data set. The processing got suspended in one of the phases. Fired the below query to see queries blocking each other.
<code>
SELECT sqltext.TEXT,req.session_id,req.status,
req.command,req.cpu_time,req.total_elapsed_time,
sqlplan.query_plan,W.*
FROM sys.dm_exec_requests req
CROSS APPLY sys.dm_exec_sql_text(sql_handle) AS sqltext
CROSS APPLY sys.dm_exec_query_plan(plan_handle) AS sqlplan
left join sys.dm_os_waiting_tasks W on W.session_id = req.session_id
</code></p>
<p>Figured out that a long running SELECT Query on TABLE A is blocking the smaller UPDATE queries on the same TABLE A . The SELECT query (a long running transaction) is an iterator over records to fetch X records each time from the database and then those X fetched records were divided and processed separately by threads which fire UPDATE query on the same table. This results in the suspension of UPDATE query.</p>
<p><span style="text-decoration: underline;"><strong>Solution</strong></span>
<strong>So changing the transaction level to READ_COMMITTED_SNAPSHOT ON  to resolve this issue. </strong>More details about this issue can be found here <a title="http://msdn.microsoft.com/en-us/library/tcbchxcb(v=vs.110).aspx" href="http://msdn.microsoft.com/en-us/library/tcbchxcb(v=vs.110).aspx">http://msdn.microsoft.com/en-us/library/tcbchxcb(v=vs.110).aspx</a></p>
<p><span style="text-decoration: underline;"><strong>Second Blocker</strong></span>
Some of the phases in the execution pipeline were really slow. So first thing that comes to mind is about the indexes not being correctly implemented. To verify that we need to see the explain plan of queries. SQL server comes with an Activity Monitor Tool. This tool gives quite useful information separated across sections . Please see snapshot below</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/07/Screenshot-from-2014-07-02-200601.png"><img class="alignnone size-full wp-image-18494" alt="Screenshot from 2014-07-02 20:06:01" src="http://xebee.xebia.in/wp-content/uploads/2014/07/Screenshot-from-2014-07-02-200601.png" width="1366" height="768" /></a></p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/07/Stats.jpg"><img class="alignnone size-full wp-image-18495" alt="Stats" src="http://xebee.xebia.in/wp-content/uploads/2014/07/Stats.jpg" width="948" height="574" /></a></p>
<p><span style="line-height: 1.5em;">Select the query from the long running transactions and see the execution plan by right clicking on it. Now the execution plan doesn't tells much about the slowness of the query though it will give the relative percentages of the execution time. So to actually see how much this query is taking time. Select the same query from the <strong>processes section</strong> and right click to analyse in </span>profiler<span style="line-height: 1.5em;">. There we can find out the quantified time for the query. The profiler showed us that each query is taking .5 sec to 1 sec to execute which is way too slow than expected.  Again looked at  index scripts and found out that both the clustered and non clustered indexes were more or less applied correctly as per the guidelines mentioned. Below are some of those guidelines</span></p>
<p><strong>Clustered Index :-</strong> This Index defines how the data is stored and sorted in the table based on a particular key. Can be only one per table. Generally created using Primary Key or Candidate Keys. Also please note the <strong>FILLFACTOR=80</strong>. This setting may end up taking more space but will speed up the index operations.
<code>
CREATE UNIQUE CLUSTERED INDEX [IDX] ON [DBO].[TABLE]
(
[COL1] ASC,
[COL2] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = OFF, FILLFACTOR=80) ON [PRIMARY];
</code>
<strong>Non Clustered Index :- </strong>Can be more than one in the table, Should be created on the columns which appear in the queries and also use composite index on all the keys of the same table that appear in a query.
<code>
CREATE NONCLUSTERED INDEX [IDX] ON [DBO].[TABLE]
(
[Col1] ASC,
[Col2] ASC,
[Col3] ASC
)WITH (SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF,ALLOW_PAGE_LOCKS = OFF, ALLOW_ROW_LOCKS= ON) ON [PRIMARY]
</code>
<span style="line-height: 1.5em;">So even after loads of permutation and combination for indexes we got no performance improvement. Which probably means indexes were already correct. We let the processing run with whatever time its taking.</span></p>
<p><span style="text-decoration: underline;"><strong>Third Blocker</strong></span>
Next we ran into deadlock issues in of the phases. This phases involved multiple threads from the application adding/updating/deleting rows across the <strong>same and different tables</strong> but none of the threads overlaps with the rows to be updated in the tables. So it appeared that the queries are taking table level locks on indexes/tables. To avoid that we updated the tables and indexes with below settings. With these setting we still got the deadlock so apparently the issue was something else.</p>
<p><code>
ALTER INDEX ... WITH (ALLOW_PAGE_LOCKS = OFF, ALLOW_ROW_LOCKS = ON);
ALTER TABLE SET LOCK_ESCALATION OFF
</code></p>
<p>Also profiled the queries (<strong>Go to Tool -&gt; SQL Server Profile -&gt; Start New Trace -&gt; Show All Events -&gt; Select All Deadlock Events</strong>). Though there were deadlock Events but no lock escalation.</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/07/m7AKT.png"><img class="alignnone size-full wp-image-18504" alt="m7AKT" src="http://xebee.xebia.in/wp-content/uploads/2014/07/m7AKT.png" width="1366" height="768" /></a></p>
<p><strong><span style="text-decoration: underline;">Solution</span> </strong></p>