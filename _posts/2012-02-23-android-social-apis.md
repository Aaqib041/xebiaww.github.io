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

Giving users “power to share ” seamlessly  is a feature we will find in all popular and frequently used Mobile applications. Being social is crucial for applications. Users love to share and Android knows that. Acting prudently, Android has provided API's as well as inherent support using [Intents][1]** **for social integration. Here, I will explicate the API's developers can use to integrate social features with their applications.

** **

** ****ContactsContract**:

We (developers) usually use this API to get user's contacts list. The important thing about this API is that one Contact row has different information(phone number, email id, name, photo etc.) for the same person aggregated from different sources like local contacts book, Facebook account, Skype, Gmail account etc. The whole contacts database is **extensible **and is managed in a **three tier data model **using following three classes under ContactsContract package: 

  * RawContacts
  * Data
  * Contacts![][2]

[caption id="attachment_12251" align="aligncenter" width="650" caption="ContactsContract"]![][3][/caption]

We can do basic operations like insert, update, delete as well as query the contacts database. For example, to insert a contact :

[sourcecode language="java"] ContentValues values = new ContentValues(); values.put(RawContacts.ACCOUNT_TYPE, accountType); values.put(RawContacts.ACCOUNT_NAME, accountName); Uri rawContactUri = getContentResolver().insert(RawContacts.CONTENT_URI, values); long rawContactId = ContentUris.parseId(rawContactUri); [/sourcecode]

**What Android4.0 (ICS) added to it:**

  * **Easy access with high resolution photos**: In this version, the people to which users are most connected will be arranged into a magazine like UI. According to Android developer team “This is to support emotional connections between humans and the devices they carry”. For this purpose higher resolution pictures will be used ( 256x256 instead of 98x98).
  * **Social Stream for Sync Adapters: **Through this “biggest new edition”, social applications can display recent online activities of all the contacts in the device user’s contact list. The activities would be visible in the contact list itself. This is achieved by **Sync Adapters. **Sync adapters can synchronize the raw contacts data  available locally on the device with the contact data available in the cloud resources like Facebook, Skype and other social  apps. In current update, the Sync Adapters now can also fetch update status, photos and other user posts via social stream. Following two classes are important:
  1. [StreamItems][4]: Stream items contain the recent social updates of a particular user under its Raw Contact data. Also contain the comments, photos and other useful information related to that social update.
  2. [StreamItemPhotos][5]: Contains the photos related to a single social update by the user.
  * **The “ME” Contact: **This will contain device user’s personal contact  information and profile data. Applications can access the “ME” profile information for display and verification purpose.
  * **Adding a new connection is easy: **This is another important update in ICS. Now user can connect to a person on any social website (Facebook, Google plus) by clicking an option in the contact detail page of that person in the device's people app.

** **

**NFC Reloaded with Beam:**

NFC or Near Field Communication is one of the latest buzzwords in mobile sphere. Besides its acclaimed uses in e-commerce, mobile payments and enterprises it is becoming a new media of sharing. This "[tap][6]" technology can be used for exchanging any type of data between two mobile devices.

Good thing is, NFC APIs are exposed to the developers and are available under [android.nfc][7] package.

In 4.0, a new application "Beam" is introduced to make tap-and-share feature  more intuitive. Using Beam, users can share apps, contacts, music, videos and other media contents and files instantly by single tap or touch of two NFC enabled devices.

Beam uses "NDEF push" to transfer data between devices when two devices come in close proximity. NDEF is a special format for NFC data and stands for "NFC Data Exchange Format". Two important classes used for NFC communication are: 

  * NDEFMessage: Container for NDEFRecords.
  * NDEFRecord: Contains the payload data, id and the type of data. if payload data is large, it is divided into   more than one NDEFRecords.
** ****Intents To Share:**

Intents are the mainstay of Android system. Apart from regular usage of intents to start another activities, passing data across activities and applications, receiving broadcasts and triggering a service, intents can perform an interesting job for us: to give apps the power to "share" and that without a much extra effort. The simplest case is to create "Activity chooser " dialog to chose a social app capable of sharing:

[sourcecode language="java"] Intent intent=new Intent(android.content.Intent.ACTION_SEND); intent.setType("text/plain"); intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_WHEN_TASK_RESET); // Add data to the intent, the receiving app will decide what to do with it. intent.putExtra(Intent.EXTRA_SUBJECT, “Here goes the subject...”); intent.putExtra(Intent.EXTRA_TEXT, “Here goes the message body...”); <pre>[/sourcecode]

**Updates in 4.0: **In this version, a new tool is introduced : [ShareActionProvider][8] . This will create a "share" button in the Action Bar. It is a sub-class of ActionProvider. ActionProviders are responsible for creating views to do the required action that will accomplish a given task. ShareActionProvider will create view for "share" task.

For using ShareActionProvider, modify the onCreateOptionsMenu() method in the Activity:

[sourcecode language="java"]

public boolean onCreateOptionsMenu(Menu menu) {

MenuItem menuItem1 = menu.findItem(R.id.menu_item);

mShareActionProvider = (ShareActionProvider) menuItem1.getActionProvider();

mShareActionProvider.setShareHistoryFileName("custom_share_history.xml");

}

[/sourcecode]

This will create a list of share targets the user can chose from to perform the sharing. The order of the share targets will depend on the past usage of the user which is saved in a history file. We can either use the default history file or we can maintain a custom history file and use that file throughout the application.

The ICS release has brought exciting goodies for developers and updates in social API's are most appealing.

   [1]: http://developer.android.com/reference/android/content/Intent.html
   [2]: http://xebee.xebia.in/wp-includes/js/tinymce/plugins/wordpress/img/trans.gif (More...)
   [3]: http://xebee.xebia.in/wp-content/uploads/2012/02/ContactsContract2-1024x621.jpg (ContactsContract)
   [4]: http://developer.android.com/reference/android/provider/ContactsContract.Contacts.StreamItems.html
   [5]: http://developer.android.com/reference/android/provider/ContactsContract.StreamItems.StreamItemPhotos.html
   [6]: http://www.slideshare.net/ParamvirSingh1/near-field-communication-10509831 (Slides: NFC, the "tap" technology)
   [7]: http://developer.android.com/reference/android/nfc/package-summary.html (android.nfc packager summary)
   [8]: http://developer.android.com/reference/android/widget/ShareActionProvider.html (ShareActionProvider)