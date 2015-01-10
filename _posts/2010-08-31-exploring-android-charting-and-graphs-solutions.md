---
layout: post
header-img: img/default-blog-pic.jpg
---

# Exploring Android Charting and Graphs solutions

In one of the Android applications that I was creating the requirement was to have graphs and charts to present the data in a meaningful way to the user. With no direct charting API support in Android to address graphs and charts creation, I explored few other possible solutions. This blog does not focus on comparing the available solutions but demonstrates the simple implementations using the approaches explored. I considered three different ways to create charts in Android device 1\. [Google chart API](http://code.google.com/apis/chart/) 2\. [Achartengine](http://www.achartengine.org/) 3\. [ChartDroid](http://code.google.com/p/chartdroid/) I tried creating a Pie Chart while using the solutions. Google charting API being the easiest one to integrate is taken first 

#### Google Charting API support

Using Google charting API, we can create charts and graphs directly by providing data to the Google charting service The call to the Google's charting API looks like below: [code language="xml"]http://chart.apis.google.com/chart?cht=&amp;chd=&amp;chs=&amp;...additional_parameters...[/code] More on Google charting API can be found [here](http://code.google.com/apis/chart/docs/making_charts.html) For creating a pie chart using Google charting solution, I created the following URI [code language="xml"]http://chart.apis.google.com/chart?cht=p3&amp;chd=t:30,60,10&amp;chs=250x100&amp;chl=cars|bikes|trucks[/code] Within my activity's onCreate() I included the code below [code language="java"]@Override public void onCreate(Bundle savedInstanceState) { super.onCreate(savedInstanceState); WebView googleChartView = new WebView(this); setContentView(googleChartView); String mUrl = "http://chart.apis.google.com/chart?cht=p3&amp;chd=t:30,60,10&amp;chs=250x100&amp;chl=cars|bikes|trucks"; googleChartView.loadUrl(mUrl); }[/code] In the onCreate() method, I am creating a WebView and just loading it with the URL response. It needs an internet access so I included the code below within AndroidManifest.xml [code language="xml"] <uses-permission android:name="android.permission.INTERNET" /> [/code] That's it!!! we are done with the first approach. The screen shot for the application I build using Google chart API looks like below: [caption id="attachment_4674" align="alignnone" width="300" caption="Google Charting API PIE Chart"]![Google Charting API PIE Chart](/wp-content/uploads/2010/08/GoogleAPIChart-300x193.png)[/caption] The only issue I had with this approach is that the device should have internet access available which in my case may not be always there. This small limitation made me look into other approaches and I came across another solution [Achartengine](http://www.achartengine.org/). 

#### Achartengine

This Achart's charting support comes in the form of a download-able [Jar](http://code.google.com/p/achartengine/downloads/list). I added this into the lib folder of my application. [caption id="attachment_4677" align="alignnone" width="247" caption="Android App Structure"]![Library Folder](/wp-content/uploads/2010/08/libFolder.png)[/caption] I created a class with a method that gives out a Pie chart intent as below [code language="java"] public class AChartExample { public Intent execute(Context context) { int[] colors = new int[] { Color.RED, Color.YELLOW, Color.BLUE }; DefaultRenderer renderer = buildCategoryRenderer(colors); CategorySeries categorySeries = new CategorySeries("Vehicles Chart"); categorySeries.add("cars ", 30); categorySeries.add("trucks", 20); categorySeries.add("bikes ", 60); return ChartFactory.getPieChartIntent(context, categorySeries, renderer); } protected DefaultRenderer buildCategoryRenderer(int[] colors) { DefaultRenderer renderer = new DefaultRenderer(); for (int color : colors) { SimpleSeriesRenderer r = new SimpleSeriesRenderer(); r.setColor(color); renderer.addSeriesRenderer(r); } return renderer; } } [/code] Within the execute method a SimpleSeriesRender is created with the required color that is required in the pie chart. These renderers are then added to the DefaultRender. Next step is to create a CategorySeries with the appropriate data. Once this is done we can get achartingengine specific intent back with the following call ` ChartFactory.getPieChartIntent(context, categorySeries, renderer); ` With this intent we launch our activity and the code goes as below [code language="java"] Intent achartIntent = new AChartExample().execute(this); startActivity(achartIntent); [/code] The screen shot below shows the generated PIE chart using Achart library [caption id="attachment_4697" align="alignleft" width="223" caption="Achart"]![](http://xebee.xebia.in/wp-content/uploads/2010/08/Achart2-223x300.png)[/caption] The other charting solution I explored is [chartDroid ](http://code.google.com/p/chartdroid/)

#### Chartdroid

This solution needs the ChartDroid Application to be installed along with the application which intends to use the charting solution. Although, we can install the application programmaticly from the Android apps market place, I directly installed the application into my device to explore it further.To install it download the apk from [here](http://code.google.com/p/chartdroid/downloads/list) and install it to the device. The Chartdroid's charting solution requires a content provider to be created. Below is the content provider's code that I created [code language="java"] public class ChartDroidDataProvider extends ContentProvider { static final String AUTHORITY = "com.xyz.contentprovider.chardroid"; @Override public String getType(Uri uri) { return "vnd.android.cursor.dir/vnd.com.googlecode.chartdroid.graphable"; } public static final Uri PROVIDER_URI = new Uri.Builder().scheme( ContentResolver.SCHEME_CONTENT).authority(AUTHORITY).build(); @Override public Cursor query(Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder) { // Fetch the actual data MatrixCursor c = new MatrixCursor(new String[] { BaseColumns._ID, "COLUMN_AXIS_INDEX", "COLUMN_SERIES_INDEX", "COLUMN_DATUM_VALUE", "COLUMN_DATUM_LABEL" }); c.newRow().add(1).add(0).add(1).add(30).add(null); c.newRow().add(2).add(0).add(1).add(10).add(null);

## Comments

**[Amir](#3111 "2010-11-01 09:33:29"):** thank you very much for your help. I have downloaded it.

**[PM_Android](#3286 "2010-11-19 16:39:07"):** Thanx Yogesh for making my life,,,,,,, this is really an awesome article , actually i was playing and facing trouble with AchartEngine, and then i have tried to create an example from your code section, but failed to do it. Later on, i have downloaded the example code that you have provided ,from the code i came to know that we need to add the following activity to AndroidManifest.xml file, that you have forgot to mentioned above: Now, i got the success !! Thanx

**[Paresh Mayani](#3282 "2010-11-19 12:46:31"):** Really an awesome article you have provided, i have referred the PieChart example using AChartEngine, but please if possible provide a sample for drawing LineChart, i am stack at here

**[Amir](#3086 "2010-10-29 13:10:42"):** hi... Its a Superb tutorial and How to download the entire source code....Im not able to do it... i have registered too..im not able to find an option there...How to do it?

**[MariyaS](#5581 "2011-05-17 18:35:38"):** I want to display the multiple charts in one activity.. that means i want to see that charts one by one using by scrollbars..I used afreechart jar file to generate the charts...... can anyone help me.... Please...... Thanks in advance..

**[Shashank](#5359 "2011-03-22 00:22:06"):** will u plz how 2 draw bar graph using same method Thanx

**[Yogesh Kapoor](#5403 "2011-04-05 19:08:18"):** I don't know if it's useful for you now, as I am too late to respond. If the problem is still not resolved rest of the response might help. You need to make sure that your jar is also dex compiled. So there's an additional step apart from including the jar into the lib folder. In eclipse this automatically happens when we have the Android plugin installed and the **jar is included into eclipse's build path**.

**[Shivakanth](#5357 "2011-03-18 13:42:06"):** I Got it. We have the source @ following shared location: http://code.google.com/p/achartengine/downloads/detail?name=achartengine-0.2.0-demo-source.zip&can=2&q= Thanks a lot for sharing this Article

**[MariyaS](#5580 "2011-05-17 18:33:42"):** I want to display the multiple charts in one activity.. that means i want to see that charts one by one using by scrollbars..I used afreechart jar file to generate the charts...... can anyone help me.... Please...... Thanks in advance...

**[david berneda](#5281 "2011-02-09 14:54:12"):** Hi ! We've just released first beta of TeeChart for Android. https://market.android.com/details?id=com.steema.teechart.android We're working on a simple demo on how to use it from other applications, via Intent, Cursor, ContentProvider and passing simple xml / text data.

**[Diana](#5047 "2011-01-25 16:55:22"):** Hi! Thanx alot for sharing with us.I am going to implement the code.

**[halfhp](#5256 "2011-02-04 12:55:57"):** Great article - nice to see working code examples. Did you happen to come across androidplot during your exploration? (http://androidplot.com) If so, what is your impression of it compared to the ones that you reviewed here?

**[Carling](#5626 "2011-06-19 21:21:12"):** If you are using Achartengine, dont forget to include within the application tag of AndroidManifest.xml file. I had to spend a lot of time for this problem. Thank you Yogesh for this great tutorial.

**[Ankit](#1061 "2010-09-08 20:50:04"):** This was a great tutorial, i'm not sure why your site doesn't come up first when searching for \android charts\\. Thanks,

**[Yogesh Kapoor](#3088 "2010-10-29 16:46:51"):** Hi Amir, I have corrected the source code download link. Give it a try now.

**[Shivakanth](#5356 "2011-03-18 13:10:47"):** this is really an awesome article , actually i was playing and facing trouble with AchartEngine, and then i have tried to create an example from your code section, but failed to do it. We are gettign anerror ststing that we need to add the activity in manifest file. So coul you please provide us the list of activites that needs to be added or could you please provide us the sample working code, So that we can download it.

**[ajay](#3028 "2010-10-14 17:23:24"):** This tutorial has helped me a lot.Great work done. I’m also thinking why your site doesn’t come up first when searching for android charts.

**[xue](#3128 "2010-11-03 14:19:38"):** hi Amir, i wanna known how to draw line chart on achartengine? ths!

**[Shashank](#5358 "2011-03-22 00:19:18"):** Can u tell how to crete bar graphs using same technique Thnx...

**[sathish](#5519 "2011-04-29 14:50:07"):** Hi shilpa, There should be two classes to run the above code. In those, one class should extend Activity, but coming to second one it is not. In the above code you mentioned , 'AChartExample' is a class which is having a method named execute(Context) and it will return an Intent object like follow public class AChartExampe{ \----- \---- private Intent execute(Context c){ \---- \---- Intent i = ChartFactory.getPieChartIntent(context, categorySeries, renderer); return i; } } and there should be another class which is nothing but our activity class, in this class we create an object for AChartExample class and using this object we call the execute(Context) method then we will get an intent and then call startActivity(intent); public class MyActivity extends Activity{ \---- \---- \---- public void userdefinedMethod(){ \--- \--- Intent in= new AChartExample().execute(this); startActivity(in); } }

**[Carling](#5627 "2011-06-19 21:24:08"):** Include activity android:name="org.achartengine.GraphicalActivity" - (for the angular brackets that line is missing in my previous comment.)

**[Shilpa](#5468 "2011-04-14 18:24:39"):** In chartengine solution i am not getting the fallowing step ChartFactory.getPieChartIntent(context, categorySeries, renderer); Intent achartIntent = new AChartExample().execute(this); startActivity(achartIntent); do we need to create seperate class that extends activity for this Please if possible give me details with code i am very mouch in need of it thanks in advance

**[wira](#5372 "2011-03-31 12:24:36"):** Hi, I use AChartEngine, but I'm fail!, My Eclipse can not resolve where is achartengine.jar reside, although I have put it in LIB folder in my project. Is there something that I forgot?

**[saurabh](#5706 "2011-07-11 13:47:37"):** HI Yogesh Excellent tutorial...this must be tagged on the first hit on google i would say.I was wondering is there a way to display the charts in 3 D. Rgds, Saurabh

**[Kazu](#5709 "2011-07-12 05:42:04"):** Thank yo soooooooooo much! Your tutorial saved my life!! :D

**[nik](#5736 "2011-07-20 09:59:42"):** i would like to attach aChartEngine0.6.0 bar chart,column chart in android. can you please provide solution? and one more question are raise in my mind is that for achartEngine0.6.0 , purchase any licence ?

**[Sravanthi](#5984 "2011-10-05 11:21:26"):** It's a very good tutorial in the fresher perspective. It helped me a lot. Thank you.

**[rans](#6117 "2011-11-05 23:40:01"):** thanks alot!!!!! very helpful

**[lerie](#6127 "2011-11-06 21:59:56"):** Im also having a problem linking the library I guess, when I place the Intent code for the new AChartExample it says.. "AChartExample cannot be resolved to a type" my code is.. Intent in = new AChartExample().execute(this); startActivity(in); and it is inside my main activity.

**[lerie](#6128 "2011-11-06 22:02:13"):** Also, when I use the google code for the online stuff, it gives an error saying it chs be at least 1 pixel.. this code was straight copy and pasted from the tutorial.. WebView googleChartView = new WebView(this); setContentView(googleChartView); String mUrl = "http://chart.apis.google.com/chart?cht=p3&chd=t:30,60,10&chs=250x100&chl=cars|bikes|trucks"; googleChartView.loadUrl(mUrl); it does show chs is at 250x100

**[najia](#6425 "2011-12-21 06:55:43"):** Hi Yogesh, I choosed to use google chart but when there is no internet connection,it simply doesn't display the report or throws any error. Can you please let me know.

**[Umar](#6352 "2011-12-07 18:17:54"):** nice tutorial, but google chart which is very simple chart not working at my sitd please try hitting that url and see the error. Thanks,

**[Abhilash](#6389 "2011-12-13 18:43:22"):** while i am running AChartEngine project i am getting error on emulator as "unfortunatly AChartEngine has stopped." please resolve my issue. Thanks in advance...

**[godsmash](#6975 "2012-01-16 12:46:44"):** Hi, First of all, I would like to thank you for this Great tutorial. Secondly, do you have any example of 1- drawing bar chart, and if possible 2- drawing bar chart by retrieving data from database thanks.

**[Fazileh](#7382 "2012-02-05 18:46:47"):** Hi, At first, thank you for this useful tutorial. and second: I used Achartengine, and I have two tab that each tab has to show a chart that their values come from database. My code does not have any error message! But both chart are the same, despite the values are different. I will appreciate if you help me. Thank you

**[Siddhant](#7100 "2012-01-24 14:22:53"):** Nice tutorial.. However, i would like to ask you whether there are any other types of Chart libraries whose source i can download and use (like AChartEngine) which have a more "fancier" or "jazzier" look and feel? Thanks in advance..

**[Swati](#8402 "2012-04-09 11:38:57"):** Great tutorial:-)

**[Swati](#8403 "2012-04-09 11:40:23"):** great tutorial

**[Maidul Ialam](#7897 "2012-03-11 22:27:35"):** Hi Yogesh Kapoor, Hope all is well .Thank you very much for your tutorial sites. I am try to download your source code But I am unable to download.Could you please send me your source code ? I am looking forward to your positive reply. Thanks Maidul Islam

**[Kennie](#9201 "2012-07-20 11:43:13"):** Hi Yogesh Kapoor, Thanks for the tutorial, however when I am running your example codes Achartgraph and ChartDroidgraphs keep force close when I click the button only google chart API is working. Can you please advise me on how to keep it running ? Thanks alot in advance. Kennie

