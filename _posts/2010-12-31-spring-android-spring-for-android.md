---
layout: post
header-img: img/default-blog-pic.jpg
author: ykapoor
description: 
post_id: 7269
created: 2010/12/31 23:16:13
created_gmt: 2010/12/31 18:16:13
comment_status: open
---

# Spring-Android: Spring for Android!!!

With an aim to ease the development of android applications [Spring-Android][1], an extension to Spring framework is released.The framework with it's first release brings in RestTemplate and commons-logging support for the Android based applications.

This blog is the result of my experience while using the Spring-Android framework in one of the android applications that I am currently working on.This post mainly focuses on the steps involved to get it working.

The sample Webservice in this scenario has a method defined to handle GET requests that return all the Projects presently configured along with certain details and an array of issues. The class is as below:

``` 
 public class Project { private String name; private Integer id; private String owner; private Issue[] issues; … //getter and setters for all … } 
 ```

On a Web-service _GET_ request, the project data is retrieved in JSON representation. A request to get all projects configured in the system gets back with the response below: 

``` 
 [{"class":"com.xyz.Project","id":1,"issues":[],"name":"TestProject0","owner":"Test0"}, {"class":"com.xyz.Project","id":2,"issues":[],"name":"TestProject1","owner":"Test1"}, {"class":"com.xyz.Project","id":3,"issues":[],"name":"TestProject2","owner":"Test2"}] 
 ```

That completes the Rest service scenario description. Now let's find out how to access it in an Android application using Spring-Android's RestTemplate.

With my basic eclipse Android project already in place, I just created a lib folder so as to include Spring-Android's jar. You can download the jar files from [here][2]

Specifically, I have included spring-android-rest-template-1.0.0.M1.jar and spring-android-commons-logging-1.0.0.M1.jar. Apart from these two, we need to have commons-httpclient 3.x jar. This jar is required for using Spring-Android's RestTemplate. Android supports commons-httpclient 4.x. and Spring-Android's RestTemplate may support this in future.

Please note that we have to **include the Jars from the lib folder to the Project's class path** for the Dex compilation process.

Now, with the basic setup done, let's move over to our simple Android Activity that makes use of the Spring-Android's RestTemplate.

[sourcecode language="java" highlight="7,8,9,10"] public class DefaultActivity extends Activity { /*_ Called when the activity is first created. _/ @Override public void onCreate(Bundle savedInstanceState) { super.onCreate(savedInstanceState);
    
    
    RestTemplate restTemplate = new RestTemplate();
    restTemplate.setRequestFactory(new CommonsClientHttpRequestFactory());
    String url = &quot;http://192.168.1.146:8080/grailsRestWS/project/&quot;;
    String data = restTemplate.getForObject(url, String.class);
    setContentView(R.layout.main);
    }
    

[/sourcecode]

Let's try to understand the code given above

**Line 7:** It creates a restTemplate object. The RestTemplate aides Restful communication with the HTTP servers.

**Line 8:** The restTemplate is assigned a requestFactory. RequestFactory is used for creating HttpClientRequest representing a client side http request.

**Line 9:** The url used is not localhost or the loop back address 127.0.0.1. You might have guessed it already. The application is deployed onto a device or an Android emulator. It is the device that takes the localhost IP not the development machine. To resolve this, I have used the IP address as per my machine's network configuration.

**Line 10:** It is a rest call to the web-service url. Please notice that it is a HTTP GET request as indicated by the name getForObject(). I have specified the String.class as the second argument to the method which means the data returned is to be interpreted as String data.

The method supported by the **RestTemplate** are listed below [code language="java" light="true"] HTTP RestTemplate DELETE delete(String, String...) GET getForObject(String, Class, String...) HEAD headForHeaders(String, String...) OPTIONS optionsForAllow(String, String...) POST postForLocation(String, Object, String...) PUT put(String, Object, String...) 
 ``` But, when I try to run this code I get an exception

[sourcecode language="java" light="true"] no suitable HttpMessageConverter found for response type [com.xebia.android.Project] and content type [application/json] [/sourcecode] That's because we are not dealing with text data here but JSON data type

Making this work requires few changes to our activity class. Below are the changes to be made [sourcecode language="java"] public class DefaultActivity extends Activity { /*_ Called when the activity is first created. _/ @Override public void onCreate(Bundle savedInstanceState) { ... RestTemplate restTemplate = <strong>getRestTemplate()</strong>; JSONData[] jsonData = restTemplate.getForObject(url,ProjectData[].class); ... }
    
    
    private RestTemplate getRestTemplate() {
        RestTemplate restTemplate = new RestTemplate();
        restTemplate.setRequestFactory(new CommonsClientHttpRequestFactory());
        &lt;strong&gt;MappingJacksonHttpMessageConverter jsonConverter = 
                                new MappingJacksonHttpMessageConverter();
        List&lt;MediaType&gt; supportedMediaTypes = new ArrayList&lt;MediaType&gt;();
        supportedMediaTypes.add(new MediaType(&quot;application&quot;,&quot;json&quot;));
        jsonConverter.setSupportedMediaTypes(supportedMediaTypes);&lt;/strong&gt;
    
        List&lt;HttpMessageConverter&lt;?&gt;&gt; listHttpMessageConverters = restTemplate
                .getMessageConverters();
        restTemplate.setMessageConverters(listHttpMessageConverters);
        return restTemplate;
    }
    

[/sourcecode]

To handle the JSON array data the response should be understandable by the restTemplate.

As such, in the code above I have registered a ** MappingJacksonHttpMessageConverter **with the restTemplate so as to read and write JSON data. 

Also, we need to convey what sort of data our converter can handle e.g. text, xml, jpeg, json etc. This is done by defining a supported **MediaType** which in our case is MediaType("application","json")

You may have noticed that the second argument to the getForObject method in the code above is changed from String.class to ProjectData.class. This is because the messageCoverters for JSON and the mediaType which our converter can handle have been implemented. We can now safely state that the response from the web service is an array of ProjectData.

Here's our **Android client **side class representing Project data

``` 
 @JsonIgnoreProperties( {"class" }) public class JSONData { private String name; private Integer id; private String owner; private Issue[] issues; … //getter and setters for all … } 
 ```

here, JsonIgnoreProperties annotation conveys the JSON mapper that these are the properties to be excluded. The JSON response contains a class variable which I am ignoring in the code above. [sourcecode language="java"] {"class":"com.xyz.Project","id":1,"issues":[],"name":"TestProject0","owner":"Test0"} [/sourcecode]

Please note that issues property is declared as an array. This is because with JSON it's easier to work with arrays.

Note: I was getting **java.lang.VerifyError ** when I tried to run the application in my environment and after including jackson-core-asl-1.6.1.jar and jackson-mapper-asl-1.6.1.jar everything seems to be working fine.

   [1]: http://www.springsource.org/spring-android
   [2]: http://www.springsource.com/download/community

## Comments

**[Wilfred Springer](#4709 "2011-01-05 12:12:28"):** Hi Yogesh, What is not totally clear to me yet is to what extent any of this depends on the core Spring libraries. I could easily imagine that depending on full Spring would be impossible, because of Spring's memory footprint. At TomTom, we worked on a tiny implementation of Spring as well, in the past. You can find all about it here: http://springframework.me/. It basically provides Spring dependency injection to Java platforms normally not supported, because of limitations in the runtime. (Like no support for reflection, or limited resources.) I would love to see if we could sort of combine it. I have seen references to people using Spring ME on Android. In all honesty, there hasn't been any real work on Spring ME in two years, but the basic idea still holds, which is that using Spring ME you can have Spring dependency injection with a 0kb runtime footprint.

**[Jojo](#4710 "2011-01-05 12:18:54"):** That's just what I need on a phone...a better logging framework! Methinks someone's head is stuck in server-land. For God's sake, it's a phone! Why would I want either logging (any at all - where will it go?!) or a framework that forces me to write half my code in XML (parsing is not cheap, and structure is at least half of *why* you have objects!!!) on a phone? There are things in life which you could do, but should not. This is one of those.

**[Yogesh Kapoor](#4737 "2011-01-06 11:09:30"):** I fully concur with Keith's thoughts. Even the blog tries to suggest this. Thank you Keith for bringing in more clarity. If you need any further clarifications please let me know.

**[emaggini](#4779 "2011-01-07 09:48:32"):** Wow, This is pretty great thanks for the article. The more I learn about Spring the more I like. :)

**[Keith Donald](#4733 "2011-01-06 08:46:04"):** Guys, Thus far, as a project Spring Android has a pretty simple goal: ensure relevant Spring client libraries work well in an Android environment. This starts with RestTemplate as a popular HTTP client, which is important any Android app that requires the network The commons logging adapter is useful because it allows existing code that depends on the commons logging API to work in an Android environment by delegating to Android's on logging framework underneath. That is all. There's no XML in Spring Android, and we're not trying to change the way people develop Android-based business applications. We simply want to make Spring client libraries accessible to Android developers and want to make it as easy as possible for folks to get started when Android is the right choice for them. Yogesh, thanks for writing this entry and getting the word out about the project! Watch for subsequent milestones to become available on a regular basis. Keith

**[Keith Donald](#4734 "2011-01-06 08:52:45"):** Wilfred, Interesting idea. Where would you recommend the Spring Android team start with a review of Spring ME? Feel free to DM me and we can setup a Skype to discuss your approach and how you could see it being applied in an Android environment. Thanks! Keith Twitter: @kdonald

**[Daniel](#5124 "2011-01-28 07:42:55"):** OK, I read until the end, and it worked fine by adding the two mentioned libraries: jackson-core-asl-1.6.1.jar and jackson-mapper-asl-1.6.1.jar For some reason, I needed to remove spring-android-commons-logging-1.0.0.M1.jar for the project to compile (Dalvik compilation issue). Great tutorial!!!!

**[Daniel](#5100 "2011-01-28 03:46:26"):** Thanks for the tutorial. However, I cannot complete it because I cannot find @JsonJsonIgnoreProperties ...

**[Daniel](#5102 "2011-01-28 03:49:25"):** ok I guess it's a question of adding another library. Will try. Thx

**[Rocky](#5917 "2011-09-15 20:42:33"):** I am getting the following error after adding two jars : jackson-core-asl-1.6.1.jar and jackson-mapper-asl-1.6.1.jar, I am getting following error 09-15 20:59:08.828: INFO/dalvikvm(383): Could not find method org.codehaus.jackson.map.ObjectMapper.getTypeFactory, referenced from method org.springframework.http.converter.json.MappingJacksonHttpMessageConverter.getJavaType anyhelp ?

**[Rocky](#5918 "2011-09-15 20:46:21"):** I am getting the floowing exception after adding the two jars jackson-core-asl-1.6.1.jar and jackson-mapper-asl-1.6.1.jar, exception : INFO/dalvikvm(383): Could not find method org.codehaus.jackson.map.ObjectMapper.getTypeFactory, referenced from method org.springframework.http.converter.json.MappingJacksonHttpMessageConverter.getJavaType

**[Rohit](#9041 "2012-06-15 13:37:19"):** Hi, i m new for Android. i am working in java/j2EE, I have seen there is spring support in Android at client side(Message Convertor,SQLite) , but i am not getting any good resource, So If you have any resource or sample code , can you please send me on my gmail id? my gmail id is javahunterAnd@gmail.com

