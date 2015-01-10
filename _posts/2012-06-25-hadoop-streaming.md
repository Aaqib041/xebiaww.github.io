---
layout: post
header-img: img/default-blog-pic.jpg
---

# Hadoop Streaming

A basic template for writing a Hadoop job using Java is 

  1. Write a Mapper class.
  2. Write a Reducer class.
  3. Write a Driver code to configure and run the job.

However, logic (mapper and reducer logic) for some jobs can be programmed using unix commands. Hadoop streaming, a contrib package provides a way to use unix commands as mappers and reducers. In this blog, I will demonstrate running a Hadoop job using Hadoop streaming.

**Hadoop Streaming**

Streaming API of Hadoop is an API to write map and reduce functions in languages other than Java. It uses Unix standard streams as the interface between Hadoop and your program, so you can use any language that can read standard input and write to standard output.

Map input data is passed over standard input to your map function, which processes it line by line and writes lines to standard output. A map output key-value pair is written as a single tab-delimited line.  The reduce function reads lines from standard input, which the framework guarantees are sorted by key, and writes its results to standard output.

Apart from standard unix commands, we can also use languages like Ruby and Python to program map and reduce functions.

**  **** **

**Example**

The prerequisite for running this example are

  1. [Hadoop installation](http://hadoop.apache.org/common/docs/r0.20.2/quickstart.html#Installing+Software)
  2. Data set, which can be downloaded from [National Bureau of Economic Research](http://www.nber.org/patents/). The file that needs to be downloaded is [Pairwise Citations](http://data.nber.org/patents/Cite75_99.zip). This file includes all US patent citations for utility patents granted in the period 1-Jan-75 to 31-Dec-99. More information about the data set is available at <http://data.nber.org/patents/Cite75_99.txt>.

The two commands that we will use in this example are **cut **and **uniq**. [Cut ](http://www.computerhope.com/unix/ucut.htm)command is used to cut out specified fields of a line, whereas [Uniq](http://www.computerhope.com/unix/uuniq.htm) command is used for filtering out repeated lines from a file.

Hadoop job that we will execute will give a list of all the unique cited patent ids. To run the job, please execute the following command from the hadoop distribution folder

`` ` `

`bin/hadoop jar contrib/streaming/hadoop-*-streaming.jar`

`

-input <input path>

-output <output path>

-mapper 'cut -f 2 -d ,'

-reducer 'uniq'

`` ` ``````````````Let's discuss the above command in detail. Since, Hadoop Streaming is a contrib package, it's jar placed in the contrib folder of the Hadoop distribution. -input and -output are standard hadoop attributes to specify the locations of input and output locations. In case of Hadoop streaming, mapper and reducer are unix commands which operate on each line of input. The mapper expression will split each line of input based on comma(,) and pick the second element of the splitted result. So, the output of the mapper is cited patent id in each line. Now, the job of the reducer is to simply remove the duplicates and getting us the desired output. 

**  **** **

**Conclusion**

As seen from the above example, with Hadoop Streaming we need not write and maintain the code for Mappers and Reducers. It combines the powers of scripting languages and parallel processing capabilities of Hadoop infrastructure to deliver jobs without having to write Java programs, thus, resulting in quicker response to the ad-hoc queries and requirements of the business.

**  **** **