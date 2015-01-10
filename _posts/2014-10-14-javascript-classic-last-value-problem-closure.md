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

**Last Value Problem**

****One can be see the problem as a problem statement where some buttons are generated  using a loop and an onclick event is binded on them. When user clicks a button its unique number is supposed to be alerted, e.g When user clicks on the second button it is supposed to alerts "**You clicked on BUTTON with unique number** : **2**". Where this unique number is supposed to be assigned during the loop execution.

**Preparation code**

<html> <body>   <button id="button1" >Button 1</button>   <button id="button2" >Button 2</button>   <button id="button3" >Button 3</button>   <button id="button4" >Button 4</button>   <button id="button5" >Button 5</button> </body> <script>   for (var i = 1; i <= 5; i++) {     document.getElementById('button' + i ).onclick = function() {       alert('You clicked on BUTTON with unique number : ' + i );     };   } </script> </html>

Go ahead, run this program and see the output.

**Output** of this program irrespective of which button you press is always : [**You clicked on BUTTON with unique number : 6**]. No matter which button you click all the button have the same unique number "6" attached to them.

The root of this confusion is that all the onclick event refer to the same variable i , and at the end of the loop the value of i is 6. So whenever you click on any button it refers to the same variable i which has a value of 6 now. This is exactly the "**last value**" problem.

So how do you solve this problem ???

You might have guessed it by now and its **CLOSURES** (_ though there are other way too_). Since closures binds the context (in our case a unique number for every button).

Before that let's have a look on what exactly closures are

**What is a Closure : **

Closure is a reference to a function together with the referencing environment or Closures are functions that refer to independent (free) variables

Two important things to note in the above definition is that closure comprises of two things which are as follow 

  1. A reference to a function.
  2. Binding function's referencing environment (in our case local variables inside the function's scope).
Also  closure is defined within the scope of its free variables, and the extent of those variables is at least as long as the lifetime of the closure itself.

Let us see an example to grasp these two points better.

function HelloWorld(){     var msg = "Hello World";     function PrintHelloWorld(){         alert(msg);     };         return PrintHelloWorld; }; 

var referenceToPrintHelloWorld = HelloWorld(); referenceToPrintHelloWorld();

Output of this program : [Hello World]

In the above code snippet, function HelloWorld() returns a **reference** to PrintHelloWorld(). PrintHelloWorld() closes over the local variable "msg" of HelloWorld(), which is in its **execution environment**. Here execution environment is the scope in which this function in declared and that is scope of HelloWorld function. Hence this satisfies definition of closure.

Notice though "msg" is defined locally inside HelloWorld's scope but still when we call **referenceToPrintHelloWorld()** from scope outside of HelloWorld() it alerts the value "msg". This is the magic of closures which closes over any local variable in its scope.

**Lets get back to our solution for the last value problem.**

Here is the solution. 
    
    
    for (var i = 1; i <= 5; i++) {
     document.getElementById('button' + i).onclick = (function(uid){
     return function(){
     alert('You clicked BUTTON with unique number : ' + uid);
     };
     })(i);
     };

For ease of purpose i have added a  **[FIDDLE][1]   **for your purpose

Also you can simply add the snippet into your html and it will solve the purpose

[code language="html"] <iframe width="100%" height="300" src="http://jsfiddle.net/sunny200745/jj9okqod/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe> [/code]

Explanation : In this fiddle i have provided the problem statement along with the solution. You will see the 5 buttons namely button1 to button5. and a checkbox which states use closure instead. If this checkbox is unchecked it will state the problem statement of classic last value problem, and if its checked then it will state the solution.

**PS.** Closure is a fun thing in JavaScript and can be applied to problems where context of execution has to remembered. Go on and take a dive into closures.

   [1]: http://jsfiddle.net/sunny200745/jj9okqod/ (FIDDLE LINK)