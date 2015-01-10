---
layout: post
header-img: img/default-blog-pic.jpg
author: priyanshu
description: 
post_id: 294
created: 2007/09/25 12:58:04
created_gmt: 2007/09/25 10:58:04
comment_status: open
---

# Hibernate component (value object) inheritance mapping

<p>Entity inheritance mapping in Hibernate can easily be done by following one of the three strategies and many a times instance of which subclass has to be created is identified by the value of the discriminator column (particularly in the case of table per class hierarchy). For such mappings there is already an inherent support in Hibernate.</p>
<p>So this is good if we want to create a sub class of a specific type for each row but what about if we want to create an instance of a specific class for a value of a column within that row. So in a way some type of component inheritance mapping. This is required if I really do not want to convert a value object (component) into an entity as that might mean sacrifice on the part of good model design as I am loosing the distinction between a component and an entity.</p>
<p>A workaround for the moment could be to somehow use a UserType to create instance of such objects based upon the value stored in the column. The concept of discriminator column can still help us. Let’s see how by taking up the following example</p>
<!--more-->

<p>Suppose we have a catalog of books where each book has a price specification depending upon the currency of where the book was originally published. Each price specification is modeled by a separate class which has a property amount. Assume here that these classes have a different behavior depending upon the currency for calculation of sales tax and stuff (otherwise we can simply create a class and do the mapping using component or one-to-one).</p>
<p>So the book table looks something like this</p>
<p>[sql]CREATE TABLE BOOK (
    ID NUMBER(19,0) NOT NULL,
    PRICE_SPEC_TYPE VARCHAR2(3) NOT NULL,
    PRICE_AMOUNT    NUMBER(19,0) NOT NULL,
    …
);[/sql]</p>
<p>And book class like this</p>
<p>[java]public class Book {
    private long id;
    private PriceSpecification priceSpecification;
    …
}[/java]</p>
<p>And one of the price specifications may be</p>
<p>[java]public class USDollarSpecification implement PriceSpecification {
    private long amount;
    public void setAmount(long amount) {
        this.amount = amount;
    }
    public long getMaximumRetailPrice() {
    }
    …
}[/java]</p>
<p>Now this can be obviously mapped using a UserType but to make it much cleaner lets use an Enum. Some sample code of PriceSpecificationUserType looks something like this.</p>
<p>[java]public Object nullSafeGet(ResultSet rs, String[] names, Object owner) throws HibernateException, SQLException {
    String type = rs.getString(names[0]);
    long amount = rs.getLong(names[1]);
    PriceSpecificationType priceSpecType = priceSpecificationType.valueOf(type);
    PriceSpecification specification = priceSpecType.getNewInstance();
    specification.setAmount();
    return specification;
}
[/java]</p>
<p>Here we create an instance of a price specification from the value in PRICE_SPEC_TYPE column using an Enum for price specification types.</p>
<p>[java]public void nullSafeSet(PreparedStatement preparedStatement, Object value, int index) throws HibernateException, SQLException {
    if (value == null) {
        preparedStatement.setNull(index, Types.VARCHAR);
        preparedStatement.setNull(index + 1, Types.NUMERIC);
    } else {
        PriceSpecification priceSpecification = (PriceSpecification) value;
        String className = priceSpecification.getClass().getName();
        PriceSpecificationType[] priceSpecTypes = PriceSpecificationType.values();
        for (int i = 0; i &lt; priceSpecTypes.length; i++) {
            PriceSpecificationType priceSpecType = priceSpecTypes[i];
            if (priceSpecType.getClassName().equals(className)) {
                preparedStatement.setString(index, priceSpecType.name());
                break;
            }
        }
        preparedStatement.setInt(index + 1, priceSpecification.getAmount());
    }
}[/java]</p>
<p>And similarly we set the price specification in PRICE_SPEC_TYPE using the Enum for price specification types.</p>
<p>Here is how Enum looks like</p>
<p>[java]public enum PriceSpecificationType {
    US_DOLLAR {
        public String getClassName() {
            return USDollarSpecification.class.getName();
        }</p>
<pre><code>    @Override
    public PriceSpecification getNewInstance() {
        return new USDollarSpecification();
    }
},
…

public abstract PriceSpecification getNewInstance();

public abstract String getClassName();
</code></pre>
<p>}
[/java]</p>
<p>And finally let’s see the mapping for this</p>
<p>[xml]<typedef name="PriceSpecification" class="PriceSpecificationUserType"/></p>
<p><class name=”Book” table=”BOOK”
    <property name="priceSpecification" type="PriceSpecification">
        <column name="PRICE_SPEC_TYPE"/>
        <column name="PRICE_AMOUNT"/>
    </property>
</class>[/xml]</p>
<p>Hibernate might add a support for this in one of the future releases going by this <a href="http://opensource.atlassian.com/projects/hibernate/browse/HHH-1152">http://opensource.atlassian.com/projects/hibernate/browse/HHH-1152</a> but for now this might be a workaround we have to live with</p>