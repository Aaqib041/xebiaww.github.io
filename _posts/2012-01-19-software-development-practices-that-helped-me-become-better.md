---
layout: post
header-img: img/default-blog-pic.jpg
author: guneetsahai
description: 
post_id: 10927
created: 2012/01/19 01:07:27
created_gmt: 2012/01/18 20:07:27
comment_status: open
---

# Software Development Practices that helped me become better

[Xebia India][1] takes great pride in paying high amount of attention to writing QUALITY code. “Doing it Right” is almost a religion here. For us the buck does not stop at showing a demo of working software to the client, but also ensuring the underlying code is written in the best possible manner using best known software development practices.

To achieve this, it needs a different kind of a culture in the organization that encourages people to go after Quality. This is a lot more difficult than it sounds. Most developers at Xebia will agree that we have a slightly different approach to work (Some call it our culture). A lot of developers joining Xebia India experience these differences first hand & in due course of time, unknowingly they end up picking up these practices themselves. In this article, I list below some changes & practices that I have picked up in the last 2 odd years of my relatively short stay here that helped me be a better programmer.

**Time Vs Quality - Quality should win**

Here's what I heard from a programmer at a Beer Session once : “Every time I pickup a programming task, a clock starts ticking in my head. It stops (or resets) only after I declare completion of this task”. IMHO this clock is one of the biggest enemy of Quality. The trouble is that if this clock continues ticking for more time then you planned (working on the task), then it gives a very uncomfortable feeling to the programmer; which leads to pressure to complete the task quickly. This usually happens by cutting corners therefore compromising QUALITY.

For me, I had this clock very loud in my head. I always wanted to declare (task) completion faster than anybody else can imagine (_I guess this is a result of my competitive schooling_). Clearly for me, speed was more important than quality earlier. I have tried to change that in recent months. I wouldn't say I have reversed the situation, but now have a different meaning to the word completion – For me now DONE means DONE. If there is an obvious improvement that can be made, then the task is NOT DONE..

 

### Test the hell out of your code

When I was younger, I used to be more confident about new code that I write than I am now. I think the confidence came simply because I hadn't seen enough failures of my code. With Time (and with discovered bugs), I became more careful & started to write more tests around my code – giving me a better sense of security. While I do see people agree with the above, but a lot of developers treat Tests as second class citizens of development practices. Not enough thought on what exactly needs to be tested is given – without that it is highly likely that tests will not be fully beneficial.

What works for me: a) Every time, I discover a problem or a bug in my code – I almost always go back and think why didn't my tests detect this problem earlier. Improve the tests so that the leak does not go undetected again. b) I spend some time thinking about what exactly needs to tested in a class/module. This sounds easier than it is, but with time & practice I think my tests are getting better at finding problems.

 

### Bug Fixing – Fix the root cause & avoid applying a patch

I have been in many situations when the first apparent fix turns out to be a patch on the surface of a bigger problem. The root cause can go undetected easily if nobody questions the fix in totality.

What works for me: a) Questions like “If this was the problem, then why didn't it appear in situation X?” go a long way in improving the probability of discovering the root cause. b) Upon finding the possible cause, talking to the author of the code may shed more light about the nature of the problem and sometimes gives a different perspective of the problem. c) Good testers already know this – I try & play more with the problem (reproduce it under different situations); not only the understanding of the problem improves, but sometimes the solution itself becomes really apparent.

 

### Mistakes are not bad 

Mistakes are not bad; but hiding them clearly is. On being pointed out for a possible mistake, my head used to go in the defensive mode and it used to try to come up with either problems in the proposed alternate solution or defend the problem in my code. After having grown some white hair, I have now realized there is no need to do that. It becomes a lot easier if I accept that I goofed up and improve the situation from where it is. That way there is more sense of achievement for everybody involved & the overall environment is geared towards problem solving rather than blaming.

What works for me: a) While there is no easy way to avoid mistakes, I think more collaboration within a team can help. If I'm working on an important piece, I try and get opinion of my team mates on my approach before I start coding. b) If somebody presents an alternate way of solving a problem, I weigh pros & cons of both approaches honestly – without being biased about the code that has already been written or about the author of the code. c) Avoid being emotionally attached to **your** code – there could be better ways to do what your code does.

 

Some other practices that I have picked up & I think are helping me but do not need much explanation are listed below.

### Invest Time in upgrading yourself

There is always something new to learn - A programming language, a new framework, a new tool etc. Have some way of adding items to your “To learn/Study” list. For me knowledge sharing sessions at Xebia (XKE) do just that.

### 

### Seek company of better/Senior Programmers

Things like Pair Programming with somebody senior is a very enriching experience. It provides an opportunity to learn & grow. I try & grab opportunities like these whenever they come my way.

Keep company of young software developers

##### Young software developers challenge practices & don't easily accept them unless proven. This forces me to think about practices that I have been following myself & re-evaluate them again. They can keep me on my toes all the time - That drives the need to improve myself. Besides having young people in the team can reflect positively in the burn-down chart :)

Hide Nothing & Collaborate more

Be Transparent & Hide nothing. This sounds easier than it actually is. However if you are really concerned about quality of your work, then quick feedback (from tests & peer coders) is the only way to find problems early. Seeking peer reviews for an important piece of code is a practice that has helped me a lot. I'm usually asking a lot of questions if there is some ambiguity. If there's anything that I don't know – saying that upfront (even to the client) and asking for help is much better than faking knowledge.

   [1]: http://xebiaindia.com/

## Comments

**[Paritosh Ranjan](#7019 "2012-01-19 11:37:12"):** Guneet, great thoughts. There is so much to learn from your blog. One question - Will it be worth following the same programming guidelines while doing a POC?

**[Ram Sharma](#7016 "2012-01-19 10:16:36"):** Nice blog. Really liked it because you mentioned a couple of things I forgot to do myself. keep posting :)

**[Ram Sharma](#7017 "2012-01-19 10:19:31"):** Infact I would like to question 1 thing on Time Vs Quality. Can we really spend much time on quality and overlook time in Indian IT companies perspective ?

**[Shailendra Pandey](#7075 "2012-01-22 23:45:29"):** Glad to know that at least some IT companies in India care about quality. I believe its the architects responsibility to inculcate a culture of quality. The Architect has to push back deadlines from client and make them understand that deadlines mean 'Dead lines of Code'. I have inherited a project where i had to clean up the mess.

**[Vipin Gupta](#7032 "2012-01-20 09:04:45"):** Every point is thought provoking and I see myself in alignment with each, but Time Vs. Quality is bit probing to me as well. With harsh committed deadlines from delivery perspective and then there is always a better way to write code, how to make the marriage between schedule and quality successful?

**[Guneet Sahai](#7057 "2012-01-21 12:53:11"):** @Vipin & @Ram: Thanks for going through my blog. It is indeed tempting to ignore Quality and finish on allotted time & it is equally tempting to continue to improve and ignore the deadline. In my opinion, finding that balance is not an easy task & comes with some experience in doing so. My rule of thumb has been if I'm feeling uncomfortable with the code (because of a problem that I know is there) then I don't declare victory and try and get rid of the problem. If I know there is an improvement that I can make but I'm not feeling uncomfortable by it, then Back Log is a good place for that improvement.

**[Tarun Sapra](#7050 "2012-01-20 19:56:23"):** Very insightful blog, for me the best part was "Avoid being emotionally attached to your code". I guess if we are emotionally attached to our code, then going into defensive mode and playing blame game might kick-in instead of actually forking out the most rational solution.

**[Pankaj Bhatt](#7090 "2012-01-23 23:26:37"):** Thanks Guneet Sir, The write up is surely an inspiring one which takes me back to the aura when our team does not conform their codes with the unit test cases. The direction given by you is really helpful and now I am much more aware about the quality and the bugs. the example of the Clock ticking hits the bulls eye in the terms of the way the development is done today. Thanks once again, for great blog, waiting to see more. I hope you remember me.

**[Anonymous](#7444 "2012-02-07 18:49:36"):** I just somehow landed up on this blog and I must say it surely is a good read. With due respect your experience and knowledge, I have a different viewpoint on Time Vs Quality concept. IMHO Time and Quality always have an equal weightage. None is superior than the other. If we keep this in mind always, It would only lead us to become what I call as an "efficient/smart" developer. Also, this is where estimations are so important where generally developers like myself lack. If "Quality always win" is our motto then "hum jeet kar bhi aksar haar jayengey" especially when it comes to POC. :P

**[Guneet Sahai](#7462 "2012-02-08 09:55:36"):** I agree with your viewpoint & share the same feeling.

