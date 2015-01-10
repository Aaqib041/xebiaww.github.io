---
layout: post
header-img: img/default-blog-pic.jpg
author: vashishtha
description: 
post_id: 714
created: 2008/08/21 18:52:13
created_gmt: 2008/08/21 16:52:13
comment_status: open
---

# Agile Maintenance - One Team Multiple Projects

<p>You may land up in situations when a project is almost stable. For developers handling issues and enhancements for the project, the work available is not sufficient. So, when team is comfortable with the project and it's already stabilized, team can start handling another project at the same time. It's good for the people working in these projects from learning perspective. They are exposed to multiple technology stacks, problems and functionality. At the same time, it works well for an organization in general.
<!--more--></p>
<h3>Scrum for One Team Handling Multiple Projects</h3>

<p>When you talk about one team handling multiple projects, Scrum point of view becomes blur. There will be two product backlogs for two projects for example. But combined team cannot focus on two Spring backlogs at a time. It's difficult to define the relative priority of the issues of two projects individually and work on them. So there is a need to have a common set of issues to be in the sprint for the whole team. This helps in assessing team's velocity for picking up the tasks for next sprint. But it’s not possible to mix the whole product backlog into one, as there are stake holders who would be interested in their individual projects. It’s also hard to know the health of the particular project.</p>
<p>To solve this problem we need to use one 'Sprint backlog', which gets items from multiple 'product backlogs'. So there will be two artifacts that can be released at the end of each sprint. The decision of which backlog items should be taken into sprint backlog should be done based on the situation/priority.</p>
<p>For this purpose team can use JIRA and wiki together as issue tracking system. JIRA can be used to maintain the product backlogs of two projects. For each sprint create sprint backlogs in each project space in JIRA followed by a combined view of both sprint backlogs which is nothing but the sprint backlog for the team. For this one can create a filter in JIRA which gives issues from a particular sprint from both projects in XML format. On wiki team can use 'JIRA view' to display the sprint backlog items.</p>
<p><img src="http://xebee.xebia.in/wp-content/uploads/2008/08/combined-sprint-backlog.jpg" alt="Combined Spring Backlog" /></p>
<p>If you talk about assigning priorities of issues, the role of product owner comes into picture. Dealing with two product owners at the same time by the combined team seems to be a difficult task. To resolve this issue, there can be one proxy product-owner from the team who works with both customers and with team to prioritize the issues for the product-backlog and sprint-backlog.</p>
<h3>Focus on Knowledge Transfer</h3>

<p>To begin with, ProjectA team takes the knowledge transfer (KT) of ProjectB project and they starts working on ProjectB. KT of ProjectA is conducted later and finally you'll have one team working on two projects doing pair-programming. You may want to take a look on <a href="http://blog.xebia.com/2008/08/15/knowledge-transfer-in-agile-maintenance-projects/">a blog on knowledge transfer</a> in Agile maintenance for details.</p>
<p>P.S. Thanks to Haroon, Ganesh and Kris for their inputs on subject-matter</p>