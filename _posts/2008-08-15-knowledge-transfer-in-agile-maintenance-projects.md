---
layout: post
header-img: img/default-blog-pic.jpg
author: vashishtha
description: 
post_id: 698
created: 2008/08/15 11:10:24
created_gmt: 2008/08/15 09:10:24
comment_status: open
---

# Knowledge Transfer in Agile Maintenance Projects

When you think of inducting a new developer in existing project, it's relatively easier to do it in an Agile software development project than in a traditional project. The atmosphere and programming culture is entirely different here compared to any traditional project. Instead of people working in isolation and being responsible for assigned tasks, people here work in a mode where frequent communication across table is necessary. Instead of one person being responsible for assigned tasks, the whole team is responsible to complete it.

The mantra is efficient communication and more interactions. So when a new developer enters the Agile project (Scrum + XP based), pair programming, communication across the table makes a person comfortable with the new project environment. Instead of going through bulk of developer's handbook and design document, conversations help to bridge the gap. However when you need to really need to refer some documentation, it's always there. Also new developer continues to develop on top of whatever existing team has built on. So you see, knowledge transfer is seamless and relatively easier compared to any traditional project.

Think about maintenance phase now. First of all, application is already existing. You may not have to enhance it further. So the scope of learning while developing software is not there. You get a production problem and now you have to see in which part problem lies. Developers who developed the application may not be there anymore. You may be in a situation where you don't have any knowledge base of the project. You may try hard to go through the application with source code analysis but a normal human becomes helpless analyzing 1 million lines of code-base in a complex project. Understanding application with source-code analysis may not be the ideal way in this case. So you see knowledge transfer in maintenance phase may be a lot more trickier than in normal development cycle.

## What all is required in knowledge transfer?

Considering all these issues, I am going to talk about an approach for knowledge transfer (KT) used in one such complex maintenance project.

To start with, in generic terms, KT can be divided in following parts: 

  * Application Overview - 20,000 feet view on the project as a whole. Generally it needs to be described on white-board. Many of the times, this discussion gets recorded as video and becomes the documentation for new joinee
    
        <li>Application Architecture and technical design - Again focusing on broad perspective of the application. But in this case, instead of taking a look from 20,000 feet, we dive a little deeper. Explained and documented in the same way as mentioned for Application Overview</li>
    
    <li>Sessions for knowledge transfer of different application components describing functional and technical aspects. Some of the documentation is available. Rest is improved with the understanding of each new joinee</li>
    
    <li>Scrum Process - How a team does the Scrum in the project. It may be a little different in maintenance project as it generally follows <a href="http://blog.xebia.com/2007/04/25/type-c-scrum-explained/">Type C Scrum</a>. The documentation is generally available on project wiki. </li>
    
    <li>Demo of running application - Whatever you talk about theoretically, may not make sense if you can't think it visually. </li>
    

## How do we do KT?

For doing the KT we adopted following methodology. 

### Set up the development environment

The first step a new joinee takes, is to set the development environment. Project knowledge-base should be prepared in a such way that time-waste is minimal. So one-touch build is recommended. Instead of getting project softwares from internet and wasting time, they may be available in the local repository. Similarly for setting up the project-environment, a screen-cast can be prepared which becomes handy instead of asking people for trivial things.

### Study the documentation available

Before new developer gets involved with the knowledge transfer along with senior developers, he need to first study the documentation available in the order mentioned by project-mentor. It provides the initial impression of the application to the new joinee.

### Create a wiki page defining schedule and review

Instead of doing the knowledge transfer on adhoc basis, it's important to properly plan it using wiki pages. Here one plans for the time require from team members and schedule the whole knowledge transfer to be effective. Keep in mind to add some slack in the schedule for the new joinee to digest the information.

### Team involvement in knowledge transfer

Instead of doing the knowledge transfer in teacher/student mode, it's more about teacher+assistant teachers transferring the knowledge to a student. The basic reason of doing that is to reinforce the application concepts in the team. It's good for teachers as well as for students. Depending on questions/answers, different perspectives come out while discussing a concept which is good for team in general.

### Last person taking KT conducts the KT for a newcomer

It works pretty good to reinforce the project-concepts for a team-member. He becomes the main teacher assisted by other senior teachers. By having the healthy discussion on project topics, it works well for the entire team. So instead of having a team of one expert, there will be multiple project experts. The methodology mentioned, encourages the teacher to ask other assistant teachers their opinion on the available information. It gives almost every time a new insight in the knowledge. Sharing/discussing knowledge increases the knowledge itself.

### Improvements in the documentation

Along with knowledge exchanged between senior developers and new joinee, a very important task is to document the gap new joinee could see in the available documentation. It's simultaneous effort along with continuing knowledge transfer. It's important because with the improved documentation in form of multimedia and written documents, next KT becomes even more easier.

## Conclusion

As you can see based on above mentioned discussion, knowledge transfer in an Agile maintenance project is not same as may be a case in an Agile development project. It poses different kind of challenges to the team. Above mentioned strategy worked very well to handle the knowledge transfer of a complex project. Any suggestions for improvements areas are welcome.