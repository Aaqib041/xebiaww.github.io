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

**Problem** I was recently working on a poc(proof of concept). The requirements were, to be able to dynamically schedule jobs using Qaurtz. After looking examples on the web I found most of the examples were of static scheduling (where we provide the jobs in a xml beforehand). But I can only find design strategies related to my requirements, no concrete implementation. So basically I will be providing a concrete implementation of the given problem in this blog.  **Design Strategy**

  

  * Create a simple web interface for creating, updating and deleting information related to Jobs([JobData][1]) in database.(I will not be showing this part)
  * Create a **PersistentJobSchedulerJob** which will schedule all the jobs stored in the database with a repeat interval. Basically the PersistentJobSchedulerJob is itself a job which will execute at regular intervals.

**The Code** Here is the link to the complete project [GituHub Repo][2]

**Solution** Lets start with the **PersistentJobSchedulerJob.java** which is the most important class.

[code language="java" highlight="13"] @Service public class PersistentJobSchedulerJob {
    
    
    private static Logger logger = Logger.getLogger(&quot;PersistentJobSchedulerJob&quot;);
    
    @Autowired
    private JobRepository jobRepository;
    
    @Autowired
    private MailService mailService;
    
    @SuppressWarnings({ &quot;rawtypes&quot;, &quot;unchecked&quot; })
    @Scheduled(fixedRate=30000)
    public void schedulePersistentJobs(){
        List&lt;JobData&gt; jobsData= jobRepository.findAll();
        logger.info(&quot;Retriving Jobs from Database and Scheduling One by One | Total Number of Jobs: &quot;+jobsData.size());
        try{
            Scheduler scheduler = new StdSchedulerFactory().getScheduler(); 
            scheduler.start();  
            for(JobData jobData: jobsData){
                JobDetail job = newJob(MailSenderJob.class)
                                .withIdentity(jobData.getJobName())
                                .usingJobData(getJobDataMap(jobData))
                                .build();
                if(!jobData.getActive()){
                    logger.info(&quot;Deleting a Job&quot;);
                    scheduler.deleteJob(new JobKey(jobData.getJobName()));
                    continue;
                }
                if(scheduler.checkExists(new JobKey(jobData.getJobName()))){
                    logger.info(&quot;Rescheduling the Job&quot;);
                    Trigger oldTrigger = scheduler.getTrigger(new TriggerKey(jobData.getJobName()+&quot;Trigger&quot;));
                    TriggerBuilder tb = oldTrigger.getTriggerBuilder();
                    Trigger newTrigger = tb.withSchedule(simpleSchedule()
                              .withIntervalInMilliseconds(jobData.getRepeatInterval()).
                              repeatForever())
                              .build();
    
                    scheduler.rescheduleJob(oldTrigger.getKey(), newTrigger);
                }else{
                    logger.info(&quot;Scheduling the Job&quot;);
                    scheduler.scheduleJob(job,getTrigger(jobData));
                }
            }
        }catch (SchedulerException e) {
            logger.error(&quot;Scheduler Exception : &quot;+e.getMessage());    
        }
    }
    
    private JobDataMap getJobDataMap(JobData jobData) {
        JobDataMap jobDataMap =  new JobDataMap();
        jobDataMap.put(&quot;recipients&quot;, jobData.getRecipients());
        jobDataMap.put(&quot;mailService&quot;, mailService);
        return jobDataMap;
    }
    
    private Trigger getTrigger(JobData jobData){
        SimpleTrigger simpleTrigger = newTrigger().withIdentity(jobData.getJobName()+&quot;Trigger&quot;)
                                        .startAt(jobData.getStartDateTime())
                                        .withSchedule(simpleSchedule()
                                          .withIntervalInMilliseconds(jobData.getRepeatInterval()).
                                          repeatForever())
                                          .build();
        return simpleTrigger;
    }
    

} 
 ```

The above job is triggered using Spring's TaskScheduling framework([applicationContext.xml][3]). You can see in **line 13** I'm providing the interval in which job should be repeated. From lines **15 through 43** I'm first reading all the jobs data from database. Then for each job's data a Quartz Job is created with a corresponding trigger. Now there are three cases, 

  * If the job is not active(that means its deleted, so when you design your CRUD application make sure for a delete operation you just turn the field active to false, do not delete the row) , check if it exists in scheduler delete it. Move to the next job.
  * If job is active, check if it exists and if it does, simply update the trigger and reschedule the job.
  * If job is active, check if it exists and if it does not, simply schedule the job.

Lets see the code for **MailSenderJob.java** ``` 
 public class MailSenderJob implements Job {
    
    
    private static Logger logger = Logger.getLogger(&quot;MailSenderJob&quot;);
    
    @SuppressWarnings(&quot;rawtypes&quot;)
    public void execute(JobExecutionContext context)
            throws JobExecutionException {
        logger.info(&quot;In MailSenderJob&quot;);
        MailService mailService = (MailService)context.getJobDetail().getJobDataMap().get(&quot;mailService&quot;);;
        Map dataMap = context.getJobDetail().getJobDataMap();
        String recipients = (String) dataMap.get(&quot;recipients&quot;);
        logger.info(&quot;Sending mails to : &quot;+recipients);
        mailService.send(recipients);
    }
    

} 
 ```

This is a pretty straight forward Quartz Job nothing new here. I am simply using the mailService and calling the send method. You can obviously run any logic you want.

**Conclusion** In this blog we learned how we can dynamically create, update and delete jobs using Quartz Scheduler.

Happy Reading.

   [1]: https://github.com/vijayrawatsan/QuartzDynamicScheduling/blob/master/src/main/java/in/xebia/domain/JobData.java
   [2]: https://github.com/vijayrawatsan/QuartzDynamicScheduling
   [3]: https://github.com/vijayrawatsan/QuartzDynamicScheduling/blob/master/src/main/resources/applicationContext.xml

## Comments

**[viswabe1](#9305 "2012-12-14 15:32:26"):** It's a great job.Thanks. Will u pls send me the complete project. My mail id is viswanathan@pointelsolutions.com

**[Vijay Rawat](#9306 "2012-12-17 19:24:49"):** Hi Viswanathan, I have shared the Github repo in the blog. https://github.com/vijayrawatsan/QuartzDynamicScheduling

