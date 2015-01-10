---
layout: post
header-img: img/default-blog-pic.jpg
author: GauravS
description: 
post_id: 12599
created: 2012/03/09 15:01:18
created_gmt: 2012/03/09 10:01:18
comment_status: open
---

# iOS vs Flex: Do I need more frameworks?

Which framework should I choose for this application? - This was the most common question which I used to think before starting any application. But since the time I am working on iOS, I hardly remember when did I ask this. As per Apple's technical guidelines, you 'always' have to stick to the default framework. Fortunately or unfortunately, the default framework is the only framework made available by Apple. But what is meant by 'only framework'?

Consider a following scenario - When you start developing any application, one of the important things is to setup the project structure. For instance, if you use a architecture/framework you need to define couple of folders to place your services, value objects, model, etc. I will be talking about Adobe (now Apache) Flex and Apple's iOS.

**Apache Flex** \- You will define folders for all entities that you require in your application (Services, Model, Controllers, Views, etc.), depending upon the framework you are using. For instance, if you are using Cairngorm you will have folders like Command, Event, Delegate, VO, etc. But if you switch to PureMVC for instance, then your folders would be Mediator, Proxy, Business, etc. So based on the framework you define the folder structure.

One thing more to note here is - There is no default framework available for Flex developers which can immediately get them started rather than first creating folders for placing different entities.

**Native iOS** \- When you start developing your application by creating a New Project, you will find that some standard files will be generated for you. For any iOS application following are the standards: 

  * Every class will have a Header file (with .h extension) and an Implementation file (with .m extension).
  * Their is an AppDelegate class available which acts as a starting point for your application.
  * iPhone/iPad is all about screens. So every screen has its own View Controllers (By convention we name it by suffixing 'Controller' to it).
  * Couple of configuration files (.plist, main.m).
So above mentioned files will be always generated for you when you create a new project. iOS application development works simply on MVC pattern. Talking about them individually,

MODEL - There is file with .xcdatamodel that is made strictly to act as your model.

VIEW - There is file with .storyboard extension that is made strictly to hold all your views.

CONTROLLER - As i mentioned above, there are files with .h and .m extension that can act as your controllers.

It is always a good practice to create folder structures and place your entities. But there is a difference in the way Flex and iOS application development works. If you create folders for a flex project, it will be reflected in your hard drive as well. But if you develop application for Native iOS (on Mac of course) and you create folders, you will find that its not reflected on the hard drive.

So you can conclude from the above read that to develop Flex applications you need to have understanding of the framework you want to use, but for native iOS applications Apple gives you a default framework to work with. So for iOS development, i don't see the need of having more frameworks.