---
layout: post
header-img: img/default-blog-pic.jpg
author: vrawat
description: 
post_id: 9961
created: 2011/11/01 21:45:47
created_gmt: 2011/11/01 16:45:47
comment_status: open
---

# Accessing Scorm Engine using Spring MVC

<p>The other day I was working on a POC for one of our clients which involved accessing ScormEngine and syncing our local databases with ScormEngine's databases(as per the need). Initially it seemed tough, but finally I was able to do it. So I thought of sharing my experiences with everyone. Here I am explaining the whole code, hoping that it might help someone as ScormEngine with Spring is not very common.</p>
<p>Here is the GitHub Repository : <a href="https://github.com/vijayrawatsan/ScormEngineWithSpring" target="_blank">GitHub ScormEngineWithSpring</a></p>
<p>Let me briefly tell what Scorm is and what ScormEngine is all about?
<!--more-->
<blockquote>Sharable Content Object Reference Model (SCORM) is a collection of standards and specifications for web-based e-learning. It defines communications between client side content and a host system called the run-time environment, which is commonly supported by a learning management system. SCORM also defines how content may be packaged into a transferable ZIP file called "Package Interchange Format".</blockquote>
-Wikipedia</p>
<p><strong>ScormEngine </strong>is a popular implementation of SCORM provided by Rustici Software. ScormEngine does provide a clean client library(even an example java-web app) to access ScormEngine, but generally we also need to map/sync some ScormEngine database tables with our local database tables which was not covered in the java client. So, I thought of creating a standalone Spring MVC web-app for the same.</p>
<p>It provides basic functionality required by any Scorm client like uploading a course, previewing a course and launching a course(other functions like deleting are pretty straight forward).</p>
<p>Domain model of my application consists of User, Course and RegistrationDetail. User and Course classes include user and course details respectively, and have one to one mapping from course to user(only one owner of a course). In the diagram below you can see how the classes are related with each other:
<img src="http://xebee.xebia.in/wp-content/uploads/2012/01/class.png" alt="Class Relationship" width="400px" height="325px"/>
<strong>RegistraionDetail </strong>is a very important domain object. We will require a little background to understand its requirement. Whenever Scorm Content(generally a tutorial/quiz/assignment) is played at client side. We need to collect some information like:
<ul>
    <li>which user played it,</li>
    <li>how much time he spent on it,</li>
    <li>does he/she completed the course,</li>
    <li>when did he/she completed the course</li>
</ul>
All this information is very important and stored in the ScormEngine. But in order to generate reports and implement business logic, we need to store that information at our end(with in our apps database) also. </p>
<p>We can see that I have used a <strong>ManyToOne</strong> mapping for User(i.e. learner) and <strong>OneToOne</strong> mapping for Course, this implies that, multiple registrations can occur for a single course(offcourse for different users).
Thats all with the domain model. Now lets see how the ScormEngine services are consumed and the new Services are created for syncing of databases within our application.</p>
<p><strong>ScormEngineService : </strong>It is the core service provided by ScormEngine through which we can access all other services like CourseService, RegistrationService, etc.
For setting up ScormEngineService as a singleton bean use the following scriptlet in your spring-context.xml (or <a href="https://github.com/vijayrawatsan/ScormEngineWithSpring/blob/master/src/main/webapp/WEB-INF/spring/appServlet/servlet-context.xml">Click to view the Complete context</a>):</p>
<p>[code language="xml"]
&lt;beans:bean class=&quot;com.rusticisoftware.hostedengine.client.ScormEngineService&quot; name=&quot;scormEngineService&quot;&gt;
        &lt;beans:constructor-arg index=&quot;0&quot; value=&quot;http://yourhost:yourport/EngineWebServices/&quot;&gt;&lt;/beans:constructor-arg&gt;
        &lt;beans:constructor-arg index=&quot;1&quot; value=&quot;appID&quot;&gt;&lt;/beans:constructor-arg&gt;
        &lt;beans:constructor-arg index=&quot;2&quot; value=&quot;password&quot;&gt;&lt;/beans:constructor-arg&gt;
&lt;/beans:bean&gt;
[/code]
The first parameter to the constructor is ScormEngine web-service end point, the second parameter is your ApplicationID and the last parameter is the Secret  Key for your application.</p>
<p><strong>So how does this bean(scormEngineService) works?</strong> Basically ScormEngine exposes a list of webServices that we can access using our bean. These web services are of the form : 
<a href="#">http://host/api?method=[service.prefix].[methodName]&amp;appid=[your app id]&amp;origin=<a href="&amp;any=other&amp;methodparams=here">your origin string</a>(&amp;security=params)</a>. 
<ul>
    <li><strong>method :</strong> This parameter specifies which method of a particular service to call.</li>
    <li>Other method specific parameters are also passed like courseId, registrationid, redirectUrl , etc.</li>
    <li><strong>appid :</strong> This parameter is your application id registered with ScormEngine.</li>
    <li><strong>origin :</strong> This will be a string specifying the organization, application and version of the software making the request.</li>
    <li><strong>security :</strong> For secured services a timestamp parameter(ts) of the form yyyyMMddHHmmss (ex. 20081217223530) is passed along with a signature(sig) parameter which is created by hashing(md5) the specifics of a request with your secret key.</li>
</ul>This bean provides us with instances of different services like CourseService, RegistrationService, UploadService, etc. And these Services will in turn call the webServices exposed by ScormEngine, abstracting the away the part where we need to create timestamp and signature. We will just need to use methods exposed by these Services.
<strong>For example :</strong> Below method from CourseService 
ImportCourse(String, File.getAbsolutePath()) is equivalent to posting a file(as binary) on this webService url 
<a href="#">http://yourhost/api?method=rustici.course.importCourse&amp;courseid=yourCourseId</a>
So in a way these Services abstract the webService calls for us and make these calls clean.</p>
<p><strong>Uploading:</strong>
For uploading a course, logic involved is as follows, first of all I will need to create a database entry in my local database and then upload the course to ScormEngine(all this stuff in a single transaction). For that, I created a course detail entry in my local database  and then use <strong>ImportCourse(String courseId, String absoluteFilePath)</strong>; method to import the course to ScormEngine. So basically the actual course file gets uploaded on ScormEngine, but not on my local machine. This one was pretty straight-forward. Below is the code snippet:</p>
<p>[code language="java"]
public ImportResult uploadCourse(File file, String title, User owner) {
        CourseService courseService = scormEngineService.getCourseService();
        List importResults = null;
        try {
            Course course = createCourseLocally(title, owner);
            importResults = courseService.ImportCourse(course.getCourseId(), file.getAbsolutePath());</p>
<pre><code>    }catch (Exception e) {
        logger.info(e.getMessage());
        return null;
    }
    return importResults.get(0);
}

private Course createCourseLocally(String title, User owner) {
    Course course = new Course();
    course.setDateCreated(new Date());
    course.setCourseId(&amp;quot;Course-&amp;quot;+UUID.randomUUID().toString());
    course.setOwner(owner);
    course.setTitle(title);
    courseDao.persist(course);
    return course;
}
</code></pre>
<p>[/code]</p>
<p><strong>Difference between previewing and launching:</strong>
Previewing means simply viewing the course stored in the ScormEngine without saving any information(usually used to check whether the course is uploaded successfully). On the other hand, Launching allows us to view the course as well as it saves the information like how much time it took, which user played the content, did he/she complete it, etc.</p>
<p>For <strong>Preview</strong> I used <strong>GetPreviewUrl(String courseId, String redirectUrl)</strong>; method to get the preview URLs as follows:
[code language="java"]</p>

## Comments

**[Vijay Rawat](#6134 "2011-11-07 09:50:54"):** Hi Janne, Thanks for the information. I have already seen JSCORM and its a very nice piece of work in Scala. All the best with JSCORM.

**[Janne Hietala](#6109 "2011-11-04 11:56:57"):** Nice work on integrating with ScormEngine! Maybe this would be intresting for you. We already have made similar implementation with our own SCORM engine. We also have the frontend developed as Liferay portlets. You can find it here: https://github.com/arcusys/JSCORM Some Liferay discussion also here: http://www.liferay.com/community/forums/-/message_boards/message/9293186

