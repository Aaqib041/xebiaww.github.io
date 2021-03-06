---
layout: post
header-img: img/default-blog-pic.jpg
author: shekhargulati
description: 
post_id: 11145
created: 2012/01/26 18:23:42
created_gmt: 2012/01/26 13:23:42
comment_status: open
---

# OpenShift Express -- The Future Of RedHat PaaS

Last couple of days I was at [JUDCon India][1] conference held in Bangalore where I gave a session on [OpenShift][2]. The JUDCon India was the biggest JUDCon conference ever with more than 800 attendees. Sessions were mainly on topics -- Infinispan, JBoss AS7 , OpenShift, CDI, Rules Engine, and other JBoss tools. I was mainly interested in hearing the thoughts of RedHat guys on OpenShift -- its future, comparison with Cloud Foundry, and when it will be open sourced.

The biggest announcement made at the conference was the unification of OpenShift Express and OpenShift Flex Platforms. At first I was disappointed to hear this as I wanted to showcase Flex in my presentation. It was not good to hear one day before the presentation that Flex will not be developed further and will be merged in express.  Someone asked why OpenShift Flex is being abandoned as it has some very good features like AutoScaling, Log Management, MongoDB Replica Sets, Tomcat support,  Cost Calculator and many more. Mark Atwood replied  to this by saying "It was getting difficult to manage two products and two teams which overlap a lot. ". [Jimmy Guerrero][3] has [commented][4] and shed more light on it. 

> Our motivation for unifying the Flex and Express platforms isn’t because it was difficult to manage two teams and two products, but instead we are being driven by the desire to bring the best features of both platforms into a single user experience called, OpenShift. For us it isn’t the end of Flex, but, rather, the beginning of a single converged solution that is easy to use and has the capabilities that you want.
> 
> This way, from the same IDE, GUI or client tools, a user can continue to get the free access to multiple instances and the broad middleware and framework support of Express, coupled with the rich functionality of Flex’s auto-scaling, performance monitoring and log management. And from, your post, it looks like you agree this is best way to go.
> 
>  

In my opinion also this is a right step forward for OpenShift. OpenShift Express has gained more popularity as compared to OpenShift Flex because it is free and easy to use . Express is lightweight, developer oriented, easy to get started with and better suited for creating new applications.  Another reason why I think it is good to have one platform is to avoid porting the applications from one flavor to another. I created a very simple Spring MongoDB application and to make the application run on both platforms I had to change database properties. Also as both the platforms support different cartridges (example RedHat messaging is supported in Flex but not in Express) it would be difficult to port applications across platforms.

Another big news about OpenShift is that it will be open source soon and you will be able to create a private PaaS on your own infrastructure behind company firewalls. I think it is very important for OpenShift if it wants to have the same momentum as Cloud Foundry. This also means that community can add support for cartridges which does not exist in OpenShift.

Overall I think OpenShift is heading in the right direction and it is a wise decision to spend time on it.

   [1]: http://www.jboss.org/events/JUDCon/2012/india
   [2]: http://www.slideshare.net/shekhargulati/a-happy-cloud-friendly-java-developer-with-openshift.
   [3]: https://www.redhat.com/openshift/community/author/jimmy-guerrero
   [4]: http://xebee.xebia.in/2012/01/26/openshift-express-the-future-of-redhat-paas/#comment-7140

## Comments

**[Shekhar Gulati](#7142 "2012-01-26 22:47:04"):** Thanks Jimmy for your comment. I will update the post.

**[Jimmy Guerrero](#7140 "2012-01-26 20:52:10"):** Shekhar, great post as usual! Just wanted to post a comment to clarify one of Mark's statements. Our motivation for unifying the Flex and Express platforms isn't because it was difficult to manage two teams and two products, but instead we are being driven by the desire to bring the best features of both platforms into a single user experience called, OpenShift. For us it isn't the end of Flex, but, rather, the beginning of a single converged solution that is easy to use and has the capabilities that you want. This way, from the same IDE, GUI or client tools, a user can continue to get the free access to multiple instances and the broad middleware and framework support of Express, coupled with the rich functionality of Flex's auto-scaling, performance monitoring and log management. And from, your post, it looks like you agree this is best way to go. Look for a blog later today from product management to speak in a little more detail about the exciting plans we have in store in the coming months. Finally, congrats on the well received talk at JUDCon. Hope to see you at Summit this year!

