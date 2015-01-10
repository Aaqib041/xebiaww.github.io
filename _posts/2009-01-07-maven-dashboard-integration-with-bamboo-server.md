---
layout: post
header-img: img/default-blog-pic.jpg
---

# Maven Dashboard integration with bamboo server

With the help of maven dashboard plugin, the dashboard is integrated with the project and the reports can be generated on local machine. One step further would be to configure the reports on the bamboo build server itself so that the reports can be viewed by everyone. For this to happen, following changes need to be done in the configuration of bamboo plan: 

  * In Bamboo plan create the Maven goal "dashboard" using  'clean install site dashboard:dashboard' command and make the build JDK version to 1.5. The change in goal will make sure that at the specified time the build would run automatically and goal called “dashboard” would be executed. This will create the reports on the Bamboo server. Though the reports have been created, you can't see them yet.
  * To see the reports, we'll use the artifact feature of Bamboo plan. Create a label say 'dashboard' in Bamboo plan (artifacts section) and in copy-pattern section specify '**/dashboard/**'. With this pattern, build execution will preserve the files on server and will save them with the build number. One can go and see the results of the build and also the dashboard results now by clicking on artifacts link in "Build Results Summary" page. Now the build is integrated with dashboard and one can click on link came in email results (sent from Bamboo) to view the report. However you (should be logged in already) can always use "<Bamboo server base url>/download/<project name>/artifacts/latest/Dashboard/index.html" to view the reports.

## Comments

**[J A](#5302 "2011-02-17 03:28:22"):** Where did this plugin go?

