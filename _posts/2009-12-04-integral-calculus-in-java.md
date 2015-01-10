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

<p>Recently, I got an opportunity to explore integral calculus in Java. For a finance project I had to calculate the <a href=" http://en.wikipedia.org/wiki/Greeks_(finance)">greek metrics </a>(Fair value, Implied Volatility, Delta etc) using <a href="http://en.wikipedia.org/wiki/Black%E2%80%93Scholes">BlackScholes model</a>. The formulas for calculating the metrics using Black Scholes model revolved around the following integration 
<img src="http://xebee.xebia.in/wp-content/uploads/2009/12/94db15fc7182a86e64c21f5721d4871b.png" alt="94db15fc7182a86e64c21f5721d4871b" title="94db15fc7182a86e64c21f5721d4871b" width="380" height="47" class="alignnone size-full wp-image-2672" />.
<!--more--></p>
<p>This integration acted as a showstopper and forced me to search for the APIs available for performing mathematical calculus in Java. Google god came to rescue and provided me with <a href="http://code.google.com/p/javacalculus/">Google code API for Java Calculus</a>.</p>
<p>With Java calculus API in hand, I wrote the program for performing following integration.<img src="http://xebee.xebia.in/wp-content/uploads/2009/12/MSP1037198b609ebc5ig7eh00001de9i627b6d99e3h.gif" alt="MSP1037198b609ebc5ig7eh00001de9i627b6d99e3h" title="MSP1037198b609ebc5ig7eh00001de9i627b6d99e3h" width="185" height="37" class="alignnone size-full wp-image-2685" />
[java]
...
 CalcParser calcParser2 = new CalcParser(&quot;x^3&quot;);
 CalcObject calcObject = calcParser2.parse();
 CalcINT calcInt = new CalcINT();
 CalcSymbol calcSymbol = new CalcSymbol(&quot;x&quot;);
 calcObject = calcInt.integrate(calcObject,calcSymbol);
 CalculusEngine calculusEngine = new CalculusEngine();
 calculusEngine.execute(calcObject.toString().replace('x', '5'));
 System.out.println(calculusEngine.getResult());
...
[/java]
<b>OUTPUT:</b>
Input: MULTIPLY(POWER(5,ADD(3,1)),POWER(ADD(3,1)),-1)
Evaluation Queue: MULTIPLY(POWER(5,ADD(3,1)),POWER(ADD(3,1)),-1)
Processed Queue: MULTIPLY(-625,POWER(4))
Output: -625*4</p>
<p>Surprised !!! It should have been 625/4.
API wrong :( I double checked my code. I tweaked into the source code of the API and figured out that there is a minor mistake in the source code of the API in the class: javacalculus\src\javacalculus\evaluator\CalcInt (line 68 to 78). [java]
return CALC.MULTIPLY.createFunction
         (CALC.POWER.createFunction(firstObj, 
          CALC.ADD.createFunction(secondObj, CALC.ONE)),
      CALC.POWER.createFunction(CALC.ADD.createFunction(secondObj, 
         CALC.ONE)), CALC.NEG_ONE); [/java]
And it should be fixed as follows:
<i>Note the difference in order of closing braces in the last line</i>[java]
return CALC.MULTIPLY.createFunction
         (CALC.POWER.createFunction(firstObj, 
          CALC.ADD.createFunction(secondObj, CALC.ONE)),
          CALC.POWER.createFunction(CALC.ADD.createFunction(secondObj, 
          CALC.ONE), CALC.NEG_ONE));[/java]
With this discovery, I had 2 options either to fix the API or search for the other solution. 
Being the fan of <a href="http://www.wolframalpha.com/">Wolfram Alpha</a> search, I decided to look for another solution. Since, the formula had only one integration, I decided to evaluate the integral offline through WolfRam. I substituted the result of integration in place of the integrals in the formulas. </p>
<p>Wolfram engine provided me the following result:<img src="http://xebee.xebia.in/wp-content/uploads/2009/12/MSP1103198b609ebc5ig7eh00002bhgh3b116edg9h2.gif" alt="MSP1103198b609ebc5ig7eh00002bhgh3b116edg9h2" title="MSP1103198b609ebc5ig7eh00002bhgh3b116edg9h2" width="234" height="48" class="alignnone size-full wp-image-2686" /></p>
<p>The "erf" function is readily available in <a href="http://commons.apache.org/math/api-1.1/index.html">Apache Commons Math</a> API.</p>
<p>This exercise enabled me to look into the relatively unexplored area of integral calculus in Java. I hope my findings above, would be of help for the people researching in this area. </p>