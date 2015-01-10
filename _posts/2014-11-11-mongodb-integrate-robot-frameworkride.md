---
layout: post
header-img: img/default-blog-pic.jpg
---

# MongoDB with Robot Framework(RIDE)

### Installation instruction of MongoDB on Windows

There are three different version of MongoDB for Windows:

  1. Windows 64-bit
  2. Windows 32-bit
  3. Windows Server 2008

To find which version of Windows you have, use the following command in the cmd: **“wmic os get osarchitecture”**

Download the MongoDB from following location as per your OS Architecture:  
<http://www.mongodb.org/downloads?_ga=1.99989726.1358002168.1411966340>

### Set up the MongoDB environment

After the installation of MongoDB on system it requires “DBPATH” so that it can use to run the Mongodb services.

Create a directory where to store the dbs name. Example: In cmd hit the following command  
C:\Program Files\MongoDB > md \data\db (Default location)

Or for any location give the path and create the directory. For example:  
> mkdir D:\data\db (user defined location)

Now start the MongoDB server on the desire dpPath

  * _For default location_ \- C:\Program Files\MongoDB\bin\mongod.exe
  * _For user defined location_ \- C:\Program Files\MongoDB\bin\mongod.exe --dbpath D:\data\db
  * If your path includes spaces, enclose the entire path in double quotes. For example:  
C:\mongodb\bin\mongod.exe --dbpath "d:\test\mongo db data"

**Mongod.exe** is the file to start the MongoDB server on the default host & port(host:- localhost & port : - 27017

**Mongo.exe** is an interactive interface to MongoDB, which provides a powerful interface for users to test queries and operations directly with the database.

### Create a DB in mongoDB:

Start the mongo to interact with Mongo Data bases. Command on cmd: c:\Program File\MongoDb\bin\mongo.exe  
![](https://lh3.googleusercontent.com/Hb8dvKM_9v-TbTaJf4WHeSYeLOKTvz4FNIf99HOAr8cT_G8BxAyTazeakWNfJrrkgYa6YiNXFXKTuPMJi5wLV7ptp_6mSUtVQoyC90wbHyyhO8vDL95gwHEASK31aR4xqD4S)

Before creating a new DB first check how many db present on the path. Command:

> shows dbs  
![](https://lh3.googleusercontent.com/v6mdWZp9s1uB2aO7IO3jRfPiQHRNqvfhJm9LI38FfmbOk1lecu8uZuRYnHqoJ77-LsEDNoA0lXVB8B-OMF8oTnnt8xfz8Be0eFI6iLslwzdMEp_AWTTKJzoWDNu0B7H-o-2T)

  1. use <new-DBName> (E.g. use sampledb). Note: At that time it does not save the DB.
  2. Insert a document into a collection  
a. Create a document – command : a = {x : “Learning DB” }  
b. Insert in “TEST” collection. Command : db.TEST.insert(a). Remember TEST is also a new collection which too create automatically once insert the document into it.  
c. Check db using command: show dbs. It will show new db also in the list i.e. “sampled”. Note: After inserting a document in any collection then it automatically save collection and DB.![](https://lh4.googleusercontent.com/DfM1Y6-jKFlBQRRl8NvqRPGp9y_LUnbOD4mQellW__9Dix3Kw0e4FdtwI7kzb-j2pqwaWyPkjLplMFAIXfKGiIwsvUljh2d2pOyLKRIXBiBWGITBvqZuO6zmsu3H0S4rKsGU)

### Integrate MongoDB with RIDE

There is an external library exist of MongoDB for robot framework. The prerequisite to integrate the MongoDB library with RF(Robot Framework) is that “pymongo” library should be installed.

  1. Installation Instruction for pymongo  
a. Using pip: _“pip install pymongo”_  
b. Alternative way is download the source code of [pymongo ](https://pypi.python.org/pypi/pymongo/)and use the command : _“python setup.py install”_

  2. Installation Instruction for MongoDBLibrary  
a. Using pip: _“pip install robotframework-MongoDBLibrary”_  
b. Alternative way is to download the source code of [MongoDBLibrary ](https://github.com/iPlantCollaborativeOpenSource/Robotframework-MongoDB-Library)and use the command: _“python setup.py install”_

After installation of both include the library in RIDE  
Steps to perform the same:

  1. Open ride using the command ride.py
  2. Create new project from FIle option → New Project as Directory
  3. Now select the folder go to “settings”  
![](https://lh3.googleusercontent.com/MLFXvFM0xqLfaDdsq72TZJQlWubPOFJPnSVeZrODFZcQZTKVzqTKs5kU_eClyAejqUucYEZAeuHB2PInGFAjpdTyJKf5GWk9WySFhILlN3eX0s4nz0yRZid23r94y4sPdQ)  

  4. Under setting click on Library button and enter the mongoDB library name i.e. “MongoDBLibrary”  
![](https://lh6.googleusercontent.com/ZnBaX8YqJoHWvo_k91V_ZebZMEyqFMqA-awFejPRV_k1u7sJ_72lYjfQhM1L1Mnu8NSqudVipkB25WtbMEAQGZT_lD2f9K100KvK92Oxq0suRVeBtl1hXaTkzz7Q875UTA)
  5. After click ok it will look like:  
![](https://lh4.googleusercontent.com/s0VlVAplhiZFuyrFClQyevFDdqsv9uBKT5MaRecBd0eXyL4MqBzrl5I7y018G3oOVzgYOUpynEA1V5utJRZlNPmc3ll82GMlAoVwUQRzXCXdizFWHAw395dNRfjfz019rg)

Note: 1. It is Case sensitive  
2\. If name comes in Red Color thats means its not imported correctly in RIDE.  
For example change the case of library name from “MongoDBLibrary” to “mongoDBLibrary” then it will show like:  
![](https://lh3.googleusercontent.com/dFAbVsDpY7Buh0cGPeRt39OZjbp8n2uFSjFDbb6DrXtpYbLcNOuw9-QRH009zeD9LGi8XciOxYUXl1NOXEGWz0gcClwdEzSB9zF5sc5yi_AXIX6ZULLoZqk2hTPEvd6s5g)

After importing the MongoDB library check that all the related function/keywords are coming are not. For that follow the below steps:

  * After importing click F5 key. It will open the search keyword dialog.
  * Under the dialog box there is source dropdown. Open it and it will show the list of library associated with project. Check it should have “MongoDBLibrary” and select it.  
![](https://lh4.googleusercontent.com/YKdU1VqGoYMbK1x9df-ri3aqAsFI2v7Xe8QGLdoAWnAQu0zGyiT0Y-GPLpuuqqSMx7FmqP9SC7UxIZBs3qOa1xjplxlj_O1MWPXOwrOE-H-TrJZkMikdNrY6N2TzNQ6_HA)  


Now use the keywords in your test cases.

### MongoDB Keywords:

_**Keyword**_

**Arguments**

**Description**

_Connect To Mongodb_
dbHost=localhost, dbPort=27017, dbMaxPoolSize=10, dbNetworkTimeout=None, dbDocClass=<type 'dict'>, dbTZAware=False
Loads pymongo and connects to the MongoDB host.

_Disconnect From Mongodb_
NA
Disconnect mongoDB server

_Drop Mongodb Collection_
dbName, dbCollName
Delete the mention collection from the mention DB

_Drop Mongodb Database_
dbDelName
Delete the mention DB from the current DBPath from which is connected.

_Get Mongodb Collection Count_
dbName, dbCollName
Returns the number of records from the mention collection.

_Get Mongodb Collections_
dbName
Returns the list of collections from the mention DB

_Get Mongodb Databases_
NA
Returns the list of Data bases from the current connected DB

_Remove Mongodb Records_
dbName, dbCollName, recordJSON
Remove some of the records from the specified collection as based on the JSON entered.

_Retrieve All Mongodb Records_
dbName, dbCollName, returnDocuments=False
Return all the mongoDB records from the specified collection

_Retrieve Mongodb Records With Desired Fields_
dbName, dbCollName, recordJSON, fields, return__id=True, returnDocuments=False
Returns number of records on the basis of field mention from the specified collection

_Retrieve Some Mongodb Records_
dbName, dbCollName, recordJSON, returnDocuments=False
Returns number of records on the basis of JSON entered in the specified collection

_Save Mongodb Records_
dbName, dbCollName, recordJSON
Used to save new record or update existing record from the specified DB collection.

**Example of Using MongoDB keywords in RIDE:**  
**Prerequisite** : Start 

Cheaper. I came it [levitra brand pills for sale](http://tuxwearhouseweddings.com/rergh/levitra-brand-pills-for-sale) the not buy hair [flowmax without prescription](http://shopglean.com/loijx/flowmax-without-prescription) 6 longer servings <http://www.penickvillagefoundation.org/jhpm/trental-400-mg-retailer> soft plastic had. My little [walmart viagra prices walmart pharmacy](http://securefuturesil.com/lnqjx/walmart-viagra-prices-walmart-pharmacy/) lotion. I it it. Hard. Never <http://www.bryancwatkins.com/idnl/generic-seroquel-online> Around time [almased turbo diet forum](http://www.bryancwatkins.com/idnl/almased-turbo-diet-forum) naturally once [cialis 1 to 2 days shipping](http://www.southsideheating.com/bhtr/cialis-1-to-2-days-shipping) it my almost <http://shopglean.com/loijx/buy-soft-generic-viagra> long fragrance. I [novaldex pay by paypal](http://ravenmccoyphotography.com/exwsk/novaldex-pay-by-paypal/) I and cloying days [buy aldactone no rx overnight shipping](http://freeofpain.org/azf/buy-aldactone-no-rx-overnight-shipping.html) bought hope always on [levitra without prescription walmart](http://tuxwearhouseweddings.com/rergh/levitra-without-prescription-walmart) all a for.

the Mongo DB server on the defined DB path. For example check the below snapshot: ![StartMongoServer](/wp-content/uploads/2014/11/StartMongoServer.jpg)

Open RIDE using command on cmd: ride.py

![MongoResult](/wp-content/uploads/2014/11/MongoResult-1024x949.jpg)

### How to create any New Library of Robot  


The Simplest way of creating user defined library for Robot Framework:

  1. Create a python file with function/s and save it with extension .py in Lib\site-packages
  2. Open ride → Go to the Test Suite → Open Setting → Add a Library with the file name
  3. After entering the name check it should not show in red.

Example:  
Create a python file with basic function like:  
def play_Robot():  
print "Pressed Play Robot Button"

save it with any name which treat as Library Name in Robot Framework. For above let save it as “ReusableModule.py”  
Open Ride and add the Library with name “ReusableModule”  
![](https://lh3.googleusercontent.com/CvZVIty6MyF2lExD0NP13m8975jyLCSgyqlqfPQBy2hw1CajV5qt9Et2KWR_C-ku3jv_XkSPl6C6PLhTxqukXiHJZ9vmua8gGzF7NV6UDAmFqXNPOeM-yXr-q5HaobA25A)

Now in test case use the functions mention in own defined library.

### How to create user defined keywords of MongoDB Library

First go the location where default MongoDBLibrary installed. On windows by default location is <PythonPath>\Lib\site-packages. Generally the path is : "C:\Python27\Lib\site-packages"  
Open MongoDBLibrary directory, it shows mainly 4 files:

  1. __init__.py
  2. mongo_connection_manager.py
  3. mongoquery.py
  4. version.py  
Might also have there compiled files too.

Now to create user defined function create a new python file and import following library to start creating functions:

  1. import pymongo
  2. from pymongo import MongoClient
  3. import json
  4. from bson.objectid import ObjectId

After that create a function as shown below:

![MongoDBUserDefinedFunction](/wp-content/uploads/2014/11/MongoDBUserDefinedFunction.jpg)

Now Open __init__.py file and import the user defined library in it:  
![MongoDBUserDefinedFunctionIntegrate](/wp-content/uploads/2014/11/MongoDBUserDefinedFunctionIntegrate.jpg)

Now open Ride import **MongoDBLibrary **and look under the function list:  
![UserDefinedKeywordInList](/wp-content/uploads/2014/11/UserDefinedKeywordInList.jpg)

And we can the use the created keyword like:  
![UserDefinedKeyword](/wp-content/uploads/2014/11/UserDefinedKeyword.jpg)

I hope it will be helpful to integrate MongoDB with Robot Framework.  
Happy Testing!!