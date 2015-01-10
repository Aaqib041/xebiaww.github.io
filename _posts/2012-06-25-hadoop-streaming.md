---
layout: post
header-img: img/default-blog-pic.jpg
author: vgupta
description: 
post_id: 14571
created: 2012/06/25 18:31:05
created_gmt: 2012/06/25 13:31:05
comment_status: open
---

# Hadoop Streaming

<p>A basic template for writing a Hadoop job using Java is
<ol>
    <li>Write a Mapper class.</li>
    <li>Write a Reducer class.</li>
    <li>Write a Driver code to configure and run the job.</li>
</ol>
<div>However, logic (mapper and reducer logic) for some jobs can be programmed using unix commands. Hadoop streaming, a contrib package provides a way to use unix commands as mappers and reducers. In this blog, I will demonstrate running a Hadoop job using Hadoop streaming.</div>
<!--more--><!--more-->
<div></div>
<div><strong>Hadoop Streaming</strong></div>
<div></div>
<div>Streaming API of Hadoop is an API to write map and reduce functions in languages other than Java. It uses Unix standard streams as the interface between Hadoop and your program, so you can use any language that can read standard input and write to standard output.</div>
<div></div>
<div>Map input data is passed over standard input to your map function, which processes it line by line and writes lines to standard output. A map output key-value pair is written as a single tab-delimited line.  The reduce function reads lines from standard input, which the framework guarantees are sorted by key, and writes its results to standard output.</div>
<div></div>
<div>Apart from standard unix commands, we can also use languages like Ruby and Python to program map and reduce functions.</div>
<div></div>
<div></div>
<div></div>
<div><strong>&nbsp;</p>
<p></strong><strong> </strong></p>
<p></div>
<div><strong>Example</strong></div>
<div></div>
<div>The prerequisite for running this example are</div>
<div>
<ol>
    <li><a href="http://hadoop.apache.org/common/docs/r0.20.2/quickstart.html#Installing+Software">Hadoop installation</a></li>
    <li>Data set, which can be downloaded from <a target="_blank" href="http://www.nber.org/patents/">National Bureau of Economic Research</a>. The file that needs to be downloaded is <a href="http://data.nber.org/patents/Cite75_99.zip">Pairwise Citations</a>. This file includes all US patent citations for utility patents granted in the period 1-Jan-75 to 31-Dec-99. More information about the data set is available at <a href="http://data.nber.org/patents/Cite75_99.txt">http://data.nber.org/patents/Cite75_99.txt</a>.</li>
</ol>
<div>The two commands that we will use in this example are <strong>cut </strong>and <strong>uniq</strong>. <a href="http://www.computerhope.com/unix/ucut.htm">Cut </a>command is used to cut out specified fields of a line, whereas <a href="http://www.computerhope.com/unix/uuniq.htm">Uniq</a> command is used for filtering out repeated lines from a file.</div>
<div></div>
<div></div>
<div>Hadoop job that we will execute will give a list of all the unique cited patent ids. To run the job, please execute the following command from the hadoop distribution folder</div>
<div></div>
<code></code></p>
<p><code> </code>
<div><code>bin/hadoop jar contrib/streaming/hadoop-*-streaming.jar</code></div>
<code>
<div>-input &lt;input path&gt;</div>
<div>-output &lt;output path&gt;</div>
<div>-mapper 'cut -f 2 -d ,'</div>
<div>-reducer 'uniq'</div>
</code><code> </code></p>
<p><code></code><code></code><code></code><code></code><code></code><code></code><code></code>Let's discuss the above command in detail. Since, Hadoop Streaming is a contrib package, it's jar placed in the contrib folder of the Hadoop distribution. -input and -output are standard hadoop attributes to specify the locations of input and output locations. In case of Hadoop streaming, mapper and reducer are unix commands which operate on each line of input.</p>
<p>The mapper expression will split each line of input based on comma(,) and pick the second element of the splitted result. So, the output of the mapper is cited patent id in each line. Now, the job of the reducer is to simply remove the duplicates and getting us the desired output.
<div></div>
<div></div>
<div></div>
<div></div>
<div><strong>&nbsp;</p>
<p></strong><strong> </strong></p>
<p></div>
<div><strong>Conclusion</strong></div>
<div></div>
<div>As seen from the above example, with Hadoop Streaming we need not write and maintain the code for Mappers and Reducers. It combines the powers of scripting languages and parallel processing capabilities of Hadoop infrastructure to deliver jobs without having to write Java programs, thus, resulting in quicker response to the ad-hoc queries and requirements of the business.</div>
<div><b>&nbsp;</p>
<p></b><b> </b></p>
<p></div>
</div>
<div></div></p>