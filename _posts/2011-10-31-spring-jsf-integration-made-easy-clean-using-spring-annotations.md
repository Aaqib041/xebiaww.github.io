---
layout: post
header-img: img/default-blog-pic.jpg
author: tarunsapra
description: 
post_id: 9021
created: 2011/10/31 22:59:34
created_gmt: 2011/10/31 17:59:34
comment_status: open
---

# Spring JSF integration made easy & clean using Spring Annotations

<p>With Spring becoming the De-facto framework for Java EE development,  developers are choosing to build their applications around the Spring framework and leveraging the robust programming model of Spring Dependency Injection and it's flexibility to integrate seamlessly with other frameworks.
JSF on the other hand is one framework whose component based model has gained wide acceptance among the developer community and is becoming the framework of choice for applications involving heavy CRUD operations. This blog intends to showcase the use of Spring annotations to develop a much cleaner and simpler integration between JSF and Spring.</p>
<!--more-->

<p>When JSF is integrated with Spring, the JSF backing beans are loaded by the Spring container and also their scope are described &amp; managed by the Spring's <strong> @Scope </strong>annotations. Lets start by looking at a snippet from the web.xml of a project using this kind of JSF-Spring integration.</p>
<p>[xml]
 &lt;listener&gt;
    &lt;listener-class&gt;org.springframework.web.context.ContextLoaderListener&lt;/listener-class&gt;
 &lt;/listener&gt;
 &lt;listener&gt;
    &lt;listener-class&gt;org.springframework.web.context.request.RequestContextListener&lt;/listener-class&gt;
 &lt;/listener&gt;
 &lt;context-param&gt;
   &lt;param-name&gt;contextConfigLocation&lt;/param-name&gt;
   &lt;param-value&gt;/WEB-INF/applicationContext.xml&lt;/param-value&gt;
 &lt;/context-param&gt;</p>
<p>[/xml]</p>
<p>In the above code ContextLoaderListener is the bootstrap listener to start up Spring's root WebApplicationContext and RequestContextListener is a listener exposes the request to the current thread.
Now lets take a look at the <strong><em>faces-config.xml</em></strong> file.</p>
<p>[xml]</p>
<p>&lt;application&gt;
  &lt;el-resolver&gt;org.springframework.web.jsf.el.SpringBeanFacesELResolver&lt;/el-resolver&gt;
 &lt;/application&gt;</p>
<p>[/xml]</p>
<p>The above xml snippet from faces-config is the pivotal part of the JSF-Spring integration, as it delegates the JSF EL calls to the Spring WebApplicationContext.</p>
<p>The following is the JSP containing the JSF form along with JSF EL.</p>
<p>[html]</p>
<p>&lt;body&gt;
 &lt;f:view&gt;
  &lt;h:form&gt;
    &lt;h:outputText value=&quot;Enter Your Name:&quot; /&gt;
    &lt;h:inputText value=&quot;#{userForm.userName}&quot; required=&quot;true&quot;/&gt;&lt;br&gt;
    &lt;h:outputText value=&quot;Enter Your Email:&quot; /&gt;
    &lt;h:inputText value=&quot;#{userForm.email}&quot; required=&quot;true&quot;/&gt;&lt;br&gt;
    &lt;h:outputText value=&quot;Enter Your Age:&quot; /&gt;&amp;nbsp;&amp;nbsp;
    &lt;h:commandButton action=&quot;#{userAction.action}&quot; value=&quot;Submit&quot; /&gt;
 &lt;/h:form&gt;
&lt;/f:view&gt;
&lt;/body&gt;</p>
<p>[/html]</p>
<p>In the above form declaration you would have noticed couple of Beans, <em>userForm</em> and <em>userAction</em>. UserForm bean is basically a POJO having setters/getters but most examples of JSF you will find on internet pollute this POJO by adding  action/logic methods inside the POJO for handling the h:commandButton action call. The reason behind this is that the developer would need to write boiler-plate code to make data of one bean(userForm) accessible in another bean(userAction). Though with JSF 2.0 you can use <strong>@ManagedProperty</strong> for achieving this purpose. But with Spring by using <strong>@Autowired </strong>annotation you not only make userForm available in userAction but also other layers of the application(Logic/Dao) can also be injected easily into one-another. Lets see some code</p>
<p>[java]
@Component
@Scope(&quot;session&quot;)
public class UserForm {</p>
<p>private String userName;</p>
<p>private String email;</p>
<p>private String orderNo;</p>
<p>private Date orderDate;</p>
<p>//setters
 //getters</p>
<p>}</p>
<p>[/java]</p>
<p>We have used Spring's @Component and @Scope annotation to declare UserForm as session scoped bean and on the UI(JSP/XHTML) this bean is accessed from JSF EL by the expression #{userForm.userName} (notice <em>userForm</em>, Spring enforces best practices by making beans available only through their corresponding camelCase!)</p>
<p>[java]</p>
<p>@Component
@Scope(&quot;request&quot;)
public class UserAction {</p>
<p>@Autowired
 UserForm userForm;</p>
<p>@Autowired
 UserLogic userLogic;</p>
<p>UserData userData;</p>
<p>public String action() {</p>
<p>userData = userLogic.getResults(userForm.getUserName());
 userForm.setOrderDate(userData.getOrderDate());
 userForm.setOrderNo(userData.getOrderNo());
 return &quot;welcome&quot;;
 }
}</p>
<p>[/java]</p>
<p>Now in the above UserAction class we have used @Autowired to access userForm bean. The UserAction itself is request scope, as it's only purpose is to handle requests. Also in the above code we have <strong>@Autowired </strong>UserLogic class,  it's a class as the name suggests dedicated for carrying out any business logic.</p>
<p>[java]</p>
<p>@Component
 @Scope(&quot;prototype&quot;)
 public class UserLogicImpl implements UserLogic {</p>
<p>@Autowired
 UserDao userDao;</p>
<p>public UserData getResults(String userName) {
 //some logic before calling dao
 return userDao.findByName(userName);
 }
 }</p>
<p>[/java]</p>
<p>Inside UserLogic we have <strong>@Autowired</strong> our DAO class which can be extending any Spring ORM support class(HibernateDaoSupport/SQLMapClientSupport) .</p>
<p>Since we are leveraging  Spring Annotations everywhere, lets take a look at Spring's applicationContext xml file</p>
<p>[xml]</p>
<p>&lt;context:component-scan base-package=&quot;com.xebia.sample&quot; /&gt;</p>
<p>[/xml]</p>
<p>You only need to specify the packages to scan.</p>
<p><span style="text-decoration: underline;"><strong>Conclusion</strong></span><strong> - </strong>Using Spring's annotations we can integrate JSF and Spring in a much cleaner and simpler way. Also you no longer need to write the "<em><strong>&lt;managed-bean&gt;</strong></em>" declaration in faces-config.xml as the bean and it's scope both are handled by the Spring Container.</p>
<p><span style="text-decoration: underline;"><strong> </strong></span></p>
<p><span style="text-decoration: underline;"><strong>
</strong></span></p>

## Comments

**[Girish](#6082 "2011-10-31 23:16:26"):** Nice article tarun keep it up

**[Amit Singh](#9032 "2012-06-13 20:54:23"):** Hi Tarun, Thanks for writing this blog. I need your help in resolving one issue here: In your example : UserForm which is the backing bean for JSF has been created by spring and added to the session for JSF. it works well if every time I have to display a blank form on the screen. My question is: Assume I query the DB and fetch one row of User table. If I have to set the UserForm with this detail and set it in session, how do I do it? becuase I do not have session available. Regards, Amit Singh

**[Tarun Sapra](#9034 "2012-06-14 09:03:02"):** Hi Amit, Thats the beauty of JSF, you don't have to do anything because the USerForm is the session bean itself and UserAction is request scope, so if you set anything in the UserForm that is automatically saved in session as UserForm is session bean. But if you need to prepopulate the data in the userform when the user first time logsin user can user prerenderView property in JSF2 or if the user fills some data and submits the form then make sure userAction returns null, so that user stays on the same page and in the userAction you can make the call to DAO/Logic layer to fetch the data and copy that data in the UserForm bean, since this bean is session bean thus it will be always there in the session.

**[Amit Singh](#9044 "2012-06-15 16:06:17"):** sorry please ignore previous comment. It was a miss again :)

**[Amit Singh](#9043 "2012-06-15 15:42:18"):** Hey Tarun, Thanks for the reply. Even I figured it after writing this query. It is there in your example only. sorry for my miss. I have ran into another problem. This time with spring security and JSF. Now, if I add a filter for spring -security, my spring managed beans are not getting created for request and session scope.

