---
layout: post
header-img: img/default-blog-pic.jpg
author: aagrawal
description: 
post_id: 655
created: 2008/07/09 04:31:49
created_gmt: 2008/07/09 02:31:49
comment_status: open
---

# Lean Software Development Methodologies: Tom and Mary Poppendieck

<p><a href='http://xebee.xebia.in/wp-content/uploads/2008/07/poppendiecks-001.jpg'><img src="http://blog.xebia.com/wp-content/uploads/2008/07/poppendiecks-001-300x224.jpg" alt="" title="poppendiecks-001" width="300" height="224" class="alignnone size-medium wp-image-662" style="float:right;"/></a><b>July 06:</b> <a HREF="http://www.xebiaindia.com" target="_blank">Xebia India IT Architects</a> hosted a session by <a HREF="http://www.poppendieck.com/people.htm" target="_blank">The Poppendiecks</a>.
I was fortunate to be one of the participants as well as the organizer of their North India trip. Having practicing Lean, Scrum and XP for quite some time now, it was a nice experience to measure our understanding of these concepts against the "creator's definitions". Here I take you on a quick(??) trip to the session through my eyes...</p>
<!--more-->

<p>We started with a cup of steaming coffee on a beautiful rainy day at <a HREF="http://www.claridges-hotels.com/Delhi/index.asp" target="_blank">Hotel Claridges</a>. When the clock struck 10 AM, I heard Mary telling me "Lets get started" - I was impressed by her punctuality which she displayed at many more ocassions during the sessions - and proceeded to quickly introduce them and thank Xebia (the sponsors) and ASCI (they are behind gettin all these great guys to India to share their learnings with us). Mary took over from there on.</p>
<p>Mary narrated how the company she was working with faced stiff competetion during mid 80s. Innovating and "cutting the waste" seemed to be the only way to survival - as they say: Necessity is the mother of all inventions :)</p>
<p>During 1998-99, Mary happened to work on a government project and was introduced to the "Waterfall Model" - she was surprised and could hardly believe people were developing software that way! On completing the project, she decided to formalize her thinking about software development and her learnings from other industries, specially 3M- her former employer.</p>
<h2>Principles of Lean Software Development</h2>

<ul>
<li>Eliminate Waste</li>
<li>Embed Quality</li>
<li>Learn First</li>
<li>Deliver Fast</li>
<li>Improve Forever</li>
<li>Respect People</li>
<li>Think System</li>
</ul>

<p>Having defined the above principles, she then moved on to giving a few crisp and concise definitions:</p>
<p><a href='http://xebee.xebia.in/wp-content/uploads/2008/07/poppendiecks-003.jpg'><img src="http://blog.xebia.com/wp-content/uploads/2008/07/poppendiecks-003-300x224.jpg" alt="" title="poppendiecks-003" width="300" height="224" class="alignnone size-medium wp-image-667" style="float:left;"/></a><b>Waste</b> is…
Anything that depletes resources of time, effort,
space, or money without adding customer value.</p>
<p><b>Value</b> is…
Seen through the eyes of those who pay for, use,
and derive value from the systems we create.</p>
<p>A <b>System</b> is…
A complete and useful product, along with whatever
is needed to create and distribute the product.</p>
<p>This was followed by drawing up an intersting parallel between "The Seven Wastes of Manufacturing Industry" as defined by <a HREF="http://en.wikipedia.org/wiki/Muda_(Japanese_term)#The_seven_wastes" target="_blank">Taiichi Ohno</a> and the <b>Seven Wastes of Software Development</b>:
<ol>
<li>Extra Features</li>
<li>Partially Done Work</li>
<li>Relearning</li>
<li>Handoffs</li>
<li>Task Switching</li>
<li>Delays</li>
<li>Defects</li>
</ol>
<br/>
The baseline being: - "Put on the customer's glasses and then decide - if its not value, its waste"
This naturally led us to the mention of the famous <b>80-20 principle</b> that states that "<b>80% value is derived from 20% features</b>"
<a href='http://xebee.xebia.in/wp-content/uploads/2008/07/poppendiecks-004.jpg'><img src="http://blog.xebia.com/wp-content/uploads/2008/07/poppendiecks-004-300x224.jpg" alt="" title="poppendiecks-004" width="300" height="224" class="alignnone size-medium wp-image-666" style="float:right;" /></a>
We then pondered over the thought that this should mean fewer lines of code are better, fewer function points (only as much as required by the customer) as better than lots of FPs - but traditionally the companies measure productivity and performance in number of function points  - thus it needs a change of mindset rather than just the processes to really adapt Lean Way successfully.<br/>
<br/>
We all nodded that there's a need to think out-of-the-box.
Mary illustrated with a beautiful example from manufacturing industry:
One of the machines took 3 days to setup whenever there was a change of die required. So the <b>"common knowledge"</b> said
<ul>
<li>Die changed have a huge overhead</li>
<li>Don’t change dies very often</li>
</ul></p>
<p>While Taiichi Ohno's out-of-the-box thinking believed that:
<ul>
<li>Economics requires frequent die change</li>
<li>One Digit Exchange of Die</li>
</ul><br/></p>
<p>As it turned out, it really was possile to do a "One Digit Exchange of Die" - <u>The setup time came down from 3 weeks to a whooping 9 minutes!</u>
If we apply this theory to software dvelopment, it turns out that while <b>"common knowledge"</b> states:</p>
<ul>
<li>Releases have a huge overhead</li>
<li>Don’t release very often</li>
</ul>

<p>The Lean would suggest:
<ul>
<li>Economics requires many frequent releases</li>
<li>One Digit Releases - welcome to the concept of iterative and incremental - did we hear "sprints" :)</li>
</ul>
<br/>
Indeed, if we come to think of it, "One digit releases" do make a lot of sense - Learning from the "real user" of the system would be the best learning.
Which in turn means frequent customer feedback that comes from frequent "one digit" releases.</p>
<p>An interesting kind of waste was discussed - <b>"Wishful thinking"</b>. Its when we "hope" or "assume" things like availability, roadblocks, capabilities et al rather than "knowing" these parameters based on facts.
<br/>
<b>"Hand offs"</b>, another kind of waste occurs when we try and seperate:
<ul>
<li>Responsibility - What to do</li>
<li>Knowledge - How to do it</li>
<li>Action - Actually doing it</li>
<li>Feedback - Learning from results</li>
</ul>
<a href='http://xebee.xebia.in/wp-content/uploads/2008/07/crossfunctionalteams.jpg'><img src="http://blog.xebia.com/wp-content/uploads/2008/07/crossfunctionalteams-300x80.jpg" alt="" title="crossfunctionalteams" width="300" height="80" class="alignnone size-medium wp-image-665" /></a>
This is when we discussed the importance of <b>"Cross Functional Teams"</b>.</p>
<p>Mary quickly discussed "Task Switching" and "Delays" as other forms of wastes, illustrating with the example of The Empire State Building  - how it was completed in 1.5 years defing the common beliefs and all odds.</p>
<p>"Pulling" the work rather than the work getting "Pushed" seemed to be the common sense way of working.</p>
<p><b>"Defects"</b>, arguably the most important waste, can be avoided by <b>"Inspection"</b>.
Again, inspection itself might be a waste!</p>
<p><i><b>Inspection to FIND defects is a waste, pro-active inspection to PREVENT defects is essential.</b></i>
<br/>
<a href='http://xebee.xebia.in/wp-content/uploads/2008/07/categoriesoftesting.jpg'><img src="http://blog.xebia.com/wp-content/uploads/2008/07/categoriesoftesting-300x165.jpg" alt="" title="categoriesoftesting" width="300" height="165" class="alignnone size-medium wp-image-664"  style="float:left;"/></a>The importance of making the system error-proof at every stage came up - taking an example from hardware industry, its impossible to plug-in the monitor cable into your laptop the "wrong" way - if it fits in, it got to be right!</p>
<p>Testing at each step was emphasized and four categories of testing were introduced.</p>
<p>At this point Tom pitched in with an enticing statement - "If you don't engage in getting what the customer really wants, in getting the big picture, in interacting with the client, then you are not a developer, but a mere coder!"</p>
<p>Mary then introduced <b>"Value Stream Map"</b> as a diagnostic tool to find the biggest waste (opportunity for improvement) in a development process.</p>