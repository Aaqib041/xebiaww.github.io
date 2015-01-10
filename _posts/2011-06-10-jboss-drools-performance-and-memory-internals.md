---
layout: post
header-img: img/default-blog-pic.jpg
---

# JBoss Drools - Performance and Memory Internals

The application I am working on nowadays extensively uses JBoss Drools. The application deals with lots of objects on which complex logic is applied in sequence. This part has been developed using JBoss Drools. The keywords here are "lots of objects" and "complex logic in sequence".

These two keywords, when found together, often cause menace for developers.

However, few days back, when I profiled the application, I was a bit amazed to see that "complex logic" on "lots of objects" was not taking that much time. And trust me, this is not what I was expecting. Before profiling, I was almost sure that this "logic on lots of objects" thing is the bottleneck in application's performance. So, the discovery of it being efficient was a bit shocking.

Ok, I accept it. Its fast. The profiler can not be wrong. But how is it that fast?

To get an answer to this question, I explored JBoss Drools internals. How JBoss Drools actually works? What's the thing that brings in this efficiency in JBoss Drools? I got few answers when I explored how it works. I will share my understanding of its architecture which, I think, makes it memory and performance efficient.

JBoss Drools uses Rete's Algorithm to execute rules. Rete's algorithm is an efficient pattern matching algorithm.

JBoss Drools has its own implementation of Rete's Algorithm. A rule in Drools is represented by a Rete tree. A Rete tree consists of nodes. These nodes are mostly conditional evaluations. Everything in a Drools rule is represented by a Rete tree node. Apart from conditional nodes, the Rete tree also consists of AND nodes, OR nodes and start/end nodes ( and few other types of nodes as well).

The main part of the algorithm is the creation of the Rete tree. Whenever a fact(object) is inserted in the Drools engine, a Rete tree is created. The creation of this Rete tree is done by some intelligent algorithm. The Rete tree, once created executes in almost no time over the facts(objects) and gives the result. Most of the time is went into creating the Rete tree rather than executing it.

This is something that I have also experienced during debugging. Whenever a fact is inserted into the Drools session, it takes a bit of time. However, the firing of rules is almost instantaneous.

So, the way this Rete Algorithm has been implemented inside Drools accounts for its efficiency. But, the million dollar question is (a million is too much, but its just a quote), what will happen to the JBoss Drools efficient Rete Algorithm when "LOTS OF OBJECTS" will come into picture?

The answer, I think, lies in two techniques that Drools uses to store nodes. Node Sharing and Node Indexing.

Drools caches nodes while building Rete’s tree which is known as Node Sharing. Whenever a new node is created, its checked whether there is an equivalend node already present in the cache. If an equivalent node is found in the cache, the cached node is used instead and the new node is discarded. This makes Drools memory efficient.

Drools keeps a hashtable of the object properties and Rete Tree Nodes .It is known as Node Indexing and is used to avoid evaluating same conditions multiple times. Node Indexing speeds up the propagation of nodes in the Rete Network which accounts for high performance of  the Drools engine.

The in depth analysis of Rete Tree creation and node propagation in the Rete Network tells that the way rules are written might also effect the performance, as it directly impacts the propagation in Rete Tree. Rules, written in intelligent way can drastically reduce the number of nodes in the Rete Tree, thus, further increasing the performance.

So, these techniques helps Drools to process large number of objects efficiently. What I have seen, is that, increasing number of objects in Drools hardly effects the total time taken to execute it.

I still need to explore how to write efficient rules, however, it looked very logical and impressive when I saw how it is done. I will try to come out with one more post which explains efficient Rule writing.

## Comments

**[Kuldeep Gulati](#5647 "2011-06-27 22:34:45"):** Wen, As you are getting into the Implementation , on the top of my head, you need to take care about the grouping ( OR and AND ) condition as drools does not behave as you expect it to be. So, its better to Group your conditions together logically . For Ex: ( A or B) and C. ( this will not work as you would like it to : A or B and C ). Also,retreating the duplicate rules from the memory will be something you need to take care of as same different set of conditions in Rules may lead to firing of same rule in memory ( read about insertLogical() etc ). Cheers

**[Kuldeep Gulati](#5617 "2011-06-10 19:07:03"):** Hello, Its good to know the kind of Analysis you have done on internals of drools. Being an open source I liked that's its API's are open and one can develop something on the top of these API's. I came around a scenario where we build the Rules using the API's of the drools, the hurdle we faced were that no supporting documentation was available for the API's but it was exciting to develop the rules using the rules API's without any support and fixing the drools drl helper class as well. Thanks for sharing your knowledge. Thanks Kuldeep

**[Ganesh Gembali](#5616 "2011-06-10 07:49:41"):** Good to know about internals of drools. Thanks for the nice post Paritosh.

**[Edson](#5619 "2011-06-10 23:12:33"):** @Paritosh Nice post! @Keldeep, You may want to check this out then: https://github.com/droolsjbpm/drools/tree/master/drools-compiler/src/main/java/org/drools/lang/api Here a few unit tests: https://github.com/droolsjbpm/drools/blob/master/drools-compiler/src/test/java/org/drools/lang/api/DescrBuilderTest.java It is work in progress, but hopefully will make your life a lot easier. Edson

**[srba](#5621 "2011-06-11 14:09:07"):** Hi, I expected some numbers too see in this post :) We had few problems with Drools. One of them is that it is single-threaded. You can not use two different threads to insert facts. There is experimental feature to fix this, but they said it might never be production ready and you have to know how to partition your data.

**[Miles Wen](#5622 "2011-06-11 14:36:39"):** Hi, I'm using drools5 too.I built a DSL for 'risk control' business in my project.This DSL is first translated to drools RDL syntax and then added to knowledge base; And the knowledge base is refreshed every 5 minutes if any rule is modified. Now I want to compile my DSL directly to drools rete tree implementation , not by translated to drools RDL first...I feel its troublesome to do , because I have to hack into drools' implementation... The design and implementation docs of drools is rare species. could you give me some advices on this? I'm also expecting your posts on efficient rule writing.

**[ivan](#7682 "2012-02-21 10:36:18"):** Nice post, I wonder is there a way to control the sequence of the facts inserted to the working memory ? I have a sorted list of objects (sorted by some condition) in my java application. I insert them as facts to the working memory in the same order, but obviously the rule engine does not get the facts 'one-by-one' and fire matching rules... What I need is a way to guarantee that the objects (facts) are processed in the order they are sorted. Is there some way to do so ? Thanks in advance

**[Paritosh Ranjan](#7683 "2012-02-21 10:43:33"):** I have also faced the same problem earlier and was not able to find any solution. Maybe multithreading is on by default ( for better performance ) which disturbs the ordering, I don't see any other sensible reason to do this. I can advice to search for this multithreaded stuff ( if it exists ) and switch it off. This might make it sequential.

