---
layout: post
header-img: img/default-blog-pic.jpg
---

# How did Liferay help me in building my app?

In our current project we are building a portlet using Spring portlet MVC and liferay 5.2.3. More details about how we configured spring Portlet MVC with Liferay can be found [here](/2011/09/04/developing-portlets-using-spring-portlet-mvc-and-liferay-servicebuilder/). Liferay has proved to be a boon for us in numerous ways. There were out of the box plugins, services and tag libraries which did hasten our development process. Whenever we got a new requirement we were able to identify some feature of the liferay to suffice our needs. So let’s see in detail, how actually liferay has helped us in solving those problems: **1**.** Liferay Service-builder**: Perhaps the most important tool and most widely used feature of liferay, We used this for rapidly generating services, persistence and sql create scripts . However, we did face several issues with Service builder, which would open a whole new chapter of discussion if we start dissecting service builder here, but for now I would say it was a good bargain. How to use service builder can be found [here](http://xebee.xebia.in/2012/02/16/liferay-service-builder/). **2**.** Search implementation**: In our application we wanted to use [Apache Solr](http://lucene.apache.org/solr/) for searching and indexing. Liferay provides us with a plug-in for using the same. The integration was seamless and was as easy as deploying solr-web plugin war. More details on how to integrate solr with Liferay is given [here](http://www.liferay.com/community/wiki/-/wiki/Main/Integrate+Solr+with+Liferay+portal). And even if you choose not to use Solr, by default there is [Lucene](http://lucene.apache.org/core/) search available. In the control panel there is a link to call indexers of various configured portlets.(Control Panel ->Indexer Admin). **3**.** Ratings and Comments**-For implementing [ratings](http://www.liferay.com/community/wiki/-/wiki/Main/Adding+Rating+to+a+portlet) we used liferays ratings tags provided by <liferay-ui:ratings> and for [comments](http://www.liferay.com/community/forums/-/message_boards/message/5292621) we used <liferay-ui:discussions> **4**.**Image Library**\- We had to upload and store images for various items used in our application.Instead of building from scratch a new image uploader, we used liferay's Image Galaery (IG). We would upload the image and store its entry in DB using IGImageLocalService. The image is stored in document library and its path is stored in database, to display this image we would need image id and image token appended to the context path. Something like this :

> ThemeDisplay themeDisplay = (ThemeDisplay) pageContext.getRequest().getAttribute(WebKeys._THEME_DISPLAY_); source = themeDisplay.getPath() + "/image_gallery?img_id=" imageId + "&t=" + ImageServletTokenUtil._ __getToken_(imageId);

And print it using :

> <img src= “${source}” />

More details I would cover in a separate blog. **6**.** PDF Generation** – Liferay provides DocumentConversionUtil which can be used to convert/generate PDFs. This service uses Open Office APIs to generate and convert documents into PDF. All we needed was to setup Open Office Integration with liferay. In case you dont want to configure OpenOffice you can always use some third party library like Itext, PDFBox etc. **7\. Liferay mail and messaging service** \- Liferay provides facility to send mails and messages to the different users of liferay .We have com.liferay.mail.service.MessageLocalServiceUtil which we used to send message notifications and mails.

> MessageLocalServiceUtil._sendSimpleNotification_( fromUser.getUserId(), toUser.getUserId(), subject, body);

**8\. Liferay Tag Framework**\- We had a requirement to provide tags to the items in our product, which was food items in our case, we used the [Tag](http://www.liferay.com/web/guest/community/wiki/-/wiki/1071674/Tags) framework provided by liferay to configure different categories of tags, and provide associations with food items.This can be found in Control Panel ->Tags **9\. Liferay roles** – We had the different roles of Admin,User,Super Admin,Coach, and various permission issues, everything was catered by liferay roles, another of liferay's gem. **10\. Liferay tags for error messages and internationalizatio**n - Liferay provides various tags life <liferay-ui:error> <liferay-ui:message> which were used for styling and i18n. Many blogs are available on how to attain i18n and L10n in liferay. This list goes on(single sign-on, Social collaboration, content management,user profile mgmt,chats,blogs,forums...) as we still explore its various services and features, Overall I would say we did reduce the development time by 30 percent by having lot of in built features and out of the box plug-ins.

## Comments

**[somasekhar](#8401 "2012-04-09 11:08:43"):** informative

