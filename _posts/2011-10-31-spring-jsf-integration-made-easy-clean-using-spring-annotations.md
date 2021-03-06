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

With Spring becoming the De-facto framework for Java EE development,  developers are choosing to build their applications around the Spring framework and leveraging the robust programming model of Spring Dependency Injection and it's flexibility to integrate seamlessly with other frameworks. JSF on the other hand is one framework whose component based model has gained wide acceptance among the developer community and is becoming the framework of choice for applications involving heavy CRUD operations. This blog intends to showcase the use of Spring annotations to develop a much cleaner and simpler integration between JSF and Spring.

When JSF is integrated with Spring, the JSF backing beans are loaded by the Spring container and also their scope are described & managed by the Spring's ** @Scope **annotations. Lets start by looking at a snippet from the web.xml of a project using this kind of JSF-Spring integration.

[xml] <listener> <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class> </listener> <listener> <listener-class>org.springframework.web.context.request.RequestContextListener</listener-class> </listener> <context-param> <param-name>contextConfigLocation</param-name> <param-value>/WEB-INF/applicationContext.xml</param-value> </context-param>

[/xml]

In the above code ContextLoaderListener is the bootstrap listener to start up Spring's root WebApplicationContext and RequestContextListener is a listener exposes the request to the current thread. Now lets take a look at the **_faces-config.xml_** file.

[xml]

<application> <el-resolver>org.springframework.web.jsf.el.SpringBeanFacesELResolver</el-resolver> </application>

[/xml]

The above xml snippet from faces-config is the pivotal part of the JSF-Spring integration, as it delegates the JSF EL calls to the Spring WebApplicationContext.

The following is the JSP containing the JSF form along with JSF EL.

[html]

<body> <f:view> <h:form> <h:outputText value="Enter Your Name:" /> <h:inputText value="#{userForm.userName}" required="true"/><br> <h:outputText value="Enter Your Email:" /> <h:inputText value="#{userForm.email}" required="true"/><br> <h:outputText value="Enter Your Age:" />&nbsp;&nbsp; <h:commandButton action="#{userAction.action}" value="Submit" /> </h:form> </f:view> </body>

[/html]

In the above form declaration you would have noticed couple of Beans, _userForm_ and _userAction_. UserForm bean is basically a POJO having setters/getters but most examples of JSF you will find on internet pollute this POJO by adding  action/logic methods inside the POJO for handling the h:commandButton action call. The reason behind this is that the developer would need to write boiler-plate code to make data of one bean(userForm) accessible in another bean(userAction). Though with JSF 2.0 you can use **@ManagedProperty** for achieving this purpose. But with Spring by using **@Autowired **annotation you not only make userForm available in userAction but also other layers of the application(Logic/Dao) can also be injected easily into one-another. Lets see some code

[java] @Component @Scope("session") public class UserForm {

private String userName;

private String email;

private String orderNo;

private Date orderDate;

//setters //getters

}

[/java]

We have used Spring's @Component and @Scope annotation to declare UserForm as session scoped bean and on the UI(JSP/XHTML) this bean is accessed from JSF EL by the expression #{userForm.userName} (notice _userForm_, Spring enforces best practices by making beans available only through their corresponding camelCase!)

[java]

@Component @Scope("request") public class UserAction {

@Autowired UserForm userForm;

@Autowired UserLogic userLogic;

UserData userData;

public String action() {

userData = userLogic.getResults(userForm.getUserName()); userForm.setOrderDate(userData.getOrderDate()); userForm.setOrderNo(userData.getOrderNo()); return "welcome"; } }

[/java]

Now in the above UserAction class we have used @Autowired to access userForm bean. The UserAction itself is request scope, as it's only purpose is to handle requests. Also in the above code we have **@Autowired **UserLogic class,  it's a class as the name suggests dedicated for carrying out any business logic.

[java]

@Component @Scope("prototype") public class UserLogicImpl implements UserLogic {

@Autowired UserDao userDao;

public UserData getResults(String userName) { //some logic before calling dao return userDao.findByName(userName); } }

[/java]

Inside UserLogic we have **@Autowired** our DAO class which can be extending any Spring ORM support class(HibernateDaoSupport/SQLMapClientSupport) .

Since we are leveraging  Spring Annotations everywhere, lets take a look at Spring's applicationContext xml file

[xml]

<context:component-scan base-package="com.xebia.sample" />

[/xml]

You only need to specify the packages to scan.

**Conclusion**** \- **Using Spring's annotations we can integrate JSF and Spring in a much cleaner and simpler way. Also you no longer need to write the "_**<managed-bean>**_" declaration in faces-config.xml as the bean and it's scope both are handled by the Spring Container.

** **

** **

## Comments

**[Girish](#6082 "2011-10-31 23:16:26"):** Nice article tarun keep it up

**[Amit Singh](#9032 "2012-06-13 20:54:23"):** Hi Tarun, Thanks for writing this blog. I need your help in resolving one issue here: In your example : UserForm which is the backing bean for JSF has been created by spring and added to the session for JSF. it works well if every time I have to display a blank form on the screen. My question is: Assume I query the DB and fetch one row of User table. If I have to set the UserForm with this detail and set it in session, how do I do it? becuase I do not have session available. Regards, Amit Singh

**[Tarun Sapra](#9034 "2012-06-14 09:03:02"):** Hi Amit, Thats the beauty of JSF, you don't have to do anything because the USerForm is the session bean itself and UserAction is request scope, so if you set anything in the UserForm that is automatically saved in session as UserForm is session bean. But if you need to prepopulate the data in the userform when the user first time logsin user can user prerenderView property in JSF2 or if the user fills some data and submits the form then make sure userAction returns null, so that user stays on the same page and in the userAction you can make the call to DAO/Logic layer to fetch the data and copy that data in the UserForm bean, since this bean is session bean thus it will be always there in the session.

**[Amit Singh](#9044 "2012-06-15 16:06:17"):** sorry please ignore previous comment. It was a miss again :)

**[Amit Singh](#9043 "2012-06-15 15:42:18"):** Hey Tarun, Thanks for the reply. Even I figured it after writing this query. It is there in your example only. sorry for my miss. I have ran into another problem. This time with spring security and JSF. Now, if I add a filter for spring -security, my spring managed beans are not getting created for request and session scope.

