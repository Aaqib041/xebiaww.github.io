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

Entity inheritance mapping in Hibernate can easily be done by following one of the three strategies and many a times instance of which subclass has to be created is identified by the value of the discriminator column (particularly in the case of table per class hierarchy). For such mappings there is already an inherent support in Hibernate.

So this is good if we want to create a sub class of a specific type for each row but what about if we want to create an instance of a specific class for a value of a column within that row. So in a way some type of component inheritance mapping. This is required if I really do not want to convert a value object (component) into an entity as that might mean sacrifice on the part of good model design as I am loosing the distinction between a component and an entity.

A workaround for the moment could be to somehow use a UserType to create instance of such objects based upon the value stored in the column. The concept of discriminator column can still help us. Let’s see how by taking up the following example

Suppose we have a catalog of books where each book has a price specification depending upon the currency of where the book was originally published. Each price specification is modeled by a separate class which has a property amount. Assume here that these classes have a different behavior depending upon the currency for calculation of sales tax and stuff (otherwise we can simply create a class and do the mapping using component or one-to-one).

So the book table looks something like this

[sql]CREATE TABLE BOOK ( ID NUMBER(19,0) NOT NULL, PRICE_SPEC_TYPE VARCHAR2(3) NOT NULL, PRICE_AMOUNT NUMBER(19,0) NOT NULL, … );[/sql]

And book class like this

[java]public class Book { private long id; private PriceSpecification priceSpecification; … }[/java]

And one of the price specifications may be

[java]public class USDollarSpecification implement PriceSpecification { private long amount; public void setAmount(long amount) { this.amount = amount; } public long getMaximumRetailPrice() { } … }[/java]

Now this can be obviously mapped using a UserType but to make it much cleaner lets use an Enum. Some sample code of PriceSpecificationUserType looks something like this.

[java]public Object nullSafeGet(ResultSet rs, String[] names, Object owner) throws HibernateException, SQLException { String type = rs.getString(names[0]); long amount = rs.getLong(names[1]); PriceSpecificationType priceSpecType = priceSpecificationType.valueOf(type); PriceSpecification specification = priceSpecType.getNewInstance(); specification.setAmount(); return specification; } [/java]

Here we create an instance of a price specification from the value in PRICE_SPEC_TYPE column using an Enum for price specification types.

[java]public void nullSafeSet(PreparedStatement preparedStatement, Object value, int index) throws HibernateException, SQLException { if (value == null) { preparedStatement.setNull(index, Types.VARCHAR); preparedStatement.setNull(index + 1, Types.NUMERIC); } else { PriceSpecification priceSpecification = (PriceSpecification) value; String className = priceSpecification.getClass().getName(); PriceSpecificationType[] priceSpecTypes = PriceSpecificationType.values(); for (int i = 0; i < priceSpecTypes.length; i++) { PriceSpecificationType priceSpecType = priceSpecTypes[i]; if (priceSpecType.getClassName().equals(className)) { preparedStatement.setString(index, priceSpecType.name()); break; } } preparedStatement.setInt(index + 1, priceSpecification.getAmount()); } }[/java]

And similarly we set the price specification in PRICE_SPEC_TYPE using the Enum for price specification types.

Here is how Enum looks like

[java]public enum PriceSpecificationType { US_DOLLAR { public String getClassName() { return USDollarSpecification.class.getName(); }
    
    
        @Override
        public PriceSpecification getNewInstance() {
            return new USDollarSpecification();
        }
    },
    …
    
    public abstract PriceSpecification getNewInstance();
    
    public abstract String getClassName();
    

} [/java]

And finally let’s see the mapping for this

[xml]

[/xml]

Hibernate might add a support for this in one of the future releases going by this <http://opensource.atlassian.com/projects/hibernate/browse/HHH-1152> but for now this might be a workaround we have to live with