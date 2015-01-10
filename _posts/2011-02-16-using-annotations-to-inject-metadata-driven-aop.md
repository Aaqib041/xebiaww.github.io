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

New releases of compilers are getting more and more cautious on taking off the pain of too much verbosity and making the code look much more cleaner, understandable and easily modifiable. A major step that has been taken in Java side is introduction of annotations. Now that they have introduced annotations, plugging in an annotation processor still seems like an unsolved job requiring changes of the level of JVM startup arguments.    
**_Issues with existing APT (Annotation Processing Tool) :_**

  * The existing API involves changes to the JVM arguments in case some change is required.
  * Changes at runtime will not be possible as once a annotation processor is started, it will add the processing instructions. You cannot change this behavior in an already running application.
I was reading through one article that involved AOP and looked at one of the sample snippets that described about instrumenting Transactional attributes, so called boiler plate code for handling transactions into the services. The text clearly mentioned that it is possible or rather very easy to add metadata based advising into the code. After reading that line, I actually thought that it can be a possibility that the metadata processor or so called [Pointcut][1] can be built based on annotation processing and the [Advice][2] implementation can contain the code that you need to instrument or simply hide from your code because you think it is boiler plate.

By implementing this mechanism, you just change your application context file and thus avoid the headache of changing the startup arguments. Also you can [configure your context][3] prior to starting in case you don't want your annotations to become active.   
**_The code part :_** Usually its the most tricky part but in our case, this seems the least complex. There is one class named [DefaultAdvisorAutoProxyCreator][4] which needs to be added to the usual **applicationContext.xml** file. You also need to create your own [Annotation][5] and define its settings. Last but not the least, you need a [PointcutAdvisor][6] which will tell spring about which classes to process and what should be the processing.

In the sample code below, I would be adding a **SpookyAnnotation** on top of a bean and will allow the **SpookyingInterceptor** to create a proxy of the bean with the injected code. The _SpookyingInterceptor_ does nothing but adds the advice of a _System.out.println_ statement in the methods of the bean which has this annotation present.

_applicationContext.xml_ [code language="xml"] <bean:bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator" /> <bean:bean class="test.beans.interceptor.SpookyingInterceptor" /></pre> 
 ```

_Annotation_ ``` 
 @Retention(value= RetentionPolicy.RUNTIME) public @interface SpookyAnnotation { boolean insertSpookiness() default true; } 
 ```

_Interceptor class_ ``` 
 package test.beans.interceptor;

import .... import test.annotations.SpookyAnnotation; public class SpookyingInterceptor extends AbstractPointcutAdvisor{
    
    
    public Pointcut getPointcut() {
        return new Pointcut() {
            public MethodMatcher getMethodMatcher() {
                return new MethodMatcher() {
                    public boolean matches(Method arg0, Class&lt;?&gt; arg1, Object[] arg2) {
                        .....
                    }
                    public boolean matches(Method arg0, Class&lt;?&gt; arg1) {
                        .....
                    }
                    public boolean isRuntime() {
                        .....
                    }
                };
            }
    
            public ClassFilter getClassFilter() {
                return new ClassFilter() {
                    public boolean matches(Class&lt;?&gt; clazz) {
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
                System.out.println(&quot;The method &quot; + method.getName() 
                                        + &quot; has been spooked!!&quot;);
            }
        };
    }
    

} 
 ```

_Bean/Service classes_ ``` 
 package test.beans; import test.annotations.SpookyAnnotation; @SpookyAnnotation(insertSpookiness = false) public class BeanWithNonSpookyCode { public void callMe() { System.out.println("non spooky one has been called"); } } 
 ```

``` 
 package test.beans; public class AnotherBeanWithNonSpookyCode { public void callMe() { System.out.println("non spooky one has been called"); } } 
 ```

``` 
 package test.beans; import test.annotations.SpookyAnnotation; @SpookyAnnotation public class BeanWithSpookyCode { public void callMe() { System.out.println("spooky one has been called"); } } 
 ```

_Bean definitions_ [code language="xml"] <bean:bean id="beanWithNonSpooky" class="test.beans.BeanWithNonSpookyCode" /> <bean:bean id="anotherBeanWithNonSpooky" class="test.beans.AnotherBeanWithNonSpookyCode" /> <bean:bean id="beanWithSpooky" class="test.beans.BeanWithSpookyCode" /> 
 ```

[MethodBeforeAdvice][7] is a standard class which instruments the code in before method before the method call.   


**_Conclusion :_**

  * This style of coding is very highly plug and play because removing the **PointcutAdvisor** bean from **applicationContext.xml** will stop annotation processing for this particular annotation and you just need to add annotation to the class which you need to apply the **Advice** to. Rest of the code would remain untouched.
  * You might feel some startup delay with this code because initializing this structure and instrumenting code surely needs some time, but execution time remains unaffected.
  * You need **CGLIB** in your class-path for using [AOP][8]. Rest is normal spring dependencies that are added to the project. 
  * Java version should at-least be **1.5** because annotations are not supported below that.

**~~~Happy Annotating~~~**

   [1]: http://static.springsource.org/spring/docs/2.0.x/api/org/springframework/aop/Pointcut.html
   [2]: http://aopalliance.sourceforge.net/doc/org/aopalliance/aop/Advice.html
   [3]: http://xebee.xebia.in/2010/12/08/spring-altering-your-applicationcontext-at-runtime/
   [4]: http://static.springsource.org/spring/docs/2.0.x/api/org/springframework/aop/framework/autoproxy/DefaultAdvisorAutoProxyCreator.html
   [5]: http://download.oracle.com/javase/1.5.0/docs/guide/language/annotations.html
   [6]: http://static.springsource.org/spring/docs/2.0.x/api/org/springframework/aop/PointcutAdvisor.html
   [7]: http://static.springsource.org/spring/docs/2.0.x/api/org/springframework/aop/MethodBeforeAdvice.html
   [8]: http://static.springsource.org/spring/docs/2.0.1/reference/aop-api.html