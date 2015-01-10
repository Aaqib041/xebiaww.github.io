---
layout: post
header-img: img/default-blog-pic.jpg
author: rgoyal
description: 
post_id: 622
created: 2008/10/16 06:33:18
created_gmt: 2008/10/16 04:33:18
comment_status: open
---

# Experimenting with OSGi on Server Side

I was introduced to OSGi quite by coincidence. I had to give a presentation and a colleague suggested to look into OSGi. And once I started looking into it, I was quite surprised to find the ease with which it provided the solutions to some of the problems that we face in the web application development. There are so many good articles and blogs which provide a great amount of material regarding OSGi. They are mentioned in the reference.

One of the concepts of OSGi that really intrigued me was how it allows bundles to export services that can be consumed by other bundles without knowing anything about the exporting bundle. OSGi takes care of this by introducing Service Registry where the exporting bundle registers the interfaces that it want to expose and any other bundle which wants to use those interface can just look up in the registry to use the implementation. The other concept of OSGi which I also found interesting was how OSGi uses version management to allow different versions of the same java class to be used within the project. So I created this blog to explore these concepts in greater detail. At this point, it would be important to mention that the intended audience for this blog are people who are new to OSGi. Also the example used are very basic and intend to show just the flow of control from one package to another.

To show how OSGi uses the Service Registry, I have created 2 bundles (Service and Dao) which register their interfaces. There are 2 more bundles (ServiceServlet and DaoServlet) which reference the registry to look for these interfaces respectively. I have created servlets to call the methods of the interface to show how we can create a dynamic web enabled project using OSGi. Finally to show the version management. I have created 2 different bundles having the same package and the same java class but different methods. These bundles would have different versions and we will see how these 2 methods can coexist.

**Environment Setup**: Prerequisites: 

  1. You must already have Java 1.5 or above installed in your system path. For my installation, I am using Java 1.6.
  2. You must already be familiar with Eclipse.
  3. You must already be familiar with how to create a bundle and how to use OSGi commands. If you are not familiar with these , then it would be a good idea to understand first how to create a simple [OSGi bundle.][1]
For this setup, I am using [Eclipse Europa][2] which comes with the Equinox framework required for running OSGi. You can also [download][3] the latest version of the eclipse. The OSGi container [Equinox][4] comes inbuilt with Eclipse 3.2 or later. Incase you are using Eclipse 3.1 or earlier , you would need to [download Eclipse Equinox][5] plugin.

For this setup, we are using Jetty as the server. But Equinox can be configured to work with Tomcat.

**Setup the Execution Environment**

Make sure that you have installed the JVMs you wish to use and that you have mapped each OSGi Execution Environment to one of the JVMs. Go to Window --> Preferences --> Installed JREs --> Execution Environments.

![][6] You can [download][7] the source. Import the packages into eclipse and skip right to the First Scenario. 

  1. **Create a bundle SampleDao with 2 packages.**
    1. **Package com.sample.sampledao** contains an interface MyDao which has a method List retrieveAllObjects()

> >         public interface MyDao {
>         public List retrieveAllObjects();
>         }

    2. Package **com.sample.sampledao.impl** contains the activator for the bundle as well as the implementation for the MyDao interface. In the activator , we are registering the interface in the service registry so that any package from outside the bundle can access it.

> >         public class Activator implements BundleActivator {
>         private ServiceRegistration daoregistration;
>         public void start(BundleContext context) throws Exception
>         {
>         System.out.println("inside sampleDao bundle");
>         MyDao mydao = new MyDaoImpl();
>         daoregistration = context.registerService(MyDao.class.getName(),
>         mydao, null);
>         }
>         
>         
>         
>         
>         public void stop(BundleContext context) throws Exception
>         {
>         System.out.println("Outside sampleDao bundle");
>         daoregistration.unregister();
>         }
>         }

    3. Make sure to add the Activator in the Overview tab of the Manifest.MF.
![][8]
    4. The implementation code would be. For now leave the commented out code as it is.

> >         public class MyDaoImpl implements MyDao {
>         public List retrieveAllObjects()
>         {
>         //Test test= new Test();
>         System.out.println("Inside retrieveAllObjects of MyDao");
>         //test.testMethod2();
>         return null;
>         }
>         }

    5. In the Runtime Tab of the Manifest.MF, click on Add button in the Exported Packages and select com.sample.sampledao to export it. Now click on properties and select the version 1.0.0.
![][9]
  2. Create a bundle **SampleService** with 2 packages. 
    1. Package **com.sample.sampleservice** has an interface MyService.java with the method List findAllObjects().

> >         public interface MyService
>         {
>         public List findAllObjects();
>         }

    2. Package **com.sample.sampleservice.impl** contains the activator for the bundle as well as the implementation for the MyService interface. In the activator, we are registering the interface in the service registry so that any package from outside the bundle can access it. We are also referencing the service registry to look for the service provided by the Dao Interface.

> >         public class Activator implements BundleActivator {
>         private ServiceRegistration registration;
>         public static MyDao myDao;
>         ServiceReference daoServiceReference;
>         public void start(BundleContext context) throws Exception
>         {
>         System.out.println("Inside sampleservice bundle");
>         MyService service = new MyServiceImpl();
>         registration = context.registerService (MyService.class.getName(),
>         service, null);
>         daoServiceReference= context.getServiceReference(MyDao.class.getName());
>         myDao =(MyDao)context.getService(daoServiceReference);
>         }
>         public void stop(BundleContext context) throws Exception
>         {
>         System.out.println("Outside sampleservice bundle");
>         registration.unregister();
>         context.ungetService(daoServiceReference);
>         }
>         }

Again make sure to add the activator in the Overview Tab of the Manifest.MF. Also go to the Dependenies Tab of the Manifest.MF and import com.sample.sampledao package.

   [1]: http://www.javaworld.com/javaworld/jw-03-2008/jw-03-osgi1.html?page=1
   [2]: http://www.eclipse.org/downloads/moreinfo/jee.php
   [3]: http://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/ganymede/R/eclipse-jee-ganymede-win32.zip
   [4]: http://www.eclipse.org/equinox/
   [5]: http://download.eclipse.org/eclipse/equinox/
   [6]: http://blog.xebia.com/wp-content/uploads/2008/07/executionenvironment-300x279.gif (Execution Environment)
   [7]: http://xebee.xebia.in/wp-content/uploads/2008/07/osgisourcecode.zip
   [8]: http://blog.xebia.com/wp-content/uploads/2008/07/addactivatorfordao-253x300.gif (Add Activator For Dao)
   [9]: http://blog.xebia.com/wp-content/uploads/2008/07/exportsampledao-300x122.gif (Export SampleDao)