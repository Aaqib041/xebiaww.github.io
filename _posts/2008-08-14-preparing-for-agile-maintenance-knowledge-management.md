---
layout: post
header-img: img/default-blog-pic.jpg
author: vashishtha
description: 
post_id: 697
created: 2008/08/14 20:38:44
created_gmt: 2008/08/14 18:38:44
comment_status: open
---

# Preparing for Agile Maintenance - Knowledge Management

When you think about documentation in Agile software development, most of the times it talks about "just enough" which definitely makes sense considering the thickness of design documents in traditional software development. The Agile mind specifically thinks what actually is required in terms of documentation.

Agile software development also gets translated into efficient communication, collocation and sitting in one room or same table and having conversations whenever there are issues. Some of the XP practices like pair-programming helps a new joinee to come upto the speed when she joins the project.

When you talk about effective communication and resolving issues in a team as and when they arrive, you lead towards a situation in which knowledge resides in the heads. People may not feel the need of documenting in detail as they seem to know everything about the project. You may end up in a situation where a new team doesn't have "enough" documentation to begin with in maintenance phase. That's why when one talks about "just enough documentation", that "enough" word needs to be quantified.

In Agile software development we focus a lot on test-driven development and sometimes we say, code is the documentation. Somehow I don't seem to agree with this argument. I don't deny the fact that source code should be written in such a way that it's highly readable and maintainable at the same time. So I do agree to this argument in literal terms. But when you want to explain the application to a developer, test-cases are too fine grained as concepts. The simplest thing to do is to explain the application on a white-board. Looking the applications from source-code perspective leads towards a situation where people are able to see different parts of the projects individually but may not be able to see the entire theme of the application. You can take a look at a very [interesting discussion][1] on this topic between Jim Coplien and Bob Martin.

Let's take a scenario. Software development is finished and the application is up and running in the production environment. The application is bound to go in maintenance phase after certain time. What happens when you don't have any developer in maintenance phase who worked on this project in development phase? You get into a lot of trouble. A lot of things like certain design decisions, functional aspects begin to look hazy.

![Blind men and an elephant][2]

### What makes the life of a new developer easier?

When a new developer enters in a maintenance project, instead of looking directly into source-code, he'll definitely want to see the project in its entirety from 20,000 feet. It may mean to get the idea about what the application is all about in holistic perspective, its functionality, stakeholders, interfacing points and the value of the application in business terms etc. It may be followed by a discussion on the architecture fundamentals of the project and then going into technicalities and functionality of each component. Depending on the parts the new developer need to focus on, she can be given details about those components individually. After that for sure she can dive deeper in the source code. It makes the life of a newcomer a lot easier as introduction to something new happens in gradual process and not from a "blind man looking at elephant" perspective.

### How do we actually do it?

Before we talk about how we really do it, let's make our assumption very clear that we want to reduce the number of project-veterans in a gradual phases. It seems quite inevitable as most of the people want to look for yet another life after a long project. In the end it should reach to a situation where old developers have minimal or nothing to do in maintenance phase. So it makes sense to proactively induct new people in the project in phases while releasing the veteran developers from the project. Essentially, end target should be a totally new team going into maintenance phase. Ideally it makes sense to think in a way that there will be no support available from old developers in maintenance cycle in ideal case.

You may say that when you recruit new people in phases, the velocity of Scrum sprint may go down. There are two aspects to it. One is - customer should be aware that reduction in team velocity in inevitable but in the end, it is going to prepare a team and set of knowledge base which will be good for them in maintenance phase. Another way to handle it is to recruit multiple people when an old developer is removed from the project. This scenario makes sense from financial point of view when you are trying to outsource the maintenance phase to a distributed team.

Just now we talked about what all is required to be effective in maintenance phase. Now let's talk about how to get it done and be prepared for it in development cycle itself.

First of all, based on our analysis to ascertain what all documentation is required for maintenance phase, we need to conduct a gap analysis. Based on the gap identified, the documentation can be done in innovative way. As discussed in [one of Xebia blogs][3] (underlines the importance of multimedia usage for documentation), when we want to do something like project overview, it's good to do that on a white board followed by a question answer session from new joinees and get it recorded as a video. The same can be done for knowledge transfer of architecture presentation and application functionality. Instead of doing knowledge transfer for each joinee, it's good to get the content recorded.

When one wants to dive deeper in application functionality, there may be documentation available to cover the functionality and technical aspects of different parts of application. Whenever required one can also use screencast also for installation guidelines. Along with high-level documentation available in form of videocast, written documentation for individual components gives a very good start for a new developer.

As new developers join the project in the last phase of development cycle, they first refer the already available documentation (videocast and written). If they find any gap, they refine it further. Now the documentation which we are talking about right now is "just enough" by definition. No thick volumes of documents. However it should be sufficient for a new Java developer to come upto the speed with the project. As project moves, old developers leave project, new developers join and enhance the knowledge base, based on their own experience, problems faced with available knowledge base.

### Conclusion

With this documentation and knowledge base in hand, it becomes relatively a lot easier for the new team to go into maintenance phase without much hiccups. Even though so far we have been talking about no experienced developers in the end in ideal scenario, in reality it's always good to have an experienced developer available for 1 day in a week to clarify issues if any for initial phases.

   [1]: http://www.infoq.com/interviews/coplien-martin-tdd
   [2]: http://xebee.xebia.in/wp-content/uploads/2008/08/bioresonance-technologies-6men.jpg
   [3]: http://blog.xebia.com/2008/05/05/agile-way-of-documentation/