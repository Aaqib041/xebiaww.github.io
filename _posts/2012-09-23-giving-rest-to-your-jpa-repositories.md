---
layout: post
header-img: img/default-blog-pic.jpg
author: vrawat
description: 
post_id: 14945
created: 2012/09/23 19:07:57
created_gmt: 2012/09/23 14:07:57
comment_status: open
---

# Giving REST to your JPA Repositories

<p>I was thinking to write this blog a month earlier, when I wrote my demo application using Spring Data Rest but due to project work I was not able to do it. Finally I've been able to get some time and write this blog. As per <a href="http://www.springsource.org/spring-data/rest" target="_blank">springsource.org</a>,
<blockquote>The goal of the Spring Data REST project is to provide a solid foundation on which to expose CRUD operations to your JPA Repository-managed entities using plain HTTP REST semantics. </blockquote>
Basically it allows you to expose your JPA Repositories over HTTP as a REST layer with some simple and intelligent configuration. No hassles of writing a controller layer to expose the Repositories. Lets see how its done(assuming you have a working knowledge of <strong><a href="http://www.springsource.org/spring-data/jpa" target="_blank">Spring Data JPA</a></strong>).
<!--more--></p>
<p><span style="font-size: medium;"><strong><span style="color: #f09021;">The Code</span></strong></span>
Here is the link to complete project : <a href="https://github.com/vijayrawatsan/Spring-Data-Rest" target="_blank">Github Repo</a>
Here is the link to live application : <a href="http://springdatarest.ap01.aws.af.cm" target="_blank"><strong>Sprig Data Rest Example App</strong></a> (Hosting by <a href="http://www.appfog.com/">AppFog.com PaaS</a>)</p>
<p><span style="font-size: medium;"><strong><span style="color: #f09021;">Domain Classes and JPA Repositories</span></strong></span>
Below are the simple domain classes and corresponding repositories used in the example.
<strong>User Class</strong>
[sourcecode language="java"]
package in.xebia.datarest.domain;
@Entity
public class User {</p>
<pre><code>@Id @GeneratedValue(strategy = GenerationType.AUTO)
private Long id;
private String userName;
@RestResource(exported = false) 
private String password;

    //getters and setters
</code></pre>
<p>}
[/sourcecode]</p>
<p><strong>User Repository</strong>
[code language="java"]
package in.xebia.datarest.repository;
public interface UserRepository extends JpaRepository&lt;User, Long&gt; {
}
[/code]</p>
<p><strong>Post Class</strong>
[code language="java"]
package in.xebia.datarest.domain;
@Entity
public class Post  {</p>
<pre><code>@Id @GeneratedValue(strategy = GenerationType.AUTO)
private Long id;
private String title;
private String content;

@ManyToOne
private User user;

//getters and setters
</code></pre>
<p>}
[/code]</p>
<p><strong>Post Repository</strong>
[code language="java"]
package in.xebia.datarest.repository;
public interface PostRepository extends JpaRepository&lt;Post, Long&gt; {</p>
<pre><code>public Post findByUserUserName(@Param(&amp;quot;userName&amp;quot;) String userName);

public Post findByTitle(@Param(&amp;quot;title&amp;quot;) String title);
</code></pre>
<p>}
[/code]
Please note in User class I have used <strong>@RestResource(exported = false)</strong> this is used so that password is not exported when requesting for a user. It Can be used with Repositories and query methods also.</p>
<p>Here is how I have configured my repositories : <a href="https://github.com/vijayrawatsan/Spring-Data-Rest/blob/master/src/main/webapp/WEB-INF/hibernate-context.xml" target="_blank">hibernate-context.xml</a> and a minimal <a href="https://github.com/vijayrawatsan/Spring-Data-Rest/blob/master/src/main/webapp/WEB-INF/applicationContext.xml" target="_blank">applicationContext.xml</a></p>
<p><span style="font-size: medium;"><strong><span style="color: #f09021;">REST Export</span></strong></span>
Now in order to export your JPA Repositories you just add a custom <strong>"RepositoryRestExporterServlet"</strong> provided by spring in your <strong>web.xml</strong>. Below is the code snippet.
[code language="xml"]
&lt;servlet&gt;
    &lt;servlet-name&gt;exporter&lt;/servlet-name&gt;
    &lt;servlet-class&gt;org.springframework.data.rest.webmvc.RepositoryRestExporterServlet&lt;/servlet-class&gt;
    &lt;load-on-startup&gt;1&lt;/load-on-startup&gt;
&lt;/servlet&gt;</p>
<p>&lt;servlet-mapping&gt;
    &lt;servlet-name&gt;exporter&lt;/servlet-name&gt;
    &lt;url-pattern&gt;/*&lt;/url-pattern&gt;
&lt;/servlet-mapping&gt;
[/code]</p>
<p>In the above code I have used <strong>RepositoryRestExporterServlet </strong>to map all incoming requests. You can also use RepositoryRestExporterServlet along with DispatcherServlet in case you want to add Spring Data Rest to your existing Spring MVC project.</p>
<p>In example application there are two repositories <strong>PostRepository</strong> and <strong>UserRepository</strong>. So two resources named <strong>"post"</strong> and <strong>"user"</strong> will be exposed by the example web application.</p>
<p><span style="font-size: medium;"><strong><span style="color: #f09021;">CRUD using Data REST</span></strong></span>
Lets start with the following :
<strong>Request</strong>
[code language="css"]</p>
<p>curl localhost:8080/datarest
[/code]</p>
<p><strong>Response</strong>
[code language="css"]
{
  &quot;links&quot; : [ {
    &quot;rel&quot; : &quot;post&quot;,
    &quot;href&quot; : &quot;http://localhost:8080/datarest/post&quot;
  }, {
    &quot;rel&quot; : &quot;user&quot;,
    &quot;href&quot; : &quot;http://localhost:8080/datarest/user&quot;
  } ],
  &quot;content&quot; : [ ]
}
[/code]
The above response shows how many repositories are exposed and under which urls. </p>
<p>Lets create a user.
<strong>Request</strong>
[sourcecode language="css" wraplines="true"]
curl -v -d '{&quot;userName&quot; : &quot;Vijay&quot;, &quot;password&quot;:&quot;123&quot;}' -H &quot;Content-Type: application/json&quot; http://localhost:8080/datarest/user 
[/sourcecode]</p>
<p>This will create a user with userName as Vijay and password as 123. Now lets see how a post can be created(<strong>Embedded Types</strong>).</p>
<p><strong>Request</strong>
[code language="css"]
curl -v -X POST -H &quot;Content-type: application/json&quot; -d '{&quot;title&quot;:&quot;Spring Data Rest&quot;, &quot;content&quot;:&quot;Spring data rest is cool&quot;, &quot;user&quot;:{ &quot;rel&quot; : &quot;user.User&quot;, &quot;href&quot; : &quot;http://localhost:8080/datarest/user/1&quot; } }' http://localhost:8080/datarest/post
[/code]</p>
<p>And now if we request for [code language="css"]http://localhost:8080/datarest/post[/code] output will be
[code language="css"]
{
  &quot;links&quot; : [ ],
  &quot;content&quot; : [ {
    &quot;links&quot; : [ {
      &quot;rel&quot; : &quot;self&quot;,
      &quot;href&quot; : &quot;http://localhost:8080/datarest/post/1&quot;
    }, {
      &quot;rel&quot; : &quot;post.Post.user&quot;,
      &quot;href&quot; : &quot;http://localhost:8080/datarest/post/1/user&quot;
    } ],
    &quot;content&quot; : &quot;Spring data rest is cool&quot;,
    &quot;title&quot; : &quot;Spring Data Rest&quot;
  } ],
  &quot;page&quot; : {
    &quot;size&quot; : 20,
    &quot;totalElements&quot; : 1,
    &quot;totalPages&quot; : 1,
    &quot;number&quot; : 1
  }
}
[/code]</p>
<p>For querying on custom methods like <strong>findByUserUserName(String userName)</strong> defined in our postRepository. We can write a query like this:
[code language="css"]
curl -v http://localhost:8080/datarest/post/search/findByUserUserName?userName=Vijay
[/code]</p>
<p>Finally deleting a post using the following request:
[code language="css"]
curl -i -H -X DELETE http://localhost:8080/datarest/post/1
[/code]
This will delete the post with id equal to 1. </p>
<p>Thats it for now guys. Hope you like it. </p>
<p><strong>Note:</strong> Since this project is actively under development and <strong>RC3 </strong>is the latest Release Candidate at the time of writing. Please change versions accordingly.</p>