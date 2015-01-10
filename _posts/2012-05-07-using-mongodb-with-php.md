---
layout: post
header-img: img/default-blog-pic.jpg
---

# Using MongoDB with PHP

I was exploring the feasibility of using MongoDB with PHP and found that it is a fairly simple process to do the same. For web development I use following system setup:   


  * Operating System : Microsoft Windows Vista 64 bit
  * PHP : 5.3.8
  * Web server : Apache/2.2.21
So in this blog we will see how we can configure and use MongoDB with PHP. MongoDB is the most popular NOSQL database written in C++. For more information on MongoDB, you can visit the following URL: <http://www.mongodb.org/display/DOCS/Home>

### Overview

Our approach can be summarized in the following steps: 

  1. Download and install MongoDB.
  2. Configure MongoDB with PHP.
  3. Use MongoDB from inside the PHP scripts.
So lets begin.. 

#### 1\. Download and install MongoDB

For downloading the MongoDB, visit the following URL: <http://www.mongodb.org/downloads> And choose the appropriate option to download the MongoDB. e.g if your Operating System version is 32 bit or 64 bit, then choose accordingly. After your download is complete, extract the archive to a specific location on your local hard disk drive. In this blog, we are extracting it to D:\MongoDB. In the extracted path, you will find a directory named "bin". Inside this directory, among the other files you will find the following two files too: 

  1. mongod.exe
  2. mongo.exe
MongoDB stores the database files in the directory _"\data\db"_ by default. So we need to create the directory _"D:\data\db"_, as we have extracted the archive in "D:\MongoDB". Now Open the Command Prompt and navigate to d:\MongoDB\bin. Type mongod.exe and press ENTER key. This will start the MongoDB server. And if everything is fine you will see the output similar to the one shown in the image below:  Do not close this terminal window as it will shutdown the MongoDB server. So up to this point we have successfully installed MongoDB on our system. 

#### 2\. Configure MongoDB with PHP

Now our next step is to configure our PHP enabled web server so that it can communicate with MongoDB server. This way we can use MongoDB from inside our PHP scripts. First of all, we need to download the _php_mongo.dll_ file from the following URL: <http://www.php.net/manual/en/mongo.installation.php#mongo.installation.windows>. So corresponding to your version of PHP installed, download the appropriate archive. Extract the archive and you will get a file named _php_mongo.dll_. Now copy this file and paste it to the  directory available inside the  directory where your PHP is installed. e.g. if you are using XAMPP this path can be like this _"D:\xampp1.7.7\php\ext"_ Now we need to specify the extension which we just saved in the  directory in the file name _"php.ini"_. So open the _"php.ini"_ file in your favorite text-editor and paste the following line: [sourcecode language="php"] extension=php_mongo.dll [/sourcecode] and then save the file. Now take the web server restart to apply the changes. 

#### 3\. Use MongoDB from inside the PHP scripts

In this step we will write a PHP script that will create a database named "school" and a collection named "students". So create a php file named "create_db.php" in the web root directory of your web server and write the following code in it. [sourcecode language="php"] //defining the hostname $host = 'localhost'; //defining the database name $databaseName = 'school'; //defining the collection name $collectionName = "students"; //populating the data for the collection students $items = array(array("id" => '1', "name" => 'John Smiths', 'address' => 'AY 22 Street', 'sports' => 'basketball'), array("id" => '2', "name" => 'Catherine Wicks', 'address' => '401 C 23rd Street', 'academic' => 'junior physics scholar'), array("id" => '3', "name" => 'Josh Hopkins', 'address' => '3rd Block 81B CA, City', 'misc' => 'spelling bee champion'), array("id" => '4', "name" => 'Bryan Alomera', 'address' => '2nd Floor, Yen Shui Villas', ''), array("id" => '5', "name" => 'Sunny Kamps', 'address' => 'A51, High Rise Towers') ); try { //making a connection to the MongoDB server $connection = new Mongo($host); //creating a database named 'school' $database = $connection->selectDB($databaseName); //creating a collection named 'students' inside the database 'school' $collection = $connection->selectCollection($databaseName, $collectionName); //inserting the data into the collection 'students' foreach ($items as $item) { /* inserting each item one by one. The option safe => true ensures that * each insert will wait until all previous insert operations are done * */ $collection->insert($item, array('safe' => true)); } } catch (MongoException $e) { print "Exception:\n"; die($e->getMessage() . "\n"); } [/sourcecode] As explained in the source code file above, the safe => true option ensures that each insert will wait for all previous insert operations to complete. Also, e.g if you have a unique index defined on "id" field, then if this option is enabled, the script will generate an exception message that can't insert duplicate records with same value for "id". Now, open another command prompt window and navigate to "D:\mongoDB\bin\". Type mongo.exe and press ENTER key. As I have mentioned before, it is necessary to open the separate command prompt window for "mongo.exe", so do not close the command prompt window where we have started the MongoDB server by running the "mongod.exe", as closing this window will shutdown the MongoDB server. After you have typed the command mongo.exe and pressed ENTER key, you will be logged into the MongoDB command line shell. From this shell you can create new databases, create new collections, query the database etc. Our purpose of logging into the shell is to verify whether the records inserted from our "create_db.php" script have been successfully inserted or not. So we will first type the following command: [sourcecode language="php"] use school; [/sourcecode] This command will select the database named "school". Then we will type the following command to see the contents of the collection named "students". [sourcecode language="php"] db.students.find(); [/sourcecode] This command will output the contents of the collection named "students". You should see the output similar to the one shown in the image below:  Now, our next step is to query the MongoDB from inside the php script to fetch the documents from the collection 'students' which we have just created from the "create_db.php" script. So again create a file named "query_db.php" in the web root directory of your web server. And write the following code inside it: [sourcecode language="php"] //defining the hostname $host = 'localhost'; //defining the database name $databaseName = 'school'; //defining the collection name $collectionName = "students"; try { //making a connection to the MongoDB server $connection = new Mongo($host); /* * selecting the already existing database named 'school'. If this database