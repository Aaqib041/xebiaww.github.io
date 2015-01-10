---
layout: post
header-img: img/default-blog-pic.jpg
author: vrawat
description: 
post_id: 9727
created: 2011/09/20 15:52:12
created_gmt: 2011/09/20 10:52:12
comment_status: open
---

# Handling DBRefs in MongoDB with Spring Data

<p>Recently I was watching a presentation titled <a href="http://www.infoq.com/presentations/Why-I-Chose-MongoDB-for-Guardian" target="_blank">Why I Chose MongoDB for guardian.co.uk</a> by Mat Wall. I thought of giving a look to mongoDB. Those who dont know what is mongoDB, well, its a document oriented database with some advantages over other relational databases. For more info you can have a look at <a title="MongoDB" href="http://www.mongodb.org/" target="_blank">MongoDb</a> website. While I was researching on how to access mongoDB easily and more efficiently I found out <a title="Spring Data" href="http://www.springsource.org/spring-data" target="_blank">Spring Data</a>. I also found some useful blogs explaining integration of both. But I was still not able to find a concrete working example which explains how to handle class relationship with MongoDB(DBRefs). So I thought of writing a complete working <strong>Spring MVC + Spring Data + MongoDB application handling DBRefs.<!--more--></strong></p>
<p>I have created a simple example with three domain classes User, Post and Comment. User can create Post and Post can have Comment(keeping it as simple as that). Below are the domain classes(of the example),</p>
<p>[sourcecode language="java"]
import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;</p>
<p>@Document
public class User {
    @Id
    private String id;
    private String userName;
    private String password;</p>
<pre><code>    //getters &amp;amp; setters
</code></pre>
<p>}
[/sourcecode]</p>
<p>[sourcecode language="java"]
import org.springframework.data.annotation.Id;</p>
<p>@org.springframework.data.mongodb.core.mapping.Document
public class Post {</p>
<pre><code>@Id
private String id;
private String subject;
private String content;
@org.springframework.data.mongodb.core.mapping.DBRef
private User user;

    //getters and setters
</code></pre>
<p>}
[/sourcecode]</p>
<p>[sourcecode language="java"]
import org.springframework.data.annotation.Id;</p>
<p>@org.springframework.data.mongodb.core.mapping.Document
public class Comment {</p>
<pre><code>@Id
private String id;
private String content;
@org.springframework.data.mongodb.core.mapping.DBRef
private User user;
@org.springframework.data.mongodb.core.mapping.DBRef
private Post post;

    //getters and setters
</code></pre>
<p>}
[/sourcecode]</p>
<p>You can see how the classes are linked with each other using <strong>@DBRef</strong> annotation. Now in order to access MongoDB and we need to create some DAO classes. Well thats a piece of cake with Spring Data's <strong>MongoRepository</strong>. Just extend MongoRepository&lt;T, X&gt; with you DAO where T is class name and X is class used for creating the key.</p>
<p>[sourcecode language="java"]
import in.xebia.mongodb.blog.domain.User;
import org.springframework.transaction.annotation.Transactional;</p>
<p>@Transactional
public interface UserDao extends org.springframework.data.mongodb.repository.MongoRepository&lt;User, String&gt; {</p>
<p>}
[/sourcecode]</p>
<p>[sourcecode language="java"]
import in.xebia.mongodb.blog.domain.Post;
import org.springframework.transaction.annotation.Transactional;</p>
<p>@Transactional
public interface PostDao extends org.springframework.data.mongodb.repository.MongoRepository&lt;Post, String&gt; {</p>
<p>}
[/sourcecode]</p>
<p>[sourcecode language="java"]
import in.xebia.mongodb.blog.domain.Comment;
import org.springframework.transaction.annotation.Transactional;</p>
<p>@Transactional
public interface CommentDao extends org.springframework.data.mongodb.repository.MongoRepository&lt;Comment, String&gt;{</p>
<p>}
[/sourcecode]</p>
<p>MongoRepository provides basic CRUD operations like findOne(),findALL(),save(),delete(), etc. but in order to handle DBRefs we need to make custom use of <strong>MongoTemplate</strong> as shown below in the CommentService class:</p>
<p>[sourcecode language="java"]
import static org.springframework.data.mongodb.core.query.Criteria.where;
import in.xebia.mongodb.blog.api.CommentDao;
import in.xebia.mongodb.blog.api.CommentService;
import in.xebia.mongodb.blog.domain.Comment;
import in.xebia.mongodb.blog.domain.Post;
import in.xebia.mongodb.blog.domain.User;
import java.util.List;
import java.util.UUID;
import javax.annotation.Resource;
import org.springframework.data.mongodb.core.MongoTemplate;
import org.springframework.data.mongodb.core.query.Query;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;</p>
<p>@Service(&quot;commentService&quot;)
@Transactional
public class CommentServiceImpl implements CommentService {
    @Resource
    private CommentDao commentDao;
    @Resource
    private MongoTemplate mongoTemplate;
    public List&lt;Comment&gt; getAll() {
        List&lt;Comment&gt; comments = commentDao.findAll();
        return comments;
    }
    public Comment get(String id) {
        Comment comment = commentDao.findOne(id);
        return comment;
    }
    public Boolean add(Comment comment) {
        try {
            if(comment.getId()==null || comment.getId().length()==0)
                comment.setId(UUID.randomUUID().toString());
           commentDao.save(comment);
           return true;
          } catch (Exception e) {
              e.printStackTrace();
              return false;
          }
    }
    public List&lt;Comment&gt; get(Post post) {
        Query query = new Query(where(&quot;post.$id&quot;).is(post.getId()));
        List&lt;Comment&gt; comments = mongoTemplate.find(query, Comment.class);
        return comments;
    }
    public List&lt;Comment&gt; get(User user) {
        Query query = new Query(where(&quot;user.$id&quot;).is(user.getId()));
        List&lt;Comment&gt; comments = mongoTemplate.find(query, Comment.class);
        return comments;
    }</p>
<p>}
[/sourcecode]</p>
<p>Last but not the least here is the config file for mongo:</p>
<p>[xml]
&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;beans xmlns=&quot;http://www.springframework.org/schema/beans&quot;
 xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot;
 xmlns:p=&quot;http://www.springframework.org/schema/p&quot;
    xmlns:mongo=&quot;http://www.springframework.org/schema/data/mongo&quot;
 xsi:schemaLocation=&quot;http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
http://www.springframework.org/schema/data/mongo
http://www.springframework.org/schema/data/mongo/spring-mongo-1.0.xsd&quot;&gt;</p>
<p>&lt;mongo:repositories base-package=&quot;in.xebia.mongodb.blog.api&quot; /&gt;</p>
<p>&lt;mongo:mongo host=&quot;localhost&quot; port=&quot;27017&quot;/&gt;
&lt;!--mongoTemplate is required as it is used by mongorepository --&gt;
&lt;bean id=&quot;mongoTemplate&quot; class=&quot;org.springframework.data.mongodb.core.MongoTemplate&quot;&gt;
&lt;constructor-arg ref=&quot;mongo&quot;/&gt;
&lt;constructor-arg name=&quot;databaseName&quot; value=&quot;test&quot;/&gt;
&lt;/bean&gt;
&lt;bean id=&quot;populatorService&quot; class=&quot;in.xebia.mongodb.blog.impl.PopulatorService&quot; init-method=&quot;init&quot;/&gt;
&lt;/beans&gt;
[/xml]</p>
<p>Thats all I have for this blog you can download the complete RESTful web-app from the following git repository: <a title="HandlingDBRefsInMongoDBWithSpringData" href="https://github.com/vijayrawatsan/SpringDataWithMongoDB" target="_blank">HandlingDBRefsInMongoDBWithSpringData</a>. In order to use the web app you will require some tools like soapUI or Poster(mozilla addon) as there is no UI for application. Just publish the app and start making json requests(see example in <a href="https://github.com/vijayrawatsan/SpringDataWithMongoDB/blob/master/readme.txt">readme.txt</a>)</p>
<p>&nbsp;</p>
<p>Have a nice day.</p>

## Comments

**[Lakshay Ghai](#5936 "2011-09-22 08:45:48"):** Useful stuff...

**[Karan](#5937 "2011-09-22 10:37:07"):** nice share! Although i havent found the time to try out ur example but wen i checked last there was a bug with 'DBRef' annotation and the Object data-types (User and Post classes in your example). How were you able to workaround with that ?? BUG: http://forum.springsource.org/showthread.php?108017-Spring-Data-Document-M2-Bug-with-DBRef

**[Vijay Rawat](#5938 "2011-09-22 13:36:23"):** Well Karan, This bug is now corrected in release 1.0.0.M4 and, moreover, I normally use BuildSnapShots which are latest nightly build.

**[Rohit](#5930 "2011-09-20 19:27:01"):** +1

**[Anand](#5934 "2011-09-21 23:15:34"):** nice post..

