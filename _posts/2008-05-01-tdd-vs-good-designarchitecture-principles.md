---
layout: post
header-img: img/default-blog-pic.jpg
author: vashishtha
description: 
post_id: 542
created: 2008/05/01 15:41:51
created_gmt: 2008/05/01 13:41:51
comment_status: open
---

# TDD vs good design/architecture principles

Sometimes back there was an interesting [debate][1] between Agile manifesto founder member Bob Martin and renowned Jim Coplien about the relevance of good design principles in TDD based development.

Jim had very thought provoking views about what people do in the name of TDD and when it leads to something where it's just impossible to do any kind of refactoring further as there was no overall sound design/architecture base.

It leads to following questions:

  * While practicing TDD and Scrum, is it really possible to focus on a good design considering we just focus on the prioritized list of requirements and don’t really see their interactions with other user stories to begin with? 
  * Without having a big picture in shape of logical diagram, class diagrams (read UML), deployment diagrams, is it really possible to create a good architecture/design? May be it’s possible in small scale projects where the level of complexity in terms of business logic is not too high. But the question is more relevant for big projects (1-2 million LOC). 

There were some interesting follow up in this respect from various people in Xebia. I thought it'll be a good idea to combine all those thoughts through this blog.

[Lars][2] responded to the first question:

> Yes of course. Why do you think you only have to focus on the prioritized list of requirements? As a professional programmer you should always keep thinking ahead (not too far ahead though…), keep the big picture in mind see the coherence between the User Stories. If it is not clear to you make sure you ask your teammates or the PO. There is nothing in TDD or Scrum that says “Forget about the big picture”. Practicing TDD in my opinion doesn’t mean you don’t do any design at all.

  
There was an interesting response from [Viktor Grgic][3]:

> Customers are just about to see importance of good architecture and role of an architect. Most of the projects I've been working on, architecture was done by developer as a secondary task and it was always a mess. Although they didn't use Scrum, I don't believe Scrum would solves this problem. Saying to developers "keep the big picture in mind" does not solve the problem. In my experience, it only creates inefficient discussions because of the lack of knowledge about all kinds of stuff (business & IT strategy, standardization, politics, and so on). The discussions are about their believes which technology is the best and they hardly see for example the complexity of conflicting requirements. By the way, architecture in this context is not only technical. All this does not mean that programmers shouldn't be involved in design and architecture. In contrary, as much as possible. Besides, defining architecture is a very dynamic process and fits well in Scrum and Agile principles. 

  


And there were quite good thoughts about the relevance of documentation (UML, Multimedia, white-boards) in the discussion. I had some interactions with people working in Agile methodology and they somehow suggested as if they don't really like UML at all. I couldn’t understand why. I still believe without a good understanding of design and architecture, it’s just not possible to create a good application from bottom-up (micro-macro) approach. Essentially it means, without understanding the dynamics between classes, you can’t write a good application. So writing a test case first and evolving further without understanding the whole context may not make sense.

[Erik Pragt][4] wrote:

> I don’t think that people working with Agile hate UML. I don’t at least. What I don’t like, however, is creating upfront sequence diagrams, which is also part of UML (mainly for the reason that I usually come up with all kinds of unnecessary abstractions which just look great, but are not needed at all). Upfront, and afterfront class diagrams help a lot to understand the system and the relations in the domain model, and also a deployment or other kind of overview diagram helps greatly in understanding the system. 

  


[Jan Vermeir][5] had an interesting view on sequence diagrams:

> In the design-to-help-you-think category I also use diagrams to understand code during audits. Sequence diagrams really shine here since they give me a quick overview of how one component uses another, which I find useful. If you're going to create diagrams for documentation I think UML is the standard. For sketching on white boards however you might as well draw rectangles. 

  
Thoughts from [Serge][6] about good documentation:

> For a while now I've come to value "good documentation" in a different way. I don't believe UML is better or worse than sketches. With Robert vL I've been doing lots of stuff with multimedia in our work. So my answer to "good documentation" is not UML vs. sketches, it's using multimedia for the stories, writing and diagrams for reference. I have found it to be very much more effective than the best document ever written. 

Please leave your comments about this topic and what you think in general about Agility in practice.

   [1]: http://www.infoq.com/interviews/coplien-martin-tdd
   [2]: http://blog.xebia.com/author/lvonk/
   [3]: http://blog.xebia.com/author/vgrgic/
   [4]: http://blog.xebia.com/author/epragt/
   [5]: http://blog.xebia.com/author/jVermeir/
   [6]: http://blog.xebia.com/author/sbeaumont/