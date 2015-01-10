---
layout: post
header-img: img/default-blog-pic.jpg
---

# when & why rule engines?

For many complex applications, business policies keep on changing very frequently and mostly they are too complex to be modified in the source code.

Such application includes applications in domains such as insurance (for example, insurance rating), financial services (loans, fraud detection, claims routing and management), government (application process and tax calculations), telecom customer care and billing (promotions for long distance calls that needs to be integrated into the billing system), ecommerce (personalizing the user's experience)

If your business logic code includes thousands of if-else statements (that too keep changing frequently), you should consider using a **Rule Engine** else not.

Few challenges in such applications:

1. The process of automating business policies, procedures, and business logic is simply too dynamic to manage effectively as application source code.

**Solution**:  A rule engine enables to accommodate the inevitable business/decision logic  that are consequences of the market changes that no one can predict or plan for during design. 2. Having business rules mixed with rest of our code is not a good approach. Doing so we end up having a tight coupling of the application-code with the business policies. **Solution: **The rule engine applies rules and actions as defined by end users without affecting how the application runs. The application is built to deal with the rules, which are designed separately. 3. Maintaining complex Boolean logic becomes a difficult task. Even a simple change to one line of code can cost an organization thousands of dollars. **Solution**: Using business rules can help you develop **more cost effective** and **more agile** applications. 4. Sometimes when there are so many rules, developers find it difficult to decide the order of multiple if- else statements. **Solution: **Rule Engines have better mechanism for the deciding execution order  of the rules through **Salience**(priority ordering). 5. Every time when a developer adds a new if-else rule to the application, whole application requires re-building, compiling and deploying.

** Solution**: A rule engine evaluates and executes rules, which are expressed as if-then statements. The power of business rules lies in their ability both to separate knowledge from its implementation logic and to be changed without changing source code.

6. Developers should least bother about the policy changes; instead this task should be delegated to some business experts (business analysts).

**Solution**: As somebody who has knowledge of business rules, you'll be able to feed the Rule Engine with what you know. The tool that is used to do this is known as the **Business Rules Management System. ** **7. Even for a small change in business logic developer has to recode.** **Solution:** We should rather look for some alternatives where business related changes can be done by non-technical business experts.In rule engine based applications we can achieve this just by minor changes in rules which even a business person can do very easily References: http://java.sun.com/developer/technicalArticles/J2SE/JavaRule.html