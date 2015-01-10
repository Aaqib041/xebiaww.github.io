---
layout: post
header-img: img/default-blog-pic.jpg
author: mjain
description: 
post_id: 2671
created: 2009/12/04 15:02:46
created_gmt: 2009/12/04 10:02:46
comment_status: open
---

# Integral calculus in Java

Recently, I got an opportunity to explore integral calculus in Java. For a finance project I had to calculate the [greek metrics ][1](Fair value, Implied Volatility, Delta etc) using [BlackScholes model][2]. The formulas for calculating the metrics using Black Scholes model revolved around the following integration ![94db15fc7182a86e64c21f5721d4871b][3]. 

This integration acted as a showstopper and forced me to search for the APIs available for performing mathematical calculus in Java. Google god came to rescue and provided me with [Google code API for Java Calculus][4].

With Java calculus API in hand, I wrote the program for performing following integration.![MSP1037198b609ebc5ig7eh00001de9i627b6d99e3h][5] [java] ... CalcParser calcParser2 = new CalcParser("x^3"); CalcObject calcObject = calcParser2.parse(); CalcINT calcInt = new CalcINT(); CalcSymbol calcSymbol = new CalcSymbol("x"); calcObject = calcInt.integrate(calcObject,calcSymbol); CalculusEngine calculusEngine = new CalculusEngine(); calculusEngine.execute(calcObject.toString().replace('x', '5')); System.out.println(calculusEngine.getResult()); ... [/java] **OUTPUT:** Input: MULTIPLY(POWER(5,ADD(3,1)),POWER(ADD(3,1)),-1) Evaluation Queue: MULTIPLY(POWER(5,ADD(3,1)),POWER(ADD(3,1)),-1) Processed Queue: MULTIPLY(-625,POWER(4)) Output: -625*4

Surprised !!! It should have been 625/4. API wrong :( I double checked my code. I tweaked into the source code of the API and figured out that there is a minor mistake in the source code of the API in the class: javacalculus\src\javacalculus\evaluator\CalcInt (line 68 to 78). [java] return CALC.MULTIPLY.createFunction (CALC.POWER.createFunction(firstObj, CALC.ADD.createFunction(secondObj, CALC.ONE)), CALC.POWER.createFunction(CALC.ADD.createFunction(secondObj, CALC.ONE)), CALC.NEG_ONE); [/java] And it should be fixed as follows: _Note the difference in order of closing braces in the last line_[java] return CALC.MULTIPLY.createFunction (CALC.POWER.createFunction(firstObj, CALC.ADD.createFunction(secondObj, CALC.ONE)), CALC.POWER.createFunction(CALC.ADD.createFunction(secondObj, CALC.ONE), CALC.NEG_ONE));[/java] With this discovery, I had 2 options either to fix the API or search for the other solution. Being the fan of [Wolfram Alpha][6] search, I decided to look for another solution. Since, the formula had only one integration, I decided to evaluate the integral offline through WolfRam. I substituted the result of integration in place of the integrals in the formulas. 

Wolfram engine provided me the following result:![MSP1103198b609ebc5ig7eh00002bhgh3b116edg9h2][7]

The "erf" function is readily available in [Apache Commons Math][8] API.

This exercise enabled me to look into the relatively unexplored area of integral calculus in Java. I hope my findings above, would be of help for the people researching in this area. 

   [1]:  http://en.wikipedia.org/wiki/Greeks_(finance)
   [2]: http://en.wikipedia.org/wiki/Black%E2%80%93Scholes
   [3]: http://xebee.xebia.in/wp-content/uploads/2009/12/94db15fc7182a86e64c21f5721d4871b.png (94db15fc7182a86e64c21f5721d4871b)
   [4]: http://code.google.com/p/javacalculus/
   [5]: http://xebee.xebia.in/wp-content/uploads/2009/12/MSP1037198b609ebc5ig7eh00001de9i627b6d99e3h.gif (MSP1037198b609ebc5ig7eh00001de9i627b6d99e3h)
   [6]: http://www.wolframalpha.com/
   [7]: http://xebee.xebia.in/wp-content/uploads/2009/12/MSP1103198b609ebc5ig7eh00002bhgh3b116edg9h2.gif (MSP1103198b609ebc5ig7eh00002bhgh3b116edg9h2)
   [8]: http://commons.apache.org/math/api-1.1/index.html