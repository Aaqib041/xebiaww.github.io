---
layout: post
header-img: img/default-blog-pic.jpg
author: vrawat
description: 
post_id: 10839
created: 2011/12/31 18:32:52
created_gmt: 2011/12/31 13:32:52
comment_status: open
---

# Sakai SSO and multiple WorkSites

Recently I was working on a project that modifies [Sakai ][1]a LMS( Learning Management System) to meet some very specific requirements. One of the requirements that forced me to write this blog was that, we have to perform SSO( Single Sign On) in Sakai, and based on the some parameters land different users in different WorkSites in Sakai. 

For **Single Sign On**(SSO allows a user who has been authenticated in a trusted external system seamlessly log into Sakai) in Sakai, you need to invoke **SakaiPortalLogin.jws** Web service from the external system(with a HTTP Client). Here is a detailed explanation of SSO in Sakai : [ Sakai Admin Guide:Sakai SSO][2]. In short you will call SakaiPortalLogin.jws web service with some specific parameters and a sakai.Session will be returned. Once you have your user created and logged into the Sakai, you need to associate this user to a particular WorkSite(in Sakai) and land him to that Worksite.

Now you have **sakai.Session** in place. You will redirect the user to a URL like this : http://somehost/portal/worksiteName?sakai.session=sakai-seesion-received-by-you. Now in order to fulfill our requirement we need to place some code in **org.sakaiproject.portal.charon.SkinnableCharonPortal.java**.

First of all we need to add a handler in SkinnableCharonPortal's init() method, that will handle request for a particular WorkSite. Lets say our WorkSite name is **testSite **so we will create a handler named **TestSiteRequestHandler.java** which will extend org.sakaiproject.portal.charon.handlers.WorksiteHandler. Code for adding a new handler in [SkinnableCharonPortal.java][3]'s the init() method : ``` 
 public void init(ServletConfig config) throws ServletException { super.init(config); portalContext = config.getInitParameter("portal.context"); if (portalContext == null || portalContext.length() == 0) { portalContext = DEFAULT_PORTAL_CONTEXT; }
    
    
     boolean findPageAliases = ServerConfigurationService.getBoolean(&quot;portal.use.page.aliases&quot;, false);
    
     siteHelper = new PortalSiteHelperImpl(this, findPageAliases);
    
     portalService = org.sakaiproject.portal.api.cover.PortalService.getInstance();
     M_log.info(&quot;init()&quot;);
    
     forceContainer = ServerConfigurationService.getBoolean(&quot;login.use.xlogin.to.relogin&quot;, true);
    
     handlerPrefix = ServerConfigurationService.getString(&quot;portal.handler.default&quot;, &quot;site&quot;);
    
     gatewaySiteUrl = ServerConfigurationService.getString(&quot;gatewaySiteUrl&quot;, null);
    
     basicAuth = new BasicAuth();
     basicAuth.init();
    
     enableDirect = portalService.isEnableDirect();
     // do this before adding handlers to prevent handlers registering 2
     // times.
     // if the handlers were already there they will be re-registered,
     // but when they are added again, they will be replaced.
     // warning messages will appear, but the end state will be the same.
     portalService.addPortal(this);
    
     galleryHandler = new GalleryHandler();
     worksiteHandler = new WorksiteHandler();
     siteHandler = new SiteHandler();
    
     addHandler(siteHandler);
     addHandler(new SiteResetHandler());
    
     addHandler(new ToolHandler());
     addHandler(new ToolResetHandler());
     addHandler(new PageHandler());
     addHandler(worksiteHandler);
     addHandler(new WorksiteResetHandler());
     addHandler(new RssHandler());
     addHandler(new PDAHandler());
     addHandler(new AtomHandler());
     addHandler(new OpmlHandler());
     addHandler(galleryHandler);
     addHandler(new GalleryResetHandler());
     addHandler(new NavLoginHandler());
     addHandler(new PresenceHandler());
     addHandler(new HelpHandler());
     addHandler(new ReLoginHandler());
     addHandler(new LoginHandler());
     addHandler(new XLoginHandler());
     addHandler(new LogoutHandler());
     addHandler(new ErrorDoneHandler());
     addHandler(new ErrorReportHandler());
     addHandler(new StaticStylesHandler());
     addHandler(new StaticScriptsHandler());
     addHandler(new DirectToolHandler());
     addHandler(new RoleSwitchHandler());
     addHandler(new RoleSwitchOutHandler());
     addHandler(new TimeoutDialogHandler());
         //Adding our custom Handler
         addHandler(new TestSiteRequestHandler());
    

}


 ``` Now the below code written in SkinnableCharonPortal's doGet() method will send the request to appropriate handler(which is TestSiteRequestHandler.java in our case). ``` 
 // recognize what to do from the path String option = req.getPathInfo(); String[] parts = option.split("/"); Map<String, PortalHandler> handlerMap = portalService.getHandlerMap(this); PortalHandler ph = handlerMap.get(parts[1]); if (ph != null) stat = ph.doGet(parts, req, res, session); 
 ``` Immediate question comes into mind how that happens? Why TestSiteRequestHandler's doGet is called? Here goes the reasoning : When you request the following url with session received in first step of SSO : http://somehost/portal/worksiteName?sakai.session=sakai-seesion-received-by-you the String variable option in the above code will get "/testSite" as its value. And parts[1] is having "testSite" as its value. Thus we are able to retrieve and call the appropriate handler. The doGet() method of TestSiteRequestHandler looks like : ``` 
 @Override public int doGet(String[] parts, HttpServletRequest req, HttpServletResponse res, Session session) throws PortalHandlerException { User user = UserDirectoryService.getCurrentUser(); if (user != null && !"".equals(user.getId())) { UserEdit userEdit; try { userEdit = UserDirectoryService.editUser(user.getId()); // do all the setting here userEdit.set userEdit.set userEdit.set // commit edit UserDirectoryService.commitEdit(userEdit); // 2nd part is site id SiteService.join(AliasService.getInstance().getTarget("testSite").split("/")[2]); } catch (UserNotDefinedException e) { log.error(e); } catch (UserPermissionException e) { log.error(e); } catch (UserLockedException e) { log.error(e); } catch (UserAlreadyDefinedException e) { log.error(e); } catch (IdUnusedException e) { log.error(e); } catch (PermissionException e) { log.error(e); } catch (InUseException e) { log.error(e); } } return 1; } 
 ```

In the above code you can find the CurrentUser using UserDirectoryService and then you can edit or validate some of the fields based on your requirements. On of the important things that I am doing in the above code is I am associating the current user with "testSite" in this line : ``` 
SiteService.join(AliasService.getInstance().getTarget("testSite").split("/")[2]);
 ```

which is very important. Basically the AliasService(Sakai's inbuilt Service) is used here to return the UUID formatted unique id of the WorkSite required by SiteService for joining a user.

**Handling Multiple WorkSites** In order to achieve our requirements of multiple worksites. Simply add another YourSiteRequestHandler.java in SkinnableCharonPortal.java's init() method. And create a similar doGet() method for YourSiteRequestHandler.java. And then you will be ready to go.

In this blog we learnt how to work with Sakai SSO and logging into different worksites based on some parameters. Hope you will find it useful.

Happy Reading.

   [1]: http://www.google.co.in/url?sa=t&rct=j&q=sakai&source=web&cd=1&ved=0CDIQFjAA&url=http%3A%2F%2Fsakaiproject.org%2F&ei=Ww7_TpTtBNDprQf8zdDcDw&usg=AFQjCNHAJoMs21PrM4GCLIeaD0XE21_LIA&sig2=YEp40b6OyZZKLRq62mpFGQ
   [2]: https://confluence.sakaiproject.org/display/DOC/Sakai+Admin+Guide+-+Advanced+Configuration+Topics
   [3]: http://qa1-nl.sakaiproject.org/codereview/trunk/api/org/sakaiproject/portal/charon/SkinnableCharonPortal.java.html