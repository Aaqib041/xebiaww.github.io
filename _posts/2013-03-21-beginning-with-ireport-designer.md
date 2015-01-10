---
layout: post
header-img: img/default-blog-pic.jpg
---

# Beginning with iReport Designer   

iReport is the free, open source report designer for JasperReports and JasperReports Server.iReport Designer and JasperReport APIs go hand in hand as the former allows you to design reports, the latter allows to execute them and generate output in a Java application .When you design a report using iReport Designer you are creating a JRXML file, which is an XML document that contains the definition of the report layout. The designer provides a comfortable User interface comprising of a report inspector which gives you the detailed view of the report design categorised under headers like style,parameters,fields,variables along with the different bands like page header,page footer etc which constitute the report.  A Jasper Report is divided into the following display sections, which you can choose to have or not to have in your report:![](/wp-includes/js/tinymce/plugins/wordpress/img/trans.gif)

  1. Title: Contains the report title. It is generated only once and appears only at the very start of the report.

  2. Page Header: This section appears at the top of each page and can be used for dates, page numbers, etc.

  3. Column Header: This section is placed top of each column.

  4. Detail: This is where the information for each record entry is placed.

  5. Column Footer: This section is placed at bottom of each column.

  6. Page Footer: This section appears at the bottom of each page.

  7. Last Page Footer: This section goes at the bottom of the last page.

  8. Summary: This section appears right at the end of the report, just after the last record.

Below is one screen shot summarizing the iReport User Interface discussed above [caption id="attachment_16145" align="aligncenter" width="300"]![iReport User Interface](/wp-content/uploads/2013/03/iReportUserInterface-300x168.png) iReport User Interface[/caption] The palette on right under the header Report Elements is the visual offering of all the elements that can be dragged into various report bands. The properties of currently selected component are shown in the properties section where you can choose to edit corresponding properties of the selected component . The Reports Problem window at the bottom is like any normal editor console displaying the issues with report compilation and running As JasperReports is a records-based engine; to print a report, you have to provide a set of records. When the report runs,JasperReports will iterate on this record set, creating and filling the bands according to the report definition. This is why JasperReports defines only one query per report. Multiple queries/data sources can be used when inserting SubReports or defining SubDatasets, each one with their own own query (or data source), fields, parameters, variables, and so on. SubDatasets are only used to feed a crosstab or a chart. Here is a list of the data sources provided by iReport for JasperReports: 

  * JDBC connection
  * JavaBean collection data source
  * XML data source
  * CSV data source
  * Hibernate connection
  * Spring-loaded Hibernate connection
  * JRDataSourceProvider 
  * Custom data source
  * Mondrian OLAP connection
  * XMLA connection
  * EJBQL connection
  * Empty data source
JRDataSource interface represents the abstract representation of a JasperReports data source. All data source types must implement this interface. If you ask me about difference between using a query inside a report and providing data using a JRDataSource from a java application, the reply will be no difference. In fact, what happens behind the scenes when iReport uses a query instead of a JRDataSource is that JasperReports executes the query using a built-in or user-defined query executor that will produce a JRDataSource In a report, there are three groups of objects that can store values: ![](/wp-includes/js/tinymce/plugins/wordpress/img/trans.gif)

  * Parameters are simple input to reports.
  * Fields are instance variables of the data source object that are passed in to the report .If input data source is java bean then the field names of the bean and if input is sql table then columns of the result set will be the field objects.
  * Variables are another kind of variables that live within Report, they are not inputs. One can perform many predefined calculation functions on the Fields using Variables.
These objects are used in expressions, they can change their values during print progression, and they are typed, that is, all these objects have a type that corresponds to a Java class such as String or Double. To start with iReport lets try creating a JDBC data source and generate a report based on query results fetching dataset from the data source rather than data being fed from Java application. Next to the data source drop down on the top left click the report datasource icon and after clicking on the new button on the pop up appearing choose the data source type .As we start by choosing a database JDBC connection below shows the required configuration for the same [caption id="attachment_16147" align="aligncenter" width="300"]![Database Connection](/wp-content/uploads/2013/03/Connection-300x168.png) Database Connection[/caption] With the database connection in place we can populate a report with a report query created as below.You can choose the query language from the drop down .The editor can automatically reads fields for you from the table.The filter expression and sort options tab help you enhance further your query.Say the sort tab helps you add fields on whose basis data should be sorted in the asc/desc orders. [caption id="attachment_16399" align="aligncenter" width="300"]![Repor tQuery](http://xebee.xebia.in/wp-content/uploads/2013/03/ReportQuery1-300x168.png) Report Query[/caption]