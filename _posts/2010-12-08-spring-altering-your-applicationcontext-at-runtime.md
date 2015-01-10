---
layout: post
header-img: img/default-blog-pic.jpg
author: rgarg
description: 
post_id: 6609
created: 2010/12/08 11:25:46
created_gmt: 2010/12/08 06:25:46
comment_status: open
---

# Spring - Altering your applicationContext at runtime

<p>In my previous blog over spring integration, I gave a hint over creating adapters dynamically. In this writeup, I would try to detail out the things a bit. Primarily spring is all about maintaining objects so called beans in the memory through a configuration file. A better design is the one which doesn't involve creation of objects in a way that its hard to find out where and why has it been created. Hence spring has gained a lot of popularity these days because it provides a mechanism to split the creation and binding part from the actual logic.
<!--more-->
<br/></p>
<p><em><strong>Practical application :</strong></em>
Spring application context needs to be modified only if there is a requirement of a parent context which provides a configuration management functionality over the actual application. In case you want to start or stop your application by giving a command or maybe some configuration that you want to change inside your context and restart it. A typical example could be a managed network messaging system that doesn't initially know the place that it needs to send data to. In such a case, the configuration is received while the application is running and the beans need to be created after the configuration has been received and broken into logical pieces.</p>
<p><em><strong>The code part :</strong></em>
For achieving this, the framework relies on a configuration file generally referred to as <em>applicationContext.xml</em> file. This file is an externalized source to describe and even bind the objects of classes written within your code together. You might have faced scenarios where you actually don't know what needs to be written inside the XML because either its being evaluated through some logic or work-flow that actually decides what needs to be created and passed.</p>
<p>Spring framework provides a simple and useful solution to this by giving the developer an option to create the beans and register them inside the context. This can be done by using an interface named <em>ConfigurableApplicationContext</em>.</p>
<p>The class hierarchy is somewhat like :
<blockquote>
<pre>   ApplicationContext
        -&gt; ConfigurableApplicationContext
            -&gt; Actually implemented classes like WebApplicationContext,
                ClassPathApplicationContext etc.
</pre>
</blockquote>
The purpose of putting in ConfigurableApplicationContext in between the hierarchy is just that you can get the <em>BeanFactory</em> object from SpringContext and register your own created objects in there. Following method is used to get the object of BeanFactory currently in use :
<blockquote>ConfigurableListableBeanFactory getBeanFactory() throws IllegalStateException;</blockquote>
We just need a <em>SingletonBeanRegistry</em> object to be able to register our custom created bean to the spring's context. The class hierarchy for the same goes like:
<blockquote>
<pre>   SingletonBeanRegistry
        -&gt; ConfigurableListableBeanFactory</pre>
</blockquote>
Thus, one can directly create a variable of type <em>SingletonBeanRegistry</em> by using this method. Now, in order to register your object to spring, just use the following code:
<blockquote>beanRegistry.registerSingleton("uniqueBeanName", object);</blockquote>
<em><strong>Points to take care :</strong></em>
There are certain things that need to be kept in mind before doing this otherwise application wil not boot up or might cause problems while running :
<ol>
    <li>The instance should be fully initialized before registering it to the context, i.e. spring will not call any callback methods that are supposed to be executed for doing the initialization like adding aspects, wiring of beans inside etc. However, Spring will call start and stop callbacks as and when the state of applicationContext changes.</li>
    <li>The instance will not receive any destructive callbacks either. Thus, one must code for that separately either in stop, or one can choose to register the <em>BeanDefinition</em> object to have such functionality. The way to create a bean definition object is just use the <em>BeanDefinitionBuilder</em> which is a creator class for the same.</li>
    <li>Bean name as mentioned above should be unique or you will get an <em>IllegalArgumentException</em> while registering.</li>
</ol></p>
<p><em><strong>Sample application :</strong></em>
Please follow this <a href="https://github.com/xebia/SIChat">URL</a> to download and try out the sample application. Refer to the README.txt file for instructions.</p>

## Comments

**[Rohit Garg](#3467 "2010-12-12 14:34:26"):** @Both, I agree with the points that you have made, but also think of a scenario where you don't even know the destination where you have to make a connection. Spring integration class structure that we used in our project needed the information in advance. Thus, the only solution that we could think of was just leave the context a little abstract and create the rest of adapters when we have enough information. @Woo, the way that you have suggested is not using any of the already provided APIs from spring integration. If I were to do that, I would end up coding all the stuff myself. @Lieven, will surely look out for JRebel. Thanks for the info..

**[Lieven Doclo](#3458 "2010-12-09 21:13:06"):** Your example for altering a spring context at runtime is a bad one, as such a scenario will rather lend itself to using factories that create the connections when they are needed, getting the connection information at that time from an arbitrary source. Normally, the only classes you'll code that manipulate the context are namespace handlers and those just manipulate the context at startup. In a well-designed system, you should never find yourself needing to alter the spring context at runtime. The only case I can make for this is when developing an application and the spring context is not yet complete. But even then I would advise against building in a reloading or altering mechanism and use JRebel instead.

**[Woo](#3465 "2010-12-12 05:16:20"):** next time try with a configuration provider class with management bean (and with some lazy init) before starting to play with the appcontext. The appcontext should be static in structure and you don't need to do any magic with it. It is a sign of the problem with your app structure. Regards...

