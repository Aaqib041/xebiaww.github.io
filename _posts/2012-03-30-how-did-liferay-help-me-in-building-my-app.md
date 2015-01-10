---
layout: post
header-img: img/default-blog-pic.jpg
author: anirudh.xebia
description: 
post_id: 12971
created: 2012/03/30 12:42:57
created_gmt: 2012/03/30 07:42:57
comment_status: open
---

# How did Liferay help me in building my app?

<p><span style="font-family: verdana, geneva;">In our current project we are building a portlet using Spring portlet MVC and liferay 5.2.3.</span></p>
<p><span style="font-family: verdana, geneva;">More details about how we configured spring Portlet MVC with Liferay can be found <a href="http://xebee.xebia.in/2011/09/04/developing-portlets-using-spring-portlet-mvc-and-liferay-servicebuilder/">here</a>.</span></p>
<p><span style="font-family: verdana, geneva;">Liferay has proved to be a boon for us in numerous ways. There were out of the box plugins, services and tag libraries which did hasten our development process. Whenever we got a new requirement we were able to identify some feature of the liferay to suffice our needs.</span></p>
<p><span style="font-family: verdana, geneva;">So let’s see in detail, how actually liferay has helped us in solving those problems:</span></p>
<p><span style="font-family: verdana, geneva;"><!--more--></span>
<span style="font-family: verdana, geneva;"> <strong>1</strong>.<b> Liferay Service-builder</b>: Perhaps the most important tool and most widely used feature of liferay, We used this for rapidly generating services, persistence and sql create scripts . However, we did face several issues with Service builder, which would open a whole new chapter of discussion if we start dissecting service builder here, but for now I would say it was a good bargain. How to use service builder can be found <a href="http://xebee.xebia.in/2012/02/16/liferay-service-builder/">here</a>.</span></p>
<p><span style="font-family: verdana, geneva;"><strong>2</strong>.<b> Search implementation</b>: In our application we wanted to use <a href="http://lucene.apache.org/solr/">Apache Solr</a> for searching and indexing. Liferay provides us with a plug-in for using the same.</span></p>
<p><span style="font-family: verdana, geneva;">The integration was seamless and was as easy as deploying solr-web plugin war. More details on how to integrate solr with Liferay is given <a href="http://www.liferay.com/community/wiki/-/wiki/Main/Integrate+Solr+with+Liferay+portal">here</a>. And even if you choose not to use Solr, by default there is <a href="http://lucene.apache.org/core/">Lucene</a> search available.</span></p>
<p><span style="font-family: verdana, geneva;">In the control panel there is a link to call indexers of various configured portlets.(Control Panel -&gt;Indexer Admin).</span></p>
<p><span style="font-family: verdana, geneva;"><strong>3</strong>.<b> Ratings and Comments</b>-For implementing <a href="http://www.liferay.com/community/wiki/-/wiki/Main/Adding+Rating+to+a+portlet">ratings</a> we used liferays ratings tags provided by</span></p>
<p><span style="font-family: verdana, geneva;">&lt;liferay-ui:ratings&gt; and for <a href="http://www.liferay.com/community/forums/-/message_boards/message/5292621">comments</a> we used &lt;liferay-ui:discussions&gt;</span></p>
<p><span style="font-family: verdana, geneva;"><strong>4</strong>.<b>Image Library</b>- We had to upload and store images for various items used in our application.Instead of building from scratch a new image uploader, we used liferay's Image Galaery (IG).</span></p>
<p><span style="font-family: verdana, geneva;">We would upload the image and store its entry in DB using IGImageLocalService.</span></p>
<p><span style="font-family: verdana, geneva;">The image is stored in document library and its path is stored in database, to display this image we would need image id and image token appended to the context path.</span></p>
<p><span style="font-family: verdana, geneva;">Something like this :</span>
<blockquote><span style="font-family: verdana, geneva;">ThemeDisplay themeDisplay = (ThemeDisplay) pageContext.getRequest().getAttribute(WebKeys.<i>THEME_DISPLAY</i>);</span></p>
<p><span style="font-family: verdana, geneva;">source = themeDisplay.getPath() + "/image_gallery?img_id="</span></p>
<p><span style="font-family: verdana, geneva;">imageId + "&amp;t=" + ImageServletTokenUtil.<i> </i><i>getToken</i>(imageId);</span></blockquote>
<span style="font-family: verdana, geneva;">And print it using :</span>
<blockquote><span style="font-family: verdana, geneva;">&lt;img src= “${source}” /&gt;</span></blockquote>
<span style="font-family: verdana, geneva;">More details I would cover in a separate blog.</span></p>
<p><span style="font-family: verdana, geneva;"><strong>6</strong>.<b> PDF Generation</b> – Liferay provides DocumentConversionUtil which can be used to convert/generate PDFs. This service uses Open Office APIs to generate and convert documents into PDF.</span></p>
<p><span style="font-family: verdana, geneva;">All we needed was to setup Open Office Integration with liferay.</span></p>
<p><span style="font-family: verdana, geneva;">In case you dont want to configure OpenOffice you can always use some third party library like Itext, PDFBox etc.</span></p>
<p><span style="font-family: verdana, geneva;"><strong>7. Liferay mail and messaging service</strong> - Liferay provides facility to send mails and messages to the different users of liferay .We have</span></p>
<p><span style="font-family: verdana, geneva;">com.liferay.mail.service.MessageLocalServiceUtil which we used to send message notifications and mails.</span>
<blockquote><span style="font-family: verdana, geneva;">MessageLocalServiceUtil.<i>sendSimpleNotification</i>(</span></p>
<p><span style="font-family: verdana, geneva;">fromUser.getUserId(), toUser.getUserId(),</span></p>
<p><span style="font-family: verdana, geneva;">subject,</span></p>
<p><span style="font-family: verdana, geneva;">body);</span></blockquote>
<span style="font-family: verdana, geneva;"><strong>8. Liferay Tag Framework</strong>- We had a requirement to provide tags to the items in our product, which was food items in our case, we used the <a href="http://www.liferay.com/web/guest/community/wiki/-/wiki/1071674/Tags">Tag</a> framework provided by liferay to configure different categories of tags, and provide associations with food items.This can be found in Control Panel -&gt;Tags</span></p>
<p><span style="font-family: verdana, geneva;"><strong>9. Liferay roles</strong> – We had the different roles of Admin,User,Super Admin,Coach, and various permission issues, everything was catered by liferay roles, another of liferay's gem.</span></p>
<p><span style="font-family: verdana, geneva;"><strong>10. Liferay tags for error messages and internationalizatio</strong>n - Liferay provides various tags life &lt;liferay-ui:error&gt; &lt;liferay-ui:message&gt; which were used for styling and i18n. Many blogs are available on how to attain i18n and L10n in liferay.</span></p>
<p><span><span style="font-family: verdana, geneva;" face="verdana, geneva">
</span></span></p>
<p><span><span style="font-family: verdana, geneva;" face="verdana, geneva">This list goes on(single sign-on, Social collaboration, content management,user profile mgmt,chats,blogs,forums...) as we still explore its various services and features, Overall I would say we did reduce the development time by 30 percent by having lot of in built features and out of the box plug-ins.</span></span></p>
<p><span face="verdana, geneva" style="font-family: verdana, geneva;">
</span></p>

## Comments

**[somasekhar](#8401 "2012-04-09 11:08:43"):** informative

