---
layout: post
header-img: img/default-blog-pic.jpg
author: vrawat
description: 
post_id: 12863
created: 2012/03/24 19:58:31
created_gmt: 2012/03/24 14:58:31
comment_status: open
---

# Handling Session Failover on a Load Balanced Tomcat using Memcached

## The problem:

In our project we use four load-balanced tomcat nodes(with Sticky-Session enabled). Everything works fine but there is just one thing that bugs me. Whenever we do a production deployment, we remove one node, deploy on that and, if everything works fine, we remove second node from the load-balancer and deploy on that and so on. The problem is that whenever we remove a node during deployment, a session expired is thrown to all the users working on the node we just removed, as he/she is redirected to another node.

It is not a huge problem in our case. But this could be serious in e-commerce domain. For example if a user is filling his/her shopping cart and the session expires for no reason. 

## The configuration:

I deployed a simple Spring App in two Tomcat instances. [Apache Webserver][1] as the load balancer. [Apache's mod_jk][2] is used for load balancing using sticky-sessions. A single [Memcached Server][3] is used as an external key value server for storing sessions. I used [xMemcachedClient][4] to interact with Memcahed Server, because it is easily available as a pom dependency and is provided by Google.

## The solution:

### Load-Balancing:

I used this [guide ][5]to load-balance my application using Apache Webserver and mod_jk. Below is my config:

``` 
 LoadModule jk_module modules/mod_jk.so JkWorkersFile conf/jkworkers.properties JkLogFile logs/mod_jk.log JkLogLevel "info" JkLogStampFormat "[%a %b %d %H:%M:%S %Y]" JkMount /app* loadbalancer 
 ```

And below is the content of my jkworkers.properties file: ``` 


# Define list of workers that will be used for mapping requests

worker.list=loadbalancer,status

# Define Node1

# modify the host as your host IP or DNS name.

worker.node1.port=8009 worker.node1.host=localhost worker.node1.type=ajp13 worker.node2.ping_mode=A worker.node1.lbfactor=1 

# Define Node2

# modify the host as your host IP or DNS name.

worker.node2.port=7009 worker.node2.host=localhost worker.node2.type=ajp13 worker.node2.ping_mode=A worker.node2.lbfactor=1

# Load-balancing behavior

worker.loadbalancer.type=lb worker.loadbalancer.balance_workers=node1,node2 worker.loadbalancer.sticky_session=1

# Status worker for managing load balancer

worker.status.type=status 
 ```

### Managing Sessions:

To manage session failover I used the following approach: 

  * Both the tomcats hold sessions locally on their respective jvm. 
  * Additionally all the sessions(of both tomcats) are stored in Memcached server.(Cache expiry in memcached can be configured, I have configured it to be 1day) 
  * Suppose a user is browsing on **node1**. In general, he will be directed to node1 as sticky-sessions are enabled and his session can be fetched from the local memory. In case of **node1** fails, the next request will go to the other tomcat instance i.e. **node2**. **node2** is asked for a session he does not know. So, **node2** will fetch the session from the Memcached Server and store it in its local jvm. Now, node2 is responsible for this session. 

Now lets see some code. [code language="java" highlight="14,15,22"] package in.xebia.vijay.controllers; //import statements @Controller public class HomeController {
    
    
    private static final String SEPARATOR = &quot;|&quot;;
    private static final Logger logger = LoggerFactory.getLogger(HomeController.class);
    @Resource
    private SessionService sessionService;
    
    @RequestMapping(value = &quot;/login&quot;, method = RequestMethod.POST)
    public String home(Model model, User user,HttpSession session, @CookieValue(&quot;JSESSIONID&quot;) String jsessionId) {
        logger.info(&quot;In home!&quot;);
        sessionService.set(session.getId()+SEPARATOR+&quot;user&quot;, user, session);
        User userFromSession = (User) sessionService.get(jsessionId+SEPARATOR+&quot;user&quot;, session);
        model.addAttribute(&quot;userName&quot;, userFromSession.getUserName());
        return &quot;home&quot;;
    }
    @RequestMapping(value = &quot;/check&quot;, method = RequestMethod.GET)
    public String check(Model model, HttpSession session, @CookieValue(&quot;JSESSIONID&quot;) String jsessionId) {
        logger.info(&quot;In Check!&quot;);
        User userFromSession = (User) sessionService.get(jsessionId+SEPARATOR+&quot;user&quot;, session);
        model.addAttribute(&quot;userName&quot;, userFromSession.getUserName());
        return &quot;check&quot;;
    }
    @RequestMapping(value = &quot;/&quot;, method = RequestMethod.GET)
    public String login() {
        logger.info(&quot;In login!&quot;);
        return &quot;login&quot;;
    }
    

} 
 ```

The lines to note in above code are **14,15 and 22**: 

  * In line **14**, I am adding the current user to session using **SessionService**. Point to note is that instead of doing something like : ``` 
sessionService.set("user", user, session);
 ``` I have written : ``` 
sessionService.set(session.getId()+SEPARATOR+"user", user, session);
 ``` I am "deliberately" appending the SessionId just to ensure that if a particular tomcat node fails, the other tomcat can retrieve failed node's sessions using the cookie **JSESSIONID** stored in the client header. 
  * And that is the reason, in lines **15** as well as **22**, I am fetching session attribute from the **SessionService **using the **JSESSIONID** retrieved from the cookie. 

All the other code is quite self explanatory. Now lets have a look at the **SessionServiceImpl.java** ``` 
 package in.xebia.vijay.impl; //import statements @Service public class SessionServiceImpl implements SessionService {
    
    
    @Autowired
    private MemcachedClient memcachedClient;
    private static int EXPIRY = 24 * 60 * 60;
    
    @Override
    public &lt;T&gt; void set(String key, T value, HttpSession session) {
        try {
            session.setAttribute(key, value);
            memcachedClient.set(key,EXPIRY,value);
        } catch (TimeoutException e) {
            e.printStackTrace();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (MemcachedException e) {
            e.printStackTrace();
        }
    }
    
    @SuppressWarnings(&quot;unchecked&quot;)
    @Override
    public &lt;T&gt; T get(String key, HttpSession session) {
        try {
            if(session.getAttribute(key)!=null)
                return (T) session.getAttribute(key);
            else if(memcachedClient.get(key)!=null){
                String[] keyPart = key.split(&quot;|&quot;);
                String newKey = session.getId()+&quot;|&quot;+keyPart[1];
                session.setAttribute(newKey, memcachedClient.get(key));
                memcachedClient.set(newKey, EXPIRY, memcachedClient.get(key));
                memcachedClient.delete(key);
                return memcachedClient.get(newKey);
            }
            else
                return null;
        } catch (TimeoutException e) {
            e.printStackTrace();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (MemcachedException e) {
            e.printStackTrace();
        }
        return null;
    }
    

} 
 ``` Points to be noted : 

  * In the implementation of method ``` 
public void set(String key, T value, HttpSession session); 
 ``` we add the attribute to HttpSession as well as the Memcahced Server, so that in case of any node failure the other node can fetch the values from Memcached Server. 
  * In the implementation of method ``` 
public T get(String key, HttpSession session); 
 ```

   [1]: http://httpd.apache.org/
   [2]: http://tomcat.apache.org/tomcat-3.3-doc/mod_jk-howto.html
   [3]: http://memcached.org/
   [4]: http://code.google.com/p/xmemcached/
   [5]: http://docs.redhat.com/docs/en-US/JBoss_Enterprise_Web_Server/1.0/html/HTTP_Connectors_Load_Balancing_Guide/Apache_HTTP_Configure.html

## Comments

**[Mukesh Shah](#8040 "2012-03-26 11:29:57"):** Very nice blog. Well explained problem and its solution. Keep blogging.

**[Martin Grotzke](#8109 "2012-03-29 03:35:59"):** Btw, there's memcached-session-manager that target the same problem, it can handle both sticky and non-sticky sessions: http://code.google.com/p/memcached-session-manager/ Cheers, Martin

**[Manish Agarwal](#9135 "2012-07-06 11:36:30"):** Hey I run this one on my eclipse and deployed the war on jetty server. I also change port to 8080 in the code. But when I ran the jetty server, it's giving errors and not connecting to the locaclhost. Please help me out regarding this issue.

**[Vijay Rawat](#8112 "2012-03-29 09:21:52"):** Hi Martin, Yea its a nice solution. Thanks for adding it in the comment. :)

