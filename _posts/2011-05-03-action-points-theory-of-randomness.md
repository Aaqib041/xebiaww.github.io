---
layout: post
header-img: img/default-blog-pic.jpg
author: sohil
description: 
post_id: 8779
created: 2011/05/03 00:07:36
created_gmt: 2011/05/02 19:07:36
comment_status: open
---

# Retrospective Action Points : Theory of Randomness

Problem : We had taken ~ 8 action point in the previous sprint and were about to start the retrospective. So while iterating over all the action points we found that we never achieved any action points in the sprint. Now this cycle carried on till next 4-5 sprints, we kept picking up ~8 action points but only managed to achieve one or two. To give you a break up of how we picked action points, We were a team of 6, there would be couple of action points for all, and everyone would pick up one or two action points based in the retrospective.

One of the reason why we believed that we could not achieve action points was not allocating time in the sprint for the action points. Since we never explicitly allocated the time for the action points in the sprint we never were able to pick it up on first place. This resulted in stacking up the action points that need to be done. Certainly this can be a demotivator as in every retrospective you end up complaining about the same thing. So we agreed on keeping some time spare for the action points.

However the above approach also didn't work well. As most of the time stories, bugs, took over the priority. Also some points needed to be kept in mind throughout sprint execution based on distributed pairing,etc. 

Like this we tried various approaches like discussing the action points one of the days after the standup. Well this approach did work partially. However discussing over 8-10 action points was too time consuming plus was difficult to concentrate on all of them at once.

So here is the actual problem we needed someone to remind us about the action points daily, but we didn't want to spend a lot of time discussing the action points. We wanted to start acting on them. Hence I developed the a application based on randomness.

Goal : Remind us about one action points everyday.

So here is what application does , Every time you start your machine , the application automatically starts up and will **randomly **show you one action point.

Following two points are very essential for the application

  * **Randomness **: The reason I choose random is because if everyday when I start my machine I will be reminded about one of the points to be done. Effectively all the 6 team mates will have one action item to think about. Also at a time you can only work on one action point. If you are shown list of 8 action points chances are you are going to ignore the list.
  * **Start of Machine** : This is the only time when my geeky brain still hasn't taken the plunge towards coding. I have sometime to think and act on.
FlashCard is a simple application that will startup when the machine starts up. If you are connected to the internet it will read out the action point for you. Its a air application I have been using it on Windows. You can download it from  <https://github.com/xebia/FlashCard>][1] . Apart from the action points I have been adding a lot of todo also for me. Click on the [Flash_Card][2] to see how to use

The idea of randomness really works for me as at any point in time **you have to think of only one particular task to do**.

   [1]: http://www.esnips.com/doc/3627e875-7cf3-4428-9a3b-ad3874ae2117/FlashCard
   [2]: http://xebee.xebia.in/wp-content/uploads/2011/05/Flash_Card.swf