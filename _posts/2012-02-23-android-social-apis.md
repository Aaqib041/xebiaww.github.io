---
layout: post
header-img: img/default-blog-pic.jpg
author: psingh
description: 
post_id: 12204
created: 2012/02/23 14:10:50
created_gmt: 2012/02/23 09:10:50
comment_status: open
---

# Android Social API's and Updates in Ice Cream Sandwich

<p>Giving users “power to share ” seamlessly  is a feature we will find in all popular and frequently used Mobile applications. Being social is crucial for applications. Users love to share and Android knows that. Acting prudently, Android has provided API's as well as inherent support using <a href="http://developer.android.com/reference/android/content/Intent.html" target="_blank">Intents</a><b> </b>for social integration. Here, I will explicate the API's developers can use to integrate social features with their applications.<!--more--></p>
<p><b> </b></p>
<p><b> </b><b>ContactsContract</b>:</p>
<p>We (developers) usually use this API to get user's contacts list. The important thing about this API is that one Contact row has different information(phone number, email id, name, photo etc.) for the same person aggregated from different sources like local contacts book, Facebook account, Skype, Gmail account etc. The whole contacts database is <b>extensible </b>and is managed in a <b>three tier data model </b>using following three classes under ContactsContract package:
<ul>
    <li>RawContacts</li>
    <li>Data</li>
    <li>Contacts<img src="http://xebee.xebia.in/wp-includes/js/tinymce/plugins/wordpress/img/trans.gif" title="More..." /></li>
</ul>
<div></p>
<p>[caption id="attachment_12251" align="aligncenter" width="650" caption="ContactsContract"]<a rel="attachment wp-att-12251" href="http://xebee.xebia.in/2012/02/23/android-social-apis/contactscontract-3/"><img class="size-large wp-image-12251   " height="400" width="650" title="ContactsContract" src="http://xebee.xebia.in/wp-content/uploads/2012/02/ContactsContract2-1024x621.jpg" /></a>[/caption]</p>
<p></div>
<div>
<div><dl id="attachment_12184"> </dl></div>
</div>
We can do basic operations like insert, update, delete as well as query the contacts database. For example, to insert a contact :</p>
<p>[sourcecode language="java"]
ContentValues values = new ContentValues();
values.put(RawContacts.ACCOUNT_TYPE, accountType);
values.put(RawContacts.ACCOUNT_NAME, accountName);
Uri rawContactUri = getContentResolver().insert(RawContacts.CONTENT_URI, values);
long rawContactId = ContentUris.parseId(rawContactUri);
[/sourcecode]</p>
<p><b>What Android4.0 (ICS) added to it:</b>
<ul>
    <li><b>Easy access with high resolution photos</b>: In this version, the people to which users are most connected will be arranged into a magazine like UI. According to Android developer team “This is to support emotional connections between humans and the devices they carry”.
For this purpose higher resolution pictures will be used ( 256x256 instead of 98x98).</li>
</ul>
<ul>
    <li><b>Social Stream for Sync Adapters: </b>Through this “biggest new edition”, social applications can display recent online activities of all the contacts in the device user’s contact list. The activities would be visible in the contact list itself. This is achieved by <b>Sync Adapters. </b>Sync adapters can synchronize the raw contacts data  available locally on the device with the contact data available in the cloud resources like Facebook, Skype and other social  apps. In current update, the Sync Adapters now can also fetch update status, photos and other user posts via social stream. Following two classes are important:</li>
</ul>
<ol>
    <li><a href="http://developer.android.com/reference/android/provider/ContactsContract.Contacts.StreamItems.html" target="_blank">StreamItems</a>: Stream items contain the recent social updates of a particular user under its Raw Contact data. Also contain the comments, photos and other useful information related to that social update.</li>
    <li><a href="http://developer.android.com/reference/android/provider/ContactsContract.StreamItems.StreamItemPhotos.html" target="_blank">StreamItemPhotos</a>: Contains the photos related to a single social update by the user.</li>
</ol>
<ul>
    <li><b>The “ME” Contact: </b>This will contain device user’s personal contact  information and profile data. Applications can access the “ME” profile information for display and verification purpose.</li>
</ul>
<ul>
    <li><b>Adding a new connection is easy: </b>This is another important update in ICS. Now user can connect to a person on any social website (Facebook, Google plus) by clicking an option in the contact detail page of that person in the device's people app.</li>
</ul>
<div><b>
</b></div>
<b>NFC Reloaded with Beam:</b></p>
<p>NFC or Near Field Communication is one of the latest buzzwords in mobile sphere. Besides its acclaimed uses in e-commerce, mobile payments and enterprises it is becoming a new media of sharing. This "<a href="http://www.slideshare.net/ParamvirSingh1/near-field-communication-10509831" title="Slides: NFC, the &quot;tap&quot; technology" target="_blank">tap</a>" technology can be used for exchanging any type of data between two mobile devices.</p>
<p>Good thing is, NFC APIs are exposed to the developers and are available under <a href="http://developer.android.com/reference/android/nfc/package-summary.html" title="android.nfc packager summary" target="_blank">android.nfc</a> package.</p>
<p>In 4.0, a new application "Beam" is introduced to make tap-and-share feature  more intuitive. Using Beam, users can share apps, contacts, music, videos and other media contents and files instantly by single tap or touch of two NFC enabled devices.</p>
<p>Beam uses "NDEF push" to transfer data between devices when two devices come in close proximity. NDEF is a special format for NFC data and stands for "NFC Data Exchange Format". Two important classes used for NFC communication are:
<ul>
    <li>NDEFMessage: Container for NDEFRecords.</li>
    <li>NDEFRecord: Contains the payload data, id and the type of data. if payload data is large, it is divided into   more than one NDEFRecords.</li>
</ul>
<b>
</b><strong>Intents To Share:</strong></p>
<p>Intents are the mainstay of Android system. Apart from regular usage of intents to start another activities, passing data across activities and applications, receiving broadcasts and triggering a service, intents can perform an interesting job for us: to give apps the power to "share" and that without a much extra effort. The simplest case is to create "Activity chooser " dialog to chose a social app capable of sharing:</p>
<p>[sourcecode language="java"]
Intent intent=new Intent(android.content.Intent.ACTION_SEND);
intent.setType(&quot;text/plain&quot;);
intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_WHEN_TASK_RESET);
// Add data to the intent, the receiving app will decide what to do with it.
intent.putExtra(Intent.EXTRA_SUBJECT, “Here goes the subject...”);
intent.putExtra(Intent.EXTRA_TEXT, “Here goes the message body...”);
&lt;pre&gt;[/sourcecode]</p>
<p><strong>Updates in 4.0: </strong>In this version, a new tool is introduced : <a href="http://developer.android.com/reference/android/widget/ShareActionProvider.html" title="ShareActionProvider" target="_blank">ShareActionProvider</a> . This will create a "share" button in the Action Bar. It is a sub-class of ActionProvider. ActionProviders are responsible for creating views to do the required action that will accomplish a given task. ShareActionProvider will create view for "share" task.</p>
<p>For using ShareActionProvider, modify the onCreateOptionsMenu() method in the Activity:</p>
<p>[sourcecode language="java"]</p>
<p>public boolean onCreateOptionsMenu(Menu menu) {</p>
<p>MenuItem menuItem1 = menu.findItem(R.id.menu_item);</p>
<p>mShareActionProvider = (ShareActionProvider) menuItem1.getActionProvider();</p>
<p>mShareActionProvider.setShareHistoryFileName(&quot;custom_share_history.xml&quot;);</p>
<p>}</p>
<p>[/sourcecode]</p>
<p>This will create a list of share targets the user can chose from to perform the sharing. The order of the share targets will depend on the past usage of the user which is saved in a history file. We can either use the default history file or we can maintain a custom history file and use that file throughout the application.</p>
<p>The ICS release has brought exciting goodies for developers and updates in social API's are most appealing.</p>