---
layout: post
header-img: img/default-blog-pic.jpg
---

# First look at Google BigQuery

In this blog I would primarily be talking about the Google BigQuery API. This has been a major step in the computing world to provide database as a service on cloud computing environment. BigQuery API has been designed to analyse huge amounts of data in really less timeframe. Interface of the service is quite straightforward, and involves HTTP calls to get data from the tables. Since Google BigTable is a NoSQL oriented database, the format of BigQuery doesn't support JOINS but is able to support lots of other features like grouping, ordering, ranges, rejection and so on.  _**Overview:**_ BigQuery API only provides a basic querying mechanism to the end users. There is no functionality that allows you to manipulate the data once uploaded into the BigTable structure. You can only append more data into the structure or fire [SELECT](http://code.google.com/apis/bigquery/docs/query-reference.html) queries against the data. This makes BigQuery engine more of an analytics tool over trillions of rows of data. BigTable is a part of Google's storage servers and can be run without the need of any additional hardware. Moreover, the API has been exposed via simple HTTP and JSON, which are easy to put in any application. One can leverage these benefits to cut down on the hardware costs of setting up high performance systems for analysis. The whole process of carrying out analysis on some dataset involves two basic steps: 

  1. Importing data into the BigTable structure (Through [Google Storage](http://code.google.com/apis/storage/) and import job)
  2. Running the queries against that data
_**Importing the data:**_ In order to analyse your data, you should do following steps to port your data into the Google BigTable system: 

  1. Ensure that you have Google Storage and BigQuery enabled account with Google.
  2. Upload the data in CSV format to Google storage using their [storage manager](https://sandbox.google.com/storage/m/) HTTP interface. Header information is not required. Only the columns of CSV must match the structure of the table as specified in subsequent steps.
  3. Use [cURL](http://curl.haxx.se/download.html) tool to get the authorization key which can be used in subsequent calls to the HTTP service exposed. Command to be executed is : 

> _curl -X POST -d accountType=GOOGLE -d Email=<email>@gmail.com -d Passwd=<password> -d service=ndev -d source=google-bigquery-manualimport-1 -H "Content-Type: application/x-www-form-urlencoded" https://www.google.com/accounts/ClientLogin_

This call will return an authorization key whose format is similar to : 

> DQAAAK8AAADlegSF4xMNrCgUrxFv3NFCYbBp4KCpdhOCOioIfJ7txXrRB9FYcvDAv7ooGvmaWoDCj_ SPHolxr9Q6gK8-cd4SR_QkIv3QcM_pQlVADtMWtv63hqzdz16KMZ1tDizeyOIe-vJNenAYqrjX4xVSTH2F _5Fy9fc1mXxL77BtKXFMlV0xqbX9swpqXXhk_Zqvw8K0fbBE2o6kkU3FW7NDN8IXLrhqC_VEEaKXGAD3-1JIyg

Just keep a note of the key as it will be used further. Down the line, I will refer to it as _AUTH_KEY_
  4. Create the table using following command. 

> _curl -H "Authorization: GoogleLogin auth=AUTH_KEY" -X POST -H "Content-type: application/json" -d '{"data":   {"fields": [    {"type": "string", "id": "name", "mode": "REQUIRED"},    {"type": "integer", "id": "age", "mode": "REQUIRED"},    {"type":"float", "id": "weight", "mode": "REQUIRED"},    {"type": "boolean", "id": "is_magic", "mode": "REQUIRED"}    ],   "name": "<bucket_name>/testtable"  }}' 'https://www.googleapis.com/bigquery/v1/tables'_

**bucket_name** is the name of bucket where you want the reference of your table to be created. Buckets are nothing but folders inside Google storage.
  5. After you have successfully added the table, import process needs to be triggered to port the uploaded CSV data into the BigTable structure. Following command will do the same : 

> _curl -H "Authorization: GoogleLogin auth=AUTH_KEY" -X POST -H "Content-type: application/json" -d '{"data": {"sources": [{"uri": "<bucket_name>/<data_file_name>.csv"}]}}' 'www.googleapis.com/bigquery/v1/tables/<bucket_name>%2Ftesttable/imports'_

    * Please note that **testtable** is the table name that you have created in step 4.
    * Response to this kind of a request is a JSON string which is of a format : {"data":"kind":"bigquery#import_id","table":"<bucket_name>/testtable","import":"<import_id>"}}
    * **import_id** is useful in getting the current state of the import process. It also returns errors in case it has encountered any. Any errors will just fail the import process and rollback everything.
  6. To know the status of your import process, just fire the following command: 

> _curl -X GET https://www.googleapis.com/bigquery/v1/tables/<bucket_name>%2Ftesttable/imports/<import_id> -H "Authorization: GoogleLogin auth=AUTH_KEY"_

_**Querying the data:**_ After the data is successfully ported, you can query the database using BigQuery. Just download the bqshell tool from [here](http://code.google.com/p/bigquery/) and build the code with all the required dependencies. The tool works on [python](http://www.python.org/) and has a detailed "how-to" to install it. This tool has a login console in which you can specify the username and password and query the sample datasets or your own datasets that you create using cURL. This tool is a parser to the JSON responses returned from the BigQuery API and displays them in a SQL output fashion. Alternatively, you can also use [cURL calls](http://code.google.com/apis/bigquery/docs/libraries.html#curl) to query the database and see the JSON response yourself. _**Conclusion :**_

  * BigQuery API shows good performance in scenarios where you have huge amounts of data that needs to be processed. Running any query on almost 28 million rows uploaded in a test data set gives back response in just 2-3 seconds. The time includes request post and response recieved times as well.
  * There are sample data sets provided by Google themselves, one of which contains 60 billion records and queries get executed in 4-8 seconds. For more reference, please visit the following link : http://code.google.com/apis/bigquery/docs/dataset-mlab.html
  * One of the drawbacks is that there is no way to insert data without the upload and import job process. One can also not delete any record from the table once inserted. In order to change the data, you will have to delete the table and do the complete import process again. One thing worth mentioning is that in case you only need to add data to an existing table, you can do so by firing another upload job for the CSV file containing additional data.

## Comments

**[Luca](#3420 "2010-11-30 18:15:35"):** Great article! Do you know if cURL calls could be done also with PHP curl commands? Or you must use bash scripts?

**[Rohit](#3429 "2010-12-02 09:32:08"):** @Luca, cURL is just a tool to make your life easy. You can also write a kind of driver using any programming language. Just take care of the headers and all. They should be in the right place in all the commands. As far as calling the API is concerned, even telnet should work if you are familiar with HTTP GET and PUT calls..

**[Shekhar Gulati](#3265 "2010-11-18 21:59:08"):** Informative Blog...thanks

**[Asjad Hassan](#6007 "2011-10-11 12:06:26"):** Hello Rohit, Great article regarding big query ,but i m facing problem in 4 step (i.e. Create the table using following command.) as i run the command i get following error (curl: (3) [globbing] unmatched brace at pos 9 curl: (3) [globbing] illegal character in range specification curl: (3) [globbing] unmatched brace at pos 7 curl: (6) Could not resolve host: string,; Host not found curl: (6) Could not resolve host: id; Host not found curl: (6) Could not resolve host: name,; Host not found curl: (6) Could not resolve host: mode; Host not found curl: (3) [globbing] unmatched close brace/bracket at pos 9 curl: (3) [globbing] unmatched brace at pos 7 curl: (6) Could not resolve host: integer,; Host not found curl: (6) Could not resolve host: id; Host not found curl: (6) Could not resolve host: age,; Host not found curl: (6) Could not resolve host: mode; Host not found curl: (3) [globbing] unmatched close brace/bracket at pos 9 curl: (3) [globbing] unmatched brace at pos 13 curl: (6) Could not resolve host: id; Host not found curl: (6) Could not resolve host: weight,; Host not found curl: (6) Could not resolve host: mode; Host not found curl: (3) [globbing] unmatched close brace/bracket at pos 9 curl: (3) [globbing] unmatched brace at pos 7 curl: (6) Could not resolve host: boolean,; Host not found curl: (6) Could not resolve host: id; Host not found curl: (6) Could not resolve host: is_magic,; Host not found curl: (6) Could not resolve host: mode; Host not found curl: (3) [globbing] unmatched close brace/bracket at pos 9 curl: (3) [globbing] unmatched close brace/bracket at pos 1 curl: (6) Could not resolve host: name; Host not found curl: (6) Could not resolve host: edwhbucket; Host not found curl: (3) [globbing] unmatched close brace/bracket at pos 1 curl: (1) Protocol 'https not supported or disabled in libcurl) following is my command: (curl -H "Authorization: GoogleLogin auth=DQAAALoAAAAYHvM_V7-FsOSN97zeXXb6-1111111111113E-4YNfjzFDQFg1111FXMfSmxzZ0FTm_knAnLVoczeI4IZVvaFIECcVsDEgw1Y6ZUPukyARA73WnOQPI3rPQxIH78JUgU1JdII-1wf2brQCXouv-_G1pOs88fn76tFFkYfAz6gp-OIbdS3cS7EBvgWx7rnmYEYupw0Gry9JAx9oiJv32oSgsKsDVJzJKy_8djwhvAm_yHwuvoDjy52cKeaCgBQLdGW4" -X POST -H "Content-type: application/json" -d'{"data": {"fields": [ {"type": "string", "id": "name", "mode": "REQUIRED"}, {"type": "integer", "id": "age", "mode": "REQUIRED"}, {"type":"float", "id": "weight", "mode": "REQUIRED"}, {"type": "boolean", "id": "is_magic", "mode": "REQUIRED"} ], "name": "edwhbucket/prediction_models/consumers" }}' 'https://www.googleapis.com/bigquery/v1/tables') please reply me asap i would be grateful to u.

