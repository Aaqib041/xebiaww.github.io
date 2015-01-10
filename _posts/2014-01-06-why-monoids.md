---
layout: post
header-img: img/default-blog-pic.jpg
---

# Why Monoids !!

Monoids can be treated as a design pattern in functional programming. Functional Programming is a paradigm where focus is on Functions as first class citizens i.e we can pass one function as params and return value of other function. Also, writing immutable code using recursions is preferred approach. Monoids can be defined as: "We have a list of things, we have** some associative way of combining** them, and we get a single aggregated object back at the end. Also we have a** identity element** when used during combining operation there is no effect (input is same as output)" **Examples**:

  * _Integers_: 
    
    
    1 + 2 = 3                           // "+" is combining operation
    1 + (2 + 3) = (1 + 2) + 3          // combining operation is **associative**
    1 + 0 = 1 and 0 + 1 = 1             // identity case = 0

Similarly we have multiplication of integers with 1 as identity element, Max. among integers with Integer.MinValue as Identity element: max( max(12,2), 3) = max(12, max(2,3)

  * _Strings_: 
    
    
    "hello" + "monoids" = "hello monoids"    
    "a" + ("b" + "c") = ("a" + "b") + "c"          
    "a" + "" = "a" and "" + "a" = "a"     // identity case = ""        

  * _Collections_:  Lists and Sets
    
    
    List(1) + List(2) = List(1,2) 
    
    List(1,2) + List(3) = List(1) + List(1,2,3)
    List(1,2) + emptyList = List(1,2)

**Why Associativity is important**

Due to associativity, combining operation for let say billion of elements can be sub divided into subtasks of combining pair of elements each executed parallel over different cores/boxes. 

 **Define Monoid in Scala**
    
    
    trait Monoid[T] {       // trait is similar to Java interface (loosely speaking)
     def Zero: T             // identity case        
     def op(x: T, y: T) => T // associative operation with two param's x and y
    }

** **** Monoids Usage**
    
    
    val list = List(1,2,3,4,5......N)                                                       
    // add as combine operation
    list.par.foldLeft(0)((x,y) => x + y)
    // multiply as combine operation
    list.par.foldLeft(1)((x,y) => x * y)

**Note**: 

  1. **par method**:  built in method of scala collections converts collection to parallel collection. Multiple Threads can parallely work on it. For details, visit http://docs.scala-lang.org/overviews/parallel-collections/overview.html                              
  2. **foldLeft method**: built in method of scala List class. foldLeft iterates a List[A] and reduces it a value of type A. For first iteration, it works on pair of base-element passed as input (0 in case of add and 1 in case of product) and first element of List[A] and applies the passed function (x+y and x*y respectively), result of 1st iteration and second element of List are processed in second iteration. And so on..finally an value of type A is returned.
**Monoids in Real World**

UseCase: Shopping Card application would like to calculate total for N OrderLines for a specific customer. Each Order line has product, qty and total value. 

**API definition**
    
    
    // scala syntax for class with primary constructor
    
    
    class OrderLine(productCode: String, qty: Integer,total: Float)