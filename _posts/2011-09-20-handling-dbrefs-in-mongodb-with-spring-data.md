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

Recently I was watching a presentation titled [Why I Chose MongoDB for guardian.co.uk][1] by Mat Wall. I thought of giving a look to mongoDB. Those who dont know what is mongoDB, well, its a document oriented database with some advantages over other relational databases. For more info you can have a look at [MongoDb][2] website. While I was researching on how to access mongoDB easily and more efficiently I found out [Spring Data][3]. I also found some useful blogs explaining integration of both. But I was still not able to find a concrete working example which explains how to handle class relationship with MongoDB(DBRefs). So I thought of writing a complete working **Spring MVC + Spring Data + MongoDB application handling DBRefs.**

I have created a simple example with three domain classes User, Post and Comment. User can create Post and Post can have Comment(keeping it as simple as that). Below are the domain classes(of the example),

[sourcecode language="java"] import org.springframework.data.annotation.Id; import org.springframework.data.mongodb.core.mapping.Document;

@Document public class User { @Id private String id; private String userName; private String password;
    
    
        //getters &amp; setters
    

} [/sourcecode]

[sourcecode language="java"] import org.springframework.data.annotation.Id;

@org.springframework.data.mongodb.core.mapping.Document public class Post {
    
    
    @Id
    private String id;
    private String subject;
    private String content;
    @org.springframework.data.mongodb.core.mapping.DBRef
    private User user;
    
        //getters and setters
    

} [/sourcecode]

[sourcecode language="java"] import org.springframework.data.annotation.Id;

@org.springframework.data.mongodb.core.mapping.Document public class Comment {
    
    
    @Id
    private String id;
    private String content;
    @org.springframework.data.mongodb.core.mapping.DBRef
    private User user;
    @org.springframework.data.mongodb.core.mapping.DBRef
    private Post post;
    
        //getters and setters
    

} [/sourcecode]

You can see how the classes are linked with each other using **@DBRef** annotation. Now in order to access MongoDB and we need to create some DAO classes. Well thats a piece of cake with Spring Data's **MongoRepository**. Just extend MongoRepository<T, X> with you DAO where T is class name and X is class used for creating the key.

[sourcecode language="java"] import in.xebia.mongodb.blog.domain.User; import org.springframework.transaction.annotation.Transactional;

@Transactional public interface UserDao extends org.springframework.data.mongodb.repository.MongoRepository<User, String> {

} [/sourcecode]

[sourcecode language="java"] import in.xebia.mongodb.blog.domain.Post; import org.springframework.transaction.annotation.Transactional;

@Transactional public interface PostDao extends org.springframework.data.mongodb.repository.MongoRepository<Post, String> {

} [/sourcecode]

[sourcecode language="java"] import in.xebia.mongodb.blog.domain.Comment; import org.springframework.transaction.annotation.Transactional;

@Transactional public interface CommentDao extends org.springframework.data.mongodb.repository.MongoRepository<Comment, String>{

} [/sourcecode]

MongoRepository provides basic CRUD operations like findOne(),findALL(),save(),delete(), etc. but in order to handle DBRefs we need to make custom use of **MongoTemplate** as shown below in the CommentService class:

[sourcecode language="java"] import static org.springframework.data.mongodb.core.query.Criteria.where; import in.xebia.mongodb.blog.api.CommentDao; import in.xebia.mongodb.blog.api.CommentService; import in.xebia.mongodb.blog.domain.Comment; import in.xebia.mongodb.blog.domain.Post; import in.xebia.mongodb.blog.domain.User; import java.util.List; import java.util.UUID; import javax.annotation.Resource; import org.springframework.data.mongodb.core.MongoTemplate; import org.springframework.data.mongodb.core.query.Query; import org.springframework.stereotype.Service; import org.springframework.transaction.annotation.Transactional;

@Service("commentService") @Transactional public class CommentServiceImpl implements CommentService { @Resource private CommentDao commentDao; @Resource private MongoTemplate mongoTemplate; public List<Comment> getAll() { List<Comment> comments = commentDao.findAll(); return comments; } public Comment get(String id) { Comment comment = commentDao.findOne(id); return comment; } public Boolean add(Comment comment) { try { if(comment.getId()==null || comment.getId().length()==0) comment.setId(UUID.randomUUID().toString()); commentDao.save(comment); return true; } catch (Exception e) { e.printStackTrace(); return false; } } public List<Comment> get(Post post) { Query query = new Query(where("post.$id").is(post.getId())); List<Comment> comments = mongoTemplate.find(query, Comment.class); return comments; } public List<Comment> get(User user) { Query query = new Query(where("user.$id").is(user.getId())); List<Comment> comments = mongoTemplate.find(query, Comment.class); return comments; }

} [/sourcecode]

Last but not the least here is the config file for mongo:

[xml] <?xml version="1.0" encoding="UTF-8"?> <beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p" xmlns:mongo="http://www.springframework.org/schema/data/mongo" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd http://www.springframework.org/schema/data/mongo http://www.springframework.org/schema/data/mongo/spring-mongo-1.0.xsd">

<mongo:repositories base-package="in.xebia.mongodb.blog.api" />

<mongo:mongo host="localhost" port="27017"/> <!--mongoTemplate is required as it is used by mongorepository --> <bean id="mongoTemplate" class="org.springframework.data.mongodb.core.MongoTemplate"> <constructor-arg ref="mongo"/> <constructor-arg name="databaseName" value="test"/> </bean> <bean id="populatorService" class="in.xebia.mongodb.blog.impl.PopulatorService" init-method="init"/> </beans> [/xml]

Thats all I have for this blog you can download the complete RESTful web-app from the following git repository: [HandlingDBRefsInMongoDBWithSpringData][4]. In order to use the web app you will require some tools like soapUI or Poster(mozilla addon) as there is no UI for application. Just publish the app and start making json requests(see example in [readme.txt][5])

 

Have a nice day.

   [1]: http://www.infoq.com/presentations/Why-I-Chose-MongoDB-for-Guardian
   [2]: http://www.mongodb.org/ (MongoDB)
   [3]: http://www.springsource.org/spring-data (Spring Data)
   [4]: https://github.com/vijayrawatsan/SpringDataWithMongoDB (HandlingDBRefsInMongoDBWithSpringData)
   [5]: https://github.com/vijayrawatsan/SpringDataWithMongoDB/blob/master/readme.txt

## Comments

**[Lakshay Ghai](#5936 "2011-09-22 08:45:48"):** Useful stuff...

**[Karan](#5937 "2011-09-22 10:37:07"):** nice share! Although i havent found the time to try out ur example but wen i checked last there was a bug with 'DBRef' annotation and the Object data-types (User and Post classes in your example). How were you able to workaround with that ?? BUG: http://forum.springsource.org/showthread.php?108017-Spring-Data-Document-M2-Bug-with-DBRef

**[Vijay Rawat](#5938 "2011-09-22 13:36:23"):** Well Karan, This bug is now corrected in release 1.0.0.M4 and, moreover, I normally use BuildSnapShots which are latest nightly build.

**[Rohit](#5930 "2011-09-20 19:27:01"):** +1

**[Anand](#5934 "2011-09-21 23:15:34"):** nice post..

