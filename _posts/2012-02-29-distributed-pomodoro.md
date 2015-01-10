---
layout: post
header-img: img/default-blog-pic.jpg
---

# Distributed Pomodoro

Pomodoro is a simple technique for time management. Here is a link if you would like to know more about Pomodoro.[http://www.pomodorotechnique.com/](http://www.pomodorotechnique.com/) . Here at Xebia we have a Open work area.This blog talk about one of the problems that we faced while working in Open work area and how we solved it.Our team was following Scrum with a two weeks sprints.This was an ongoing project for last 6 months which had a team of 2.Project was recently just scaled up to 8 team members. Which means there were 6 team members who had joined new. 

Following are the challenges that we (new guys on team) faced

  * No domain knowledge
  * Less idea about the latest frameworks used in project.
  * No idea about the architecture of the current project.

With the above challenges one of the common point of Retrospective was we were disturbing

the 2 guys who have been in the project for the longest time. This is one of the disadvantages of this open work enviornment. You can be disturbed every easily. "Get the image for open work area". If someone has a questions he can just interrupt you when you are actually in the zone. This further affected the productivity of the 2 guys.

 

To solve this problem Distributed Pomodoro was the solution that we came up with. Like pomodoro is a time management technique for one to be more productive and focused on work.The idea was simple We all have our clocks synched. We formed a protocol that we all work for 40 minutes and take a break for 10 minutes.Use this 10 minutes to discuss questions if we have , take a break , etc. Basically you can make all the noise you want to in a room in this 10 min.

This all looked good but there were a lot of challenges in executing this

  * Someone has to keep time.
  * Not everyone starts the day together.

 

To solve the above challenge I have made a simple application based on P2P Solution. The application creates a virtual group

on LAN. Everyone can join the group in the start of the day.Anyone can start the clock. Once the clock is started it will start a clock on everyone's laptop. Once 40 mins is over there will be a loud alarm.If someone joins in the middle he will have the current clock timings synchronized on his laptop.

 

This is a simple application that works on P2P based network.Having P2P message for communicating and synchronizing everyone connected.Also because of this application not only one person but the whole team follows Pomodoro.With some practicing this will help increase the teams productivity to good extend.Here is the [Source Code](http://code.google.com/p/team-pomodoro/source/checkout)

of the application. Enjoy!!

## Comments

**[Sohil](#8018 "2012-03-23 20:43:37"):** Thank you Shrikant :)

**[Rajesh](#7833 "2012-03-02 12:08:29"):** The 10 minutes break would not be sufficient for the new guys to deliver the output efficiently. It would obviously increase the productivity of the older guys, but decrease the productivity of the newer guys who need more time at least in their beginning sprints. Again, the hard-fast rule of 10 minute discussion may not be work for design level discussions wherein relevant people may be needing a more of brainstorming session. If a person don't wants to get disturbed, can put a headset ON (an indication of "DON'T DISTURB) while doing some of the critical tasks. Probably the technique may not work all the times through the sprint cycles. This technique curbs the team from having effective communication required.

**[Sohil](#7834 "2012-03-02 12:51:08"):** Hi Rajesh, To increase the productivity of newer guys pair programing, etc Xtreme practices should be used. There is no hard and fast decision on when to start the clock. If you think this required a brain storming session timebox the session and have one.Putting the headset on can be a good solution but not while people are pair programing. We had these challenges hence we came up with this idea.

**[ShriKant Vashishtha](#7878 "2012-03-08 18:52:10"):** Liked the concept and dare to fix the odds.

