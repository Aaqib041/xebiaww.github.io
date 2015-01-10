---
layout: post
header-img: img/default-blog-pic.jpg
author: ankitk
description: 
post_id: 3917
created: 2010/06/29 08:49:55
created_gmt: 2010/06/29 03:49:55
comment_status: open
---

# Soap web service call in Android

Recently I started using the [Android SDK][1] and came across a requirement to make a [SOAP][2] based web service call on the Android platform. In this blog I will explain how this can be implemented.

Android SDK does not contain any framework/library for making SOAP calls hence we are left with either messing with XML and HTTP call or searching for an open-source library which does this for us.

**[kSOAP 2][3]** is one such light weight Soap communication framework that handles the XML and HTTP work in the background.

**[Download kSOAP 2 jar][4]**

If you are building your project with eclipse you need to add the jar file ksoap2-android-assembly-2.4-jar-with-dependencies in the build path.

**Maven dependency** [java] <dependency> <groupId>com.google.code.ksoap2-android</groupId> <artifactId>ksoap2-android</artifactId> <version>2.4</version> </dependency>[/java]

**Code Snippet for making a Soap call in Android** [java]package com.dummy.soap.android;

import org.ksoap2.SoapEnvelope; import org.ksoap2.serialization.SoapObject; import org.ksoap2.serialization.SoapSerializationEnvelope; import org.ksoap2.transport.AndroidHttpTransport;

import android.app.Activity; import android.os.Bundle; import android.view.View; import android.view.View.OnClickListener; import android.widget.Button; import android.widget.EditText; import android.widget.TextView;

public class DummyAppAndroid extends Activity { private static final String SOAP_ACTION = "getSuggestionResult"; private static final String METHOD_NAME = "getSuggestionResult"; private static final String NAMESPACE = "http://www.dummynamespace.com/MyNameSpace"; private static final String URL = "http://192.168.2.10:8080/DummyApp/services/DummyApp?wsdl"; private SoapObject resultRequestSOAP = null;
    
    
    /** Called when the activity is first created. */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        TextView tv = new TextView(this);
        setContentView(R.layout.main);
        Button cmd_submit = (Button) findViewById(R.id.widget35);
        cmd_submit.setOnClickListener(new OnClickListener() {
    
            @Override
            public void onClick(View arg0) {
                AndroidHttpTransport androidHttpTransport =
                     new AndroidHttpTransport(URL);
                try {
                    /** Get what user typed to the EditText. */
                    String searchNameString =
                           ((EditText) findViewById(R.id.widget42))
                            .getText().toString();
    
                    SoapObject request = new SoapObject(NAMESPACE,
                           METHOD_NAME);
                    // Add the input required by web service
                    request.addProperty(&quot;input&quot;, searchNameString);
                    SoapSerializationEnvelope envelope =
                           new SoapSerializationEnvelope(SoapEnvelope.VER11);
                    envelope.setOutputSoapObject(request);
                    // Make the soap call.
                    androidHttpTransport.call(SOAP_ACTION, envelope);
    
                    // Get the SoapResult from the envelope body.
                    resultRequestSOAP = (SoapObject) envelope.bodyIn;
    
                    SoapObject nameResult = (SoapObject) resultRequestSOAP
                            .getProperty(0);
                    int count = nameResult.getPropertyCount();
                    StringBuilder stringBuilder = new StringBuilder();
                    /**
                     * Retrieve one property from the complex SoapObject
                     * response
                     */
                    for (int i = 0; i &amp;lt; count - 1; i++) {
                        SoapObject simpleSuggestion = (SoapObject) nameResult
                                .getProperty(i);
                        stringBuilder.append(simpleSuggestion.getProperty(
                                &quot;representation&quot;).toString());
                        stringBuilder.append(&quot;\n&quot;);
                    }
                    String temp = stringBuilder.toString();
    
                    /** Show the suggestions in a text area field called
                    * lblStatus */
                    ((TextView) findViewById(R.id.widget32)).setText(temp
                            .toString());
                } catch (Exception aE) {
                    System.out.println(aE.toString());
                    aE.printStackTrace();
                }
            }
        });
    }
    

}[/java] **Modifications in AndroidManifest.xml **In order to make soap calls on a web service exposed on the internet, additional permissions need to be added. [java] <uses-permission android:name="android.permission.INTERNET"> </uses-permission> [/java] 

**Parsing the response received** The best way to figure out how to retrieve the attributes (and properties) is to put a breakpoint straight where you get the raw response from httpclient in your code. Then you can just browse through the structure with your debugger and try retrieving the values you are looking for by chaining getProperty(“somename”) and getAttribute(“someothername”) the way you need. You will often need to access parts of the structure by getting some property as a SoapObject and then e.g. looping through its properties and retrieving the attributes from each.

   [1]: http://developer.android.com/guide/basics/what-is-android.html
   [2]: http://en.wikipedia.org/wiki/SOAP
   [3]: http://ksoap2.sourceforge.net/
   [4]: http://code.google.com/p/ksoap2-android/downloads/detail?name=ksoap2-android-assembly-2.4-jar-with-dependencies.jar&can=2&q=

## Comments

**[Tejas Mehta](#125 "2010-08-27 12:38:32"):** This working perfectly fine. But i want to call a soap web service with https protocol. Can you have any idea regarding how to call soap https web service in android..

**[Gábor Auth](#3038 "2010-10-19 20:39:27"):** Hi, I’ve created a new SOAP client for the Android platform, it is use a JAX-WS generated interfaces, but it is only a proof-of-concept yet. If you are interested, please try the example and/or watch the source: http://wiki.javaforum.hu/display/ANDROIDSOAP/Home :)

**[Ankit Kumar](#3220 "2010-11-12 16:40:02"):** Hi Kishore Did you try creating a soap request as explained in the blog above if yes then what is the exception/error/problem you see. From the soap request published in the comment I cannot make out the problem. Can you give some more details around the problem so that I could be of help. Regards Ankit

**[kishore](#3199 "2010-11-11 10:49:44"):** How should I call this Soap Webservice from an android string string string int int string string string string string string string string string int string boolean boolean string string The class clsAddCarRequest as 2 properties clsCredentials x, clsCar c; clsCredentials x; x.getXML returns the below string string string clsCar c; c.getXML returns the below int int string string string string string string string string string int string boolean boolean string string clsAddCarRequest myReq; myReq.getXML returns the below string string string int int string string string string string string string string string int string boolean boolean string string How do I call the SoapObject request = new SoapObject the webservices returns string

**[kishore](#3200 "2010-11-11 10:53:19"):** How should I call this Soap Webservice from an android i deleated the" clsAddCarRequest> clsCredentials> EmailAddress>string/EmailAddress> Password>string/Password> TokenID>string/TokenID> /clsCredentials> clsCar> CarID>int/CarID> UserID>int/UserID> RegisteredOwner>string/RegisteredOwner> Make>string/Make> Model>string/Model> Color>string/Color> BodyType>string/BodyType> PlateType>string/PlateType> PlateNumber>string/PlateNumber> State>string/State> VINNumber>string/VINNumber> YearOfRegistration>int/YearOfRegistration> ExpiryDate>string/ExpiryDate> IsAutoFightOn>boolean/IsAutoFightOn> IsDeleted>boolean/IsDeleted> DateAddedTimeStamp>string/DateAddedTimeStamp> LastUpdatedTimeStamp>string/LastUpdatedTimeStamp> /clsCar> /clsAddCarRequest> /AddCar> The class clsAddCarRequest as 2 properties clsCredentials x, clsCar c; clsCredentials x; x.getXML returns the below clsCredentials> EmailAddress>string/EmailAddress> Password>string/Password> TokenID>string/TokenID> /clsCredentials> clsCar c; c.getXML returns the below clsCar> CarID>int/CarID> UserID>int/UserID> RegisteredOwner>string/RegisteredOwner> Make>string/Make> Model>string/Model> Color>string/Color> BodyType>string/BodyType> PlateType>string/PlateType> PlateNumber>string/PlateNumber> State>string/State> VINNumber>string/VINNumber> YearOfRegistration>int/YearOfRegistration> ExpiryDate>string/ExpiryDate> IsAutoFightOn>boolean/IsAutoFightOn> IsDeleted>boolean/IsDeleted> DateAddedTimeStamp>string/DateAddedTimeStamp> LastUpdatedTimeStamp>string/LastUpdatedTimeStamp> /clsCar> clsAddCarRequest myReq; myReq.getXML returns the below clsAddCarRequest> clsCredentials> EmailAddress>string/EmailAddress> Password>string/Password> TokenID>string/TokenID> /clsCredentials> clsCar> CarID>int/CarID> UserID>int/UserID> RegisteredOwner>string/RegisteredOwner> Make>string/Make> Model>string/Model> Color>string/Color> BodyType>string/BodyType> PlateType>string/PlateType> PlateNumber>string/PlateNumber> State>string/State> VINNumber>string/VINNumber> YearOfRegistration>int/YearOfRegistration> ExpiryDate>string/ExpiryDate> IsAutoFightOn>boolean/IsAutoFightOn> IsDeleted>boolean/IsDeleted> DateAddedTimeStamp>string/DateAddedTimeStamp> LastUpdatedTimeStamp>string/LastUpdatedTimeStamp> /clsCar> /clsAddCarRequest> How do I call the SoapObject request = new SoapObject the webservices returns AddCarResponse xmlns="http://abcd.com/"> AddCarResult>string/AddCarResult> /AddCarResponse>

**[Ankit Kumar](#207 "2010-08-30 08:58:15"):** Hi Tejas, Did you try just putting https based URL in the above code? If not then please try it. If this does not work then you could try doing it the way described on this link. http://devcentral.f5.com/Tutorials/TechTips/tabid/63/articleType/ArticleView/articleId/102/The-Minimum-Steps-to-utilize-KSOAP2.aspx Regards Ankit

**[Harshal](#3104 "2010-10-31 18:32:46"):** Hello Ankit Can u just help me, I am stuck at some point. I have used two methods in my webservice. First for login and second for fetching data f d user who has logged in. My login part is working fine. Call to the second webservice is also working. Values get extracted and can be printed(as print statements are written in web service code) but new soap object which I am using to put data into in from new envelope is not having data which it should have, It's showing data which was there in soap object 1(of login method) , Don't know where I am getting wrong. If you can figure it out from this, den good, otherwise I will post my code soon.

**[khalil](#4202 "2010-12-19 15:09:06"):** Hi Ankit, I am getting soap response like that. getRatingResponse{return=ContestInfo{item=anyType{name=Ankitha; totalimages=2; rating=2.5; }; item=anyType{name=Anushka; totalimages=4; rating=9.5; }; item=anyType{name=Apsara; totalimages=1; rating=0; }; ). how to parse it ? and is there something missing for getting correct response which comes in XML format. Plz help me.

**[judi](#402 "2010-08-31 12:22:31"):** stringBuilder.append(simpleSuggestion.getProperty( "representation").toString()); Error : NullPointerException

**[PM Patel](#408 "2010-09-01 11:39:32"):** Hello Ankit, i found this article really helpful, but let me ask "is there any other way to have SOAP Webservice CALL in ANDROID?"..pls help me

**[Karthik](#466 "2010-09-02 20:29:27"):** That was fabulous Ankit!! Thank you so much for a wonderfully written article. I have been searching for this.

**[Raghav Rajagopalan](#5353 "2011-03-17 17:34:45"):** Hi all!! I am Raghav. I am new to android development. I have a query to be solved. Sub: connect to server using web service in Android. My query is: I have a login screen created. I have 2 fields username and password. When user enters username and password, i need to create a web service to validate user input(User name and password). If the user has entered the wrong input show error message. In case of correct input move to next screen. I need a code for this query. Please i tried a code but its not working fine. Thanks in advance. Regards, Raghav Rajagopalan

**[Chirag](#4834 "2011-01-10 11:29:20"):** Hi ankit, I have try your code for call my webservice in android app as per your instaction but , after below line my app was stop androidHttpTransport.call(SOAP_ACTION, envelope); please help me.

**[Ankit Kumar](#4841 "2011-01-10 18:31:38"):** What is the soap response that is generated from the web service through another client ? I guess there might be a problem in the request sent out. Can you try to debug the application and see until what stage does it process. Regards Ankit

**[sivachandran](#5259 "2011-02-04 15:55:15"):** hi i am using same concpet in my program .. but i am getting request time out exception.. could you please help me to solve this issue..

**[Zion](#5960 "2011-09-30 11:01:37"):** Hi, Can you send me please all the project? Unfotunetly there are errors when I copy and paste it. Thanks

**[arun](#6008 "2011-10-11 18:07:23"):** Hi after getting the response how to parse can u post the code

**[Prashant](#6028 "2011-10-17 09:44:58"):** Hi Ankit, I want to use authentication at the client side.how can i use this service at the client side?

**[mamatha](#6079 "2011-10-31 14:08:28"):** i done webservices in asp.net.I want to pass that webservices in android.i dont know how to pass and which webservices i want to use.please help me

**[Virginia](#6353 "2011-12-07 19:41:08"):** Hi! Please, Could you post the complete example? :)

**[Rahul Kumar](#8162 "2012-03-31 13:03:57"):** Hi where i have to put the maven dependency in ecillpse.. Please Let me Know . I am waiting for your reply.

**[Rooban Ponraj](#8476 "2012-04-17 12:41:23"):** Your post helped me lot Thank you :)

**[Rohit](#9096 "2012-06-25 19:00:50"):** i have used the SOAP for calling web service in android.It works fine for login module.But after the login if i want to get list of Items i m getting problem i m getting respose in showdialog.I have not passed any filter parameter also. So it will get all rows from the server.But i m getting response.getPropertyCount() = 1 even though i have 5 rows in database.May i send my complete code with respose to your mail id? May i know ur Mail id? my mail id is javahunterand@gmail.com. Please help me i m getting problem from 3 days.

**[Rahul](#8142 "2012-03-30 23:00:19"):** Where i Have to put this com.google.code.ksoap2-android ksoap2-android 2.4 Also Iam getting Unexpected error when i run the application

