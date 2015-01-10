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

<p><span style="font-size: medium;"><strong><span style="color: #f09021;">Background</span></strong></span>
I have been working with a Liferay project for quite sometime. Recently I was caught up in a quite common scenario where I needed to access a pdf stored in Liferay's Document Library. The problem was only for the dynamic jsp pages used in my portlet but not for the static Web Content. More specifically when I use the document library generated url in the static web content pages on development environment and export the .lar (Liferay Archive) file from development to any other environment the pdf link in the static page works. But when I do the same thing with jsp pages the link to pdf works on development environment only, but breaks on any other environment.
<!--more-->
<span style="font-size: medium;"><strong><span style="color: #f09021;">Whos the culprit?</span></strong></span>
After a lot digging, I found that the url of the pdf stored in document library contains a <strong>group_id </strong>information. Basically every document(any file) belongs to a community in Liferay. That community's id is the group_id for that document. So whenever we export a .lar file all the urls used in static web content containing<strong> &amp;group_id=some_random_grp_id</strong> is converted to <strong>@group_id@</strong> and that is replced with actual <strong>group_id </strong>when we<strong> import that lar</strong>. But for JSP pages residing in your own portlet the urls remain same because .lar only exports static web content and thus a content not found error is shown when you try to access that url.</p>
<p><span style="font-size: medium;"><strong><span style="color: #f09021;">What did I do?</span></strong></span>
Just like any other developer out there I searched google, liferay forums and stackoverflow for the problem but to my surprise I could not find a straight forward solution. Then I looked into Liferay's Document Library source code, just to understand how it works, in the hope of getting a solution. There was a file called <a href="http://docs.liferay.com/portal/5.1/javadocs/portal-impl/com/liferay/portlet/documentlibrary/action/GetFileAction.java.html" target="_blank">GetFileAction.java</a> , in that file there was a service called <strong>DLFileEntryLocalServiceUtil </strong>which is the key service for my solution below.</p>
<p><span style="font-size: medium;"><strong><span style="color: #f09021;">Solution</span></strong></span>
First of all <strong>ThemeDisplay</strong> can be retrieved using the<strong> req.getAttribute(WebKeys.THEME_DISPLAY)</strong> method and its quite a trivial thing among liferay developers. <strong>ThemeDisplay </strong>can then be used to get the groupId as shown below:
[code language="java"]
ThemeDisplay themeDisplay = (ThemeDisplay) req.getAttribute(WebKeys.THEME_DISPLAY);
long groupId = themeDisplay.getLayout().getGroupId();
[/code]</p>
<p>After you have obtained the groupId the easy part is complete. Now <strong>DLFileEntryLocalServiceUtil </strong>has a method called <strong>getGroupFileEntries(long groupId, int start, int end)</strong>. This method returns all the documents of the passed groupId starting from start to end (int parameters).
[code language="java"]
String documentUrl = null;
List&lt;DLFileEntry&gt; fileEntries = DLFileEntryLocalServiceUtil.getGroupFileEntries(groupId , 0, 50);
for(DLFileEntry fileEntry : fileEntries) {
     if(fileEntry.getTitle().equalsIgnoreCase(&quot;Uploaded_Document_Name&quot;)) {
          documentUrl = &quot;/c/document_library/get_file?uuid=&quot; + fileEntry.getUuid()+ &quot;&amp;groupId=&quot; + fileEntry.getGroupId();
     }
}
[/code]</p>
<p>Now after obtaining all the <strong>FileEntries </strong>you can simply loop through the List and check for <strong>title </strong>as I have done above or any other parameter you like. And simply generate the documentUrl. Now this document url can be used on jsp without worrying about the enviournment, since its coming directly from document_library.</p>
<p><span style="font-size: medium;"><strong><span style="color: #f09021;">Conclusion</span></strong></span>
So today we learnt how to access CMS controlled documents within JSP in liferay. More importantly we learnt that one should always have a look at the source code of things you are using.</p>
<p><strong>Note : </strong>
<ul>
    <li>I personally dont think this is the best solution, but in my case there were only 2 different terms and condition documents so there is no problem using this code. But if your application deals with a lot of documents you should consider using a better solution. </li>
    <li>Same thing can be done for Image Gallery images also. For that we have a service called IGFolderLocalServiceUtil.</li>
</ul></p>