---
layout: post
header-img: img/default-blog-pic.jpg
---

# JQuery Cycle Plugin: Sync multiple slideshows on same page

I was working on a website where I wanted to create a slideshow on the homepage, where I have to show  
a set of questions and answers coming from left-to-right and right-to-left of the screen, respectively. To add to the  
mystry, the question comes first and then the answer comes after a wait of 4 seconds. But both question  
and answer should move out of the screen simultaneously, and then the next question comes...and this  
continues till the cycle reaches the last slide.

So, here it goes...  


First thing first, get the JQuery Cycle Plugin from [here](http://jquery.malsup.com/cycle/) and include it in your source.

I created this website in Drupal so I will use some specific code of Drupal in some places.

**Gibberish in Drupal starts**

To add cycle plugin in your node/page template just insert the following code at the top:

[sourcecode language="php"]<br /> &lt;?php<br /> drupal_add_js(drupal_get_path('theme', 'my_theme').'/js/jquery.cycle.all.js')<br /> ?&gt;<br /> [/sourcecode]

The above line will add the cycle javascript file my first going into the themes folder and then in your theme directory,  
in my case it was 'my_theme'. Please note it is 'theme' and not 'themes' many people get confused and take this as  
the actual directory name but this is just Drupal syntax to get to the themes directory.

**Gibberish in Drupal ends**

I am not going to be modular here and would just do everthing in a single file. For all innovative minds please feel  
free to put code where ever you feel happy.

Here is the HTML page:  
[sourcecode language="html"]<br /> &lt;html&gt;<br /> &lt;head&gt;<br /> &lt;!-- let's leave this empty for now --&gt;<br /> &lt;script&gt;&lt;/script&gt;<br /> &lt;/head&gt;<br /> &lt;body&gt;<br /> &lt;div id='homepage'&gt;<br /> &lt;div id='questions'&gt;<br /> &lt;div id='ques-1'&gt;Is Scrum really going to work in one month project?&lt;/div&gt;<br /> &lt;div id='ques-2'&gt;Do you know what TDD is?&lt;/div&gt;<br /> &lt;div id='ques-3'&gt;When was the last time you used CI?&lt;/div&gt;<br /> &lt;div id='ques-4'&gt;Are you Agile?&lt;/div&gt;<br /> &lt;div id='ques-5'&gt;2 developers takes 4 months, therefor 4 developers will take 2 months. What do you say?&lt;/div&gt;<br /> &lt;/div&gt;<br /> &lt;div id='answers'&gt;<br /> &lt;div id='ans-1'&gt;Yes, I am 200% sure that it might work.&lt;/div&gt;<br /> &lt;div id='ans-2'&gt;Yes, I do. It means The Developer's Death.&lt;/div&gt;<br /> &lt;div id='ans-3'&gt;OMG! What do I need a Crime Investigator for?!&lt;/div&gt;<br /> &lt;div id='ans-4'&gt;Yes, I proudly say that I am A-jahil (ignorant).&lt;/div&gt;<br /> &lt;div id='ans-5'&gt;Thank God!! This is not happening in the real life, otherwise 9 people would be making a baby in a month.&lt;/div&gt;<br /> &lt;/div&gt;<br /> &lt;/div&gt;<br /> &lt;/body&gt;<br /> &lt;/html&gt;<br /> [/sourcecode]

I defined two separate DIVs to show questions and answers because cycle plugin works on a container element and then container's each child element will become a slide.

Add the following code in the <script> tag:

[sourcecode language="javascript"]<br /> jQuery(window).bind(&quot;load&quot;, function() {</p> <p> jQuery('#questions').cycle({<br /> fx : 'scrollRight', // scroll left-to-right<br /> speed : 'slow',<br /> timeoutFn : calculateTimeout, // described below<br /> nowrap : true, // stop slideshow after the last slide<br /> })<br /> jQuery('#answers').cycle({<br /> fx : 'scrollLeft', // scroll right-to-left<br /> speed : 'slow',<br /> timeoutFn : calculateTimeoutAnswers, // described below<br /> nowrap : true<br /> })<br /> });<br /> [/sourcecode]

and here are the timeout functions:

[sourcecode language="javascript"]<br /> var timeouts = [9, 9, 9, 9, 9];<br /> function calculateTimeout(currElement, nextElement, opts, isForward) {<br /> var index = opts.currSlide;<br /> return timeouts[index] * 1000;<br /> } </p> <p>var timeoutsAnswers = // explained below ;<br /> function calculateTimeoutAnswers(currElement, nextElement, opts, isForward) {<br /> var index = opts.currSlide;<br /> return timeoutsAnswers[index] * 1000;<br /> }<br /> [/sourcecode]

I created these to set a delay for each slide and left enough delay, or 4 sec, to display the questions' corresponding answer.

But the tricky part is that we should slide out both question and answer at the same time. I tried to set different intervals  
for each answer slide but all in vain, spent a few hous googling and on calcuator/paper trying to figure out a formula to  
do the trick. But then it hit-me!! 

Why not add some dummy child elements to 'answers' container or what I called them 'delay' elements!!

Hence the HTML for anwers container became:  
[sourcecode language="html"]<br /> &lt;div id='answers'&gt;<br /> &lt;div class='delay'&gt;&lt;/div&gt;<br /> &lt;div id='ans-1'&gt;Yes, I am 200% sure that it might work.&lt;/div&gt;<br /> &lt;div class='delay'&gt;&lt;/div&gt;<br /> &lt;div id='ans-2'&gt;Yes, I do. It means The Developer's Death.&lt;/div&gt;<br /> &lt;div class='delay'&gt;&lt;/div&gt;<br /> &lt;div id='ans-3'&gt;OMG! What do I need a Crime Investigator for?!&lt;/div&gt;<br /> &lt;div class='delay'&gt;&lt;/div&gt;<br /> &lt;div id='ans-4'&gt;Yes, I proudly say that I am A-jahil (ignorant).&lt;/div&gt;<br /> &lt;div class='delay'&gt;&lt;/div&gt;<br /> &lt;div id='ans-5'&gt;Thank God!! This is not happening in the real life, otherwise 9 people would be making a baby in a month.&lt;/div&gt;<br /> &lt;/div&gt;<br /> [/sourcecode]

and the timeoutAnswers array became:

[sourcecode language="javascript"]<br /> // do some hit-n-trail because the slide out time for both questions and answers<br /> // will not be exactly same even though we set it to 4+5=9 sec<br /> // you may need to change 5 to something like 4.6 or less.<br /> var timeoutsAnswers = [4, 5, 4, 5, 4, 5, 4, 5, 4, 5]<br /> [/sourcecode]

Here is the full HTML:

[sourcecode language="html"]<br /> &lt;html&gt;<br /> &lt;head&gt;<br /> &lt;!-- add jquery and cycle plugins --&gt;<br /> &lt;script type='text/javascript'&gt;<br /> jQuery(window).bind(&quot;load&quot;, function() {</p> <p> jQuery('#questions').cycle({<br /> fx : 'scrollRight', // scroll left-to-right<br /> speed : 'slow',<br /> timeoutFn : calculateTimeout, // described below<br /> nowrap : true, // stop slideshow after the last slide<br /> })<br /> jQuery('#answers').cycle({<br /> fx : 'scrollLeft', // scroll right-to-left<br /> speed : 'slow',<br /> timeoutFn : calculateTimeoutAnswers, // described below<br /> nowrap : true<br /> })<br /> });</p> <p> var timeouts = [9, 9, 9, 9, 9];<br /> function calculateTimeout(currElement, nextElement, opts, isForward) {<br /> var index = opts.currSlide;<br /> return timeouts[index