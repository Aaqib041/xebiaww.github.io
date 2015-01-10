---
layout: post
header-img: img/default-blog-pic.jpg
author: rjaiswal
description: 
post_id: 2160
created: 2009/11/26 15:49:18
created_gmt: 2009/11/26 10:49:18
comment_status: open
---

# Accessing JBoss managed resources using MBeans

<p>Recently I was working on an application that enables users to view the message count and the message content on all Queues deployed on a JBoss instance. Mainly, this application can be used by administrators to manage JMS Queues, it also provides a simple interface to create / delete queues as well as monitor messages on them.</p>
<p>The challenge was to dynamically retrieve all the Queues deployed on the server and to make this application portable to any JBoss server instance.</p>
<p><strong>The Solution</strong>
In JBoss all components/services are Mbeans. An MBean is a Java object that implements one of the standard MBean interfaces and follows the associated design patterns. The MBean for a resource exposes all necessary information and operations that a management application needs to control the resource.</p>
<p>So using the MBeans API you can, let's say create a queue at runtime programatically. In the following exercise, we look at the code that will retrieve all JMS Queues deployed on a particular JBoss instance.</p>
<!--more-->

<p>An interesting application to note is the <span style="color:#ff0000;">jmx-console</span> application that is packaged within JBoss. This application kind of does what we want to do. It displays all the MBeans deployed on the server. See screenshot -</p>
<p><img src="http://xebee.xebia.in/wp-content/uploads/2009/11/jmx-console.png" alt="JMX Console"  height="100%" width="100%"/></p>
<p>To see jmx-console, start the JBoss server on your local machine and go to <span style="color:#0000ff;">http://localhost:8080/jmx-console</span></p>
<p>Straight away lets start with some code, below is the listing of a Servlet that contains our core logic -</p>
<p><strong>QBrowser.java</strong></p>
<p>[code language="java"]
package net.rocky.qsoft.web;</p>
<p>import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.TreeMap;</p>
<p>import javax.management.JMException;
import javax.management.MBeanServer;
import javax.management.ObjectName;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;</p>
<p>import net.rocky.qsoft.model.MBeanData;</p>
<p>import org.jboss.mx.util.MBeanServerLocator;</p>
<p>public class QBrowser extends HttpServlet {</p>
<pre><code>protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    PrintWriter writer = resp.getWriter();
    try {
        Map&amp;lt;String, List&amp;lt;MBeanData&amp;gt;&amp;gt; domainData = getDomainData(&amp;quot;jboss.messaging.destination:*&amp;quot;);

        for (String key : domainData.keySet()) {
            writer.print(&amp;quot;&amp;lt;b&amp;gt;Domain Name - &amp;quot; + key + &amp;quot;&amp;lt;/b&amp;gt;&amp;lt;/br&amp;gt;&amp;quot;);
            List&amp;lt;MBeanData&amp;gt; data = domainData.get(key);
            for (MBeanData beanData : data) {
                writer.print(beanData.getNameProperties() + &amp;quot;&amp;lt;/br&amp;gt;&amp;quot;);
            }
        }
    } catch (Exception e) {
        e.printStackTrace();
    }
}

private Map&amp;lt;String, List&amp;lt;MBeanData&amp;gt;&amp;gt; getDomainData(String filter) throws JMException {
    MBeanServer server = getMBeanServer();
    Map&amp;lt;String, List&amp;lt;MBeanData&amp;gt;&amp;gt; domainDataMap = new TreeMap&amp;lt;String, List&amp;lt;MBeanData&amp;gt;&amp;gt;();

    if (server != null) {
        ObjectName filterName = null;
        if (filter != null)
            filterName = new ObjectName(filter);

        Set&amp;lt;ObjectName&amp;gt; objectNames = server.queryNames(filterName, null);

        for (ObjectName objectName : objectNames) {
            List&amp;lt;MBeanData&amp;gt; data = null;

            MBeanData mbeanData = new MBeanData(objectName, server.getMBeanInfo(objectName));
            String domainName = objectName.getDomain();
            System.out.println(domainName + &amp;quot;-&amp;gt;&amp;quot; + objectName.toString());

            if (domainDataMap.containsKey(domainName)){
                data = (List&amp;lt;MBeanData&amp;gt;) domainDataMap.get(domainName);
                data.add(mbeanData);
            }
            else {
                data = new ArrayList&amp;lt;MBeanData&amp;gt;();
                data.add(mbeanData);
                domainDataMap.put(domainName, data);
            }
        }
    }
    return domainDataMap;
}

private MBeanServer getMBeanServer() {
    return MBeanServerLocator.locateJBoss();
}
</code></pre>
<p>}
[/code]</p>
<p>The main piece of code here lies in the method <span style="color:#0000ff;">getMBeanServer()</span>, here we locate a reference to the main JBoss MBean. Thereafter, using the MBean API we retrieve the ObjectNames that match our filter -<span style="color:#ff0000;"> "jboss.messaging.destination:*"</span>, this helps us retrieve references only to messaging destinations MBeans (you can use some other filter to retrieve other types of MBeans, or leave filter as null to get all the MBeans).</p>
<p>Then we iterate over the returned Set of ObjectNames and create a bean of type MBeanData - containing the ObjectName and the MBeanInfo (which describes the set of attributes and operations which are available for management operations, thus is quite important).</p>
<p>Since all the Queues will have the same domain name, in the value part of the map we have a List of MBeanData. We then iteratively add beans to this list and later print them out.</p>
<p>We create 1 helper classes -</p>
<p><strong>MBeanData.java</strong></p>
<p>[code language="java"]
package net.rocky.qsoft.model;</p>
<p>import javax.management.MBeanInfo;
import javax.management.ObjectName;</p>
<p>/<em><em>
 * An mbean ObjectName and MBeanInfo pair that is orderable by ObjectName.
 </em>
 * @author Scott.Stark@jboss.org
 * @version $Revision: 81038 $
 </em>/
public class MBeanData implements Comparable {</p>
<pre><code>private ObjectName objectName;
private MBeanInfo metaData;

public MBeanData() {
}

public MBeanData(ObjectName objectName, MBeanInfo metaData) {
    this.objectName = objectName;
    this.metaData = metaData;
}

/**
 * Getter for property objectName.
 *
 * @return Value of property objectName.
 */
public ObjectName getObjectName() {
    return objectName;
}

/**
 * Setter for property objectName.
 *
 * @param objectName
 *            New value of property objectName.
 */
public void setObjectName(ObjectName objectName) {
    this.objectName = objectName;
}

/**
 * Getter for property metaData.
 *
 * @return Value of property metaData.
 */
public MBeanInfo getMetaData() {
    return metaData;
}

/**
 * Setter for property metaData.
 *
 * @param metaData
 *            New value of property metaData.
 */
public void setMetaData(MBeanInfo metaData) {
    this.metaData = metaData;
}

/**
 * @return The ObjectName.toString()
 */
public String getName() {
    return objectName.toString();
}

/**
 * @return The canonical key properties string
 */
public String getNameProperties() {
    return objectName.getCanonicalKeyPropertyListString();
}

/**
 * @return The MBeanInfo.getClassName() value
 */
public String getClassName() {
    return metaData.getClassName();
}

/**
 * Compares MBeanData based on the ObjectName domain name and canonical key
 * properties
 *
 * @param the
 *            MBeanData to compare against
 * @return &amp;lt; 0 if this is less than o, &amp;gt; 0 if this is greater than o, 0 if
 *         equal.
 */
public int compareTo(Object o) {
    MBeanData md = (MBeanData) o;
    String d1 = objectName.getDomain();
    String d2 = md.objectName.getDomain();
    int compare = d1.compareTo(d2);
    if (compare == 0) {
        String p1 = objectName.getCanonicalKeyPropertyListString();
        String p2 = md.objectName.getCanonicalKeyPropertyListString();
        compare = p1.compareTo(p2);
    }
    return compare;
}

public boolean equals(Object o) {
    if (o == null || (o instanceof MBeanData) == false)
        return false;
    if (this == o)
        return true;
    return (this.compareTo(o) == 0);
}
</code></pre>
<p>}
[/code]</p>
<p>Here's my <strong>pom.xml</strong></p>
<p>[code language="XML"]
&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;project xmlns=&quot;http://maven.apache.org/POM/4.0.0&quot; xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot;
  xsi:schemaLocation=&quot;http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd&quot;&gt;
  &lt;modelVersion&gt;4.0.0&lt;/modelVersion&gt;
  &lt;groupId&gt;net.rocky&lt;/groupId&gt;
  &lt;artifactId&gt;qsoft&lt;/artifactId&gt;
  &lt;packaging&gt;war&lt;/packaging&gt;</p>

## Comments

**[Anubhav](#6003 "2011-10-10 09:55:11"):** ...

