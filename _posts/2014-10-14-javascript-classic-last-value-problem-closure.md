---
layout: post
header-img: img/default-blog-pic.jpg
author: sunny200745
description: 
post_id: 19066
created: 2014/10/14 12:02:12
created_gmt: 2014/10/14 07:02:12
comment_status: open
---

# Javascript : Classic last value problem {Closure}

<p><span style="text-decoration: underline"><span style="font-size: 16px"><strong>Last Value Problem</strong></span></span></p>
<p><strong></strong>One can be see the problem as a problem statement where some buttons are generated  using a loop and an onclick event is binded on them. When user clicks a button its unique number is supposed to be alerted, e.g When user clicks on the second button it is supposed to alerts "<strong>You clicked on BUTTON with unique number</strong> : <strong>2</strong>". Where this unique number is supposed to be assigned during the loop execution.</p>
<p><span style="text-decoration: underline"><span style="font-size: 16px"><b>Preparation code</b></span></span></p>
<p><span style="color: #3366ff">&lt;html&gt;</span>
<span style="color: #3366ff">&lt;body&gt;</span>
<span style="color: #3366ff">  &lt;button id="button1" &gt;Button 1&lt;/button&gt;</span>
<span style="color: #3366ff">  &lt;button id="button2" &gt;Button 2&lt;/button&gt;</span>
<span style="color: #3366ff">  &lt;button id="button3" &gt;Button 3&lt;/button&gt;</span>
<span style="color: #3366ff">  &lt;button id="button4" &gt;Button 4&lt;/button&gt;</span>
<span style="color: #3366ff">  &lt;button id="button5" &gt;Button 5&lt;/button&gt;</span>
<span style="color: #3366ff">&lt;/body&gt;</span>
<span style="color: #3366ff">&lt;script&gt;</span>
<span style="color: #3366ff">  for (var i = 1; i &lt;= 5; i++) {</span>
<span style="color: #3366ff">    document.getElementById('button' + i ).onclick = function() {</span>
<span style="color: #3366ff">      alert('You clicked on BUTTON with unique number : ' + i );</span>
<span style="color: #3366ff">    };</span>
<span style="color: #3366ff">  }</span>
<span style="color: #3366ff">&lt;/script&gt;</span>
<span style="color: #3366ff">&lt;/html&gt;</span></p>
<p><span style="line-height: 1.5em">Go ahead, run this program and see the output.</span></p>
<p><strong>Output</strong> of this program irrespective of which button you press is always : [<b>You clicked on BUTTON with unique number : 6</b>].
No matter which button you click all the button have the same unique number "6" attached to them.</p>
<p>The root of this confusion is that all the onclick event refer to the same variable i , and at the end of the loop the value of i is 6. So whenever you click on any button it refers to the same variable i which has a value of 6 now. This is exactly the "<b>last value</b>" problem.</p>
<p>So how do you solve this problem ???<!--more--></p>
<p>You might have guessed it by now and its <strong>CLOSURES</strong> (<i> though there are other way too</i>). Since closures binds the context (in our case a unique number for every button).</p>
<p>Before that let's have a look on what exactly closures are</p>
<p><strong><span style="text-decoration: underline">What is a Closure</span> : </strong></p>
<p>Closure is a reference to a function together with the referencing environment or Closures are functions that refer to independent (free) variables</p>
<p>Two important things to note in the above definition is that closure comprises of two things which are as follow
<ol>
    <li>A reference to a function.</li>
    <li>Binding function's referencing environment (in our case local variables inside the function's scope).</li>
</ol>
Also  closure is defined within the scope of its free variables, and the extent of those variables is at least as long as the lifetime of the closure itself.</p>
<p>Let us see an example to grasp these two points better.</p>
<p><span style="color: #3366ff">function HelloWorld(){</span>
<span style="color: #3366ff">    var msg = "Hello World";</span>
<span style="color: #3366ff">    function PrintHelloWorld(){</span>
<span style="color: #3366ff">        alert(msg);</span>
<span style="color: #3366ff">    };    </span>
<span style="color: #3366ff">    return PrintHelloWorld;</span>
<span style="color: #3366ff">}; </span></p>
<p><span style="color: #3366ff">var referenceToPrintHelloWorld = HelloWorld();</span>
<span style="color: #3366ff">referenceToPrintHelloWorld();</span></p>
<p>Output of this program : [Hello World]</p>
<p>In the above code snippet, function HelloWorld() returns a <b>reference</b> to PrintHelloWorld(). PrintHelloWorld() closes over the local variable "msg" of HelloWorld(), which is in its <b>execution environment</b>. Here execution environment is the scope in which this function in declared and that is scope of HelloWorld function. Hence this satisfies definition of closure.</p>
<p>Notice though "msg" is defined locally inside HelloWorld's scope but still when we call <b>referenceToPrintHelloWorld()</b> from scope outside of HelloWorld() it alerts the value "msg". This is the magic of closures which closes over any local variable in its scope.</p>
<p><strong>Lets get back to our solution for the last value problem.</strong></p>
<p>Here is the solution.
<pre>for (var i = 1; i &lt;= 5; i++) {
 document.getElementById('button' + i).onclick = (function(uid){
 return function(){
 alert('You clicked BUTTON with unique number : ' + uid);
 };
 })(i);
 };</pre>
For ease of purpose i have added a  <span style="color: #3366ff"><strong><a title="FIDDLE LINK" href="http://jsfiddle.net/sunny200745/jj9okqod/">FIDDLE</a>   </strong></span>for your purpose</p>
<p>Also you can simply add the snippet into your html and it will solve the purpose</p>
<p>[code language="html"]
&lt;iframe width=&quot;100%&quot; height=&quot;300&quot; src=&quot;http://jsfiddle.net/sunny200745/jj9okqod/embedded/&quot; allowfullscreen=&quot;allowfullscreen&quot; frameborder=&quot;0&quot;&gt;&lt;/iframe&gt;
[/code]</p>
<p>Explanation : In this fiddle i have provided the problem statement along with the solution. You will see the 5 buttons namely button1 to button5. and a checkbox which states use closure instead. If this checkbox is unchecked it will state the problem statement of classic last value problem, and if its checked then it will state the solution.</p>
<p><span style="font-size: 16px"><strong>PS.</strong></span>
Closure is a fun thing in JavaScript and can be applied to problems where context of execution has to remembered. Go on and take a dive into closures.</p>