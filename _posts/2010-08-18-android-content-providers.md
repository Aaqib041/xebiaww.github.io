---
layout: post
header-img: img/default-blog-pic.jpg
author: pranjan
description: 
post_id: 4536
created: 2010/08/18 01:43:08
created_gmt: 2010/08/17 20:43:08
comment_status: open
---

# Android Content Providers

The database in an Android application is available only to the application which created it. In normal scenarios, applications need to share data quiet frequently. There is a concept of content provider  in Android which can be used to share data between different applications. The content provider is a type of data store which has an URI. This URI is the signature of the content providers. The URI is used to get the handle to the content provider and perform operations on it.

Android comes with several in built content providers like call log info, contacts, medial files etc which provide information about the calls, the contacts and the media files stored. Each is having a URI through which the data inside them can be accessed.

You can also create your own content providers and assign an URI to it.  In this blog, we will create our own content provider and use it to insert/access data in/from it.

First of all we need to create a class which extends the ContentProvider class provided by Android.

[sourcecode language="java"] public class AddressProvider extends ContentProvider {

public static final Uri CONTENT_URI = Uri.parse("content://com.example.AddressProvider");

private SQLiteDatabase db; private String DATABASE_TABLE = "address";

@Override public boolean onCreate() { DatabaseHelper dbHelper = new DatabaseHelper(getContext()); db = dbHelper.getWritableDatabase(); return (db == null) ? false : true; } ... } [/sourcecode]

Here we have extended the ContentProvider class and added instance variables for using database, and an URI which is the URI of the content provider. Then we have overridden the onCreate() to initialize the SQLiteDatabase db using DatabaseHelper which was created in my [previous post about Android databases][1].

The ContentProvider is an abstract class and subclassing it needs implementation for these five methods

getType() onCreate() query() insert() delete() update()

Implement all methods by the default implementation given by Eclipse(or other IDE). Then we will change the implementation for insert() and query(). insert() is used to insert record and query() is used to fetch data.

[sourcecode language="java"] @Override public Uri insert(Uri uri, ContentValues values) {
    
    
      long rowID = db.insert(DATABASE_TABLE, &quot;&quot;, values);
    
      if (rowID &gt; 0) {
           Uri _uri = ContentUris.withAppendedId(CONTENT_URI, rowID);
           getContext().getContentResolver().notifyChange(_uri, null);
           return _uri;
      }
      throw new SQLException(&quot;Failed to insert row into &quot; + uri);
    

}

@Override public Cursor query(Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder) { String[] columns = new String[] { “id”, “city”, “street”, “country”}; return db.query(DATABASE_TABLE, columns, null, null, null, null, null); } [/sourcecode]

The implementation of insert() and delete simply uses the db instantiated in onCreate() to insert/fetch the record. The insert() then notifies other users using the content provider about the insertion of a row in the table.

This content provider resembles a service in Service Oriented Architecture(SOA). For this we need to change the AndroidManifest.xml. This is just like publishing an wsdl for web services. Add the content provider as a provider in AndroidManifest.xml.

[sourcecode language="xml"] <application android:icon="@drawable/icon" android:label="@string/app_name"> <activity android:name=".AddressProviderActivity" android:label="@string/app_name1"> ... </activity> <provider android:name="AddressProvider" android:authorities="com.example.AddressProvider" /> </application> [/sourcecode]

Suppose any Activity wants to access the address db through this service provider. The Activity can use getContentResolver() method to get the handle to the provider. To insert a row from an activtiy we just need to call the insert() using getContentResolver().

[sourcecode language="java"] private void insert(AddressDAO addressDAO) { ContentValues values = new ContentValues(); values.put("city", "Gurgaon"); values.put("street", "sector 14"); values.put("country", "India"); getContentResolver().insert(AddressProvider.CONTENT_URI, values); } [/sourcecode]

Same is the case for fetching the data.

[sourcecode language="java"] private void get(AddressDAO addressDAO) { Cursor c = getContentResolver().query(AddressProvider.CONTENT_URI, null, null, null, null); if (c.moveToFirst()) DisplayAddress(c); else Toast.makeText(this, "No address found", Toast.LENGTH_LONG).show(); }

public void DisplayAddress(Cursor c) { final EditText editText = (EditText) findViewById(R.id.EditText01); editText.setText(new String(c.getString(2))); Toast.makeText(this, "id: " \+ c.getString(0) + "\n" \+ "ISBN: " \+ c.getString(1) + "\n" \+ "TITLE: " \+ c.getString(2) + "\n" \+ "PUBLISHER: "\+ c.getString(3), Toast.LENGTH_LONG).show(); } [/sourcecode]

   [1]: http://xebee.xebia.in/2010/08/16/database-implementation-in-android/

## Comments

**[Paritosh Ranjan](#3074 "2010-10-28 10:44:45"):** The open method was a utilitiy created in AddressDAO. DatabaseHelper extends SQLiteOpenHelper which has a method getWritableDatabase().

**[Sandeep](#3069 "2010-10-27 17:02:05"):** /* public boolean onCreate() { 10 DatabaseHelper dbHelper = new DatabaseHelper(getContext()); 11 db = dbHelper.getWritableDatabase(); 12 return (db == null) ? false : true; 13 } */ Change the line number 11. call dbHelper.open() instead of dbHelper.getWritableDatabase()

**[Rob Miles](#6159 "2011-11-09 16:26:59"):** Hey Paritosh Ranjan, this is great article thanks for sharing with us. here is another nice article on content provider in android application.... http://mindstick.com/Articles/70072812-e6d0-41e4-89ab-da670d747a44/?Content%20Provider%20in%20Android which explain very nice. Thanks!!

