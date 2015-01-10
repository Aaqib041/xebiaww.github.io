---
layout: post
header-img: img/default-blog-pic.jpg
author: Manish Kumar Thakur
description: 
post_id: 19090
created: 2014/10/27 16:57:03
created_gmt: 2014/10/27 11:57:03
comment_status: open
---

# Being Functional With Scala

<p>Functional programming in on the hype now a days and every body is talking of it. After all its a new thing every body should try, especially the people who are bored with object oriented programming. Scala is both object oriented and functional as it treats both objects and functions as first class citizens of the lanaguage.</p>
<p>Just like objects, functions in Scala enjoys the royalty as there is no restriction on how you use them. You can store functions in variables, pass them around as parameters to other functions or if you like return them from other functions. Let see how Scala gives you that power of functional programming.Here we are going to talk about functions and not methods (functions that are invoked on an object) to keep concepts clear and seperated. We'll start with simple functions and then will move to more complex functions. In REPL define a function using its prototype starting with function which doesn't accept any parameters and returns nothing.
<code>scala&gt; () =&gt; {}</code>
<code>res0: () =&gt; Unit = &lt;function0&gt;</code></p>
<p>Blank parenthesis shows function doesn't accept any parameters and empty curly braces means the function do nothing(return Unit). This function is an instance of the trait function0 as our function have zero arguments. We have these traits till function22. Beyond that we don't need any. Since we didn't provide any variable to store the function, the REPL stored it automatically in res0. We can store it in a variable and call it, which does nothing.
<code>scala&gt; val v = res0</code>
<code>v: () =&gt; Unit = &lt;function0&gt;</code>
<code>scala&gt; v()</code>
<code>scala&gt;</code></p>
<p>You can actually define a function which do something meaningful and can store it in variable. The next function accepts an Int as parameter and returns an Int.
<code>scala&gt; val doubleIt = (i:Int) =&gt; {2*i}</code>
<code>doubleIt: Int =&gt; Int = &lt;function1&gt;</code>
<code>scala&gt; doubleIt(2)</code>
<code>res3: Int = 4</code></p>
<p>This is shorthand for creating a function which uses Scala powerful type inference. We can explicitly define the prototype and then can provide the actual implementation of the function.
<code>scala&gt; val addThem: (Int,Int) =&gt; Int = { (i,j) =&gt; i+j }</code>
<code>addThem: (Int, Int) =&gt; Int = &lt;function2&gt;</code>
<code>scala&gt; addThem(4,-9)</code>
<code>res4: Int = -5</code></p>
<p>Let see how we can create a function which accepts another function as a parameter commonly called as higher order function. This is quite complex as the function we create accepts 1 function and two Int as parameters and returns an Int. The function that it accepts itself accepts two Int parameters and returns an Int.
<code>scala&gt; val execFunc: ((Int,Int) =&gt; Int, Int, Int) =&gt; Int = (f,i,j) =&gt; {f(i,j)}</code>
<code>execFunc: ((Int, Int) =&gt; Int, Int, Int) =&gt; Int = &lt;function3&gt;</code></p>
<p>Now we have execFunc as a higher order function which can accept any function as long as it matches the signature of the function that execFunc accepts. We give it an anonymous function which adds the other two parameters and returns the result. Here (i, j) =&gt; i+j is an anonymous function.
<code>scala&gt; execFunc((i,j) =&gt; i+j, 5, 6)</code>
<code>res5: Int = 11</code></p>
<p>If this looks confusing we can define a function separately and store it in variable and then pass it in the execFunc function.
<code>scala&gt; val add: (Int, Int) =&gt; Int = (i,j) =&gt; { i+j }</code>
<code>add: (Int, Int) =&gt; Int = &lt;function2&gt;</code>
<code>scala&gt; execFunc(add, 3, 4)</code>
<code>res14: Int = 7</code></p>
<p>Lets return the difference. We pass a new function which returns the difference.
<code>scala&gt; execFunc((i,j) =&gt; i-j, 5, 6)</code>
<code>res6: Int = -1</code></p>
<p>This is similar to strategy pattern in object oriented programming (swapping algorithms). You can define incredibly complex functions where a function can accept other two functions as parameters and do something with them and then return a function in the result.
Lets see how can we return a function from a function. To understand how to do it we can ask it from the REPL. We create a function first and then define it as the return value in REPL.
<code>scala&gt; () =&gt; println("Hello")</code>
<code>res8: () =&gt; Unit = &lt;function0&gt;</code>
<code>scala&gt; () =&gt; res8</code>
<code>res9: () =&gt; () =&gt; Unit = &lt;function0&gt;</code></p>
<p>As we see, We defined a function which print “Hello” to the console. REPL stores it in res8 which we use to define the return value of the next function and tada!. The res9 shows us how to define a function which returns another function.
Lets define our function which accepts a greeting message and returns a function which greets while returning a result.
<code>scala&gt; val greetWithResult = (message: String) =&gt; (i: Int, j: Int) =&gt; {val res = i+j; message + " " + res }</code>
<code>greetWithResult: String =&gt; ((Int, Int) =&gt; String) = &lt;function1&gt;</code></p>
<p>Now we create our helloGreeter function which prints “Hello” message with the result. For that we create a function preloaded with “Hello” message and return that function which is assigned to helloGreeter.
<code>scala&gt; val greetWithResult = (message: String) =&gt; (i: Int, j: Int) =&gt; {val res = i+j; message + " " + res }</code>
<code>greetWithResult: String =&gt; ((Int, Int) =&gt; String) = &lt;function1&gt;</code></p>
<p>Now we call our helloGreeter.
<code>scala&gt; helloGreeter(6,7)</code>
<code>res12: String = Hello 13</code></p>
<p>Simple enough. Let do a challenging task, create the above greetWithResult function by first defining its prototype and then providing its actual implementation. It would be like :
<code>scala&gt; val greetWithResult: (String) =&gt; (Int, Int) =&gt; String = (message) =&gt; (i, j) =&gt; {val res = i+j; message + " " + res }</code>
<code>greetWithResult: String =&gt; ((Int, Int) =&gt; String) = &lt;function1&gt;</code></p>
<p>Now this looks overwhelming but we can decipher it if dissect it section by section. The first section after the colon : declares the prototype of the function that it accepts a String parameter and returns a function. The next section after the first =&gt; defines the return type which is a function which accepts and two Int parameters and return a String. The next section after the = is actual implementation of both the functions.
Here we talked about only functions to keep things simple. The same can be done with methods also. You define methods in scala using def. Here are some examples.
<code>scala&gt; def fun() = {}</code>
<code>fun: ()Unit</code>
<code>scala&gt; def add(i: Int, j:Int): Int = { i + j }</code>
<code>add: (i: Int, j: Int)Int</code></p>
<p>In the first example we declared a method func which takes no parameter and returns Unit and the second one takes two Int and return an Int. You can do same things with methods as we do with functions. Methods belongs to a class they are not an instance of function0 or any related trait. Keep in mind the difference between functions and methods and decide where to use them.</p>