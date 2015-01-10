---
layout: post
header-img: img/default-blog-pic.jpg
author: rrawat
description: 
post_id: 9185
created: 2011/06/20 18:07:32
created_gmt: 2011/06/20 13:07:32
comment_status: open
---

# Is Scala really Faster than Java? 

Recently published [paper][1] from Google benchmarking different languages. It shows Scala to be faster compared to Java. I attempted to run the same benchmarks this weekend and am publishing my observations in this Blog. Observations in the blog may have been already made in the Google paper and by the developers involved.

This topic was discussed in [Scala debate mailing list][2] and i really appreciate the answer given by Martin Odersky. Analysis in Google paper attributes the lesser time taken by Scala to lesser time spent in garbage collection. Martin(and Google Analysis) also hints at use of ArrayList in java implementation(due to lack of typed Arrays) as the main culprit for lower speed of Java code. Martin did not compare the pro versions saying the implementations were not comparable.

My basic ( starter) understanding of Functional programming tells me that there should be more object churn in a functional language (implementation) and thus Scala should have trouble with GC not the Java implementation. I am looking forward to an explanation for this.

This debate seems a familiar one of whether Java is faster than C/C++. Thinking logically if some language is built on some other language then the derived language can be at most as fast as the underlying language. So how can Scala be faster than Java (after all Scala runs on JVM)? Going back to Java/C/C++ debate, it may sometimes be possible that the Hotspot generated code is faster as compared to hand written native code. So this brings in the human factor, a Scala program may perform as good as a Java program purely because of the programming model offered by Scala or Under the Hood optimizations.

I checked out the ``` 
[3] for the benchmark , removed the -X UseCompressedOops setting as it was not available on my machine. I created a script to record -gcutil data from jstat utility and added it to run _target_ in the MakeFiles. Data from jstat was then plotted using gnuplot.

A few basic assumptions/notes first: 1\. Programs were run using no special GC settings. So given equivalent type of code (Basic and Pro versions) and same environment which one would run fast. 2\. Comparisions are purely from a Garbage collection point of view, they do not account for implementation differences(Martin's point on pro versions). 3\. Observations do not discount the warm up period of the benchmark (but observations here are not absolute anyway).

**Observations:**

****_Java and Scala basic versions:_

1\. Java implementation uses more memory (by comparing max memory before full GC) 2\. Java code spends more time in garbage collection (nothing new here). 3\. GC Analysis

For Java Implementation GC Count java: 119 (minor & major) Full GC Count java: 20 (major) GC Time: Young : Full : Total 31.495 21.131 52.626

For Scala Implementation GC Count scala: 77 Full GC Count scala: 5 GC Time: Young : Full : Total 8.419 2.625 11.044

4\. Time collecting the young generation is significantly more in Java (more temporary objects) 5\. Size of old generation keeps on building till the Full GCs and much migher compared to scala implementation.

These facts reflect the different big difference in implemantation details as the number of objects being created differ a lot between the implementations. Finer points like use of while being better than for loop.

_Java and Scala Pro versions_

1\. GC Analysis

For Java Pro Implementation: GC Count java_pro: 31 Full GC Count java_pro: 1 GC Time: Young : Full : Total 3.623 0.286 3.910

For Scala Pro Implementation: GC Count scala_pro: 18 Full GC Count scala_pro: 3 GC Time: Young : Full : Total 1.261 0.781 2.042

2\. Java implementation spends a lot of time in collecting Young generation (object churn). 3\. There is nothing much to read in size of old generations but a marked difference in Java Pro implementation is that the old generation becomes constant in size like the scala one.

**So what is the conclusion.**

It seems that difference in implementations is the cause for difference in the time taken by Java/Scala programs.  This is reflected in the different object creation patterns for the implementations. As you can see in the graphs below that the Java pro implementation managed to get similar old generation size patterns as to Scala. I have not gone in the details of implementations to be able to comment on why.

Like they say for cricket statistics, "they hide more than they reveal". It won't be fair to conclude that Scala is faster than Java in all cases. But having a language like Scala which provides better abstractions on top the existing JVM is great. Great work by guys at Google to create a benchmark which evaluates different aspects of a language.

But i am still looking for an answer on why there is more object churn in Java implementation compared to Scala (where i would expect a lot of immutables to be created).

PS: Results shown are only for a single run. Tests were run multiple time and in randon order by a script, but the general trends were same.

Update: Discussion on Hacker News (http://news.ycombinator.com/item?id=2678353)

![][4]
![][5]

Java - Scala (old generation)
Java Pro & Scala Pro Old generation

![][6]
![][7]

Java Scala ( Time take for collecting young generation)
Java Pro & Scala Pro ( time taken for collecting young generation)

   [1]: http://research.google.com/pubs/archive/37122.pdf
   [2]: http://groups.google.com/group/scala-debate/browse_thread/thread/d42777ef7834517c
   [3]: http://code.google.com/p/multi-language-bench/
   [4]: http://xebee.xebia.in/wp-content/uploads/2011/06/java-scala-old-generation1-300x225.png (Java - Scala (old generation))
   [5]: http://xebee.xebia.in/wp-content/uploads/2011/06/java_pro-scala_pro-old-generation1-300x225.png (Java Pro & Scala Pro Old generation)
   [6]: http://xebee.xebia.in/wp-content/uploads/2011/06/java-scala-y-gc-300x225.png (Java Scala ( Time take for collecting young generation))
   [7]: http://xebee.xebia.in/wp-content/uploads/2011/06/java_pro-scala_pro-y-gc-300x225.png (Java Pro & Scala Pro ( time taken for collecting young generation))

## Comments

**[novitaOnCT2](#5629 "2011-06-20 18:36:12"):** Hi, I think the link you've provided for the paper published by Google in the first line is broken.

**[Age](#5630 "2011-06-20 22:55:47"):** Nice post. The link to the source code is broken though. I get a 404 from Google Code. Don't forget that Martin Odersky was wrote the Java compiler up to Java 1.4, so when it comes to generating optimized byte code, he knows what he's doing.

**[Ravindra](#5632 "2011-06-21 09:08:44"):** Yes, and that code already had generics in it. Martin has not taken this benchmark as marketing opportunity as others would probably have. PS: thanks to Novita & Age for pointing out the broken links.

**[Iwein Fuld](#5634 "2011-06-21 14:18:23"):** On your final question about a lot of immutables being created: Many developers know that object creation has a price tag and they conclude that code that creates a lot of objects must perform worse than code that reuses mutable objects. There are two reasons to let go of this assumption: 1\. Creating objects is dead cheap. It used to be more expensive in Java 1.3 or thereabouts, but not anymore. Garbage collection has improved a lot to a point where in most cases object creation is pretty much the last thing you need to worry about. 2\. Guarding mutable state is more expensive than creating objects. An API that pushes developers towards immutability pushes them towards better performing code, usually. In essence it all comes down to wether you put your trust in your own wits, or in the garbage collector. I have learned to go for the latter.

**[Ravindra](#5635 "2011-06-21 15:04:33"):** Totally agree with you on trusting GC. That is why the functional languages are back today. My question as around a non functional (Java) implementation generating more object churn. A few points like foreach creating a new Object in each loop (Java Tunings section of the paper)

**[eevilspock](#5636 "2011-06-21 20:12:27"):** Java is not built "on top" of C/C++. It compiles to JVM bytecode which is JIT compiled to native machine code. Likewise Scala is not built on top of Java. It compiles to JVM bytecode. So, "thinking logically", it is entirely possible that Scala tends to compile to more efficient bytecode than Java.

**[eevilspock](#5640 "2011-06-21 23:14:43"):** This article is getting many more comments on Hacker News: http://news.ycombinator.com/item?id=2678353

**[Jonathan](#5641 "2011-06-22 01:04:54"):** I was able to bump the Java version's speed significantly by adding equals/hashcode to the BasicBlock class and enabling the following JVM option: -XX:+UseCompressedStrings There's more optimizations that can be done... I guess the point of the paper was to show how ridiculously complex C++ is vs Java, and that Scala was a good compromise. In no way was it "Google's opinion" either... which a lot of people are taking it as.

**[Anand](#5645 "2011-06-22 19:22:43"):** This is a good analysis Rawat. Great job.

**[Gaurav Marwaha](#5719 "2011-07-15 21:01:40"):** As an article used to publish findings of re-run of Google test on a machine which was probably not 64bit & multi-core this is fine. But the results cannot be used further to decide which language to choose if someone want to make a decision. Choosing between languages is a tricky topic which may need further analysis of features provided by Scala like multi-core support and around Ruby...

**[Bap](#8044 "2012-03-26 18:38:50"):** Heya i'm for the first time here. I came across this board and I in finding It really helpful & it helped me out a lot. I'm hoping to give one thing back and aid others like you helped me.

