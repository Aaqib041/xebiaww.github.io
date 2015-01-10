---
layout: post
header-img: img/default-blog-pic.jpg
author: nikhil.srivastava
description: 
post_id: 9885
created: 2011/10/09 03:11:43
created_gmt: 2011/10/08 22:11:43
comment_status: open
---

# when & why rule engines?

<p><head>
<script type="text/javascript"></p>
<p>var _gaq = _gaq || [];
  _gaq.push(['_setAccount', 'UA-30218999-1']);
  _gaq.push(['_trackPageview']);</p>
<p>(function() {
    var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
    ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
    var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
  })();</p>
<p></script>
</head>
<span style="line-height: normal;">For many complex applications, business policies keep on changing very frequently and mostly they are too complex to be modified in the source code.</span>
<p style="line-height: normal;" class="MsoNormal"><span><span style="background: white;">Such application includes applications in domains such as insurance (for example, insurance rating), financial services (loans, fraud detection, claims routing and management), government (application process and tax calculations), telecom customer care and billing (promotions for long distance calls that needs to be integrated into the billing system), ecommerce (personalizing the user's experience)</span></span><span><span style="mso-bidi-font-family: Arial; color: black; background: white;"> </span></span></p>
<p style="line-height: normal;" class="MsoNormal"><span><span style="background: white;">If your business logic code includes thousands of if-else statements (that too keep changing</span></span><span><span style="font-size: 10.0pt; background: white;"> </span><span style="background: white;">frequently), you should consider using a</span></span><span><span style="font-size: 10.0pt; background: white;"> </span><strong><span style="background: white;">Rule Engine</span></strong></span><span><span style="font-size: 10.0pt; background: white;"> </span><span style="background: white;">else not</span></span><span><span style="font-size: 10.0pt; background: white;">.</span></span></p>
<!--more-->
<p style="line-height: normal;" class="MsoNormal"><span><span style="mso-bidi-font-family: Arial; color: black; background: white;">Few challenges in such applications:</span></span></p>
<p style="line-height: normal;" class="MsoNormal"><span><span><span>1.<span style="font: 7.0pt &quot;Times New Roman&quot;;"> </span></span></span></span><span>The process of automating business policies, procedures, and business logic is simply too dynamic to manage effectively as application source code.</span></p>
<strong>Solution</strong>: <span> </span>A rule engine enables to accommodate the inevitable business/decision logic <span> </span>that are consequences of the market changes that no one can predict or plan for during design.</p>
<p><span style="line-height: normal;"><span><span><span>2.<span style="font: 7.0pt &quot;Times New Roman&quot;;"> </span></span></span></span><span>Having business rules mixed with rest of our code is not a good approach. Doing so we end up having a tight coupling of the application-code with the business policies.</span></span></p>
<p><strong>Solution: </strong>The rule engine applies rules and actions as defined by end users without affecting how the application runs. The application is built to deal with the rules, which are designed separately.</p>
<p><span style="line-height: normal;"><span><span><span>3.<span style="font: 7.0pt &quot;Times New Roman&quot;;"> </span></span></span></span><span>Maintaining complex Boolean logic becomes a difficult task. Even a simple change to one line of code can cost an organization thousands of dollars.</span></span></p>
<p><strong>Solution</strong>:<span> </span>Using business rules can help you develop <strong>more cost effective</strong> and <strong>more agile</strong> applications.</p>
<p><span style="line-height: normal;"><span><span><span>4.<span style="font: 7.0pt &quot;Times New Roman&quot;;"> </span></span></span></span><span>Sometimes when there are so many rules, developers find it difficult to decide the order of multiple if- else statements.</span></span></p>
<p><strong>Solution: </strong>Rule Engines have better mechanism for the deciding execution order <span> </span>of the rules through <strong>Salience</strong>(priority ordering).</p>
<p><span style="line-height: normal;"><span><span><span>5.<span style="font: 7.0pt &quot;Times New Roman&quot;;"> </span></span></span></span><span>Every time when a developer adds a new if-else rule to the application, whole application requires re-building, compiling and deploying.</span></span>
<p style="text-indent: -.25in; line-height: normal; mso-list: l0 level1 lfo1;" class="MsoListParagraphCxSpMiddle"><span> <strong>
Solution</strong>: A rule engine evaluates and executes rules, which are expressed as if-then statements. The power of business rules lies in their ability both to separate knowledge from its implementation logic and to be changed without changing source code.</span></p>
<p style="text-indent: -.25in; line-height: normal; mso-list: l0 level1 lfo1;" class="MsoListParagraphCxSpMiddle"><span><span><span>6.<span style="font: 7.0pt &quot;Times New Roman&quot;;"> </span></span></span></span><span>Developers should least bother about the policy changes; instead this task should be delegated to some business experts (business analysts).</span></p>
<strong>Solution</strong>: As somebody who has knowledge of business rules, you'll be able to feed the Rule Engine with what you know. The tool that is used to do this is known as the <strong>Business Rules Management System.
</strong></p>
<p><strong><span style="font-weight: normal; line-height: normal;"><span><span><span>7.<span style="font: 7.0pt &quot;Times New Roman&quot;;"> </span></span></span></span><span>E</span>ven for a small change in business logic developer has to recode.</span></strong></p>
<p><strong>Solution:</strong> We should rather look for some alternatives where business related changes can be done by non-technical business experts.In rule engine based applications we can achieve this just by minor changes in rules which even a business person can do very easily</p>
<p>References: http://java.sun.com/developer/technicalArticles/J2SE/JavaRule.html</p>