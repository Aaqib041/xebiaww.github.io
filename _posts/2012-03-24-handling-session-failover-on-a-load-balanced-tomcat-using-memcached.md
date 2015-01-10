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

<h2>The problem:</h2>

<p>In our project we use four load-balanced tomcat nodes(with Sticky-Session enabled). Everything works fine but there is just one thing that bugs me. Whenever we do a production deployment, we remove one node, deploy on that and, if everything works fine, we remove second node from the load-balancer and deploy on that and so on. The problem is that whenever we remove a node during deployment, a session expired is thrown to all the users working on the node we just removed, as he/she is redirected to another node.</p>
<p>It is not a huge problem in our case. But this could be serious in e-commerce domain. For example if a user is filling his/her shopping cart and the session expires for no reason. <!--more--></p>
<h2>The configuration:</h2>

<p>I deployed a simple Spring App in two Tomcat instances. <a href="http://httpd.apache.org/" target="_blank">Apache Webserver</a> as the load balancer. <a href="http://tomcat.apache.org/tomcat-3.3-doc/mod_jk-howto.html" target="_blank">Apache's mod_jk</a> is used for load balancing using sticky-sessions. A single <a href="http://memcached.org/" target="_blank">Memcached Server</a> is used as an external key value server for storing sessions. I used <a href="http://code.google.com/p/xmemcached/" target="_blank">xMemcachedClient</a> to interact with Memcahed Server, because it is easily available as a pom dependency and is provided by Google.</p>
<h2>The solution:</h2>

<h3>Load-Balancing:</h3>

<p>I used this <a href="http://docs.redhat.com/docs/en-US/JBoss_Enterprise_Web_Server/1.0/html/HTTP_Connectors_Load_Balancing_Guide/Apache_HTTP_Configure.html" target="_blank">guide </a>to load-balance my application using Apache Webserver and mod_jk. Below is my config:</p>
<p>[code]
LoadModule jk_module modules/mod_jk.so
JkWorkersFile conf/jkworkers.properties
JkLogFile logs/mod_jk.log
JkLogLevel &quot;info&quot;
JkLogStampFormat &quot;[%a %b %d %H:%M:%S %Y]&quot;
JkMount /app* loadbalancer
[/code]</p>
<p>And below is the content of my jkworkers.properties file:
[code]</p>
<h1>Define list of workers that will be used for mapping requests</h1>
<p>worker.list=loadbalancer,status</p>
<h1>Define Node1</h1>
<h1>modify the host as your host IP or DNS name.</h1>
<p>worker.node1.port=8009
worker.node1.host=localhost
worker.node1.type=ajp13
worker.node2.ping_mode=A
worker.node1.lbfactor=1 </p>
<h1>Define Node2</h1>
<h1>modify the host as your host IP or DNS name.</h1>
<p>worker.node2.port=7009
worker.node2.host=localhost
worker.node2.type=ajp13
worker.node2.ping_mode=A
worker.node2.lbfactor=1</p>
<h1>Load-balancing behavior</h1>
<p>worker.loadbalancer.type=lb
worker.loadbalancer.balance_workers=node1,node2
worker.loadbalancer.sticky_session=1</p>
<h1>Status worker for managing load balancer</h1>
<p>worker.status.type=status
[/code]</p>
<h3>Managing Sessions:</h3>

<p>To manage session failover I used the following approach:
<ul>
<li>Both the tomcats hold sessions locally on their respective jvm.
</li>
<li>Additionally all the sessions(of both tomcats) are stored in Memcached server.(Cache expiry in memcached can be configured, I have configured it to be 1day)
</li>
<li>Suppose a user is browsing on <strong>node1</strong>. In general, he will be directed to node1 as sticky-sessions are enabled and his  session can be fetched from the local memory. In case of <strong>node1</strong> fails, the next request will go to the other tomcat instance i.e. <strong>node2</strong>. <strong>node2</strong> is asked for a session he does not know. So, <strong>node2</strong> will fetch the session from the Memcached Server and store it in its local jvm. Now, node2 is responsible for this session.
</li>
</ul></p>
<p>Now lets see some code.
[code language="java" highlight="14,15,22"]
package in.xebia.vijay.controllers;
//import statements
@Controller
public class HomeController {</p>
<pre><code>private static final String SEPARATOR = &amp;quot;|&amp;quot;;
private static final Logger logger = LoggerFactory.getLogger(HomeController.class);
@Resource
private SessionService sessionService;

@RequestMapping(value = &amp;quot;/login&amp;quot;, method = RequestMethod.POST)
public String home(Model model, User user,HttpSession session, @CookieValue(&amp;quot;JSESSIONID&amp;quot;) String jsessionId) {
    logger.info(&amp;quot;In home!&amp;quot;);
    sessionService.set(session.getId()+SEPARATOR+&amp;quot;user&amp;quot;, user, session);
    User userFromSession = (User) sessionService.get(jsessionId+SEPARATOR+&amp;quot;user&amp;quot;, session);
    model.addAttribute(&amp;quot;userName&amp;quot;, userFromSession.getUserName());
    return &amp;quot;home&amp;quot;;
}
@RequestMapping(value = &amp;quot;/check&amp;quot;, method = RequestMethod.GET)
public String check(Model model, HttpSession session, @CookieValue(&amp;quot;JSESSIONID&amp;quot;) String jsessionId) {
    logger.info(&amp;quot;In Check!&amp;quot;);
    User userFromSession = (User) sessionService.get(jsessionId+SEPARATOR+&amp;quot;user&amp;quot;, session);
    model.addAttribute(&amp;quot;userName&amp;quot;, userFromSession.getUserName());
    return &amp;quot;check&amp;quot;;
}
@RequestMapping(value = &amp;quot;/&amp;quot;, method = RequestMethod.GET)
public String login() {
    logger.info(&amp;quot;In login!&amp;quot;);
    return &amp;quot;login&amp;quot;;
}
</code></pre>
<p>}
[/code]</p>
<p>The lines to note in above code are <strong>14,15 and 22</strong>:
<ul>
<li>In line <strong>14</strong>, I am adding the current user to session using <strong>SessionService</strong>. Point to note is that instead of doing something like : 
[code language="java"]sessionService.set(&quot;user&quot;, user, session);[/code]
I have written :
[code language="java"]sessionService.set(session.getId()+SEPARATOR+&quot;user&quot;, user, session);[/code]
I am "deliberately" appending the SessionId just to ensure that if a particular tomcat node fails, the other tomcat can retrieve failed node's sessions using the cookie <strong>JSESSIONID</strong> stored in the client header.
</li>
<li>And that is the reason, in lines <strong>15</strong> as well as <strong>22</strong>, I am fetching session attribute from the <strong>SessionService </strong>using the <strong>JSESSIONID</strong> retrieved from the cookie.
</li>
</ul></p>
<p>All the other code is quite self explanatory. Now lets have a look at the <strong>SessionServiceImpl.java</strong>
[code language="java"]
package in.xebia.vijay.impl;
//import statements
@Service
public class SessionServiceImpl implements SessionService {</p>
<pre><code>@Autowired
private MemcachedClient memcachedClient;
private static int EXPIRY = 24 * 60 * 60;

@Override
public &amp;lt;T&amp;gt; void set(String key, T value, HttpSession session) {
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

@SuppressWarnings(&amp;quot;unchecked&amp;quot;)
@Override
public &amp;lt;T&amp;gt; T get(String key, HttpSession session) {
    try {
        if(session.getAttribute(key)!=null)
            return (T) session.getAttribute(key);
        else if(memcachedClient.get(key)!=null){
            String[] keyPart = key.split(&amp;quot;|&amp;quot;);
            String newKey = session.getId()+&amp;quot;|&amp;quot;+keyPart[1];
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
</code></pre>
<p>}
[/code]
Points to be noted : 
<ul>
<li>In the implementation of method 
[code language="java"]public  void set(String key, T value, HttpSession session); [/code]
we add the attribute to HttpSession as well as the Memcahced Server, so that in case of any node failure the other node can fetch the values from Memcached Server.
</li>
<li>In the implementation of method 
[code language="java"]public  T get(String key, HttpSession session); [/code]</p>

## Comments

**[Mukesh Shah](#8040 "2012-03-26 11:29:57"):** Very nice blog. Well explained problem and its solution. Keep blogging.

**[Martin Grotzke](#8109 "2012-03-29 03:35:59"):** Btw, there's memcached-session-manager that target the same problem, it can handle both sticky and non-sticky sessions: http://code.google.com/p/memcached-session-manager/ Cheers, Martin

**[Manish Agarwal](#9135 "2012-07-06 11:36:30"):** Hey I run this one on my eclipse and deployed the war on jetty server. I also change port to 8080 in the code. But when I ran the jetty server, it's giving errors and not connecting to the locaclhost. Please help me out regarding this issue.

**[Vijay Rawat](#8112 "2012-03-29 09:21:52"):** Hi Martin, Yea its a nice solution. Thanks for adding it in the comment. :)

