---
layout: post
header-img: img/default-blog-pic.jpg
author: rgarg
description: 
post_id: 7531
created: 2011/02/16 19:42:04
created_gmt: 2011/02/16 14:42:04
comment_status: open
---

# Using Annotations To Inject - Metadata Driven AOP

<p>New releases of compilers are getting more and more cautious on taking off the pain of too much verbosity and making the code look much more cleaner, understandable and easily modifiable. A major step that has been taken in Java side is introduction of annotations. Now that they have introduced annotations, plugging in an annotation processor still seems like an unsolved job requiring changes of the level of JVM startup arguments. 
<!--more-->
<br/>
<strong><em>Issues with existing APT (Annotation Processing Tool) :</em></strong>
<ul>
<li>The existing API involves changes to the JVM arguments in case some change is required.</li>
<li>Changes at runtime will not be possible as once a annotation processor is started, it will add the processing instructions. You cannot change this behavior in an already running application.</li>
</ul>
I was reading through one article that involved AOP and looked at one of the sample snippets that described about instrumenting Transactional attributes, so called boiler plate code for handling transactions into the services. The text clearly mentioned that it is possible or rather very easy to add metadata based advising into the code. After reading that line, I actually thought that it can be a possibility that the metadata processor or so called <a href="http://static.springsource.org/spring/docs/2.0.x/api/org/springframework/aop/Pointcut.html">Pointcut</a> can be built based on annotation processing and the <a href="http://aopalliance.sourceforge.net/doc/org/aopalliance/aop/Advice.html">Advice</a> implementation can contain the code that you need to instrument or simply hide from your code because you think it is boiler plate.</p>
<p>By implementing this mechanism, you just change your application context file and thus avoid the headache of changing the startup arguments.
Also you can <a href="http://xebee.xebia.in/2010/12/08/spring-altering-your-applicationcontext-at-runtime/">configure your context</a> prior to starting in case you don't want your annotations to become active.
<br/>
<strong><em>The code part :</em></strong>
Usually its the most tricky part but in our case, this seems the least complex. There is one class named <a href="http://static.springsource.org/spring/docs/2.0.x/api/org/springframework/aop/framework/autoproxy/DefaultAdvisorAutoProxyCreator.html">DefaultAdvisorAutoProxyCreator</a> which needs to be added to the usual <strong>applicationContext.xml</strong> file. You also need to create your own <a href="http://download.oracle.com/javase/1.5.0/docs/guide/language/annotations.html">Annotation</a> and define its settings. Last but not the least, you need a <a href="http://static.springsource.org/spring/docs/2.0.x/api/org/springframework/aop/PointcutAdvisor.html">PointcutAdvisor</a> which will tell spring about which classes to process and what should be the processing.</p>
<p>In the sample code below, I would be adding a <strong>SpookyAnnotation</strong> on top of a bean and will allow the <strong>SpookyingInterceptor</strong> to create a proxy of the bean with the injected code. The <em>SpookyingInterceptor</em> does nothing but adds the advice of a <em>System.out.println</em> statement in the methods of the bean which has this annotation present.</p>
<p><center><em>applicationContext.xml</em></center>
[code language="xml"]
&lt;bean:bean 
  class=&quot;org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator&quot; /&gt;
&lt;bean:bean 
  class=&quot;test.beans.interceptor.SpookyingInterceptor&quot; /&gt;&lt;/pre&gt;
[/code]</p>
<p><center><em>Annotation</em></center>
[code language="java"]
@Retention(value= RetentionPolicy.RUNTIME)
public @interface SpookyAnnotation {
    boolean insertSpookiness() default true;
}
[/code]</p>
<p><center><em>Interceptor class</em></center>
[code language="java"]
package test.beans.interceptor;</p>
<p>import ....
import test.annotations.SpookyAnnotation;
public class SpookyingInterceptor extends AbstractPointcutAdvisor{</p>
<pre><code>public Pointcut getPointcut() {
    return new Pointcut() {
        public MethodMatcher getMethodMatcher() {
            return new MethodMatcher() {
                public boolean matches(Method arg0, Class&amp;lt;?&amp;gt; arg1, Object[] arg2) {
                    .....
                }
                public boolean matches(Method arg0, Class&amp;lt;?&amp;gt; arg1) {
                    .....
                }
                public boolean isRuntime() {
                    .....
                }
            };
        }

        public ClassFilter getClassFilter() {
            return new ClassFilter() {
                public boolean matches(Class&amp;lt;?&amp;gt; clazz) {
                    if(clazz.getAnnotation(SpookyAnnotation.class) == null) {
                        return false;
                    } else {
                        return clazz.getAnnotation(SpookyAnnotation.class)
                            .insertSpookiness();
                    }
                }
            };
        }
    };
}

public Advice getAdvice() {
    return new MethodBeforeAdvice() {
        public void before(Method method, Object[] methodArgs, Object objOfClazz)
                throws Throwable {
            System.out.println(&amp;quot;The method &amp;quot; + method.getName() 
                                    + &amp;quot; has been spooked!!&amp;quot;);
        }
    };
}
</code></pre>
<p>}
[/code]</p>
<p><center><em>Bean/Service classes</em></center>
[code language="java"]
package test.beans;
import test.annotations.SpookyAnnotation;
@SpookyAnnotation(insertSpookiness = false)
public class BeanWithNonSpookyCode {
    public void callMe() {
        System.out.println(&quot;non spooky one has been called&quot;);
    }
}
[/code]</p>
<p>[code language="java"]
package test.beans;
public class AnotherBeanWithNonSpookyCode {
    public void callMe() {
        System.out.println(&quot;non spooky one has been called&quot;);
    }
}
[/code]</p>
<p>[code language="java"]
package test.beans;
import test.annotations.SpookyAnnotation;
@SpookyAnnotation
public class BeanWithSpookyCode {
    public void callMe() {
        System.out.println(&quot;spooky one has been called&quot;);
    }
}
[/code]</p>
<p><center><em>Bean definitions</em></center>
[code language="xml"]
    &lt;bean:bean id=&quot;beanWithNonSpooky&quot; class=&quot;test.beans.BeanWithNonSpookyCode&quot;  /&gt;
    &lt;bean:bean id=&quot;anotherBeanWithNonSpooky&quot; 
                class=&quot;test.beans.AnotherBeanWithNonSpookyCode&quot;  /&gt;
    &lt;bean:bean id=&quot;beanWithSpooky&quot; class=&quot;test.beans.BeanWithSpookyCode&quot;  /&gt;
[/code]</p>
<p><a href="http://static.springsource.org/spring/docs/2.0.x/api/org/springframework/aop/MethodBeforeAdvice.html">MethodBeforeAdvice</a> is a standard class which instruments the code in before method before the method call.
<br/></p>
<p><strong><em>Conclusion :</em></strong>
<ul>
<li>This style of coding is very highly plug and play because removing the <strong>PointcutAdvisor</strong> bean from <strong>applicationContext.xml</strong> will stop annotation processing for this particular annotation and you just need to add annotation to the class which you need to apply the <strong>Advice</strong> to. Rest of the code would remain untouched.</li>
<li>You might feel some startup delay with this code because initializing this structure and instrumenting code surely needs some time, but execution time remains unaffected.</li>
<li>You need <strong>CGLIB</strong> in your class-path for using <a href="http://static.springsource.org/spring/docs/2.0.1/reference/aop-api.html">AOP</a>. Rest is normal spring dependencies that are added to the project. </li>
<li>Java version should at-least be <strong>1.5</strong> because annotations are not supported below that.</li>
</ul></p>
<p><center><strong>~~~Happy Annotating~~~</strong></center></p>