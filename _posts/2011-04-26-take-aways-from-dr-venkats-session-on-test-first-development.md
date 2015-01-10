---
layout: post
header-img: img/default-blog-pic.jpg
author: pranjan
description: 
post_id: 8739
created: 2011/04/26 01:35:28
created_gmt: 2011/04/25 20:35:28
comment_status: open
---

# Take Aways from Dr. Venkat's Session on Test First Development

<p style="text-align: justify;">This Sunday (24th April) Xebia organized a session on Test First Development by Dr. Venkat. He is a well known personality in Agile and Extreme Programming field. He has authored several books and has conducted numerous workshops on several areas of Agile.</p>

<p style="text-align: justify;">The session consisted of three parts, first a question answer round, then an interactive TDD workshop and then an open discussion.</p>

<p style="text-align: justify;">Several topics were discussed throughout the day and Dr. Venkat as well as the audience shared their thoughts on them. To begin with, the discussion started with the importance of creating an environment to make people do the right thing than to force them to do it. Dr. Venkat gave huge emphasis on creating a healthy environment which can motivate people. He said that forcing people to do things without letting them know "why" are we doing this, never works in longer run. People will start doing it, then slowly lose enthusiasm as they do not know the reason behind it, and after some time, they would not do it anymore. However, when we create an environment where it looks very natural to do certain things, really helps. He cited examples from his life, where he used it and it worked. The right environment can be created through leading by example and assertive communication, to name a few.<!--more--></p>

<p style="text-align: justify;">Then, the topic of scaling up an organization was discussed.  The problem mentioned was that, when the organization needs to scale up (grow in size by hiring more people) rapidly, it is very tough to align the new joiners with the organizations principles and ethics. When we need to grow at a great pace, we might not get people who can easily fit into the organization and we might end up hiring some unfits for the organization. Even after providing ample training to the people, sometimes they simply do not fit in. Dr. Venkat proposed two solutions to this problem by citing two different examples. He talked about organizations who simply refuse to grow until they find the best suitable people for their organization. But, they grow at very slow rate. However, there are different organizations also, which hire people who need some training/learning etc before they fit it, and, they ask these people to work in different departments of the organization and to learn and adapt how things work. He cited companies where new joiners are on probation for two years before becoming a permanent employee. So, there are pros and cons of both things and the organization will have to decide which way it wants to go.</p>

<p style="text-align: justify;">Then he started the workshop on Test First Development. The project was to create a TicTacToe game. Before starting the workshop, he talked about the strategic design and the tactical design of a system. He said that the strategic design is the high level design of any system telling about the major components doing certain things and the interaction between them. However, the strategic design does not go into the details of the implementation. On the other hand, the tactical design would be to know how exactly the component is going to work. The tactical  design contains each and every minute detail of that component. Comparing it with a real world example, he told that strategic design could be locating a restaurant in Delhi, and the strategic design can say that it is located in the South-West of Delhi. However, the tactical design will tell about each and every road, street, corner, turn and landmarks describing exactly how to reach there.</p>

<p style="text-align: justify;">Talking about test driven development, he said the strategic design can be done on a whiteboard or a piece of paper. However, the tactical design is done while developing the application through tests. He said that the TDD is of no use if you can not think of the evolving design sideways. And there came the question on design patterns. On which, he said that generally the patterns make the design complex. So, never try to insert a design into the application. Rather create a simple design first and try to see any design pattern evolving in it, insert a design pattern only if you can see it while developing it. Never try to force a design pattern into the code.</p>

<p style="text-align: justify;">There were questions about asserting void methods for which he proposed to create an internal method just for test purposes. When people objected on it by citing examples from oops principles. People also objected on the thought of changing code just for testing. To answer this question, he gave a magnificent example by citing a movie "A Few Good Men". He told that during an argument inside the court in the movie, someone quoted a rule. And then, Tom Cruise came with the rule book in his hand asking the other person to show him rules for each and everything that he did ( "You eat 3 times a day, can you show it in the rule book. Oh God, How can you eat 3 times a day when its not written in the rule book"). This was a great example and the best take away from the day. Dr. Venkat told to look around, experience thing, see people doing things and see what works and what does not instead of sticking to a rule book.</p>

<p style="text-align: justify;">The main take away of the Test First Development workshop was to always keep the mindset of developing what is needed now. Even though the audience was not a beginner one, most of them were old time TDD practitioners. Even though, the main bottleneck during workshop was found to be people thinking beyond what was needed.</p>

<p style="text-align: justify;">After the TDD workshop ended, the open discussion begun. The discussion started with mocking. People said that the Mocking code consumes a lot of code and thus decreases the readability. So, after the discussion finished, it was found that mock was not the problem, the problem was the design where Single Responsibility principle was not applied to its best.</p>

<p style="text-align: justify;">After that, the question came that the rule book says that every instance variable should be private with setters and getters. But it might not be needed and the instance variables can be simply made public. It can have to benefits, one, get rid of the extra code and make the classes shorter and more readable, two, the junit coverage increases as testing setters and getters is just foolish. He then opened up his laptop and gave a live demo of Groovy in which if a setter or getter is present, the code uses it, other wise, the variable is accessed directly. So, by citing this example, he told, that the new languages are thinking on the same lines of thoughts of developers.</p>

## Comments

**[Anurag](#5504 "2011-04-26 05:47:10"):** Keeping people on two year probation sounds like an unworkable idea. Let's not forget that people join organization for job security and money too. While most organizations have right to let non-performers go and they also excecise this option when appropriate, having people on long probation would only discourage good people from joining the company. Good blog. Very interesting to read.

**[ShriKant Vashishtha](#5510 "2011-04-27 07:12:19"):** Good gist of the session. It shows how valuable session was for those who missed it ;-). I will be explaining the key concepts in the XKE today. Nice post Paritosh!

**[Sourabh](#5506 "2011-04-26 10:31:41"):** nice post! Thanks Paritosh for sharing insights of the session in depth.

**[Karan](#5507 "2011-04-26 11:14:53"):** I am not an HR guy but would like to share my views on this. Usually when a firm needs to scale up in a short span of time, the drive is more customer centric. You are doing so in order to meet a customer's demand and so really need to be prompt on this or else the customer might drift towards your competitors. There might be cases where you need to choose between the options at hand and select a resource to be intimated to the customer. In such a scenario, first filter the resources based upon the bare minimum prerequisites that you definitely expect the resource to possess. Once you have the filtered set at hand, apply the Richard Branson principle.... 'Hire for attitude, train for skills'

