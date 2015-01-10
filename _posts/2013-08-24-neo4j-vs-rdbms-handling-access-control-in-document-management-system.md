---
layout: post
header-img: img/default-blog-pic.jpg
author: gagrawal
description: 
post_id: 16967
created: 2013/08/24 11:26:32
created_gmt: 2013/08/24 06:26:32
comment_status: open
---

# Neo4j vs RDBMS : Handling Access Control in Document Management System

<p><a href="http://en.wikipedia.org/wiki/Graph_database" target="_blank">Graph Databases</a> are great at storing connected data. In this blog post I am going to cover how <a href="http://www.neo4j.org/" target="_blank">Neo4j</a>, most widely used graph database, can be used in Document Management System. The most complex part of document management system is the Access Control Management. When a user logs in, he/she should be able to view only folders/document that they have access for. Also if a user searches for documents, he  should be shown only those documents for which he has access. I will first start with RDBMS and show how complex it is to model this requirement in RDBMS. And then will move to Graph Database modeling for same requirement and we will see how easy and effortless it is to implement this with Neo4j and <a href="http://docs.neo4j.org/chunked/stable/cypher-query-lang.html" target="_blank">Cypher</a> Query Langague.<!--more--></p>
<p>Let's take a scenario and create it's model in RDBMS. Say we have 2 users, "User1" and "User2". Both are respectively mapped to roles "Role1" and "Role2". Suppose we have following hierarchy of folders and documents</p>
<p><span style="color: #3366ff;"><em>Project1 -&gt; Requirements Doc1 -&gt; Doc1.pdf</em></span></p>
<p><span style="color: #3366ff;"><em>Project2 -&gt; Requirements Doc2 -&gt; Doc2.pdf</em></span></p>
<p>Let's say, all users with role "Role1" has access to view folder "Project1" and it's sub-folders/documents. And say all users with "Role2" will have access to view folders "Project1" and "Project2" and it's sub-folders/documents. We also assume that if a user with a role has access to a folder, then they will also have access to all it's sub-folders and documents unless a specific configuration restricts a user to access it. We can represent this requirement in following diagram.</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2013/08/user-access-1.jpg"><img class="alignnone size-full wp-image-17036" alt="user access (1)" src="http://xebee.xebia.in/wp-content/uploads/2013/08/user-access-1.jpg" width="431" height="280" /></a>
<div style="height: 25px;"></div>
According to this requirement, when "User1" performs a search for documents, he should be able to view only "Doc1.pdf". However when "User2" performs similar search, he should be able to view both "Doc1.pdf" and "Doc2.pdf". Now let's try to create RDBMS model for this requirement. In RDBMS world, we normally map entities to tables. So for this particular requirement, we have entities like User, Role and Document (file or folder). Accordingly we will end up with following tables.</p>
<p><strong><span style="color: #993300;">User</span></strong> : To store user ids and their details</p>
<p><strong><span style="color: #993300;">Role</span></strong> : To store role details</p>
<p><strong><span style="color: #993300;">Document</span> </strong>: To store file and folder details and has reference to their parent for hierarchy in which they can be navigated. Here we will only keep reference of immediate parent.</p>
<p><strong><span style="color: #993300;">User_Role</span> </strong>: To assign Roles to Users</p>
<p><span style="color: #993300;"><strong>Document_Role</strong> </span>: To configure what all Roles can access a Document (file or folder).</p>
<p>Here is the sample data to represent our requirement.
<div style="height: 25px;"></div>
<table border="0" frame="VOID" rules="NONE" cellspacing="0"><colgroup> <col width="86" /> <col width="86" /></colgroup>
<tbody>
<tr>
<td align="LEFT" bgcolor="#99CCFF" width="86" height="17"><b>Table : User</b></td>
<td align="LEFT" width="86"></td>
</tr>
<tr>
<td align="LEFT" bgcolor="#E6E6E6" height="17"><b>id</b></td>
<td align="LEFT" bgcolor="#E6E6E6"><b>name</b></td>
</tr>
<tr>
<td align="LEFT" bgcolor="#CCCCCC" height="17">1</td>
<td align="LEFT" bgcolor="#CCCCCC">User1</td>
</tr>
<tr>
<td align="LEFT" bgcolor="#CCCCCC" height="17">2</td>
<td align="LEFT" bgcolor="#CCCCCC">User2</td>
</tr>
</tbody>
</table>
<div style="height: 25px;"></div>
<table border="0" frame="VOID" rules="NONE" cellspacing="0"><colgroup> <col width="86" /> <col width="86" /></colgroup>
<tbody>
<tr>
<td align="LEFT" bgcolor="#99CCFF" width="86" height="17"><b>Table : Role</b></td>
<td align="LEFT" width="86"></td>
</tr>
<tr>
<td align="LEFT" bgcolor="#E6E6E6" height="17"><b>id</b></td>
<td align="LEFT" bgcolor="#E6E6E6"><b>name</b></td>
</tr>
<tr>
<td align="LEFT" bgcolor="#CCCCCC" height="17">1</td>
<td align="LEFT" bgcolor="#CCCCCC">Role1</td>
</tr>
<tr>
<td align="LEFT" bgcolor="#CCCCCC" height="17">2</td>
<td align="LEFT" bgcolor="#CCCCCC">Role2</td>
</tr>
</tbody>
</table>
<div style="height: 25px;"></div>
<table border="0" frame="VOID" rules="NONE" cellspacing="0"><colgroup> <col width="86" /> <col width="131" /> <col width="131" /> <col width="86" /></colgroup>
<tbody>
<tr>
<td colspan="2" align="LEFT" bgcolor="#99CCFF" width="217" height="17"><b>Table : Document</b></td>
<td align="LEFT" width="131"></td>
<td align="LEFT" width="86"></td>
</tr>
<tr>
<td align="LEFT" bgcolor="#E6E6E6" height="17"><b>id</b></td>
<td align="LEFT" bgcolor="#E6E6E6"><b>name</b></td>
<td align="LEFT" bgcolor="#E6E6E6"><b>type</b></td>
<td align="LEFT" bgcolor="#E6E6E6"><b>parent_id</b></td>
</tr>
<tr>
<td align="LEFT" bgcolor="#CCCCCC" height="17">1</td>
<td align="LEFT" bgcolor="#CCCCCC">Project1</td>
<td align="LEFT" bgcolor="#CCCCCC">FOLDER</td>
<td align="LEFT" bgcolor="#CCCCCC"></td>
</tr>
<tr>
<td align="LEFT" bgcolor="#CCCCCC" height="17">2</td>
<td align="LEFT" bgcolor="#CCCCCC">Requirement Docs1</td>
<td align="LEFT" bgcolor="#CCCCCC">FOLDER</td>
<td align="LEFT" bgcolor="#CCCCCC">1</td>
</tr>
<tr>
<td align="LEFT" bgcolor="#CCCCCC" height="17">3</td>
<td align="LEFT" bgcolor="#CCCCCC">Doc1.pdf</td>
<td align="LEFT" bgcolor="#CCCCCC">FILE</td>
<td align="LEFT" bgcolor="#CCCCCC">2</td>
</tr>
<tr>
<td align="LEFT" bgcolor="#CCCCCC" height="17">4</td>
<td align="LEFT" bgcolor="#CCCCCC">Project2</td>
<td align="LEFT" bgcolor="#CCCCCC">FOLDER</td>
<td align="LEFT" bgcolor="#CCCCCC"></td>
</tr>
<tr>
<td align="LEFT" bgcolor="#CCCCCC" height="17">5</td>
<td align="LEFT" bgcolor="#CCCCCC">Requirement Docs2</td>
<td align="LEFT" bgcolor="#CCCCCC">FOLDER</td>
<td align="LEFT" bgcolor="#CCCCCC">4</td>
</tr>
<tr>
<td align="LEFT" bgcolor="#CCCCCC" height="17">6</td>
<td align="LEFT" bgcolor="#CCCCCC">Doc2.pdf</td>
<td align="LEFT" bgcolor="#CCCCCC">FILE</td>
<td align="LEFT" bgcolor="#CCCCCC">5</td>
</tr>
</tbody>
</table>
<div style="height: 25px;"></div>
<table border="0" frame="VOID" rules="NONE" cellspacing="0"><colgroup> <col width="86" /> <col width="86" /></colgroup>
<tbody>
<tr>
<td colspan="2" align="LEFT" bgcolor="#99CCFF" width="171" height="17"><b>Table : User_Role</b></td>
</tr>
<tr>
<td align="LEFT" bgcolor="#E6E6E6" height="17"><b>user_id</b></td>
<td align="LEFT" bgcolor="#E6E6E6"><b>role_id</b></td>
</tr>
<tr>
<td align="LEFT" bgcolor="#CCCCCC" height="17">1</td>
<td align="LEFT" bgcolor="#CCCCCC">1</td>
</tr>
<tr>
<td align="LEFT" bgcolor="#CCCCCC" height="17">2</td>
<td align="LEFT" bgcolor="#CCCCCC">1</td>
</tr>
<tr>
<td align="LEFT" bgcolor="#CCCCCC" height="17">2</td>
<td align="LEFT" bgcolor="#CCCCCC">2</td>
</tr>
</tbody>
</table>
<div style="height: 25px;"></div>
<table border="0" frame="VOID" rules="NONE" cellspacing="0"><colgroup> <col width="98" /> <col width="86" /></colgroup>
<tbody>
<tr>
<td colspan="2" align="LEFT" bgcolor="#99CCFF" width="183" height="17"><b>Table : Document_Role</b></td>
</tr>
<tr>
<td align="LEFT" bgcolor="#E6E6E6" height="17"><b>document_id</b></td>
<td align="LEFT" bgcolor="#E6E6E6"><b>role_id</b></td>
</tr>
<tr>
<td align="LEFT" bgcolor="#CCCCCC" height="17">1</td>
<td align="LEFT" bgcolor="#CCCCCC">1</td>
</tr>
<tr>
<td align="LEFT" bgcolor="#CCCCCC" height="17">4</td>
<td align="LEFT" bgcolor="#CCCCCC">2</td>
</tr>
</tbody>
</table>
<div style="height: 25px;"></div></p>