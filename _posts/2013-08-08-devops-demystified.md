---
layout: post
header-img: img/default-blog-pic.jpg
author: anirudh.xebia
description: 
post_id: 16872
created: 2013/08/08 14:13:27
created_gmt: 2013/08/08 09:13:27
comment_status: open
---

# DevOps Demystified

The new buzz world in the world of Agile is "Dev Ops". So what exactly is devOps and Why do we need it? When development got married to deployment (sys-admin/operations) ; what is born is a new advanced species which is known to us today as "Dev Ops". So we get developers who are concerned about operations, deployments, builds. And we have the operational guys writing code to deploy and get infrastructure up and running!! Isn't it amazing?

But, Seriously? Thats it? Whats more? How do they do it? And more importantly; Why? Lets take a dive into it:  **Mindset** \- DevOps by concept means collaboration between Developers and IT operations so that the software delivery process as a whole is ownership of entire team (Dev+QA+Ops) and there is no blame game between different teams in case of some failure.

**Collaboration** \- In IT operations there could be multiple tasks maintaining servers, deployment, security, DBA, So all the developers will have visibility of these tasks and Ops people will be involved with developers while they develop, plan,estimate and gather requirement. Read more about tasks segregation [here][1].

**Why?** \- As Martin Fowler describes the 'last mile' problem of deployments, where deployments are difficult because of fragile and ever changing environments with very less or almost no automation. (http://martinfowler.com/books/continuousDelivery.html)

**How? **\- Deployment pipelines using Jenkins, provisioning tools like Chef and Puppet,custom deployment solutions like [DeployIt][2], Go etc.. (CI+ Automation tests + Continuous Delivery) Present day devOps are basically helping companies reduce IT costs by automating deployments and making this process smooth.

**Continuous Delivery** : "_Continuous Delivery is a software development discipline where you build software in such a way that the software can be released to production at any time_" - Martin Fowler. This means that it is a mechanism where we can deploy our change in one step and as many times as possible with confidence and without any hassles.

So how do we achieve it and what are the parts involved in it :

**a.) Continuous Integration**. - this is the very first step towards achieving continuous delivery. Whenever a new code is checked in it should be compiled and test cases should pass.

**b.) Automated acceptance test cases** \- A working and unit test code by itself can not be straight away deployed to QA /UAT /prod. The new code/stories would need to be verified and tested functionally so that they meet the acceptance criteria. So we would need automated acceptance test cases which would make sure that the acceptance criteria is met and the code is good to be deployed. There are various tools in the market for this like cucumber, Fitnesse, Selenium etc.

**c.) Automated build and release. **\- It is very important that the release and deployment process is automated. There should be mechanisms to automatically identify the delta and prepare the artifact and deploy in the desired environment. Jenkins is one powerful tool which can be used for the same

**d.) Automated Provisioning and configuration management** (**Infrastructure as code)**.- The final step in the process of continuous delivery is provisioning of environment. The environment could be on cloud (EC2, rakspace..etc.) on VM or on real hardware infrastructure. Configuration management can be very tricky and tedious process. Writing scripts has been the de-facto way for all the configuration related automation so far, but we can do better. The problem with scripts are : \- difficult to maintain and are fragile. \- difficult to read and do changes. \- non-idempotent

So we need a better mechanism which should be robust, maintainable and idempotent. Hence we introduce the concept of infrastructure as Code.

**Infrastructure as Code** : This is basically creating a blueprint of your infrastructure that enables you to build or rebuild, automatically in minutes or hours. This is flexible, configurable, loosely coupled, maintainable and idempotent code which is stored and versioned in a repository. Whenever there is an update we can just run it without any risk as it is idempotent, so the system converges to stability as it progresses. Moreover we don’t have to reinvent the wheel by writing scripts to download tars and execute rpms on an environment, everything is available ready made in community, we just have to configure it for our case, and as it is loosely coupled the changes are easy. In case the system crashes or a new node needs to be added to the current infrastructure set-up it is just a matter of minutes to get things up and running. Both chef and puppet uses ruby DSLs so it is very easy to read and intuitive.

Below is the diagram showing the complete picture :

![photo][3]

In the coming blogs, I would be covering how chef can be leveraged for the same.

I also have a nice presentation on the same topic, which can be viewed [here][4]

See [Xebia][5]'s offering in the field Continuous Delivery.

   [1]: http://devops.com/2013/02/11/defining-the-dev-and-the-ops-roles-in-devops/
   [2]: http://www.xebialabs.com/
   [3]: http://xebee.xebia.in/wp-content/uploads/2013/08/photo-300x225.jpg
   [4]: http://www.slideshare.net/AnirudhBhatnagar/dev-ops-finalbeingagile2013-24232828
   [5]: http://continuousdelivery.xebia.com/