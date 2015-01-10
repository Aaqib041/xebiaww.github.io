---
layout: post
header-img: img/default-blog-pic.jpg
author: pranjan
description: 
post_id: 7515
created: 2011/01/18 00:57:26
created_gmt: 2011/01/17 19:57:26
comment_status: open
---

# Extreme Programming - Designing With CRC Cards

We were having problems in most of the design discussions we did as a team. We couldn't get out of the discussion with everyone's consensus on the selected design. In the end, people still nagged about the loopholes in the design or kept proposing a better design.

One day we came across “Designing Using CRC Cards” in extreme programming. It looked a bit interesting to us. However, we did not have any references to fall back on. So, we thought of experimenting with it and the results were promising.

Let me describe this process of designing in brief. CRC stands for Class, Responsibilities, and Collaboration. The class of the object is written at the top of the card, responsibilities listed down the left side, collaborating classes are listed to the right of each responsibility. The discussion happens around these cards which evolves as a design. Please [refer this][1] for further reading.

Lets talk about the traditional approach of designing first. Here, the team assembles, someone takes the lead and describes the problem and his thoughts on the design. Generally, he is the senior most guy of the team or the person who is supposed to implement it. Then a series of discussions start around it. The problem with this approach is that the discussions generally get out of hand and become generic in nature. The discussions can deviate from the current functionality to other parts of the application.

Here people talk around boxes drawn as components. And when people talk around boxes, they can never get into the nitty gritties which can later become a bottleneck somewhere.

To some, these sessions become an opportunity to exercise/display their finesse of design principles and concepts. Behavioral problems also come into play in these discussions. Some people like to dominate the meeting by never giving another person any chance to speak, no matter how good/bad their idea is. Some introvert people don’t even bother to speak even if they have some good idea with them.

So, when we tried CRC cards with designing, the first thing we noticed was, no one was leading/dictating the design discussion. The whole team sat together and each person created one card per class as per their design. This also ensured that ideas from each team member come up. Then each member presented their cards, talking about the responsbilites it will have.

The first round of describing cards was not that deep. So, it took very less time, and faulty/incorrect ideas where dismissed in the beginning. In other words, we can say that the best of the ideas were filtered out pretty quickly.

Now, instead of talking around boxes, we were talking about real classes where we were able to state their responsbilities. This allowed us to think in Object Oriented way. The selected cards were further discussed and their weaknesses and problems were easily uncovered. We continued doing this until everyone agreed upon the design.

The cards kept a check on the discussion, so that it could not divert to other areas of application. As the discussions were card centric, so, the discussions revolved around them only and we ended up designing exactly what was required. Due to all these, the time taken was comparatively one fifth of what it would have been in a traditional designing session.

Also, in the end we had a written design, which was more than a set of high level boxes. We had classes, its responsbilities and how they were going to communicate. Sometimes, we used white board also to explain things a little bit, but even that was card centric.

Therefore, I will say that “Designing With CRC Cards” makes designing comfortable and effective. If traditional design sessions are giving you headaches, and you want to design as a team, then try out “Designing With CRC Cards”, it will work.

   [1]: http://www.extremeprogramming.org/rules/crccards.html (Designing With CRC Cards)

## Comments

**[Tarun Sapra](#4963 "2011-01-19 09:54:52"):** Very interesting, every team member would be an equal participant in this activity.

**[Alexander](#5349 "2011-03-13 16:02:51"):** You may want to have a look at CRC cards for Android app here: http://dw3105.blogspot.com/2011/03/crc-cards-released.html

**[Pratik](#4946 "2011-01-18 10:46:03"):** Interesting stuff , will give it a try in our team.

**[Ignacio](#5784 "2011-07-29 16:52:46"):** Nice post! I'm studying Agile methods and I like to see some practices work fine for many people. CRC Cards on Android are funny but I think it's tricky to work with cards in mobiles. It would be nice CRC Cards on muti-touch tables :) Anyway, I'm impatient for practicing with our team. Thanks!

