---
layout: post
header-img: img/default-blog-pic.jpg
author: PGarg
description: 
post_id: 3527
created: 2010/05/24 10:31:25
created_gmt: 2010/05/24 05:31:25
comment_status: open
---

# Functional Programming in Java Using "lambdaj"

<pre><code>The new JVM based Post Modern Functional Languages (as creator of Scala, Martin Ordesky named them) like Scala and JRuby which are influenced from both Object Oriented and Functional programming languages  have gone far ahead in the terms of functionality that they offer as compared to Java.

Apart from the object richness, these languages are loaded with most characteristics of a FL like (first class functions, closures, traits static/dynamic type inference, lazy evaluation, etc) which helps in bringing more modularity to the code and from aesthetics perspective these languages are less verbose and more readable as compared to java.

Few people have put efforts and wrote libraries which can help in leveraging some of these features in Java too. The motivation is not to compete with scala or ruby as they are far ahead. It’s like if you don’t have the option to switch over to these languages (especially scala) or you are still waiting for the Java7 release.

The ones which I have been using are &lt;a href="http://code.google.com/p/lambdaj/"&gt;lambdaj&lt;/a&gt; written by Mario Fusco and &lt;a href="http://www.jroller.com/ghettoJedi/entry/enumerable_java_0_2_2"&gt;Enumerable&lt;/a&gt; written by Hakan Raberg. Others I am aware of but not used are &lt;a href="http://functionalj.sourceforge.net/"&gt;FunctionalJ&lt;/a&gt; and &lt;a href="file:///C:/Users/Pratik/Desktop/functionaljava.org/"&gt;FunctionalJava&lt;/a&gt; .Most of them provide a similar set of features and differ bit in syntax , it only like the one to which you get accustomed too.
</code></pre>
<p><!--more-->
    lambdaj offers more features compared to Enumerable. Just include the jar and it works, since it uses the proxies for the implementation it’s a bit slow. Enumerable takes different approach for closure support it uses ASM byte code manipulation to capture expressions as closures. So you have to perform an extra simple step of compiling the lamdas before hand.</p>
<pre><code>Lets dive into some of the code snippets that we regularly write in java and see how they can be made concise and readable using these libraries.

&lt;span style="text-decoration: underline;"&gt;Constructs for Collections&lt;/span&gt; - Iterate through a Collection&lt;String&gt; and return a collection&lt;String&gt; starting with "inv"

&lt;em&gt;Using Java&lt;/em&gt;

[sourcecode lang="java"]
List&amp;lt;String&amp;gt; list = Arrays.asList(&amp;quot;invalid1&amp;quot;, &amp;quot;invalid2&amp;quot;);

List&amp;lt;String&amp;gt; filteredList = new ArrayList&amp;lt;String&amp;gt;();

for(String string : list) {

  if(string.startsWith(&amp;quot;inv&amp;quot;))

    filteredList.add(string);

}
return filteredList;

[/sourcecode]

&lt;em&gt;Using lambdaj&lt;/em&gt;
[sourcecode lang="java"]
 List&amp;lt;String&amp;gt; list = Arrays.asList(&amp;quot;invalid1&amp;quot;, &amp;quot; invalid2&amp;quot;);

return filter(startsWith(&amp;quot;inv&amp;quot;), list);
[/sourcecode]
&lt;em&gt;Using Enumerable&lt;/em&gt;
[sourcecode lang="java"]
List&amp;lt;/span&amp;gt; list = Arrays.asList(&amp;quot;invalid1&amp;quot;, &amp;quot; invalid2&amp;quot;);

return Collect(list,fn(s,s.startsWith(&amp;quot;inv&amp;quot;));
[/sourcecode]
Filter/Collect are static methods provided in the lambdaj/Enumerable library which do the actual filtering/collection on collection. "&lt;strong&gt;startsWith"&lt;/strong&gt; is expressed as a hamcrest matcher and the lambda expression fn(s,s.startsWith("inv")) will be converted to below , after you do the lambda expression weaving.
[sourcecode lang="java"]
new Fn1() {
public Object call(Object arg) {
return (String) arg.startsWith(&amp;quot;inv&amp;quot;);
}};
[/sourcecode]

In this case the 6 lines of code are just reduced to 1.
</code></pre>
<p><!--more-->
    <span style="text-decoration: underline;">Closures</span></p>
<pre><code>We have a scenario where you have search for an employee, compare its name with some pattern and if it matches, send the message. Now gradually you can have more criteria on which you want to match, you want a regex match or don’t want to use ignore case and many more.  For all those cases you will end up duplicating the sendMessage() method with different line instead of the one shown marked with dashes(-).

&lt;em&gt;Using Java&lt;/em&gt;&lt;em&gt; &lt;/em&gt;
[sourcecode lang="java"]
public boolean  sendMessage(String pattern) {

    FinderService service = context.getService(&amp;quot;finderService&amp;quot;);

    boolean found = false;

        try {

        Employee employee = service.find();

        if (null != employee) {

    --------  if (employee.getName().equalsIgnoreCase(pattern)) -------
           found = true;

        }

        if(found)  service.sendMessage(&amp;quot;Found the employee&amp;quot;);

       } catch (Exception e) {

         found = false;

       } 
    return found;  
}


[/sourcecode]
&lt;em&gt;Using lambdaj&lt;/em&gt;&lt;em&gt; &lt;/em&gt;
[sourcecode lang="java"]
public boolean sendMessageUsingIgnoreCase() {

    Closure2&amp;lt;Employee, String&amp;gt; ignoreCaseClosure  = closure(Employee.class, String.class);

    {of(this).ignore(var(Employee.class), var(String.class));}

    return sendMessage(&amp;quot;pattern&amp;quot;, ignoreCaseClosure);

}



public boolean sendMessageUsingRegexCriteria() {

    Closure2&amp;lt;Employee, String&amp;gt; regexClosure   =closure(Employee.class,String.class);

    {of(this).regex(var(Employee.class), var(String.class));}

    return sendMessage(&amp;quot;pattern&amp;quot;, regexClosure);
}



private boolean ignore(Employee emp, String pattern) {

    return emp.getName().equalsIgnoreCase(pattern);
}



private boolean regex(Employee employee, String pattern) {

    Pattern p = Pattern.compile(pattern);

    return p.matcher(employee.getName()).find();

}



private boolean sendMessage(String pattern,Closure2&amp;lt;Employee, String&amp;gt; closure) {

        FinderService service = context.getService(&amp;quot;finderService&amp;quot;);

        boolean found = false;

            try {

            Employee employee = service.find();

            if (null != employee) {

    ----------  if ((Boolean) closure.apply(employee, pattern)){ ---------
               found = true;

            }

            if(found)  service.sendMessage(&amp;quot;Found the employee&amp;quot;);

           } catch (Exception e) {

             found = false;

           } 
        return found;  
}
[/sourcecode]
We created two closures ignoreCaseClosure and regexClosure which are basically the ignore and regex methods that will be used inside the sendMessage() method . This was you can create as many closures as you want based on different criteria’s and pass them to sendMessage().Here we extracted the logic which is variable and avoid duplication.



&lt;span style="text-decoration: underline;"&gt;Matchers&lt;/span&gt;

Consider this code snippet in java, which iterates though a collection and creates a concatenation string in the format "key = Value &amp;amp; key = value &amp;amp;...".While processing it ignores the entries which have keys starting with "graphType" or "type".

&lt;em&gt;Using Java&lt;/em&gt;
[sourcecode lang="java"]
for (Entry&amp;lt;String, String&amp;gt; param : params.entries()) {

    if (!&amp;quot;graphPage&amp;quot;.equals(param.getKey()) &amp;amp;&amp;amp; !&amp;quot;type&amp;quot;.equals(param.getKey())) {

        if (sb == null) {

            sb= &amp;lt;strong&amp;gt;new&amp;lt;/strong&amp;gt; StringBuffer();

    } else {

    sb.append('&amp;amp;');

}

sb.append(param.getKey()).append('=').append(param.getValue());

[/sourcecode]

&lt;em&gt;Using lambdaj &lt;/em&gt;&lt;em&gt; &lt;/em&gt;
[sourcecode lang="java"]
OrMatcher&amp;lt;String&amp;gt; toExclude = or(equalTo(&amp;quot;type&amp;quot;), equalTo(&amp;quot;graphPage&amp;quot;));

List&amp;lt;Entry&amp;lt;String, String&amp;gt;&amp;gt; paramsList = select(params.entries(),  not(having(on(Entry.class).getKey(), toExclude)));

String graphLink = join(paramsList, &amp;quot;&amp;amp;&amp;quot;);
[/sourcecode]

The code is almost self explanatory you create an orMatcher for the types to be excluded. Then you apply select on the list using that matcher which return you a List&lt;Entries&gt;. Finally a join on it to return a string of concatenated entries.

There are more features available with these libraries which are quite useful. One way of using them in you application is to create a seperate maven project with the available source code of library and then use it. This is also help in modifying/adding new stuff to the library as per project requirements.
</code></pre>