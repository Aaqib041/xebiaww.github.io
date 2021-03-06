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

Functional programming in on the hype now a days and every body is talking of it. After all its a new thing every body should try, especially the people who are bored with object oriented programming. Scala is both object oriented and functional as it treats both objects and functions as first class citizens of the lanaguage.

Just like objects, functions in Scala enjoys the royalty as there is no restriction on how you use them. You can store functions in variables, pass them around as parameters to other functions or if you like return them from other functions. Let see how Scala gives you that power of functional programming.Here we are going to talk about functions and not methods (functions that are invoked on an object) to keep concepts clear and seperated. We'll start with simple functions and then will move to more complex functions. In REPL define a function using its prototype starting with function which doesn't accept any parameters and returns nothing. `scala> () => {}` `res0: () => Unit = <function0>`

Blank parenthesis shows function doesn't accept any parameters and empty curly braces means the function do nothing(return Unit). This function is an instance of the trait function0 as our function have zero arguments. We have these traits till function22. Beyond that we don't need any. Since we didn't provide any variable to store the function, the REPL stored it automatically in res0. We can store it in a variable and call it, which does nothing. `scala> val v = res0` `v: () => Unit = <function0>` `scala> v()` `scala>`

You can actually define a function which do something meaningful and can store it in variable. The next function accepts an Int as parameter and returns an Int. `scala> val doubleIt = (i:Int) => {2*i}` `doubleIt: Int => Int = <function1>` `scala> doubleIt(2)` `res3: Int = 4`

This is shorthand for creating a function which uses Scala powerful type inference. We can explicitly define the prototype and then can provide the actual implementation of the function. `scala> val addThem: (Int,Int) => Int = { (i,j) => i+j }` `addThem: (Int, Int) => Int = <function2>` `scala> addThem(4,-9)` `res4: Int = -5`

Let see how we can create a function which accepts another function as a parameter commonly called as higher order function. This is quite complex as the function we create accepts 1 function and two Int as parameters and returns an Int. The function that it accepts itself accepts two Int parameters and returns an Int. `scala> val execFunc: ((Int,Int) => Int, Int, Int) => Int = (f,i,j) => {f(i,j)}` `execFunc: ((Int, Int) => Int, Int, Int) => Int = <function3>`

Now we have execFunc as a higher order function which can accept any function as long as it matches the signature of the function that execFunc accepts. We give it an anonymous function which adds the other two parameters and returns the result. Here (i, j) => i+j is an anonymous function. `scala> execFunc((i,j) => i+j, 5, 6)` `res5: Int = 11`

If this looks confusing we can define a function separately and store it in variable and then pass it in the execFunc function. `scala> val add: (Int, Int) => Int = (i,j) => { i+j }` `add: (Int, Int) => Int = <function2>` `scala> execFunc(add, 3, 4)` `res14: Int = 7`

Lets return the difference. We pass a new function which returns the difference. `scala> execFunc((i,j) => i-j, 5, 6)` `res6: Int = -1`

This is similar to strategy pattern in object oriented programming (swapping algorithms). You can define incredibly complex functions where a function can accept other two functions as parameters and do something with them and then return a function in the result. Lets see how can we return a function from a function. To understand how to do it we can ask it from the REPL. We create a function first and then define it as the return value in REPL. `scala> () => println("Hello")` `res8: () => Unit = <function0>` `scala> () => res8` `res9: () => () => Unit = <function0>`

As we see, We defined a function which print “Hello” to the console. REPL stores it in res8 which we use to define the return value of the next function and tada!. The res9 shows us how to define a function which returns another function. Lets define our function which accepts a greeting message and returns a function which greets while returning a result. `scala> val greetWithResult = (message: String) => (i: Int, j: Int) => {val res = i+j; message + " " + res }` `greetWithResult: String => ((Int, Int) => String) = <function1>`

Now we create our helloGreeter function which prints “Hello” message with the result. For that we create a function preloaded with “Hello” message and return that function which is assigned to helloGreeter. `scala> val greetWithResult = (message: String) => (i: Int, j: Int) => {val res = i+j; message + " " + res }` `greetWithResult: String => ((Int, Int) => String) = <function1>`

Now we call our helloGreeter. `scala> helloGreeter(6,7)` `res12: String = Hello 13`

Simple enough. Let do a challenging task, create the above greetWithResult function by first defining its prototype and then providing its actual implementation. It would be like : `scala> val greetWithResult: (String) => (Int, Int) => String = (message) => (i, j) => {val res = i+j; message + " " + res }` `greetWithResult: String => ((Int, Int) => String) = <function1>`

Now this looks overwhelming but we can decipher it if dissect it section by section. The first section after the colon : declares the prototype of the function that it accepts a String parameter and returns a function. The next section after the first => defines the return type which is a function which accepts and two Int parameters and return a String. The next section after the = is actual implementation of both the functions. Here we talked about only functions to keep things simple. The same can be done with methods also. You define methods in scala using def. Here are some examples. `scala> def fun() = {}` `fun: ()Unit` `scala> def add(i: Int, j:Int): Int = { i + j }` `add: (i: Int, j: Int)Int`

In the first example we declared a method func which takes no parameter and returns Unit and the second one takes two Int and return an Int. You can do same things with methods as we do with functions. Methods belongs to a class they are not an instance of function0 or any related trait. Keep in mind the difference between functions and methods and decide where to use them.