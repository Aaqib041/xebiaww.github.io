---
layout: post
header-img: img/default-blog-pic.jpg
author: tarunsapra
description: 
post_id: 7592
created: 2011/01/31 11:05:59
created_gmt: 2011/01/31 06:05:59
comment_status: open
---

# Integrate Wolfram|Alpha capabilities in your Application

<p>Wolfram|Alpha is a well known computation knowledge engine. Recently with the release of it's API version 2.0 it's offering free development account with up to 2000 API calls per month. The API is implemented in a standard REST protocol using HTTP GET method.<!--more--> To get started you first need to <a href="http://products.wolframalpha.com/api/libraries.html">download</a> from their site the language binding of choice which is basically the client that you wish to use for querying the Wolfram|Alpha engine. While signing up for a free account you would be asked to provide the name of your application which would be calling the Wolfram|Alpha services and a App ID would be generated, you would need to provide this App ID in your client code. To understand the API it's important to first understand the Wolfram|Alpha output format.
<a href="http://xebee.xebia.in/2011/01/31/integrate-wolframalpha-capabilities-in-your-application/tarun-1/" rel="attachment wp-att-7616"><img src="http://xebee.xebia.in/wp-content/uploads/2011/01/tarun.1.png" alt="logx integration" title="logx integration" width="470" height="253" class="alignleft size-full wp-image-7616" /></a>
<br><br><br><br><br><br><br><br><br><br><br><br><br><br>
The above image is an example of Wolfram|Alpha output for the term 'logx', as you can see from the image the output contains all the relevant mathematical information about 'logx' and the result also contains the plot diagram for logx which was not shown in the above image for brevity reasons.
The output is divided into rectangular regions called pods each of which corresponds to one category of results. Each pod has a title ('Root', 'Derivative' etc) and a content which is a GIF image. Pods have subpods that enclose the actual content, the GIF image. The API provides you the flexibility to request the result in number of formats(Image, HTML, PlainText etc). The returned result is an XML document with the pods/subpods represented in the format as requested by the client code. We will take look at the client code requesting the result in the Image format since the actual content being displayed on the Wolfram|Alpha query results are images. To execute the code you need to have the following jars in your class path
<strong>commons-codec-1.3.jar, httpclient-4.0.1.jar, httpcore-4.0.1.jar, commons-logging.jar </strong> (you can use other versions of these also)
Also the <strong>WolframAlpha.jar</strong> is required which you can download from <a href="http://products.wolframalpha.com/api/libraries.html">here</a>.
Here's the code.
[java]
public class WolframAlphaSample {</p>
<pre><code>// Your APP id:
private static String appid = &amp;quot;XXXXXXXXXXX&amp;quot;;

public static void main(String[] args) {

    //user query
    String input = &amp;quot;integrate logx&amp;quot;;

    // The WAEngine is a factory for creating WAQuery objects,
    // and it also used to perform those queries. You can set properties of
    // the WAEngine (such as the desired API output format types) that will
    // be inherited by all WAQuery objects created from it. Most
    // applications will only need to crete one WAEngine object, which is used throughout
    // the life of the application.
    WAEngine engine = new WAEngine();

    // These properties will be set in all the WAQuery objects created from
    // this WAEngine.
    engine.setAppID(appid);
    engine.addFormat(&amp;quot;image&amp;quot;);

    // Create the query.
    WAQuery query = engine.createQuery();

    // Set properties of the query.
    query.setInput(input);

    try {
        // This sends the query to the Wolfram|Alpha server, gets the XML
        // result and parses it into an object hierarchy held by the WAQueryResult object.
        WAQueryResult queryResult = engine.performQuery(query);

        if (queryResult.isError()) {
            System.out.println(&amp;quot;Query error&amp;quot;);
            System.out.println(&amp;quot;  error code: &amp;quot; + queryResult.getErrorCode());
            System.out.println(&amp;quot;  error message: &amp;quot; + queryResult.getErrorMessage());
        } else if (!queryResult.isSuccess()) {
            System.out.println(&amp;quot;Query was not understood; no results available.&amp;quot;);
        } else {
            // Got a result.
            System.out.println(&amp;quot;Successful query. Pods follow:\n&amp;quot;);
            for (WAPod pod : queryResult.getPods()) {
                if (!pod.isError()) {
                    System.out.println(pod.getTitle());
                    System.out.println(&amp;quot;------------&amp;quot;);
                    for (WASubpod subpod : pod.getSubpods()) {
                        for (Object element : subpod.getContents()) {
                            System.out.println(((WAImage) element).getTitle());
                            System.out.println(((WAImage) element).getURL());
                            System.out.println(&amp;quot;&amp;quot;);
                        }
                    }
                    System.out.println(&amp;quot;&amp;quot;);
                }
            }
            // We ignored many other types of Wolfram|Alpha output, such as
            // warnings, assumptions, etc.
            // These can be obtained by methods of WAQueryResult or objects
            // deeper in the hierarchy.
        }
    } catch (WAException e) {
        e.printStackTrace();
    }
}
</code></pre>
<p>}
[/java]</p>
<p>Be sure to provide your APP id in the above code. From the above code you can see that each WAQueryResult holds the Arrays of pods which are further iterated to extract subpods and contents respectively. In a typical scenario in a web application the above code return the pods objects which can be set into the requestScope and then iterated on the UI. Finally the getTitle() and getURL() methods of the WAImage instance can be used to set the HTML img tag url and the corresponding description.
<code>
[html]
&lt;h2&gt;${waImage.title}&lt;/h2&gt;
&lt;img src=&quot;${waImage.url}&quot; alt=&quot;${waImage.title}&quot; /&gt;
[/html]
</code>
Thus you can now place the Wolfram|Alpha search results on specific parts of your web-page and not just images but you can also request 'plaintext' and 'html' by specifying the same in the client code.</p>