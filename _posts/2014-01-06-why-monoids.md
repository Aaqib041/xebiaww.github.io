---
layout: post
header-img: img/default-blog-pic.jpg
author: nikhil
description: 
post_id: 17844
created: 2014/01/06 09:17:41
created_gmt: 2014/01/06 04:17:41
comment_status: open
---

# Why Monoids !!

<p><span style="color: #333333;font-family: Constantia, 'Lucida Bright', Lucidabright, 'Lucida Serif', Lucida, 'DejaVu Serif', 'Bitstream Vera Serif', 'Liberation Serif', Georgia, serif">Monoids can be treated as a design pattern in functional programming. Functional Programming is a paradigm where focus is on Functions as first class citizens i.e we can pass one function as params and return value of other function. Also, writing immutable code using recursions is preferred approach.</span></p>
<p>Monoids can be defined as:</p>
<p><span style="color: #333333;font-family: Constantia, 'Lucida Bright', Lucidabright, 'Lucida Serif', Lucida, 'DejaVu Serif', 'Bitstream Vera Serif', 'Liberation Serif', Georgia, serif">"We have a list of things, we have</span><b> some associative way of combining</b><span style="color: #333333;font-family: Constantia, 'Lucida Bright', Lucidabright, 'Lucida Serif', Lucida, 'DejaVu Serif', 'Bitstream Vera Serif', 'Liberation Serif', Georgia, serif"> them, and we get a single aggregated object back at the end. Also we have a</span><b> identity element</b><span style="color: #333333;font-family: Constantia, 'Lucida Bright', Lucidabright, 'Lucida Serif', Lucida, 'DejaVu Serif', 'Bitstream Vera Serif', 'Liberation Serif', Georgia, serif"> when used during combining operation there is no effect (input is same as output)"</span></p>
<p><span style="color: #333333;font-family: Constantia, 'Lucida Bright', Lucidabright, 'Lucida Serif', Lucida, 'DejaVu Serif', 'Bitstream Vera Serif', 'Liberation Serif', Georgia, serif"><b>Examples</b>:</span>
<ul>
    <li><span style="color: #333333;font-family: Constantia, 'Lucida Bright', Lucidabright, 'Lucida Serif', Lucida, 'DejaVu Serif', 'Bitstream Vera Serif', 'Liberation Serif', Georgia, serif"><i>Integers</i>: </span></li>
</ul>
<pre><code>1 + 2 = 3                           // "+" is combining operation</code>
<code>1 + (2 + 3) = (1 + 2) + 3          // combining operation is <b>associative</b></code>
<span style="font-family: Consolas, 'Andale Mono WT', 'Andale Mono', 'Lucida Console', 'Lucida Sans Typewriter', 'DejaVu Sans Mono', 'Bitstream Vera Sans Mono', 'Liberation Mono', 'Nimbus Mono L', Monaco, 'Courier New', Courier, monospace">1 + 0 = 1 and 0 + 1 = 1             // identity case = 0</span></pre>
<span style="color: #333333;font-family: Constantia, 'Lucida Bright', Lucidabright, 'Lucida Serif', Lucida, 'DejaVu Serif', 'Bitstream Vera Serif', 'Liberation Serif', Georgia, serif">Similarly we have multiplication of integers with 1 as identity element, Max. among integers with Integer.MinValue as Identity element: </span><span style="color: #333333;font-family: Consolas, 'Andale Mono WT', 'Andale Mono', 'Lucida Console', 'Lucida Sans Typewriter', 'DejaVu Sans Mono', 'Bitstream Vera Sans Mono', 'Liberation Mono', 'Nimbus Mono L', Monaco, 'Courier New', Courier, monospace">max( max(12,2), 3) = </span><span style="color: #333333;font-family: Consolas, 'Andale Mono WT', 'Andale Mono', 'Lucida Console', 'Lucida Sans Typewriter', 'DejaVu Sans Mono', 'Bitstream Vera Sans Mono', 'Liberation Mono', 'Nimbus Mono L', Monaco, 'Courier New', Courier, monospace">max(12, max(2,3)</span>
<div>
<ul>
    <li><span style="color: #333333;font-family: Constantia, 'Lucida Bright', Lucidabright, 'Lucida Serif', Lucida, 'DejaVu Serif', 'Bitstream Vera Serif', 'Liberation Serif', Georgia, serif"><i>Strings</i>: </span></li>
</ul>
<pre><code>"hello" + "monoids" = "hello monoids"    </code>
<code>"a" + ("b" + "c") = ("a" + "b") + "c"          </code>
<span style="font-family: Consolas, 'Andale Mono WT', 'Andale Mono', 'Lucida Console', 'Lucida Sans Typewriter', 'DejaVu Sans Mono', 'Bitstream Vera Sans Mono', 'Liberation Mono', 'Nimbus Mono L', Monaco, 'Courier New', Courier, monospace">"a" + "" = "a" and "" + "a" = "a"     // identity case = ""        </span></pre>
<ul>
    <li><span style="color: #333333;font-family: Constantia, 'Lucida Bright', Lucidabright, 'Lucida Serif', Lucida, 'DejaVu Serif', 'Bitstream Vera Serif', 'Liberation Serif', Georgia, serif"><span><i>Collections</i>:  Lists and Sets</span></span></li>
</ul>
<div>
<pre><code>List(1) + List(2) = List(1,2) 
</code>
<code>List(1,2) + List(3) = List(1) + List(1,2,3)</code>
<code>List(1,2) + emptyList = List(1,2)</code></pre>
<div></p>
<p><b>Why Associativity is important</b></p>
<p></div>
</div>
Due to associativity, combining operation for let say billion of elements can be sub divided into subtasks of combining pair of elements each executed parallel over different cores/boxes.
<div></p>
<p><b>Define Monoid in Scala</b></p>
<p></div>
<pre><code>trait Monoid[T] {       // trait is similar to Java interface (loosely speaking)
 def Zero: T             // identity case      <br />
 def op(x: T, y: T) =&gt; T // associative operation with two param's x and y
}</code></pre>
<div></p>
<p><b>
</b><b> Monoids Usage</b></p>
<p></div>
<pre><span style="font-family: Consolas, 'Andale Mono WT', 'Andale Mono', 'Lucida Console', 'Lucida Sans Typewriter', 'DejaVu Sans Mono', 'Bitstream Vera Sans Mono', 'Liberation Mono', 'Nimbus Mono L', Monaco, 'Courier New', Courier, monospace">val list = List(1,2,3,4,5......N)                                                       </span>
<span style="font-family: Consolas, 'Andale Mono WT', 'Andale Mono', 'Lucida Console', 'Lucida Sans Typewriter', 'DejaVu Sans Mono', 'Bitstream Vera Sans Mono', 'Liberation Mono', 'Nimbus Mono L', Monaco, 'Courier New', Courier, monospace">// add as combine operation</span>
<span style="font-family: Consolas, 'Andale Mono WT', 'Andale Mono', 'Lucida Console', 'Lucida Sans Typewriter', 'DejaVu Sans Mono', 'Bitstream Vera Sans Mono', 'Liberation Mono', 'Nimbus Mono L', Monaco, 'Courier New', Courier, monospace">list.par.foldLeft(0)((x,y) =&gt; x + y)</span>
<span style="font-family: Consolas, 'Andale Mono WT', 'Andale Mono', 'Lucida Console', 'Lucida Sans Typewriter', 'DejaVu Sans Mono', 'Bitstream Vera Sans Mono', 'Liberation Mono', 'Nimbus Mono L', Monaco, 'Courier New', Courier, monospace">// multiply as combine operation</span>
<span style="font-family: Consolas, 'Andale Mono WT', 'Andale Mono', 'Lucida Console', 'Lucida Sans Typewriter', 'DejaVu Sans Mono', 'Bitstream Vera Sans Mono', 'Liberation Mono', 'Nimbus Mono L', Monaco, 'Courier New', Courier, monospace">list.par.foldLeft(1)((x,y) =&gt; x * y)</span></pre>
<div></p>
<p><strong>Note</strong>:
<ol>
    <li><span style="color: #333333;font-family: Constantia, 'Lucida Bright', Lucidabright, 'Lucida Serif', Lucida, 'DejaVu Serif', 'Bitstream Vera Serif', 'Liberation Serif', Georgia, serif"><b>par method</b>:  built in method of scala collections converts collection to parallel collection. Multiple Threads can parallely work on it. For details, visit http://docs.scala-lang.org/overviews/parallel-collections/overview.html                              </span></li>
    <li><span style="color: #333333;font-family: Constantia, 'Lucida Bright', Lucidabright, 'Lucida Serif', Lucida, 'DejaVu Serif', 'Bitstream Vera Serif', 'Liberation Serif', Georgia, serif"><span><b>foldLeft method</b>: built in method of scala List class. foldLeft iterates a List[A] and reduces it a value of type A. For first iteration, it works on pair of base-element passed as input (0 in case of add and 1 in case of product) and first element of List[A] and applies the passed function (x+y and x*y respectively), result of 1st iteration and second element of List are processed in second iteration. And so on..finally an value of type A is returned.</span></span></li>
</ol>
<b>Monoids in Real World</b></p>
<p></div>
<div></p>
<p>UseCase: Shopping Card application would like to calculate total for N OrderLines for a specific customer. Each Order line has product, qty and total value.</p>
<p></div>
<div></p>
<p><b>API definition</b>
<pre><code><span>// scala syntax for class with primary constructor</span></code></pre>
</div>
<pre><code>class OrderLine(productCode: String, qty: Integer,total: Float)</code></p>