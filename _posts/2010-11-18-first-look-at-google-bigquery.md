---
layout: post
header-img: img/default-blog-pic.jpg
author: rgarg
description: 
post_id: 6236
created: 2010/11/18 19:12:01
created_gmt: 2010/11/18 14:12:01
comment_status: open
---

# First look at Google BigQuery

<p>In this blog I would primarily be talking about the Google BigQuery API. This has been a major step in the computing world to provide database as a service on cloud computing environment. BigQuery API has been designed to analyse huge amounts of data in really less timeframe. Interface of the service is quite straightforward, and involves HTTP calls to get data from the tables. 
Since Google BigTable is a NoSQL oriented database, the format of BigQuery doesn't support JOINS but is able to support lots of other features like grouping, ordering, ranges, rejection and so on.</p>
<!--more-->

<p><em><strong>Overview:</strong></em></p>
<p>BigQuery API only provides a basic querying mechanism to the end users. There is no functionality that allows you to manipulate the data once uploaded into the BigTable structure. You can only append more data into the structure or fire <a href="http://code.google.com/apis/bigquery/docs/query-reference.html">SELECT</a> queries against the data. This makes BigQuery engine more of an analytics tool over trillions of rows of data.</p>
<p>BigTable is a part of Google's storage servers and can be run without the need of any additional hardware. Moreover, the API has been exposed via simple HTTP and JSON, which are easy to put in any application. One can leverage these benefits to cut down on the hardware costs of setting up high performance systems for analysis.</p>
<p>The whole process of carrying out analysis on some dataset involves two basic steps:
<ol>
    <li>Importing data into the BigTable structure (Through <a href="http://code.google.com/apis/storage/">Google Storage</a> and import job)</li>
    <li>Running the queries against that data</li>
</ol>
<em><strong>Importing the data:</strong></em></p>
<p>In order to analyse your data, you should do following steps to port your data into the Google BigTable system:
<ol>
    <li>Ensure that you have Google Storage and BigQuery enabled account with Google.</li>
    <li> Upload the data in CSV format to Google storage using their <a href="https://sandbox.google.com/storage/m/">storage manager</a> HTTP interface. Header information is not required. Only the columns of CSV must match the structure of the table as specified in subsequent steps.</li>
    <li>Use <a href="http://curl.haxx.se/download.html">cURL</a> tool to get the authorization key which can be used in subsequent calls to the HTTP service exposed. Command to be executed is :
<blockquote><em>curl -X POST -d accountType=GOOGLE -d Email=&lt;email&gt;@gmail.com -d Passwd=&lt;password&gt; -d service=ndev -d source=google-bigquery-manualimport-1 -H "Content-Type: application/x-www-form-urlencoded" https://www.google.com/accounts/ClientLogin</em></blockquote>
This call will return an authorization key whose format is similar to :
<blockquote>DQAAAK8AAADlegSF4xMNrCgUrxFv3NFCYbBp4KCpdhOCOioIfJ7txXrRB9FYcvDAv7ooGvmaWoDCj_
SPHolxr9Q6gK8-cd4SR_QkIv3QcM_pQlVADtMWtv63hqzdz16KMZ1tDizeyOIe-vJNenAYqrjX4xVSTH2F
_5Fy9fc1mXxL77BtKXFMlV0xqbX9swpqXXhk_Zqvw8K0fbBE2o6kkU3FW7NDN8IXLrhqC_VEEaKXGAD3-1JIyg</blockquote>
Just keep a note of the key as it will be used further. Down the line, I will refer to it as <em>AUTH_KEY</em></li>
    <li>Create the table using following command.
<blockquote><em>curl -H "Authorization: GoogleLogin auth=AUTH_KEY" -X POST -H "Content-type: application/json" -d '{"data":   {"fields": [    {"type": "string", "id": "name", "mode": "REQUIRED"},    {"type": "integer", "id": "age", "mode": "REQUIRED"},    {"type":"float", "id": "weight", "mode": "REQUIRED"},    {"type": "boolean", "id": "is_magic", "mode": "REQUIRED"}    ],   "name": "&lt;bucket_name&gt;/testtable"  }}' 'https://www.googleapis.com/bigquery/v1/tables'</em></blockquote>
<strong>bucket_name</strong> is the name of bucket where you want the reference of your table to be created. Buckets are nothing but folders inside Google storage.</li>
    <li>After you have successfully added the table, import process needs to be triggered to port the uploaded CSV data into the BigTable structure. Following command will do the same :
<blockquote><em>curl -H "Authorization: GoogleLogin auth=AUTH_KEY" -X POST -H "Content-type: application/json" -d '{"data": {"sources": [{"uri": "&lt;bucket_name&gt;/&lt;data_file_name&gt;.csv"}]}}' 'www.googleapis.com/bigquery/v1/tables/&lt;bucket_name&gt;%2Ftesttable/imports'</em></blockquote>
<ul>
    <li>Please note that <strong>testtable</strong> is the table name that you have created in step 4.</li>
    <li>Response to this kind of a request is a JSON string which is of a format : {"data":"kind":"bigquery#import_id","table":"&lt;bucket_name&gt;/testtable","import":"&lt;import_id&gt;"}}</li>
    <li><strong>import_id</strong> is useful in getting the current state of the import process. It also returns errors in case it has encountered any. Any errors will just fail the import process and rollback everything.</li>
</ul>
</li>
    <li> To know the status of your import process, just fire the following command:
<blockquote><em>curl -X GET https://www.googleapis.com/bigquery/v1/tables/&lt;bucket_name&gt;%2Ftesttable/imports/&lt;import_id&gt; -H "Authorization: GoogleLogin auth=AUTH_KEY"</em></blockquote>
</li>
</ol>
<em><strong>Querying the data:</strong></em></p>
<p>After the data is successfully ported, you can query the database using BigQuery. Just download the bqshell tool from <a href="http://code.google.com/p/bigquery/">here</a> and build the code with all the required dependencies. The tool works on <a href="http://www.python.org/">python</a> and has a detailed "how-to" to install it.</p>
<p>This tool has a login console in which you can specify the username and password and query the sample datasets or your own datasets that you create using cURL. This tool is a parser to the JSON responses returned from the BigQuery API and displays them in a SQL output fashion. Alternatively, you can also use <a href="http://code.google.com/apis/bigquery/docs/libraries.html#curl">cURL calls</a> to query the database and see the JSON response yourself.</p>
<p><em><strong>Conclusion :</strong></em>
<ul>
    <li>BigQuery API shows good performance in scenarios where you have huge amounts of data that needs to be processed. Running any query on almost 28 million rows uploaded in a test data set gives back response in just 2-3 seconds. The time includes request post and response recieved times as well.</li>
</ul>
<ul>
    <li>There are sample data sets provided by Google themselves, one of which contains 60 billion records and queries get executed in 4-8 seconds. For more reference, please visit the following link : http://code.google.com/apis/bigquery/docs/dataset-mlab.html</li>
</ul>
<ul>
    <li>One of the drawbacks is that there is no way to insert data without the upload and import job process. One can also not delete any record from the table once inserted. In order to change the data, you will have to delete the table and do the complete import process again. One thing worth mentioning is that in case you only need to add data to an existing table, you can do so by firing another upload job for the CSV file containing additional data.</li>
</ul></p>

## Comments

**[Luca](#3420 "2010-11-30 18:15:35"):** Great article! Do you know if cURL calls could be done also with PHP curl commands? Or you must use bash scripts?

**[Rohit](#3429 "2010-12-02 09:32:08"):** @Luca, cURL is just a tool to make your life easy. You can also write a kind of driver using any programming language. Just take care of the headers and all. They should be in the right place in all the commands. As far as calling the API is concerned, even telnet should work if you are familiar with HTTP GET and PUT calls..

**[Shekhar Gulati](#3265 "2010-11-18 21:59:08"):** Informative Blog...thanks

**[Asjad Hassan](#6007 "2011-10-11 12:06:26"):** Hello Rohit, Great article regarding big query ,but i m facing problem in 4 step (i.e. Create the table using following command.) as i run the command i get following error (curl: (3) [globbing] unmatched brace at pos 9 curl: (3) [globbing] illegal character in range specification curl: (3) [globbing] unmatched brace at pos 7 curl: (6) Could not resolve host: string,; Host not found curl: (6) Could not resolve host: id; Host not found curl: (6) Could not resolve host: name,; Host not found curl: (6) Could not resolve host: mode; Host not found curl: (3) [globbing] unmatched close brace/bracket at pos 9 curl: (3) [globbing] unmatched brace at pos 7 curl: (6) Could not resolve host: integer,; Host not found curl: (6) Could not resolve host: id; Host not found curl: (6) Could not resolve host: age,; Host not found curl: (6) Could not resolve host: mode; Host not found curl: (3) [globbing] unmatched close brace/bracket at pos 9 curl: (3) [globbing] unmatched brace at pos 13 curl: (6) Could not resolve host: id; Host not found curl: (6) Could not resolve host: weight,; Host not found curl: (6) Could not resolve host: mode; Host not found curl: (3) [globbing] unmatched close brace/bracket at pos 9 curl: (3) [globbing] unmatched brace at pos 7 curl: (6) Could not resolve host: boolean,; Host not found curl: (6) Could not resolve host: id; Host not found curl: (6) Could not resolve host: is_magic,; Host not found curl: (6) Could not resolve host: mode; Host not found curl: (3) [globbing] unmatched close brace/bracket at pos 9 curl: (3) [globbing] unmatched close brace/bracket at pos 1 curl: (6) Could not resolve host: name; Host not found curl: (6) Could not resolve host: edwhbucket; Host not found curl: (3) [globbing] unmatched close brace/bracket at pos 1 curl: (1) Protocol 'https not supported or disabled in libcurl) following is my command: (curl -H "Authorization: GoogleLogin auth=DQAAALoAAAAYHvM_V7-FsOSN97zeXXb6-1111111111113E-4YNfjzFDQFg1111FXMfSmxzZ0FTm_knAnLVoczeI4IZVvaFIECcVsDEgw1Y6ZUPukyARA73WnOQPI3rPQxIH78JUgU1JdII-1wf2brQCXouv-_G1pOs88fn76tFFkYfAz6gp-OIbdS3cS7EBvgWx7rnmYEYupw0Gry9JAx9oiJv32oSgsKsDVJzJKy_8djwhvAm_yHwuvoDjy52cKeaCgBQLdGW4" -X POST -H "Content-type: application/json" -d'{"data": {"fields": [ {"type": "string", "id": "name", "mode": "REQUIRED"}, {"type": "integer", "id": "age", "mode": "REQUIRED"}, {"type":"float", "id": "weight", "mode": "REQUIRED"}, {"type": "boolean", "id": "is_magic", "mode": "REQUIRED"} ], "name": "edwhbucket/prediction_models/consumers" }}' 'https://www.googleapis.com/bigquery/v1/tables') please reply me asap i would be grateful to u.

