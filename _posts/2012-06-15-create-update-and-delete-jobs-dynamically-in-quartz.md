---
layout: post
header-img: img/default-blog-pic.jpg
author: vrawat
description: 
post_id: 14490
created: 2012/06/15 16:48:12
created_gmt: 2012/06/15 11:48:12
comment_status: open
---

# Create, Update and Delete Jobs Dynamically in Quartz

<p><span style="font-size: 135%;"><strong><span style="color: #f09021;">Problem</span></strong></span>
I was recently working on a poc(proof of concept). The requirements were, to be able to dynamically schedule jobs using Qaurtz. After looking examples on the web I found most of the examples were of static scheduling (where we provide the jobs in a xml beforehand). But I can only find design strategies related to my requirements, no concrete implementation. So basically I will be providing a concrete implementation of the given problem in this blog.
<!--more-->
<span style="font-size: medium;"><strong><span style="color: #f09021;">Design Strategy</span></strong></span>
<ul>  <br />
        <li>Create a simple web interface for creating, updating and deleting information related to Jobs(<a href="https://github.com/vijayrawatsan/QuartzDynamicScheduling/blob/master/src/main/java/in/xebia/domain/JobData.java" target="_blank">JobData</a>) in database.(I will not be showing this part)</li>
    <li>Create a <strong>PersistentJobSchedulerJob</strong> which will schedule all the jobs stored in the database with a repeat interval. Basically the PersistentJobSchedulerJob is itself a job which will execute at regular intervals.</li>
</ul></p>
<p><span style="font-size: 135%;"><strong><span style="color: #f09021;">The Code</span></strong></span>
Here is the link to the complete project <a href="https://github.com/vijayrawatsan/QuartzDynamicScheduling" target="_blank">GituHub Repo</a></p>
<p><span style="font-size: 135%;"><strong><span style="color: #f09021;">Solution</span></strong></span>
Lets start with the <strong>PersistentJobSchedulerJob.java</strong> which is the most important class.</p>
<p>[code language="java" highlight="13"]
@Service
public class PersistentJobSchedulerJob {</p>
<pre><code>private static Logger logger = Logger.getLogger(&amp;quot;PersistentJobSchedulerJob&amp;quot;);

@Autowired
private JobRepository jobRepository;

@Autowired
private MailService mailService;

@SuppressWarnings({ &amp;quot;rawtypes&amp;quot;, &amp;quot;unchecked&amp;quot; })
@Scheduled(fixedRate=30000)
public void schedulePersistentJobs(){
    List&amp;lt;JobData&amp;gt; jobsData= jobRepository.findAll();
    logger.info(&amp;quot;Retriving Jobs from Database and Scheduling One by One | Total Number of Jobs: &amp;quot;+jobsData.size());
    try{
        Scheduler scheduler = new StdSchedulerFactory().getScheduler(); 
        scheduler.start();  
        for(JobData jobData: jobsData){
            JobDetail job = newJob(MailSenderJob.class)
                            .withIdentity(jobData.getJobName())
                            .usingJobData(getJobDataMap(jobData))
                            .build();
            if(!jobData.getActive()){
                logger.info(&amp;quot;Deleting a Job&amp;quot;);
                scheduler.deleteJob(new JobKey(jobData.getJobName()));
                continue;
            }
            if(scheduler.checkExists(new JobKey(jobData.getJobName()))){
                logger.info(&amp;quot;Rescheduling the Job&amp;quot;);
                Trigger oldTrigger = scheduler.getTrigger(new TriggerKey(jobData.getJobName()+&amp;quot;Trigger&amp;quot;));
                TriggerBuilder tb = oldTrigger.getTriggerBuilder();
                Trigger newTrigger = tb.withSchedule(simpleSchedule()
                          .withIntervalInMilliseconds(jobData.getRepeatInterval()).
                          repeatForever())
                          .build();

                scheduler.rescheduleJob(oldTrigger.getKey(), newTrigger);
            }else{
                logger.info(&amp;quot;Scheduling the Job&amp;quot;);
                scheduler.scheduleJob(job,getTrigger(jobData));
            }
        }
    }catch (SchedulerException e) {
        logger.error(&amp;quot;Scheduler Exception : &amp;quot;+e.getMessage());    
    }
}

private JobDataMap getJobDataMap(JobData jobData) {
    JobDataMap jobDataMap =  new JobDataMap();
    jobDataMap.put(&amp;quot;recipients&amp;quot;, jobData.getRecipients());
    jobDataMap.put(&amp;quot;mailService&amp;quot;, mailService);
    return jobDataMap;
}

private Trigger getTrigger(JobData jobData){
    SimpleTrigger simpleTrigger = newTrigger().withIdentity(jobData.getJobName()+&amp;quot;Trigger&amp;quot;)
                                    .startAt(jobData.getStartDateTime())
                                    .withSchedule(simpleSchedule()
                                      .withIntervalInMilliseconds(jobData.getRepeatInterval()).
                                      repeatForever())
                                      .build();
    return simpleTrigger;
}
</code></pre>
<p>}
[/code]</p>
<p>The above job is triggered using Spring's TaskScheduling framework(<a href="https://github.com/vijayrawatsan/QuartzDynamicScheduling/blob/master/src/main/resources/applicationContext.xml" target="_blank">applicationContext.xml</a>). 
You can see in <strong>line 13</strong> I'm providing the interval in which job should be repeated.
From lines <strong>15 through 43</strong> I'm first reading all the jobs data from database. Then for each job's data a Quartz Job is created with a corresponding trigger. 
Now there are three cases, 
<ul>
<li>If the job is not active(that means its deleted, so when you design your CRUD application make sure for a delete operation you just turn the field active to false, do not delete the row) , check if it exists in scheduler delete it. Move to the next job.</li>
<li>If job is active, check if it exists and if it does, simply update the trigger and reschedule the job.</li>
<li>If job is active, check if it exists and if it does not, simply schedule the job.</li>
</ul></p>
<p>Lets see the code for <strong>MailSenderJob.java</strong>
[code language="java"]
public class MailSenderJob implements Job {</p>
<pre><code>private static Logger logger = Logger.getLogger(&amp;quot;MailSenderJob&amp;quot;);

@SuppressWarnings(&amp;quot;rawtypes&amp;quot;)
public void execute(JobExecutionContext context)
        throws JobExecutionException {
    logger.info(&amp;quot;In MailSenderJob&amp;quot;);
    MailService mailService = (MailService)context.getJobDetail().getJobDataMap().get(&amp;quot;mailService&amp;quot;);;
    Map dataMap = context.getJobDetail().getJobDataMap();
    String recipients = (String) dataMap.get(&amp;quot;recipients&amp;quot;);
    logger.info(&amp;quot;Sending mails to : &amp;quot;+recipients);
    mailService.send(recipients);
}
</code></pre>
<p>}
[/code]</p>
<p>This is a pretty straight forward Quartz Job nothing new here. I am simply using the mailService and calling the send method. You can obviously run any logic you want.</p>
<p><span style="font-size: 135%;"><strong><span style="color: #f09021;">Conclusion</span></strong></span>
In this blog we learned how we can dynamically create, update and delete jobs using Quartz Scheduler.</p>
<p>Happy Reading.</p>

## Comments

**[viswabe1](#9305 "2012-12-14 15:32:26"):** It's a great job.Thanks. Will u pls send me the complete project. My mail id is viswanathan@pointelsolutions.com

**[Vijay Rawat](#9306 "2012-12-17 19:24:49"):** Hi Viswanathan, I have shared the Github repo in the blog. https://github.com/vijayrawatsan/QuartzDynamicScheduling

