---
layout: post
header-img: img/default-blog-pic.jpg
author: psingh
description: 
post_id: 12262
created: 2012/03/01 17:51:22
created_gmt: 2012/03/01 12:51:22
comment_status: open
---

# NFC in Android

Near Field Communication or NFC is one of the hottest and most talked about feature in present day smartphones. The reason it is being so popular with smartphone users is that it is intuitive and easy to use. Also, NFC is being considered as one of the most appealing future technology in Mobile domain. Invented by NXP and Sony jointly in 2002, NFC was first adopted by Nokia for their smartphones to make mobile payments easy for their users. Now it is being supported and encouraged by [NFC -Forum][1], which has nearly 160 members including mobile manufaturers and developers. The member list of this forum boasts of major players like Samsung, Motorola, Google, Sony Ericsson and Microsoft among others.

Google is particularly ambitious about this feature in its latest range of Android devices. Here is a summary of recent efforts by Google to make NFC popular among users and developers: 

  * **NFC API's for developers: **In OS version 2.3 (Gingerbread) of Android, Google introduced NFC support and also exposed API's for developers to create applications for NFC enabled Android devices. Using this API's, "Developers can create new applications that offer proximity-based information and services to users, organizations, merchants, and advertisers".
  * **Google Wallet: **Google's ambitious project in the area of mCommerce. This application for smart-phones empowers the devices to act as a virtual wallet which store user's credit card and other payment information safely. Users can use their mobile devices at stores to pay and benefit from offers provided by Google directly. The transaction will happen by tapping the mobile device at NFC readers installed at point of sales.
  * **Android Beam: **In Android 4.0(Ice-cream Sandwich), a new application called "Beam" is introduced. This application makes it more easy and intuitive for users to share contacts, apps, songs and videos between two Android devices using NFC. Beam uses NFC Android API's which are fully exposed to developers.
Let's dig into the important API’s and technical details we can use to make our application NFC enabled. Two terms frequently used for NFC are: 
  * **NFC Tag: **The important characteristic of NFC is that an NFC powered mobile device can read and write passive targets. These passive targets are usually NFC tags which may be very small and simple and can be in the form of small stickers, movie poster or visting cards.
  * **NFC device:** NFC device is the powered device which can read or write another NFC device or tag. In our case, it is NFC enabled Android phone.
**Detecting an NFC tag:**

NFC device detects a NFC tag or other device when they come in close proximity(distance < 4cm). On detecting the target, the device opens up the desired Activity to handle the required job. For instance, when a video is to be shared, Youtube application opens up and when user wants to share an app, Android market gets opened. If the device detects a tag with a link, it redirects to that link on browser.

This astute process of detecting a target, extracting information from that target and then pinpointing the Activity to accomplish the intended task without user intervention is called "**Tag dispatching**". Tags contain small amount of data in a special format called NDEF (NFC Data Exchange Format) standardized by NFC forum. This data and its MIME type is exchanged from the target to the device. On device, this data is further attached to an Intent which is sent to the interested applications. The applications which intend to handle this type of data will have to register appropriate Intent-filter.

We deal with following classes: 

  1. **NdefMessage: **Container class for storing the NDEF data. It contains a group of records which  contain the actual payload data, the type of data and other information about it.
  2. **NdefRecord: **Represents the records in NdefMessage. These records contain different fields to identify the required information about the data like format, type, ID and the actual payload. One of the field is **Type Name Format (TNF)** which is used to detect the MIME type or URI of the NdefMessage. When the data is transferred from target to receiver,the first NdefRecord is looked up for getting information about the whole NdefMessage. After that an Intent is created with action **ACTION_NDEF_DISCOVERED **and the MIME type and URI is attached to it.
The appropriate application is chosen to do the job on the bases of intent and the data attached to it. The applications willing to handle the NFC tag detection declare intent filter in their manifest file:

[sourcecode language="xml"]

<intent-filter>

<action android:name="android.nfc.action.NDEF_DISCOVERED"/>

<category android:name="android.intent.category.DEFAULT"/>

<data android:scheme="http"

android:host="xebia.in"

android:pathPrefix="/company.html" />

</intent-filter>

[/sourcecode]

This Intent filter will filter intent generated on detecting a tag having a URI as data and will result in opening www.xebia.in/company.html in browser. 

![][2]

**Extracting required data from the NFC intent:**

When the selected activity opens up after tag dispatch, required data is extracted from the incoming intent to proceed further. This data is contained in "extras" of the intent:

1**.  EXTRA_TAG: **The extra for containing payload data and tag technology information bound into a  Tag object.

[sourcecode language="java"] Tag tag = intent.getParcelableExtra(NfcAdapter.EXTRA_TAG); [/sourcecode]

2. **EXTRA_NDEF_MESSAGES: **The extra for containing all the NDEF messages in Tag object. These messages are obtained in an Array of Parcelable objects.

[sourcecode language="java"]

public void onResume() { super.onResume(); Intent parentIntent = getIntent(); if (NfcAdapter.ACTION_NDEF_DISCOVERED.equals(parentIntent.getAction())){ Parcelable[] rawMsgs = intent.getParcelableArrayExtra(NfcAdapter.EXTRA_NDEF_MESSAGES); if (rawMsgs != null) { msgs = new NdefMessage[rawMsgs.length]; for (int i = 0; i < rawMsgs.length; i++){ msgs[i] = (NdefMessage) rawMsgs[i]; } } } } [/sourcecode]

**Using Beam to push data:**

Beam can be used to push data from an application while it is running in foreground. Beam gets activated when the "beaming" device is put in physical contact with another device for a moment. At this instance, user is prompted for confirmation in special Beam UI and style. Any third party application can integrate Beam by using **setNdefPushMessage() **on **NfcAdapter**. This method will handle the beaming of data when devices come in required distance.

**CreateNdefMessageCallback **interface is implemented to add Ndef push functionality in the Activity. This implementation requires following method to be defined:

[sourcecode language="java"] public NdefMessage createNdefMessage(NfcEvent event) { String message = "This is NFC message"; NdefRecord mimeRecord = createMimeRecord("application/param.android.sample.beam", message.getBytes()); NdefRecord appRecord = NdefRecord.createApplicationRecord("param.android.sample.beam"); NdefRecord[] ndefRecords = new NdefRecord[] { mimeRecord, appRecord }; NdefMessage ndefMessage = new NdefMessage(ndefRecords); return ndefMessage; }

public NdefRecord createMimeRecord(String mimeType, byte[] payload) { byte[] mimeBytes = mimeType.getBytes(Charset.forName("US-ASCII")); NdefRecord mimeRecord = new NdefRecord( NdefRecord.TNF_MIME_MEDIA, mimeBytes, new byte[0

   [1]: http://www.nfc-forum.org/home/ (NFC forum)
   [2]: http://xebee.xebia.in/wp-content/uploads/2012/02/Tag_dispatching.png (Tag_dispatching)