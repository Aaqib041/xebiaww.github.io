---
layout: post
header-img: img/default-blog-pic.jpg
author: rnagpal
description: 
post_id: 7841
created: 2011/02/19 21:12:06
created_gmt: 2011/02/19 16:12:06
comment_status: open
---

# Should I Re-estimate?

<p>You have already estimated story points for different stories in a project but in the midst of the  sprint, you find that your estimates are amiss. This situation leads you in two minds about whether to re-estimate your user stories or not. There has already been much discussion surrounding this subject by various experts. But agile experts opinions are too many and at times very different also. There might be situations when none of their conceptions fit your project needs and you your self have to find a way out. In this blog I will talk about some of these view points and then I'll explain what adjustments we had to make to these processes to fit our project.</p>
<!--more-->

<p>First lets see <strong>when do we need to re-estimate</strong>.
James Shore says that you should re-estimate when your understanding of a user story changes, which further affects the effort involved in completing the story. According to him
<blockquote>You only need to re-estimate stories when your understanding of the story changes in a way that affects its difficulty or scope.</blockquote>
Chris Johnson shares some similar thoughts on this. He says
<blockquote>If, before playing a card, more information becomes available that directly reflects the implementation of a card, that card should be re-estimated.</blockquote>
Mike Cohn adds some crucial information to these thoughts. He says that we should re-estimate only if the relative size for a story has changed.
<blockquote>You should re-estimate only when your opinion of the relative size of one or more stories has changed.</blockquote>
<br/>
<strong>When should we not re-estimate</strong>
Chris Jhonson further suggests that we should not re-estimate when we are done with the user story. He explains,
<blockquote>I think there is a time and place for re-estimating a story card, but after the card has been played it not one of them.</blockquote>
Mike Cohn affirms his previous statement by explaining how velocity normalizes every thing, if effort for each story has increased rather than relative effort for a story.
<blockquote>Do not re-estimate solely because progress is not coming as rapidly you as you’d expected.
Let velocity, the great equalizer, take care of most estimation inaccuracies.</blockquote></p>
<p>Mike Cohn elaborates on all this by taking an example, and explaining various approaches that one may take for re-estimation. Below explained is an example similar to the one taken by him.
<br/>
<a rel="attachment wp-att-7849" href="http://xebee.xebia.in/2011/02/19/should-i-re-estimate/estimation/"><img class="aligncenter size-full wp-image-7849" title="estimation" src="http://xebee.xebia.in/wp-content/uploads/2011/02/estimation.jpg" alt="" width="679" height="483" /></a></p>
<p><br/>
Suppose that team's velocity is 11 and team members decide to pick user stories 1,2 and 3 in 1st sprint, and user stories 3 and 4 in 2nd  sprint. Now when they start  working on user story 2, they find that this story is more complex and its complexity is actually equal to 8 story points. Since story 4 is similar to story 2, they feel that actual estimate of this story also, should be 8. As teams capacity is 11 story points, only user story 1 and 2 could be completed in sprint 1. Teams velocity for sprint 1 turns out to be 6.</p>
<p>Mike Cohn further explains scenarios for dealing with these types of situations.</p>
<p><strong>Scenario 1: No Re-estimation</strong>
Team does not re-estimate any of the user stories and takes the velocity of this sprint as 6. The average velocity in these situations will decrease, reflecting that the actual estimates were more than the initial estimates. Even though average velocity compensates for the bigger size of user story 2 and 4, but team still faces issues while committing for the next sprint as their velocity is 6, but they can actually commit 11 story points for next sprint.</p>
<p><strong>Scenario 2: Re-Estimating the Finished Story</strong>
As user story 2 was actually equal to 8 story points, team re-estimates this story when it completes this story. This keeps the average velocity of 11. This helps team for the next sprint estimation, as team is capable of actually covering 11 story points, which is equal to the velocity of sprint after re-estimation. However, still team will not be able to complete 11 story points in the sprint in which user story 4 is taken.</p>
<p><strong>Scenario 3: Re-Estimating When Relative Size Changes</strong>
In this approach, we re-estimate all the user stories whose relative size we feel have changed. In this case we re-estimate story 2 and 4 and give the new estimates to it. This approach helps in keeping the average velocity same(11 in this case), and helps team in making their commitment for next sprints.</p>
<p>Mike Cohn further talks about re-estimating partially  completed stories. He says
<blockquote>I do not recommend giving partial credit for partially finished user stories. My preference is for a team to count the entire estimate toward their velocity (if they completely finished and the feature has been accepted by the product owner) or for them to count nothing toward their story otherwise.</blockquote>
I personally feel that re-estimating doesn't lead to much value addition, but there might be some situations where re-estimation is the only option you are left with.</p>
<p>One such situation was when our client wanted an accurate estimate rather than the Fibonacci series that is followed initially during the estimation.   After every sprint, client felt, that as we had already worked on ticket, we should be able to give a much accurate estimate of effort spent. This was because he wanted to calculate  velocity more accurately or wanted to compare effort of stories completed with the ones that were there in the backlog. This is primarily due to client's lack of knowledge in Agile estimation and they don't understand how average velocity takes care of all these scenarios.</p>
<p>It is very important to understand the core need for re-estimation. I generally go with the third scenario explained by Mike Cohn in the above example i.e. "Re-Estimating When Relative Size Changes" and I dont mind twisting and tweaking the recommendations given by Agile experts.</p>
<p>There is no wisdom in blindly following the ideas of Agile experts. Every project has its own needs and scope.</p>