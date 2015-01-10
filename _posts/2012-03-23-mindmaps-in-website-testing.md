---
layout: post
header-img: img/default-blog-pic.jpg
author: srentala
description: 
post_id: 12814
created: 2012/03/23 13:15:37
created_gmt: 2012/03/23 08:15:37
comment_status: open
---

# MindMaps in Website Testing

<p>Writing Test cases or Scenarios is one of the significant part during testing. One must ensure full coverage of scenarios efficiently within time.MindMaps provides us with the solution. I am currently handling the testing of one of our client's website, where i had to create Test scenarios for different features in every sprint.
Initially i was documenting the scenarios in the test management tool, but i started using Mindmaps to generate my Test Scenarios. It was a real help, both in time efficiency and completeness of coverage. So, let me give a brief introduction to MindMaps. <!--more--></p>
<p><strong>What is MindMap ?</strong></p>
<p>Mindmaps is a way to visualize your thoughts ,ideas and creativity in order to effectively plan your testing process.It uses lots of colors and keywords to make it more presentable catchy and impressive.
There are certain steps to use Mindmaps, which are :<a rel="attachment wp-att-12819" href="http://xebee.xebia.in/2012/03/23/mindmaps-in-website-testing/1-4/"><img height="187" width="300" src="http://xebee.xebia.in/wp-content/uploads/2012/03/11-300x187.jpg" title="Figure 1" class="alignright size-medium wp-image-12819" /></a>
<ol>
    <li> Place your central idea or subject at the center as a an image.</li>
    <li> Use branching of the Central Idea.</li>
    <li>Connect each branch with its sub branches.</li>
    <li> Use various colors throughout</li>
    <li> Use keywords and key phrases</li>
    <li> Use as many images as possible so that one can remember.</li>
</ol>
<strong>An Example to Illustrate Mindmaps:</strong></p>
<p>The following image shows how to create Cases with MindMaps(Figure 1). It shows "Human" as the central idea and its subsequent branches: Personal, Work, Business , Career and Education.Each of its branches are further divided into sub branches. It has used lots of color and images to give a catchy and expressive look. Now starting with the central idea we can go to each subsequent branches till the branch ends and that makes a single test case. This is how Test Scenarios can be generated with ease.</p>
<p><strong>Tools that can be used to create MindMaps:</strong></p>
<p>There are various tools to generate Test Scenarios using Mindmaps like : Freeminding Software, MindManager. Now that we have a fair idea about Mindmaps, let us see how we can ensure using the same in Website Testing.</p>
<p><strong>Mindmaps used in my current Projects:</strong></p>
<p>MindMaps for Website Testing: I am currently testing our client's Website which provides a meal plan for the day to a user. I have generated test cases using the tool Freeminds. Let us see what this site does. It gives the user a meal plan for the day. Various meals are provided to different meal slot : Breakfast, Snack, Lunch and Dinner. There are various other features associated with each meal slot.So, lets create a Mind Map tree for the following Feature. Create a Central idea - MEAL PLAN. Create sub branches of it : Breakfast, Lunch, Dinner and Snack. Each branch will have its own sub branches: Various food items (1 or more than 1). Now each food item will have its subbranches: Delete, Change meal, Add Calories, I ate this ,Substitute, Making a food Favorite, Facebook/Twitter like, Total Calories. Now using this data we can create a Mindmap which is shown as below using Freeminds (Figure 2).</p>
<p>Using the above Mindmaps the test cases which we can create out of it are as follows:<a rel="attachment wp-att-12818" href="http://xebee.xebia.in/2012/03/23/mindmaps-in-website-testing/2-5/"><img height="280" width="300" src="http://xebee.xebia.in/wp-content/uploads/2012/03/22-300x280.jpg" title="2" class="alignright size-medium wp-image-12818" /></a>
<ol>
    <li> Breakfast-&gt; Food-&gt; Delete.</li>
    <li> Breakfast-&gt; Food-&gt; Change Meal-&gt; Recent.</li>
    <li> Breakfast-&gt; Food-&gt; Change Meal-&gt; Custom.</li>
    <li> Breakfast-&gt; Food-&gt; Change Meal-&gt; Search.</li>
    <li> Breakfast-&gt; Food-&gt; Change Meal-&gt; Favorite.</li>
    <li> Breakfast-&gt; Food-&gt; Add Calories.</li>
    <li> Breakfast-&gt; Food-&gt; I ate this.</li>
    <li> Breakfast-&gt; Food-&gt; Substitute.</li>
    <li> Breakfast-&gt; Food-&gt; Favorite.</li>
    <li> Breakfast-&gt; Food-&gt; Unfavorite.</li>
    <li> Breakfast-&gt; Food-&gt; Facebook Like.</li>
    <li>Breakfast-&gt; Food-&gt; Twitter Like.</li>
    <li>Breakfast-&gt; Food-&gt; Calculate Total Calories.</li>
</ol>
We have created test cases for Breakfast. Similarly we can use the same data for other branches: Lunch , Snack and Dinner. In all we have 52 test cases with us and we have covered each area of this feature. So the error or probability of missing test cases is minimized to a great extent.</p>
<p><strong>Usefulness of Mindmaps:</strong></p>
<p>Let me share my personal advantages which i faced while using Mindmaps.
1) Very easy to implement.
2) Less time taken to construct Test scenarios.
3) Probability of missing a test case is minimized to a great extent.
4) Under critical time constraints it is the best way to create test scenarios.</p>
<p>Having said that, i would personally like to recommend every tester to at least try using Mindmaps during their test case generation. It will be something which would be used for a very long time.</p>

## Comments

**[Thushara](#8019 "2012-03-23 21:43:16"):** Really nice. So much important information.

