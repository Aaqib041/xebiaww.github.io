---
layout: post
header-img: img/default-blog-pic.jpg
author: PGarg
description: 
post_id: 13715
created: 2012/06/17 14:35:52
created_gmt: 2012/06/17 09:35:52
comment_status: open
---

# Liferay Service Builder Finders

<p>In the last post we saw how Liferay service builder helps in generation of Services and Dao/Persistence classes which perform the basic CRUD operations. Along with the basic crud operation liferay also comes with some finder module methods using which can be used to find entities based upon one or more of the fields present in it. Let’s continue with the example of email entity to better insights on finders.</p>
<p><span style="font-size: large;"><strong>Email Example Extended </strong></span></p>
<p>Suppose we want to search for emails based upon the following two criteria's
<ol>
    <li>Subject matching to a particular text string</li>
    <li>Email from a particular user</li>
</ol>
Add the following xml inside the email entry declared in service.xml</p>
<p>[sourcecode language="xml"]
&lt;!--  Finders --&gt;
&lt;finder return-type=&quot;Collection&quot; name=&quot;Subject&quot;&gt;
         &lt;finder-column name=&quot;subject&quot;&gt;&lt;/finder-column&gt;
&lt;/finder&gt;
&lt;finder return-type=&quot;Collection&quot; name=&quot;From&quot;&gt;
        &lt;finder-column name=&quot;from&quot;&gt;&lt;/finder-column&gt;
&lt;/finder&gt;
[/sourcecode]</p>
<!--more-->

<p>For the above declaration, we will see the following method in EmailPersistence and their implementation in EmailPersistenceImpl</p>
<p>[sourcecode language="xml"]
public java.util.List&lt;com.xebia.model.email&gt; findBySubject(
        java.lang.String subject)
        throws com.liferay.portal.kernel.exception.SystemException;</p>
<p>public java.util.List&lt;com.xebia.model.email&gt; findByFrom(
        java.lang.String from)
        throws com.liferay.portal.kernel.exception.SystemException;
[/sourcecode]</p>
<p>There are some additional helper methods generated for accessing data corresponding to these fields. These methods can be used to find count or limit the number of results etc.</p>
<p>Now an additional requirement is, to find all emails that are received on a particular date i.e. within those 24 hours.  The first thing that will come to mind is the "Where" clause. But there is no out of the box facility available in liferay that will automatically generate this method. The only only solution for these type of cases is writing a custom sql query and using the finder functionality to execute this query and return the results.</p>
<p><span style="font-size: large;"><strong>
</strong></span></p>
<p><span style="font-size: large;"><strong>Configuration</strong></span></p>
<p>Lets first start with writing of the custom query which will help us in retrieving all the emails between two dates. Our query will look like this</p>
<p>[sourcecode language="xml"]
SELECT Email.* FROM Email   WHERE    Email.date  between  ? AND  ?
[/sourcecode]</p>
<p>Include this sql in /docroot/WEB-INF/src/custom-sql/default.xml file. So that default.xml sql will look something like this</p>
<p>Note : Portlet structure should adhere to liferay standards and we have to create a custom-sql folder inside src and then create a file default.xml</p>
<p>We can also create custom file and then mention the reference of that file in default.xml.</p>
<p>[sourcecode language="xml"]
&lt;!--?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?--&gt;
&lt;custom-sql&gt;
    &lt;sql id=&quot;com.xebia.service.persistence.EmailFinder.getEmailBetweenDates&quot;&gt;
    &lt;![CDATA[ SELECT Email.* FROM Email   WHERE    Email.date  between  ? AND  ? ]]&gt;
    &lt;/sql&gt;
&lt;/custom-sql&gt;
[/sourcecode]</p>
<p>the id attribute is the unique identifier through which this query is recognized.</p>
<p><span style="font-size: large;"><strong>Code Generation</strong></span></p>
<p>We will be calling this query from finder class of liferay. Following are the steps for code generation for Liferay finder classes</p>
<p>Create a class EmailFinderImpl in the persistence package and Extend this class from EmailPersistenceImpl</p>
<p>Run ant build-service. This will generate the EmailFinder interface and EmailFinderUtil</p>
<p>Now go to EmailFinderImpl and make it implement EmailFinder</p>
<p>After this add a method in EmailFinderImpl and will run service builder, the same method will be reflected in EmailFinder interface and EmailFinderUtil class as well.</p>
<p>The class hierarchy and structure of Finder is almost similar to the structure of services which were shown in the last blog. Following is the class diagram for Finder.</p>
<p><a rel="attachment wp-att-13790" href="http://xebee.xebia.in/2012/06/17/liferay-service-builder-finders/finder_class_diag/"><img width="547" height="276" class="alignleft size-full wp-image-13790" title="finder_class_diag" src="http://xebee.xebia.in/wp-content/uploads/2012/05/finder_class_diag.png" /></a></p>
<p>[sourcecode language="xml"]
    public static String GET_EMAIL_BETWEEN_DATES = EmailFinder.class
            .getName() + &quot;.getEmailBetweenDates&quot;;</p>
<pre><code>public List&amp;lt;email&amp;gt; getEmailBetweenDates(Date startDate,Date endDate) throws ParseException, SystemException {

    Session session = null;
    List&amp;lt;email&amp;gt; emails = null;
    try {
        session = openSession();

        String sql = CustomSQLUtil.get(GET_EMAIL_BETWEEN_DATES);
        SQLQuery q = session.createSQLQuery(sql);

        q.addEntity(&amp;quot;Email&amp;quot;, EmailImpl.class);
        QueryPos qPos = QueryPos.getInstance(q);

        qPos.add(startDate);
        qPos.add(endDate);

        emails = (List&amp;lt;email&amp;gt;) q.list();
        if (emails == null) {
            emails = Collections.emptyList();
                                 }
           } finally {
        closeSession(session);
     }

    return emails;
}
</code></pre>
<p>[/sourcecode]</p>
<p><span style="font-size: large;"><strong>Code Utilization</strong></span></p>
<p>Calling of the finder methods is almost same as calling any of the service methods.</p>
<p>[sourcecode language="xml"]
List&lt;email&gt; emailsByDate = EmailFinderUtil.getEmailBetweenDates(Date startDate,
Date endDate);
[/sourcecode]</p>
<p><span style="font-size: large;"><strong>Object Mappings</strong></span></p>
<p>So far we have seen how to perform cruds and advanced search on a entity. Now let’s consider this example.</p>
<p>Fetch all the emails that have an attachments.</p>
<p>At Database Level : we will have an Email table and an Attachment table with email Id as the foreign key to support one-to-many relationships.</p>
<p>At Object level : we will have an Email object with Collection of type attachment as one of properties.</p>
<p>When we retrieve the email we also expect the attachement to get populated and vice-versa during the save (Given that we have done the required relationship mappings between the entities in service.xml).</p>
<p>But this is not how it works in the liferay. We cannot create a relation between the entities using the service.xml which basically means no work will be done automatically either at the DB level (foreign keys) and at object level (has a relationship) , yes there are workarounds but out of the box this is not provided. In the liferay <a href="http://www.liferay.com/dtd/liferay-service-builder_5_2_0.dtd%20">DTD</a> for service.xml , there is section that will describe how to create one-2-one / many-2-many relationships but even if we follow that convention , service builder will not create the mappings in generated java classes.
This is how the attachment entity will look like.</p>
<p>[sourcecode language="xml"]
&lt;entity name=&quot;Attachment&quot; local-service=&quot;true&quot; remote-service=&quot;false&quot; table=&quot;Email&quot;&gt;
     &lt;column name=&quot;id&quot; type=&quot;long&quot; primary=&quot;true&quot;&gt;
     &lt;column name=&quot;type&quot; type=&quot;String&quot;&gt;
     &lt;column name=&quot;emailId&quot; type=&quot;long&quot;&gt;
     &lt;column name=&quot;content&quot; type=&quot;String&quot;&gt;
     &lt;!--  Finders --&gt;
     &lt;finder name=&quot;id&quot;&gt;
         &lt;finder-column name=&quot;to&quot;&gt;&lt;/finder-column&gt;
          &lt;/finder&gt;
     &lt;finder name=&quot;emailId&quot; return-type=&quot;Collection&quot;&gt;
         &lt;finder-column name=&quot;from&quot;&gt;&lt;/finder-column&gt;
         &lt;/finder&gt;
&lt;/column&gt;&lt;/column&gt;&lt;/column&gt;&lt;/column&gt;&lt;/entity&gt;
[/sourcecode]</p>
<p>To achieve the above scenario :
1) Create an EmailDTO to contain email and its corresponding Attachment. Code below</p>
<p>[sourcecode language="xml"]</p>
<p>public EmailDTO{
private Email;
private Attachment;</p>