---
layout: post
header-img: img/default-blog-pic.jpg
---

# Neo4j vs RDBMS : Handling Access Control in Document Management System

[Graph Databases](http://en.wikipedia.org/wiki/Graph_database) are great at storing connected data. In this blog post I am going to cover how [Neo4j](http://www.neo4j.org/), most widely used graph database, can be used in Document Management System. The most complex part of document management system is the Access Control Management. When a user logs in, he/she should be able to view only folders/document that they have access for. Also if a user searches for documents, he  should be shown only those documents for which he has access. I will first start with RDBMS and show how complex it is to model this requirement in RDBMS. And then will move to Graph Database modeling for same requirement and we will see how easy and effortless it is to implement this with Neo4j and [Cypher](http://docs.neo4j.org/chunked/stable/cypher-query-lang.html) Query Langague. Let's take a scenario and create it's model in RDBMS. Say we have 2 users, "User1" and "User2". Both are respectively mapped to roles "Role1" and "Role2". Suppose we have following hierarchy of folders and documents _Project1 -> Requirements Doc1 -> Doc1.pdf_ _Project2 -> Requirements Doc2 -> Doc2.pdf_ Let's say, all users with role "Role1" has access to view folder "Project1" and it's sub-folders/documents. And say all users with "Role2" will have access to view folders "Project1" and "Project2" and it's sub-folders/documents. We also assume that if a user with a role has access to a folder, then they will also have access to all it's sub-folders and documents unless a specific configuration restricts a user to access it. We can represent this requirement in following diagram. ![user access \(1\)](/wp-content/uploads/2013/08/user-access-1.jpg)

According to this requirement, when "User1" performs a search for documents, he should be able to view only "Doc1.pdf". However when "User2" performs similar search, he should be able to view both "Doc1.pdf" and "Doc2.pdf". Now let's try to create RDBMS model for this requirement. In RDBMS world, we normally map entities to tables. So for this particular requirement, we have entities like User, Role and Document (file or folder). Accordingly we will end up with following tables. **User** : To store user ids and their details **Role** : To store role details **Document **: To store file and folder details and has reference to their parent for hierarchy in which they can be navigated. Here we will only keep reference of immediate parent. **User_Role **: To assign Roles to Users **Document_Role** : To configure what all Roles can access a Document (file or folder). Here is the sample data to represent our requirement. 

**Table : User**

**id**
**name**

1
User1

2
User2

**Table : Role**

**id**
**name**

1
Role1

2
Role2

**Table : Document**

**id**
**name**
**type**
**parent_id**

1
Project1
FOLDER

2
Requirement Docs1
FOLDER
1

3
Doc1.pdf
FILE
2

4
Project2
FOLDER

5
Requirement Docs2
FOLDER
4

6
Doc2.pdf
FILE
5

**Table : User_Role**

**user_id**
**role_id**

1
1

2
1

2
2

**Table : Document_Role**

**document_id**
**role_id**

1
1

4
2