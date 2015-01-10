---
layout: post
header-img: img/default-blog-pic.jpg
author: dbansal
description: 
post_id: 18584
created: 2014/07/09 21:58:43
created_gmt: 2014/07/09 16:58:43
comment_status: open
---

# JSONPATH like Navigation in R for JSON Data

<p>According to Wiki:
<blockquote>R is a free software programming language and software environment for statistical computing and graphics.</blockquote>
However, before any analysis, the input data should be restructured. If the input data is JSON, this restructring becomes very complex and time-consuming. But, if we get some mechanism to navigate through that JSON data using JSONPATH mechanism, it becomes, all the more easy to rearrange JSON data to other formats like table and arrays.</p>
<p><span style="text-align: justify;line-height: 1.5em">Last week, we had an Innovation Day in our organization, rather project, to be more precise. Everyone was working, mostly in groups, on some or the other ideas. I was also working on an idea proposed by my teammate. When it came to technology, we chose to pick my current love of that time, R as it required some kind of statistical analysis.</span>
<p style="text-align: justify">Most of us should agree on the fact that the hardest part in any statistical analysis is data munging. That is, bringing the input data in analyzable format. It becomes even harder when the source data is in JSON. JSON is best supported by data transfer or communication. Rather, we should say, data communication or transfer is best supported by JSON ( or XML ? Ok I agree on both, no hard feelings !). But when it comes to Data Analysis, it is, definitely, not the best format. So the main obstacle that I had was to break this JSON data into separate rows and columns i.e. cells of a table or a matrix or a dataframe.</p>
<span style="font-size: 18px"><strong>JSONLITE:</strong></span>
<p style="text-align: justify">So, a nursery standard googling took me to R's package for JSON, <strong>JSONLITE</strong>: A smarter JSON encoder/decoder for R. This package is a fork of the RJSONIO package by Duncan Temple Lang. More information about this package can be found <a href="http://cran.r-project.org/web/packages/jsonlite/index.html">here</a>. It converts your JSON string into a dataframe or list and vice-versa.</p></p>
<h2><strong>Problem Scenario:</strong></h2>

<p>I will try to simulate the problem using a dummy <a href="http://78.46.48.103/sample/hourly_14.json.gz">JSON data file</a> that I found on <a href="http://openweathermap.org/current#bulk">http://openweathermap.org/current#bulk</a>. Each line of this file contains a JSON record of the form:</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2014/07/Selection_009.png"><img class="alignnone  wp-image-18585" alt="Selection_009" src="http://xebee.xebia.in/wp-content/uploads/2014/07/Selection_009.png" width="438" height="477" /></a>
<p class="none" style="text-align: justify">Each record here shows the weather information of a particular city for every three hours. Each element in the array “<em>Data</em>” contains the weather info for a particular timestamp (number of seconds from epoch). Various data fields in each element of “<em>Data</em>” consists of <em>temp, temp_min, temp_max, pressure, weather description</em> (nested in “<em>Weather</em>” array), <em>clouds</em> data etc.</p></p>
<h2 style="text-align: justify"><strong>Problem Statement:</strong></h2>

<p class="none" style="text-align: justify">For all the cities in the file I had to get a list of <em>temp_min</em> values at <em>dt</em>(timestamp) = “1394863200” (i.e. for datetime -&gt; 2014-03-15 06:00:00). So the JSON structure has to be <strong>drilled down and refined (filtered)</strong> on the basis of a value that persists deep in the structure in an array.</p>

<p class="none" style="text-align: justify">So, we need to use some JSONPATH kind of mechanism to drill down this JSON and filter according to this value. But it is not available (atleast not readily) through any package in R.</p>

<h2><strong>Solution:</strong></h2>

<p class="none">First lets convert this data to an R object.</p>

<p class="none">To convert the above data into an R object, use <em>fromJSON()</em> method from JSONLITE package:</p>

<pre>allCities &lt;- readLines("hourly_14.json")
firstCity &lt;- allCities[[1]]
jsonData&lt;- fromJSON(firstCity)</pre>

<p class="none" style="text-align: justify">Now, we have jsonData object in R which contains all the information about the weather of first city in the file. We have to apply some JSONPATH like mechanism on this object to fetch out the precise information from the whole JSON object. JSONLITE, which is wonderful in creating the R object, however, does not provide any mechanism to go inside the document using JSONPATH. So it is the responsibility of R's code to dive into this JSON object to fetch the relevant information denoted by the JSONPATH.</p>

<p class="none" style="text-align: justify">You can use the following custom (read basic) method to get the relevant field by passing the json string and the JSONPATH. It first converts the jsonString into an object in R and then applies the JSONPATH like navigation to that object using jsonPath (the second argument) .</p>

<pre>drillDownJSON &lt;- function (jsonString, jsonPath) {
  jsonData &lt;- fromJSON(jsonString, flatten = T)
  out &lt;- tryCatch({
    for(ele in jsonPath) {
      if (class(ele) == "list") {
        jsonData &lt;- jsonData[[unlist(ele)]]
      }
      else {
        jsonData &lt;- subset(jsonData, eval(parse(text=ele)))
      }
    }
  unname(jsonData)
  }, error = function(cond) {
    print(cond)
    return (NA_character_)
  })
  if (is.null(out))
    return (NA_character_)
  else return (out)
}</pre>

<p>Here:
<em>jsonString -&gt; character : the JSON document</em>
<em>jsonPath -&gt; list of (list and strings) : this argument specifies the JSONPATH to search for.</em></p>
<p>The caveat here is that the second argument should be passed in a specific format viz.,<em> </em>
<ul>
    <li><span style="line-height: 1.5em">a list with each element either a list of strings or a single string. </span></li>
    <li><span style="line-height: 1.5em">if it is a list of strings( i.e. the list of json keys) then it will be used to navigate into the document </span></li>
    <li><span style="line-height: 1.5em">if it is a string then it should be a condition which selects one element from array.</span></li>
    <li>obviously the order of elements in the list should be according to the json structure.</li>
</ul>
<p style="text-align: justify">In our example, to get the <em>temp_min</em> values at <em>dt</em>(timestamp) = “1394863200” (i.e. for datetime -&gt; 2014-03-15 06:00:00), we would pass the second argument as:</p></p>
<pre>list(list("data"),"dt == 1394863200", list("main","temp_min"))</pre>

<p><span style="line-height: 1.5em">It says:</span>
<ul>
    <li><span style="line-height: 1.5em">From root of the document, go inside "<em>data</em>" field which is an array.</span></li>
    <li><span style="line-height: 1.5em">Select element from the array which has <em>dt</em>'s value as 1394863200.</span></li>
    <li><span style="line-height: 1.5em">Now in that go inside  field "<em>main"</em>.</span></li>
    <li><span style="line-height: 1.5em">At last go inside "<em>temp_min"</em> and return its value.</span></li>
</ul>
Call to this method for our specific example would be:
<pre>drillDownJSON(jsonString = firstCity, jsonPath = list(list("data"),"dt == 1394863200", list("main","temp_min")))</pre>
So it returns the <em>temp_min</em> for <em>firstCity</em> at time: “2014-03-15 06:00:00”.</p>
<p>To get the list of these values for all the cities, we can use <em>sapply </em>function of R:
<pre>Y &lt;- sapply(x, drillDownJSON, list(list("data"),"dt == 1394863200", list("main","temp_min")))
temp_min_list &lt;- unname(y)</pre>
In <em>temp_min_list</em> we have the list of the <em>temp_min</em> values for all the cities.
<h2><strong>Conclusion</strong></h2>
<p class="none" style="text-align: justify">Using JSONLITE, we can convert a JSON data to an R object and wrap it to write a custom method like above to easily mock a JSONPATH like interface which you can use for other JSON objects. The above method is a very basic implementation which can be extended further to have more drill down and navigational features.</p></p>