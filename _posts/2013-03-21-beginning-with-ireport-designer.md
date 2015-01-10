---
layout: post
header-img: img/default-blog-pic.jpg
author: shelley nanda
description: 
post_id: 16142
created: 2013/03/21 11:13:32
created_gmt: 2013/03/21 06:13:32
comment_status: open
---

# Beginning with iReport Designer   

<p>iReport is the free, open source report designer for JasperReports and JasperReports Server.iReport Designer and JasperReport APIs go hand in hand as the former allows you to design reports, the latter allows to execute them and generate output in a Java application .When you design a report using iReport Designer you are creating a JRXML file, which is an XML document that contains the definition of the report layout.</p>
<p>The designer provides a comfortable User interface comprising of a report inspector which gives you the detailed view of the report design categorised under headers like style,parameters,fields,variables along with the different bands like page header,page footer etc which constitute the report.</p>
<!--more-->

<p>A Jasper Report is divided into the following display sections, which you can choose to have or not to have in your report:<img title="More..." alt="" src="http://xebee.xebia.in/wp-includes/js/tinymce/plugins/wordpress/img/trans.gif" />
<ol>
    <li>
<p align="LEFT"><span style="color: #0000ff">Title</span>: Contains the report title. It is generated only once and appears only at the very start of the report.</p>
</li>
    <li>
<p align="LEFT"><span style="color: #0000ff">Page Header</span>: This section appears at the top of each page and can be used for dates, page numbers, etc.</p>
</li>
    <li>
<p align="LEFT"><span style="color: #0000ff">Column Header</span>: This section is placed top of each column.</p>
</li>
    <li>
<p align="LEFT"><span style="color: #0000ff">Detail</span>: This is where the information for each record entry is placed.</p>
</li>
    <li>
<p align="LEFT"><span style="color: #0000ff">Column Footer</span>: This section is placed at bottom of each column.</p>
</li>
    <li>
<p align="LEFT"><span style="color: #0000ff">Page Footer</span>: This section appears at the bottom of each page.</p>
</li>
    <li>
<p align="LEFT"><span style="color: #0000ff">Last Page Footer</span>: This section goes at the bottom of the last page.</p>
</li>
    <li>
<p align="LEFT"><span style="color: #0000ff">Summary</span>: This section appears right at the end of the report, just after the last record.</p>
</li>
</ol>
Below is one screen shot summarizing the iReport User Interface discussed above</p>
<p>[caption id="attachment_16145" align="aligncenter" width="300"]<a href="http://xebee.xebia.in/wp-content/uploads/2013/03/iReportUserInterface.png"><img class="size-medium wp-image-16145" alt="iReport User Interface" src="http://xebee.xebia.in/wp-content/uploads/2013/03/iReportUserInterface-300x168.png" width="300" height="168" /></a> <span style="text-decoration: underline"><span style="color: #0000ff;text-decoration: underline">iReport User Interface</span></span>[/caption]</p>
<p>The palette on right under the header Report Elements is the visual offering of all the elements that can be dragged into various report bands.</p>
<p>The properties of currently selected component are shown in the properties section where you can choose to edit corresponding properties of the selected component .</p>
<p>The Reports Problem window at the bottom is like any normal editor console displaying the issues with report compilation and running</p>
<p>As JasperReports is a records-based engine; to print a report, you have to provide a set of records. When the report runs,JasperReports will iterate on this record set, creating and filling the bands according to the report definition. This is why JasperReports defines only one query per report. Multiple queries/data sources can be used when inserting SubReports or defining SubDatasets, each one with their own own query (or data source), fields, parameters, variables, and so on. SubDatasets are only used to feed a crosstab or a chart.</p>
<p>Here is a list of the data sources provided by iReport for JasperReports:
<ul>
    <li><span style="color: #0000ff">JDBC connection</span></li>
    <li><span style="color: #0000ff">JavaBean collection data source</span></li>
    <li><span style="color: #0000ff">XML data source</span></li>
    <li><span style="color: #0000ff">CSV data source</span></li>
    <li><span style="color: #0000ff">Hibernate connection</span></li>
    <li><span style="color: #0000ff">Spring-loaded Hibernate connection</span></li>
    <li><span style="color: #0000ff">JRDataSourceProvider </span></li>
    <li><span style="color: #0000ff">Custom data source</span></li>
    <li><span style="color: #0000ff">Mondrian OLAP connection</span></li>
    <li><span style="color: #0000ff">XMLA connection</span></li>
    <li><span style="color: #0000ff">EJBQL connection</span></li>
    <li><span style="color: #0000ff">Empty data source</span></li>
</ul>
JRDataSource interface represents the abstract representation of a JasperReports data source. All data source types must implement this interface.</p>
<p>If you ask me about difference between using a query inside a report and providing data using a JRDataSource from a java application, the reply will be no difference. In fact, what happens behind the scenes when iReport uses a query instead of a JRDataSource is that JasperReports executes the query using a built-in or user-defined query executor that will produce a JRDataSource</p>
<p>In a report, there are three groups of objects that can store values:</p>
<p><img title="More..." alt="" src="http://xebee.xebia.in/wp-includes/js/tinymce/plugins/wordpress/img/trans.gif" />
<ul>
    <li><span style="color: #0000ff">Parameters</span> are simple input to reports.</li>
    <li><span style="color: #0000ff">Fields</span> are instance variables of the data source object that are passed in to the report .If input data source is java bean then the field names of the bean and if input is sql table then columns of the result set will be the field objects.</li>
    <li><span style="color: #0000ff">Variables</span> are another kind of variables that live within Report, they are not inputs. One can perform many predefined calculation functions on the Fields using Variables.</li>
</ul>
These objects are used in expressions, they can change their values during print progression, and they are typed, that is, all these objects have a type that corresponds to a Java class such as String or Double.</p>
<p>To start with iReport lets try creating a JDBC data source and generate a report based on query results fetching dataset from the data source rather than data being fed from Java application. Next to the data source drop down on the top left click the report datasource icon and after clicking on the new button on the pop up appearing choose the data source type .As we start by choosing a database JDBC connection below shows the required configuration for the same</p>
<p>[caption id="attachment_16147" align="aligncenter" width="300"]<a href="http://xebee.xebia.in/wp-content/uploads/2013/03/Connection.png"><img class="size-medium wp-image-16147" alt="Database Connection" src="http://xebee.xebia.in/wp-content/uploads/2013/03/Connection-300x168.png" width="300" height="168" /></a> <span style="text-decoration: underline"><span style="color: #0000ff;text-decoration: underline">Database Connection</span></span>[/caption]</p>
<p>With the database connection in place we can populate a report with a report query created as below.You can choose the query language from the drop down .The editor can automatically reads fields for you from the table.The filter expression and sort options tab help you enhance further your query.Say the sort tab helps you add fields on whose basis data should be sorted in the asc/desc orders.</p>
<p>[caption id="attachment_16399" align="aligncenter" width="300"]<a href="http://xebee.xebia.in/wp-content/uploads/2013/03/ReportQuery1.png"><img class="size-medium wp-image-16399" alt="Repor tQuery" src="http://xebee.xebia.in/wp-content/uploads/2013/03/ReportQuery1-300x168.png" width="300" height="168" /></a> <span style="text-decoration: underline;color: #0000ff">Report Query</span>[/caption]</p>