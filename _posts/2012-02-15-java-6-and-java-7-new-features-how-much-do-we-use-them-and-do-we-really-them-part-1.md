---
layout: post
header-img: img/default-blog-pic.jpg
---

# JAVA 6 and JAVA 7 new features - How much do we use them and do we really need them ? Part 1

My first introduction to JAVA was that it is a _"pure"_ object oriented language, that it is a strong building block for any software application and the biggest thing -it is platform independent giving it the _"write once run anywhere"_ capability. There have been many upgrades and new version launches but  sometimes I feel that all that has been done in these new versions is it really worth it, are we actually using the new features or are the new versions simply bundling everything possible into the JDK and making it heavy.Lets have a look at the last 2 versions of JAVA 6 & 7 and discuss the new features that these versions boast of.

**JAVA 6**

  1. Support for Web Services:Before addition of web services to JDK 1.6 they were being shipped with JAVA Enterprise Edition. I dont think there was any need to bundle it into JDK. They should have left the JDK to do the basics, after all every JAVA  based application does not require Web Services support and those that required could have used JAVA EE.Keeping different  functionality in different components is a basic object oriented and software design concept - how can the makers of the first pure object oriented language forget this. 
  2. Bundled JAVA DB:JDK 6 comes bundled with JAVA DB which is loosely based on Derby.Now many of us must have been using JDK 6 since past 2-3 years but there must be a very few percentage using JAVA DB. Huge data heavy enterprise applications have already adopted the best solutions for their needs and they will not migrate their old database to JAVA DB just because it comes bundled with JDK 6. It may however be helpful to students learning JAVA and Databases.But at the end of the day it is just reinvention of the wheel , a feature which we hardly use and is of no value addition.Instead of this they could have provided ,more powerful connectors. 
  3. Monitoring and Management Tools:JDK 6 has tools like jmap and jhat for analyzing memory, heap and threads/objects count. A good utility to have but most of us who needed these tools are already using other tools like JMeter for this.
  4. Security API: With JDK 6 we get a bundled security API. Now most of this is available in JAVA Cryptography Edition and should have been kept that way. Bundling everything in one makes no sense, different users have different needs and its better to keep them in separate components. JAVA is not a framework in itself it is a framework builder and we should try to keep it this way.
Well all is not bad with JAVA 6 but if some good features which make it more robust  and an even stronger building block are added to it it, it would truly enhance the power of JAVA and make it the star it truly deserves to be.My wish-list includes addition of more design patterns related classes the way Observer Pattern classes are included in JAVA.