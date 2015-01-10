---
layout: post
header-img: img/default-blog-pic.jpg
author: vaibhav sehgal
description: 
post_id: 6549
created: 2011/01/10 11:18:12
created_gmt: 2011/01/10 06:18:12
comment_status: open
---

# The Curious case of Field and Property Access in Hibernate

<div id="_mcePaste">Have you ever wondered whether placing Hibernate annotations over both the fields and getters(properties) in an entity class would land you in trouble. Well as <a href="http://xebee.xebia.in/author/rajdeep/">Rajdeep</a> and I discovered, if you haven't tried it out, it will. <!--more-->

 As shown in the following code, CustomerHotelBooking is an entity bean which acts as a link between Customer and Hotel. Ideally, this entity would be mapped by Hibernate, to a CustomerHotelBooking table having hotel_id as a foreign key with type as bigint. However, it does not happen this way. On the contrary the CustomerHotelBooking table has a column named hotel with type as tinyblob. Can you figure out why this happens?
</div>

<p>[sourcecode langauage="java"]
@Entity
public class CustomerHotelBooking {</p>
<pre><code>    @Id
    @GeneratedValue
    private long id;

    private Hotel hotel;

    private Customer customer ;

    @ManyToOne
    @Cascade(value = { CascadeType.ALL })
    public Hotel getHotel() {
            return hotel;
    }
</code></pre>
<p>...
[/sourcecode]</p>
<p>Well, this points to a classic rule of Hibernate Access types (the focus of this blog) ; "<strong>Be consistent while placing your annotations or use @Access instead</strong>". By default if we don't use @Access, Hibernate looks for <strong>@Id</strong> or <strong>@EmbeddedId</strong> and then decides whether it should look for other annotations on the fields or on the getters(properties).Thus either place the annotations on the fields only or on the getters(properties) only. Mixing them and not using @Access will cause abnormal behaviour.</p>
<p>So if you still want to play mix-n-match then do it the following way (using <strong>@Access(AccessType.PROPERTY) </strong>or<strong> @Access(AccessType.FIELD)</strong> )</p>
<p>[sourcecode langauage="java"]
@Entity
public class CustomerHotelBooking {</p>
<pre><code>    @Id
    @GeneratedValue
    private long id;

    private Hotel hotel;

    private Customer customer ;

    /**
     * @return the hotel
     */
    @Access(AccessType.PROPERTY)
    @ManyToOne
    @Cascade(value = { CascadeType.ALL })
    public Hotel getHotel() {
            return hotel;
    }
</code></pre>
<p>...
[/sourcecode]</p>
<p>You could also place the @Access(AccessType.PROPERTY) or @Access(AccessType.FIELD) annotation on the class instead to mark the properties or fields as the access types. Thats a personal preference that we leave to you, but for us Field access wins hands down for its clarity.</p>

## Comments

**[Ganesh Gembali](#4835 "2011-01-10 11:29:42"):** Nice blog. Faced this problem in my previous project. It really took a lot of time figure this out.

**[Rohit](#4836 "2011-01-10 11:31:30"):** Nice find! Well, the behavior was not actually abnormal, in case hibernate doesn't find any annotation over a field or getter that even doesn't have a @Transient annotation, it takes the default settings. In your case, the field was an object, so the table was having a tiny_blob column. By default for classes, hibernate stores the contents as serialized object in the database, and that needs blob as the datatype.

