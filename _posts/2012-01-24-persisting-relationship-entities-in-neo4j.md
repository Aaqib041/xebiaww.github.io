---
layout: post
header-img: img/default-blog-pic.jpg
author: sunil.inteti
description: 
post_id: 10920
created: 2012/01/24 01:33:31
created_gmt: 2012/01/23 20:33:31
comment_status: open
---

# Persisting relationship entities in Neo4j

<p><a href="http://neo4j.org/">Neo4j</a> is a high-performance, NOSQL graph database with all the features of a mature and robust database. In Neo4j data gets stored in nodes connected to each other by relationship entities that carry its own properties. These relationships are very important in graphs and helps to traverse the graph and make decisions. This blog discusses the two ways to persist a relationship between nodes and also the scenario's which suits their respective usage. <a href="http://neo4j.org/spring/">Spring-data-neo4j</a> by springsource gives us the flexibility of using the spring programming model when working with neo4j database. The code examples in this blog will be using spring-data-neo4j.
<!--more-->
Let's consider a case where we have two NodeEntities Employee and Project. A Employee has a role in a project which is a relationship entity.
Here is the code snippet for these entities and the relationship between them.
[sourcecode lang="java"]
@NodeEntity
public class Employee {
    @GraphId
    @Indexed
    private Long nodeId;</p>
<pre><code>@Indexed
private String name;

@RelatedToVia(elementClass = Role.class, type=&amp;quot;WORKED_IN&amp;quot;, direction=Direction.BOTH)
private Set&amp;lt;Role&amp;gt; roles;
</code></pre>
<p>}</p>
<p>@NodeEntity
public class Project {</p>
<pre><code>@GraphId
@Indexed
private Long nodeId;

@Indexed
private String name;

@RelatedTo( elementClass=Employee.class, type=&amp;quot;WORKED_IN&amp;quot;, direction=Direction.BOTH)
private Set&amp;lt;Employee&amp;gt; teamMembers;
</code></pre>
<p>}</p>
<p>@RelationshipEntity(type=&quot;WORKED_IN&quot;)
public class Role {</p>
<pre><code>@GraphId
private Long nodeId;

@Indexed
private String name;

@StartNode
private Employee employee;

@EndNode
private Project project;
</code></pre>
<p>}
[/sourcecode]
1) First way to persist relationship : Using the Neo4jTemplate class to create relationship. For example
[sourcecode lang="java"]
Role stint = template.createRelationshipBetween(sunil, beachbody, Role.class,
                                                                      &quot;WORKED_IN&quot;, false);
template.save(stint);
 [/sourcecode]
createRelationship method takes the two node entities and the relationship class and relation type defined by the RelationshipEntity and a boolean value to allow/disallow duplicate relationships.</p>
<p>2) Second way is to instantiate the role and add it to a employee entity and persist the employee entity. Persisting the entity also persists the Role entity too, thus establishing the link between employee and project nodes.
[sourcecode lang="java"]
        Employee sunil = new Employee(&quot;Sunil&quot;);
        sunil = employeeRepo.save(sunil);
        Project beachbody = new Project(&quot;Beachbody&quot;);
        Project project = projectRepo.save(beachbody);</p>
<pre><code>    Role stint = sunil.workedIn(beachbody, &amp;quot;Java developer&amp;quot;);
    template.save(sunil);
</code></pre>
<p>[/sourcecode]</p>
<p>It's curious to know by a simple unit test that the second way is performing almost 30 times better than the first way in persisting just the relationship. I looked at the source code for both the ways, I noticed that the first way fetches the persistent state of both the start and the end nodes and then tries to establish a relation between the two. The second option just tries to create the relation directly on the employee which is already persisted. Clearly given a scenario that we have two persisted nodes and we need to create the relationship we must always go with the option2 in my opinion. This can greatly improve the performance. The only scenario that I can think of using the first option can be that we have to allow duplicate relationships between nodes which can generally happen when relationship entities are very rich ie have a lot of properties.</p>

## Comments

**[Santiago Baldrich](#7238 "2012-01-30 22:08:37"):** I'm having a little trouble trying to create a relationship between two already persisted entities. The case is like this: I've got some users and activities, both annotated with @NodeEntity. I've created 10 users and 10 activities using a repository for each type, and now want to create a relation "ASSISTS" between say user 0 and activity 9. When trying to add the activity to the set of activities on the user entity, I get a NotInTransaction exception. Could you please help me?

**[Sunil Prakash Inteti](#7253 "2012-01-31 10:03:37"):** Santiago, this exception occurs when some mutating operations that requires a transaction are performed when no transaction is available. My guess is the relationship creation part is not under any trnasaction. So when you add a new assist relationship make sure you put that in transaction programatically. Transaction tx = graphDb.beginTx(); try { ... // any operation that works with the node space tx.success(); } finally { tx.finish(); } Hope this helps :)

