---
layout: post
header-img: img/default-blog-pic.jpg
---

# Lean Software Development Methodologies: Tom and Mary Poppendieck

![](http://blog.xebia.com/wp-content/uploads/2008/07/poppendiecks-001-300x224.jpg)**July 06:** [Xebia India IT Architects](http://www.xebiaindia.com) hosted a session by [The Poppendiecks](http://www.poppendieck.com/people.htm). I was fortunate to be one of the participants as well as the organizer of their North India trip. Having practicing Lean, Scrum and XP for quite some time now, it was a nice experience to measure our understanding of these concepts against the "creator's definitions". Here I take you on a quick(??) trip to the session through my eyes...  We started with a cup of steaming coffee on a beautiful rainy day at [Hotel Claridges](http://www.claridges-hotels.com/Delhi/index.asp). When the clock struck 10 AM, I heard Mary telling me "Lets get started" - I was impressed by her punctuality which she displayed at many more ocassions during the sessions - and proceeded to quickly introduce them and thank Xebia (the sponsors) and ASCI (they are behind gettin all these great guys to India to share their learnings with us). Mary took over from there on. Mary narrated how the company she was working with faced stiff competetion during mid 80s. Innovating and "cutting the waste" seemed to be the only way to survival - as they say: Necessity is the mother of all inventions :) During 1998-99, Mary happened to work on a government project and was introduced to the "Waterfall Model" - she was surprised and could hardly believe people were developing software that way! On completing the project, she decided to formalize her thinking about software development and her learnings from other industries, specially 3M- her former employer. 

## Principles of Lean Software Development

  * Eliminate Waste
  * Embed Quality
  * Learn First
  * Deliver Fast
  * Improve Forever
  * Respect People
  * Think System
Having defined the above principles, she then moved on to giving a few crisp and concise definitions: ![](http://blog.xebia.com/wp-content/uploads/2008/07/poppendiecks-003-300x224.jpg)**Waste** is… Anything that depletes resources of time, effort, space, or money without adding customer value. **Value** is… Seen through the eyes of those who pay for, use, and derive value from the systems we create. A **System** is… A complete and useful product, along with whatever is needed to create and distribute the product. This was followed by drawing up an intersting parallel between "The Seven Wastes of Manufacturing Industry" as defined by [Taiichi Ohno](http://en.wikipedia.org/wiki/Muda_\(Japanese_term\)#The_seven_wastes) and the **Seven Wastes of Software Development**: 

  1. Extra Features
  2. Partially Done Work
  3. Relearning
  4. Handoffs
  5. Task Switching
  6. Delays
  7. Defects
  
The baseline being: - "Put on the customer's glasses and then decide - if its not value, its waste" This naturally led us to the mention of the famous **80-20 principle** that states that "**80% value is derived from 20% features**" ![](http://blog.xebia.com/wp-content/uploads/2008/07/poppendiecks-004-300x224.jpg) We then pondered over the thought that this should mean fewer lines of code are better, fewer function points (only as much as required by the customer) as better than lots of FPs - but traditionally the companies measure productivity and performance in number of function points - thus it needs a change of mindset rather than just the processes to really adapt Lean Way successfully.  
  
We all nodded that there's a need to think out-of-the-box. Mary illustrated with a beautiful example from manufacturing industry: One of the machines took 3 days to setup whenever there was a change of die required. So the **"common knowledge"** said 

  * Die changed have a huge overhead
  * Don’t change dies very often
While Taiichi Ohno's out-of-the-box thinking believed that: 
  * Economics requires frequent die change
  * One Digit Exchange of Die
  
As it turned out, it really was possile to do a "One Digit Exchange of Die" - _The setup time came down from 3 weeks to a whooping 9 minutes!_ If we apply this theory to software dvelopment, it turns out that while **"common knowledge"** states: 

  * Releases have a huge overhead
  * Don’t release very often
The Lean would suggest: 
  * Economics requires many frequent releases
  * One Digit Releases - welcome to the concept of iterative and incremental - did we hear "sprints" :)
  
Indeed, if we come to think of it, "One digit releases" do make a lot of sense - Learning from the "real user" of the system would be the best learning. Which in turn means frequent customer feedback that comes from frequent "one digit" releases. An interesting kind of waste was discussed - **"Wishful thinking"**. Its when we "hope" or "assume" things like availability, roadblocks, capabilities et al rather than "knowing" these parameters based on facts.   
**"Hand offs"**, another kind of waste occurs when we try and seperate: 

  * Responsibility - What to do
  * Knowledge - How to do it
  * Action - Actually doing it
  * Feedback - Learning from results
![](http://blog.xebia.com/wp-content/uploads/2008/07/crossfunctionalteams-300x80.jpg) This is when we discussed the importance of **"Cross Functional Teams"**. Mary quickly discussed "Task Switching" and "Delays" as other forms of wastes, illustrating with the example of The Empire State Building - how it was completed in 1.5 years defing the common beliefs and all odds. "Pulling" the work rather than the work getting "Pushed" seemed to be the common sense way of working. **"Defects"**, arguably the most important waste, can be avoided by **"Inspection"**. Again, inspection itself might be a waste! _**Inspection to FIND defects is a waste, pro-active inspection to PREVENT defects is essential.**_   
![](http://blog.xebia.com/wp-content/uploads/2008/07/categoriesoftesting-300x165.jpg)The importance of making the system error-proof at every stage came up - taking an example from hardware industry, its impossible to plug-in the monitor cable into your laptop the "wrong" way - if it fits in, it got to be right! Testing at each step was emphasized and four categories of testing were introduced. At this point Tom pitched in with an enticing statement - "If you don't engage in getting what the customer really wants, in getting the big picture, in interacting with the client, then you are not a developer, but a mere coder!" Mary then introduced **"Value Stream Map"** as a diagnostic tool to find the biggest waste (opportunity for improvement) in a development process.