---
layout: post
header-img: img/default-blog-pic.jpg
author: vashishtha
description: 
post_id: 6102
created: 2010/11/03 19:12:30
created_gmt: 2010/11/03 14:12:30
comment_status: open
---

# Agile Testing - Incremental Functional Test Approach

Software development is all about incremental functional development.

However we find a great impedance mismatch between development and testing synchronization. Though we talk about continuous integration and verification, most of the user stories come for verification only at the end of sprint which pose a major challenge in front of the testers just because of mismatch between the time available and amount of work to test.

To this point, developers may argue that they cannot give unfinished feature to the testers as they broke their work (user-story) according to technical subtasks. So for a developer Controller class has been written but Service still needs to be written. So tester may not be able test the technical tasks. Tester is more interested towards functional part completion instead of unit tests. This results in a impedance mismatch between developers and testers.
    
    
    ![][1]

In our project, as a team we agreed in retrospectives that we will push the functional features as soon as they are finished but eventually it doesn't happen. We come back to the square one and the same problem is raised again in next retrospective.

So what do you do in this kind of situation?

Recently [Vikrant][2] our lead tester came out with a very good idea.

The argument was - why don't we break user-stories into functional subtasks also apart from breaking them into technical tasks. That way it's not either-or case but we'll have both functional and technical tasks. Developers then are also forced to think in terms of functional subtasks and are able to provide functionality in small chunks to testers.

Let's see how it works in real action.

As soon as, a developer finishes a functional subtask, he moves it to "Ready for Test" status in JIRA which indicates it's available for tester to test. This way testers are able to to test the user stories in incremental fashion. When team breaks the user stories into functional subtasks, both developer and tester sit together, collaborate and agree upon the functional division of the story.

This experiment worked pretty well for our team. Though we still see some areas of improvement, it enabled us to fix to the problem we had in our so many projects.

   [1]: http://xebee.xebia.in/wp-content/uploads/2010/11/tester-at-the-end-300x225.png (tester-at-the-end)
   [2]: http://in.linkedin.com/pub/vikrant-manchanda/1b/369/8bb (Vikrant)

## Comments

**[ShriKant Vashishtha](#3216 "2010-11-12 09:57:19"):** @Javo This is possible when user-stories are small enough. Sometimes user-stories are big and take time to finish. I agree that pair-testing at the development time itself will help in this scenario but that too depends on many factors like for 7 members of team, testers is available for each member at the desired time (before commit), distributed nature of the project where time-difference doesn't permit or allow to do pair-testing always. In those scenarios, though you'd not like to add one more process step, we didn't find much choice and the solution worked for us. If the whole team is collocated, pair-testing only with developer will work I think.

**[sonnygill](#3135 "2010-11-04 14:19:11"):** Good to see Vikrant is still giving you all a hard time :)

**[ShriKant Vashishtha](#3136 "2010-11-04 14:38:18"):** He is one of the best we have :P

**[Javo](#3176 "2010-11-09 23:52:45"):** Seems like you're doing small cascades frequently. Why do we need to break functionality into sub functionalities. Even if you're doing testing earlier than before, you're adding multiple final testing phases and very probably increasing the management for each one. I suggest for the entire process, just to use exploratory sessions which could include, pair testing with developers in order to verify each component previous to any integration. Testers could take each session as a great way to learning the functionalities, meanwhile they could detect critical scenarios, possible failures and/or defects, and enhancement opportunities also. I really think you don't need to wait to start testing and you don't need more details into your projects management.

**[ShriKant Vashishtha](#3334 "2010-11-23 14:16:25"):** There can be distributed teams working in the same time zone. Collaborating in distributed way with existing toolset available is not a problem at all. Think about a time difference of 12 hours (US and India) or 4 hours (Europe and India). In practical terms you get a lot less shared distributed hours. In this case, it's not possible to do the pair-testing with developer in real time. The solution which I mentioned worked for us and I should say it's more suitable for distributed teams in different time zones.

**[Javo](#3255 "2010-11-18 03:17:22"):** do not focus on technique, but in the approach. I mentioned 'Exploratory Testing' and if the team is able to do pair-testing is not so relevant as all you could do before try any functional testing approach. In the other hand, today we don't have great impediments because the technology that facilitate us to collaborate. I've been working whit not-collocated teams and just in a few situations was not possible to do some type of testing very early. In regards with how big a User Story is, I think that if something is big enough that avoid you to start some verification and/or validation, very probably the team needs to change the way the work is partitioned. For me the main reason to avoid start working in testing since the first day is just a misconception issue and in the most cases a lack of interest from Testing guys. I insist, if testing is embedded in the entire team, you don't need to break functionalities in sub functions and wait until the end to start testing. Probably I need to know a little bit about your point of view. Kind regards, Javo.

