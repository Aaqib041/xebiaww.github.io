---
layout: post
header-img: img/default-blog-pic.jpg
author: aagrawal
description: 
post_id: 806
created: 2008/11/19 22:50:18
created_gmt: 2008/11/19 20:50:18
comment_status: open
---

# Flex Testing Toolkit

My recent assignment at **Albumprinter **gave an opportunity to play with flex testing tools available out there. Coming back to serious flex development after more than a year was a pleasant surprise. The state of flex when I last did something on it was for sure not “mature”. There weren’t many frameworks around.

But things have changed a great deal since then. Not only do we have frameworks implementing MVC (**Cairngorm**) but also some standard extensions (**UM extensions**) to those frameworks, we also have spring like IOC frameworks (**Prana**) for Flex.  Maybe I will keep the discussion about these frameworks out for now, as they each deserve a dedicated blog. Here I would like to share my surprise with the testing tools available for flex:  
**[FlexUnit][1]** – As the name suggests, this is a sibling to XUnit frameworks, and doesn’t really has any learning curve for anyone coming from JUnit or CUnit genre.

A typical test would look like:

> 
>     public function testDeposit():void {
>         var bankAccount:BankAccount=new BankAccount();
>         bankAccount.deposit(50);
>         assertTrue("Balance on a new account after 50 deposit is 50",
>                                 bankAccount.getBalance() == 50);
>         bankAccount.deposit(25);
>         assertEquals("Balance after 50 deposit and another 25 deposit is 75",
>                               75,bankAccount.getBalance());
>       }
>     
>     public function testWithdraw():void {
>         var bankAccount:BankAccount=new BankAccount();
>         bankAccount.deposit(100);
>         bankAccount.withdraw(50);
>         assertTrue("Balance on a new account after 100 deposit and a 50 withdraw is 50",
>                               bankAccount.getBalance() == 50);
>       }
>     

And then we add a testrunner (similar to JUint Runner plugin of eclipse) to the main mxml:

> `<flexunit:TestRunnerBase id="testRunner" width="100%" height="100%" />`

We add the test suite to the above test runner like this:

> 
>     private function onCreationComplete():void
>     {
>         testRunner.test = createSuite();
>         testRunner.startTest();
>     }
>     
>     // Creates the test suite to run
>     private function createSuite():TestSuite {
>         var testSuite:TestSuite = new TestSuite();
>     
>         testSuite.addTestSuite( BankAccountTest );
>     
>         return testSuite;
>     }   

  
On running, this might look like this:

![][2]

  
  
**[dpUInt/fluint][3]** Though FlexUnit is a great advance for serious unit testing of presentation layer, it lacked some features like easy support of asynchronous tests. This becomes a limitation, given that flex finds application in many event-driven asynchronous paradigms. Although there is support for async operations in FlexUnit, they need some work. To resolve this bottleneck “**digital primates**” introduced dpUInt – The Digital Primate Unit and Integration testing frameworjk and with their first official release renamed it to fluint. This looks very similar to FlexUnit in use, but is not an extension of the same.

**[Flex Monkey][4]** FlexMonkey is a unit testing framework for Flex apps that provides for automating the testing of Flex UI functionality. If you are already in love with Selenium, you can think of FlexMonkey as “Selenium for Flex”. FlexMonkey can record and play back Flex UI interactions, and generates ActionScript-based testing scripts that can easily be included within a continuous integration process. It uses the Flex Automation API and was created by adapting Adobe's sample application, AutoQuick.

FlexMonkey has been donated to the Flex community by [Gorilla Logic][5], who developed FlexMonkey because of their belief that only a monkey would develop code without being able to automate their unit testing. 
    
    
    Essentially you use it in 3 modes:
    
    
    
    
    
    
      1. Record user interactions and play them back as required.
    
    
      2. Write unit tests to test UI functionality and play them programmatically.
    
    
      3. Probably the most interesting aspect is to combine the above two: Manually do the user interactions and record it.
    This automatically generates test case in action script!
    Just use this generated code in your testing environment and adapt it as required!
    

Additionally it has a “**flex spy**” tool that helps investigate the container hierarchy. This comes handy when you start anew on an already-developed product. This tool helps to quickly understand the code and the flex components used. It’s not just simple but also exciting to look at, when its in action. I started with this more as a flex toy to play around, but pretty soon, I realized its worth – A Monkey Test I had implemented to demonstrate a new feature broke after a week. It immediately indicated that the new features under development were affecting the existing ones. Monkey helped us to “fail early”, as is the spirit of Agile, and thus saved some time and resource for later-on “hot fixes” As much as its nice to look monkey-in-action, its also difficult to capture it in snapshots. But hereunder, I try to give a sample run of a Monkey test having two test cases:

![][6]

  
  
**[Flex Cover][7]** This is a test coverage tool. It not only gives the percentage coverage of lines and branches of code, but also updates itself on-the-fly at run time – as the test cases run, it updates its percent figures. It also marks the code that was not covered. Additionally, unlike clover test reports it specifically gives the number of times each branch was executed. For example, while clover indicates that a particular “if” statement is RED as it was called 2 times but one of the branches was not called even once - it doesn’t really indicates exactly which branch (if-branch or else-branch) was not executed. (As per my limited knowledge of clover, may be this is configurable; in that case; excuse me for my limited exposure to clover). The Flex Cover in above example explicitly mentions the number of times each branch was executed.

It incorporates a modified version of the AS3 compiler which inserts extra function calls in the code within the SWF or SWC output file. At runtime, these function calls send information on the application's code coverage to a separate tool; The modified compiler also emits a separate "coverage metadata" file that describes all the possible packages, classes, functions, code blocks and lines in the code, as well as the names of the associated source code files.

![][8]

  
  
For sure, its early days and I have been using these tools only for about a week now. But definitely its exciting times in flex world, and I’ll look forward to exploring more. Will keep you guys posted!

Abhishek.

   [1]: http://opensource.adobe.com/wiki/display/flexunit/FlexUnit;jsessionid=EA83719A76E96A6E06573D20920CF0EE
   [2]: http://xebee.xebia.in/wp-content/uploads/2008/11/flexunit.jpg ()
   [3]: http://code.google.com/p/dpuint/
   [4]: http://code.google.com/p/flexmonkey/
   [5]: http://www.gorillalogic.com/
   [6]: http://xebee.xebia.in/wp-content/uploads/2008/11/flexmonkey.jpg ()
   [7]: http://code.google.com/p/flexcover/
   [8]: http://xebee.xebia.in/wp-content/uploads/2008/11/flexcover.jpg ()