---
layout: post
header-img: img/default-blog-pic.jpg
author: vrawat
description: 
post_id: 15300
created: 2012/11/04 20:01:57
created_gmt: 2012/11/04 15:01:57
comment_status: open
---

# Liferay | Accessing CMS controlled documents within JSP

**Background** I have been working with a Liferay project for quite sometime. Recently I was caught up in a quite common scenario where I needed to access a pdf stored in Liferay's Document Library. The problem was only for the dynamic jsp pages used in my portlet but not for the static Web Content. More specifically when I use the document library generated url in the static web content pages on development environment and export the .lar (Liferay Archive) file from development to any other environment the pdf link in the static page works. But when I do the same thing with jsp pages the link to pdf works on development environment only, but breaks on any other environment.  **Whos the culprit?** After a lot digging, I found that the url of the pdf stored in document library contains a **group_id **information. Basically every document(any file) belongs to a community in Liferay. That community's id is the group_id for that document. So whenever we export a .lar file all the urls used in static web content containing** &group_id=some_random_grp_id** is converted to **@group_id@** and that is replced with actual **group_id **when we** import that lar**. But for JSP pages residing in your own portlet the urls remain same because .lar only exports static web content and thus a content not found error is shown when you try to access that url.

**What did I do?** Just like any other developer out there I searched google, liferay forums and stackoverflow for the problem but to my surprise I could not find a straight forward solution. Then I looked into Liferay's Document Library source code, just to understand how it works, in the hope of getting a solution. There was a file called [GetFileAction.java][1] , in that file there was a service called **DLFileEntryLocalServiceUtil **which is the key service for my solution below.

**Solution** First of all **ThemeDisplay** can be retrieved using the** req.getAttribute(WebKeys.THEME_DISPLAY)** method and its quite a trivial thing among liferay developers. **ThemeDisplay **can then be used to get the groupId as shown below: ``` 
 ThemeDisplay themeDisplay = (ThemeDisplay) req.getAttribute(WebKeys.THEME_DISPLAY); long groupId = themeDisplay.getLayout().getGroupId(); 
 ```

After you have obtained the groupId the easy part is complete. Now **DLFileEntryLocalServiceUtil **has a method called **getGroupFileEntries(long groupId, int start, int end)**. This method returns all the documents of the passed groupId starting from start to end (int parameters). ``` 
 String documentUrl = null; List<DLFileEntry> fileEntries = DLFileEntryLocalServiceUtil.getGroupFileEntries(groupId , 0, 50); for(DLFileEntry fileEntry : fileEntries) { if(fileEntry.getTitle().equalsIgnoreCase("Uploaded_Document_Name")) { documentUrl = "/c/document_library/get_file?uuid=" \+ fileEntry.getUuid()+ "&groupId=" \+ fileEntry.getGroupId(); } } 
 ```

Now after obtaining all the **FileEntries **you can simply loop through the List and check for **title **as I have done above or any other parameter you like. And simply generate the documentUrl. Now this document url can be used on jsp without worrying about the enviournment, since its coming directly from document_library.

**Conclusion** So today we learnt how to access CMS controlled documents within JSP in liferay. More importantly we learnt that one should always have a look at the source code of things you are using.

**Note : **

  * I personally dont think this is the best solution, but in my case there were only 2 different terms and condition documents so there is no problem using this code. But if your application deals with a lot of documents you should consider using a better solution. 
  * Same thing can be done for Image Gallery images also. For that we have a service called IGFolderLocalServiceUtil.

   [1]: http://docs.liferay.com/portal/5.1/javadocs/portal-impl/com/liferay/portlet/documentlibrary/action/GetFileAction.java.html