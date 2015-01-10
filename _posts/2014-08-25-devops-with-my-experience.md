---
layout: post
header-img: img/default-blog-pic.jpg
author: atrivedi
description: 
post_id: 18853
created: 2014/08/25 14:46:17
created_gmt: 2014/08/25 09:46:17
comment_status: open
---

# Devops with my experience

<p>Here I am at the very first implementation assignment for the continuous delivery tool I am working with. I would say I am rather new to the buzzword which is “Devops” but the more I have learned and read about it it is making so much sense.</p>
<p>The term devops was coined from the devops days in belgium in 2009. In my opinion, devops is certainly very important for small businesses or product development startups who cannot afford to have long development cycles before getting customer feedback as it might throw them out of business if they are not working on the right things. The only way to get frequent customer feedback is to hand them a “minimum viable product” as early as possible. Devops and continuous delivery allows you to do that.</p>
<p>Be it waterfall or Agile methods of working we have always had this friction among the Development and Operations teams for the simple reason of lack of communication and understanding of each others problems. Just when I was talking to my clients at this assignment the might have mentioned at least 10 times in the last two weeks that these developers never understand what is expected to be delivered and same is the story on the other side.</p>
<p>Devops tries to answer some of these problems.</p>
<p>But having said that, it is not a tool or a service that you buy after spending a few thousand dollars. It is a mindset or a way of working that has to be understood by both the development and operations teams and they should be mentored while they are in this transition from traditional way of working to devops way of working.</p>
<p>When we had the same tools deployed for both Dev and production environments and forced them to learn the same things in a training it brought them together and they learnt that working together is not all that bad. In fact the tool implementation went much faster because the development team supported in writing some customisations and the operations team supported in resolving the server and infrastructure connectivity issues. So really it’s just matter of working together as a team to deliver better products faster.</p>
<p>Of course if you have common set of tools for Dev and operations it helps them speak the same language which aids  in the communication process.</p>
<p>One main reason of the operations people is that they always do these deployments at the most odd hours which results in lack of sleep and then the next morning if there is an issue and the development team is asking for some support the poor guy will definitely be pissed. So again the tools here come to rescue which make sure that we  automate all the deployments so that people do not have to work in these odd hours.</p>
<p>Of course if we cannot trust the automation and after automation also we are continually monitoring the process it is sheer waste of money and resources so the automation has to be robust and proper checks should be in place. All this is easier said than done but it is rational to do it.</p>
<p>For all people who are just embarking on this journey to understand the space here are my recommendations for resources to get you started.
<ol>
    <li><a href="http://www.infoq.com/articles/preparing-for-cd-in-the-enterprise">http://www.infoq.com/articles/preparing-for-cd-in-the-enterprise</a></li>
    <li><a href="http://continuousdelivery.com/2014/02/visualizations-of-continuous-delivery">http://continuousdelivery.com/2014/02/visualizations-of-continuous-delivery</a></li>
    <li>Book - Continuous Delivery by Jez Humble</li>
    <li><a href="https://www.youtube.com/watch?v=skLJuksCRTw">https://www.youtube.com/watch?v=skLJuksCRTw</a></li>
</ol>
Some examples of the typical problems that the project teams face.
<ol>
    <li>Long time to do the deployment.</li>
    <li>Effort wastage for teams waiting on deployments</li>
    <li>Configuration drift from environments</li>
    <li>Inconsistent environments</li>
    <li>Difficulty to track the deployment progress in case of a multi technology set up</li>
</ol>
All these challenges make it very difficult for the deployments to happen on time and cause lot of frustration for both development and operation teams because when something that has been delivered by development and it does not work in production, the blame game starts which results in loss of business hours.</p>
<p>Also, the tools that we need to solve these complex problems have to be flexible enough to deal with lot of inconsistencies that have crept up over all these years in the systems. There are all sorts of legacy applications which an organisation cannot get rid of overnight, simply to keep the lights on. The tools should be customisable to integrate with all these applications and of course as part of the initiative the project sponsor should feel the need to replace these applications.</p>
<p>The tools also force you on most cases to do things the “right way”. As per my experience with the operations teams it is quite inevitable to end up in a spaghetti of scripts while they are doing the deployments because they do not get timely support simply because the huge amount of co-ordination involved during deployments. They almost always write some quick fix scripts which keep lying there for future deployments as well. These scripts grow over time and become difficult to manage. With the deployment tools in place the deployment is considered as a package and has been tried as the same package on other environments before. So the probability of it working becomes more.</p>
<p>I’ll sign off with a quote from one of  the ops guys who happens to love the new way of doing things in a continuos delivery way. “Continuous delivery is the end of tribal knowledge as we know it”. Of course, he is referring to the way of deployment here.</p>
<p>In my next blog I’ll talk about how your existing deployment process can be transformed and the time to the deployment can be drastically reduced. Also, by using the devops processes how we can visualise the delivery pipeline in an entirely new way which allows us to cut the overheads as we go.</p>