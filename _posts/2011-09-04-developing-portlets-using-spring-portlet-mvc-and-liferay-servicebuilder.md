---
layout: post
header-img: img/default-blog-pic.jpg
author: vgupta
description: 
post_id: 9582
created: 2011/09/04 21:56:56
created_gmt: 2011/09/04 16:56:56
comment_status: open
---

# Developing portlets using Spring Portlet MVC and Liferay ServiceBuilder

In my current project, I was exploring options to use a framework to develop portlets in Liferay portal. Finally, we decided to use Sprint Portlet MVC. We were successful in developing data driven portlets using Spring Portlet MVC and Hibernate. However, we were told to use Liferay ServiceBuilder to develop services instead of Spring and Hibernate. In the end, the tech stack to develop portlets was Spring Portlet MVC to develop the portlets and Liferay ServiceBuilder to develop services. This post mentions the problem we faced in integrating Spring Portlet MVC with Liferay ServiceBuilder. 

Liferay ServiceBuilder generates Spring and Hibernate configuration using ant scripts dynamically and loads them into Portlet Context using PortletContextLoaderListener. Whereas, by default, Spring Portlet MVC loads the Spring Configuration from the file named as portletname-portlet.xml when the portlet is getting deployed. While deploying the portlet, we got the following error

` 21:29:43,468 INFO [STDOUT] 21:29:43,466 ERROR [PortletHotDeployListener] java.lang.IllegalStateException: Root context attribute is not of type WebApplicationContext: com.liferay.portal.spring.context.PortletApplicationContext@1beaba: display name [Root WebApplicationContext]; startup date [Sun Sep 04 21:29:31 IST 2011]; root of context hierarchy java.lang.IllegalStateException: Root context attribute is not of type WebApplicationContext: com.liferay.portal.spring.context.PortletApplicationContext@1beaba : display name [Root WebApplicationContext]; startup date [Sun Sep 04 21:29:31 IST 2011]; root of context hierarchy at org.springframework.web.portlet.context.PortletApplicationContextUtils.getWebApplicationContext( PortletApplicationContextUtils.java:79) at org.springframework.web.portlet.FrameworkPortlet.initPortletApplicationContext( FrameworkPortlet.java:295) at org.springframework.web.portlet.FrameworkPortlet.initPortletBean(FrameworkPortlet.java:269) at org.springframework.web.portlet.GenericPortletBean.init(GenericPortletBean.java:116) ` It is clear from the error message when spring is initializing the portlet application context, it is expecting that Root ApplicationContext should be of type WebApplicationContext but it is finding root ApplicationContext to be of type PortletApplicationContext, which contains Liferay Services configuration and loaded by the container before Spring Portlet MVC configuration. A bit more investigation revealed that PortletApplicationContext extended XmlPortletApplicationContext which is not compatible with WebApplicationContext. As I entered into the source code, I found the following line of code were responsible for creating ApplicationContext for Spring Portlet MVC

` ApplicationContext parent = PortletApplicationContextUtils.getWebApplicationContext(getPortletContext()); ApplicationContext pac = createPortletApplicationContext(parent); `

The first line was getting the root applicationContext and second line creates an applicationContext with it's parent as the ServiceBuilder specific applicationcontext. If the execution of the above code was successful, we would have been successful in injecting ServiceBuilder specific beans in Spring Portlet MVC controllers. But, it was not necessary as ServiceBuilder provides utility classes to access ServiceBuilder services. Hence, we did not need to inject ServiceBuilder services inside Spring Portlet MVC controllers. So, we created a custom portlet which extended org.springframework.web.portlet.DispatcherPortlet which further extends org.springframework.web.portlet.FrameworkPortlet and overrided the initPortletApplicationContext changing the above two lines of code to 

` **ApplicationContext parent = null;** ApplicationContext pac = createPortletApplicationContext(parent); `

After making the above, our portlet got deployed successfully.

As a final note, the above mentioned problem occurred in Liferay 5.2.3 and when I checked the source code of Liferay 6.0, I found that Liferay's PortletContextLoaderListener, after loading ServiceBuilder applicationcontext, removes the applicationcontext from the portlet context. Further investigation of the source code indicates that the above mentioned problem should not occur in Liferay 6.0.

## Comments

**[patrizio](#6011 "2011-10-11 22:05:54"):** Thank you. With your mofication I was able to make it work. Since I'm a newbe of Spring MVC portlets development I was wondering how you linked the ModelAttribute annotation to the LR service builder model. I mean how did you make work the following code: @ModelAttribute("books") public List getBooks() { return BookLocalServiceUtil.getBooks(); } I think that Book must be the LR Service Model not a custom model. or not?? Your help would be appreciated. Thanks again

**[Uday Kumar](#6577 "2012-01-03 17:10:45"):** Hi , I'm trying to use service builder with spring MVC portlets. When i used normal portlet with service builder everything worked fine. But when i shifted to Spring MVC portlet with service builder i'm getting the below exception. Caused by: com.liferay.portal.kernel.bean.BeanLocatorException: BeanLocator has not been set for servlet context Slogans-portlet at com.liferay.portal.kernel.bean.PortletBeanLocatorUtil.locate(PortletBeanLocatorUtil.java:42) at com.ca.database.service.SloganLocalServiceUtil.getService(SloganLocalServiceUtil.java:256) at com.ca.database.service.SloganLocalServiceUtil.sayHello(SloganLocalServiceUtil.java:247) at org.apache.jsp.jsp.viewSlogans_jsp._jspService(viewSlogans_jsp.java:62) at org.apache.jasper.runtime.HttpJspBase.service(HttpJspBase.java:70) at javax.servlet.http.HttpServlet.service(HttpServlet.java:717) at org.apache.jasper.servlet.JspServletWrapper.service(JspServletWrapper.java:377) can you please help me to find out what went wrong. below line is cauing the above problem. SloganLocalServiceUtil.getSlogans(); i implemented the above method in Impl class and re ran the service builder. Please help me,

**[Jelmer](#5902 "2011-09-05 00:36:04"):** Using the "utils" makes your code really hard to to test. Did you try referencing your service class like this. That way you can inject it

**[Ranvijay](#6212 "2011-11-16 19:03:14"):** Hi, Could you pls, describe how exactly we are require to configure the above changes, as i am not able to configure it and so this is not working for me. Thanks

**[patrizio](#6095 "2011-11-02 18:52:56"):** no reply? :-(

**[Vijay Rawat](#9147 "2012-07-11 09:49:45"):** Thanks it helped. :)

