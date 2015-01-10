---
layout: post
header-img: img/default-blog-pic.jpg
---

# JMatter Framework

The aim of this blog is to introduce the audience about JMatter framework and its features and capabilities. JMatter is a software application framework. It is designed specifically for building business software applications for work groups. Variety of applications can be developed using JMatter like accounting software, software for legal firms etc. The main advantage of JMatter applications is the massive reduction in the development time. Development time is unbelievably small when compared to traditional methods of developing software. **Let see how this is possible.** Most of the applications have a lot of common features like login dialogs, authentication etc. Developers spend most of their time in developing these common features. Technically these can be referred as Orthogonal services which are general in nature across different applications. Some of these Orthogonal features are User Interface , Query Services like display all persons who live in a city for example, Validation, Persistence , Calendaring , Logging ,Authorization , Reporting-The ability to produce printable (PDF) reports from the information that an application manages. JMatter comes up with most of these Orthogonal services and so developers don't have to start from scratch. This in-turn will help to reduce developement time. Let's look at the architecture of JMatter framework..... JMatter and its applications are written in Java. As we know, Model-view-Controller is the most widely used architecture, JMatter provides generic view-Controller implementation leaving the developers to work on model. In JMatter applications all data is persisted to a back end relational database. The JMatter framework uses the Hibernate framework to communicate to back-end databases. JMatter uses the Java Swing Toolkit to produce client applications. The overall architecture has two tiers: the client application communicates over the network to the back-end database. Multiple clients can be installed on multiple workstations, and multiple users can access the system concurrently. JMatter framework is based on Naked Object pattern. This means, the framework auto creates the object-oriented user interface for the Model Objects that we create. **Installation:** Its very easy to install JMatter. JMatter requires J2SE 5 or later and Ant 1.6 or later. Now download the [JMatter framework zip version](http://www.jmatter.org) and unzip it and start using it. **Sample Application:** Lets look at how to create a zero code Contact Manger Application using JMatter. A typical contact manager application manages contacts of different persons and provides CRUD activities on Person. To create a new project in JMatter, invoke the new-project-ui ant target: _**$ cd jmatter $ ant new-project-ui**_ Then a dialog appears asking for the project name and directory. Give the details and click OK. JMatter creates the project directory with the following structure. _**$ cd ../CM $ ls build.xml doc/ resources/ src/ test/**_ JMatter creates the above directory structure for your project, along with its own ant build file. JMatter provides a number of universal business objects, including persons, their contact information, addresses, and businesses. In this example application I reused the predefined implementations of Person and Business, whose fully-qualified class names are com.u2d.type.composite.Person and com.u2d.type.composite.Business, respectively. Before we can generate our database schema, we have to communicate to JMatter the various types of objects we would like to persist. We can do this by adding an annotation (@Persist) directly on the model classes that we define. However in this application, since we’re using pre-existing model classes, we do this by editing the underlying template, src/persistClasses.st, directly: 
    
    
    ..
    com.u2d.type.composite.Person
    com.u2d.type.composite.Business
    ..
    

We’re now ready to generate the database schema: _**$ ant schema-export**_ Note that, behind the scenes, JMatter generates a Hibernate database mapping file for each class, and finally asks hibernate to generate the DDL (data definition language) instructions and send them to the database. By default JMatter uses the H2 database but we can configure it to use any database. **The Class Bar: ** JMatter gives the default screen for all its applications. A class bar is present on the left side of the JMatter application screen which contains different icons like Users,roles, types , folders etc. By right clicking on these icons we can do all CRUD operations on them.All these operations are provided by JMatter pertaining to Naked Object pattern . We can also add our own model objects to the class bar after writing the classes. We can edit src/class-list.xml to add our own Model Objects to Class Bar. 
    
    
    
    Class List
    
    
    Model
    
    com.u2d.type.composite.Person
    com.u2d.type.composite.Business
    com.u2d.type.composite.Folder
    ...
    

Here we have added the person object already provided by JMatter. Running the application: **run the ant target , ant run**. Login to the application with default admin (admin) login. We can also change our password. Go on to create the person objects with his contacts. We have written _0 lines of code _for this application. Makes developer's life easy!! JMatter provides the CRUD capabilities for Person or any other Object you develop following the Naked Object pattern. All the database operation and hibernate mappings are taken care by JMatter. Orthogonal services like login dialog, logging, persistence , localization etc are provided by JMatter. We just need to implement the Model Objects. Following is the example for a sample class for Object Song. 
    
    
    package com.u2d.mytunes;
    import com.u2d.type.atom.StringEO;
    import com.u2d.type.atom.ImgEO;
    import com.u2d.model.AbstractComplexEObject;
    import com.u2d.model.Title;
    import com.u2d.list.RelationalList;
    import com.u2d.persist.Persist;
    @Persist
    public class Album extends AbstractComplexEObject
    {
    private final StringEO _name = new StringEO();
    private final ImgEO _cover = new ImgEO();
    public static final String[] fieldOrder = {"name", "cover"};
    public Album() {}
    public StringEO getName() { return _name; }
    public ImgEO getCover() { return _cover; }
    public Title title() { return _name.title(); }
    }
    

We can add this class to class bar and start using it. JMatter provides all the CRUD operations on it. Now let us see what the above code (Album class.) is doing... **AbstractComplexEObject** is the base class extended by all JMatter model objects. JMatter gives us many atomic types to choose from, including types for phone numbers, social security numbers, time durations, images, email addresses, colors, and much more. StringEO is just extension to String. There are two differences between a StringEO and a TextEO: 1) StringEO are saved to the database as varchar types (limit is typically 255) whereas TextEO’s are saved as text types or some other similar large text object; and 2) StringEO editors in the GUI are text fields whereas TextEO editors are text areas. We can think of the EO part of the type name as being an acronym for Enhanced Object. **Conclusion: ** I hope this gave you detailed introduction about JMatter. I am still not sure how much we can use JMatter if the application complexity increases. But it is absolutely terrific for small applications for Works Groups connected by LAN. I would be glad to your experience about handling complex projects with JMatter. references: jmatter.org For more information visit <http://www.jmatter.org>.