---
layout: post
header-img: img/default-blog-pic.jpg
author: nkhattar
description: 
post_id: 7981
created: 2011/03/24 08:56:55
created_gmt: 2011/03/24 03:56:55
comment_status: open
---

# Flex: Composite Charts with Multiple Series & Axes

<p>Recently in my project, I came across a requirement to introduce Flex Charting components in the application as a means of <strong>Data Visualization</strong>. The application deals with Business Analysis of tons of raw data in tabular format with various categories &amp; sub-categories, alphabetical &amp; numeric data. That’s why for an end-user Data Visualization is a must feature.</p>
<p>Flex provides a wide range of charting controls with varied types &amp; configurable properties. Flex supports the most common types of two-dimensional charts (such as bar, column, and pie charts) and gives you a great deal of control over the appearance of charts.</p>
<p>Creating Simple Charts turned out to be an easy task with the help of <strong><a href="http://help.adobe.com/en_US/flex/using/WS2db454920e96a9e51e63e3d11c0bf69084-7bdf.html" target="_blank">Adobe Help Reference</a></strong> and <strong><a href="http://blog.flexexamples.com/category/charting/" target="_blank">Flex Blog examples</a></strong>, but creating composite charts was not that easy as it appeared to me. So in this blog, I will elaborate on how to create composite charts and will showcase few examples on creating composite charts using multiple chart series and multiple axes.</p>
<!--more-->

<p>Creating <strong>Composite chart</strong> accounts for an important consideration that one should keep in mind:
<ul>
    <li>Configuring specific chart type with its specific properties &amp; behavior and then clubbing each of them in a composite way such that it suits for each chart type or series in a perfect manner.</li>
</ul>
Ok, let’s start exploring this concept with few examples:
<a name="compositeChartEg1"></a>
<strong>Example 1 - Composite Chart (Column &amp; Line) with multiple column series</strong></p>
<p>Let’s consider a problem statement:</p>
<p>As a sales manager, I want to visualize the Data for the Sales (Bottle &amp; Canned) of few types of Soft Drink (Coke, Pepsi &amp; Sprite) in a City (Delhi) in the form of a Column Chart and a separate Line Chart depicting the Total Sales.</p>
<p>From the above mentioned statement we could easily make out some basic facts that ‘product’ as soft drink  will serve as the <strong>horizontal axis</strong>, ‘sales’ will serve as the <strong>vertical axis</strong>. Chart will consist of two <strong>column series</strong> (one for ‘bottled sales’ &amp; another for ‘canned sales’) and one<strong> line series</strong> for ‘total sales’.</p>
<p>The chart may appear like this:
<div style="width: 100%; align: center;"><img class="aligncenter" title="Single ColumnSet" src="http://xebee.xebia.in/wp-content/uploads/2011/03/SingleColumnSet.jpg" alt="" width="388" height="296" /></div>
Ok, I think the problem is quite straightforward and the basic fact is to make a <strong>Stacked</strong> Column Chart with two column series and one line Series. So, following is a common approach &amp; unluckily a <strong>common mistake</strong> that we all may commit ..</p>
<p>[sourcecode language="java"]
...
[Bindable]
private var delhi:ArrayCollection = new ArrayCollection([
       {productName:&quot;Coke&quot;,bottledSales:200,cannedSales:500,total:700},
       {productName:&quot;Pepsi&quot;,bottledSales:400,cannedSales:700,total:1100},
       {productName:&quot;Sprite&quot;,bottledSales:300,cannedSales:600,total:900}
]);
...
&lt;mx:ColumnChart id=&quot;multipleAxisColumn&quot; showDataTips=&quot;true&quot; dataProvider=&quot;{delhi}&quot;
       type=&quot;stacked&quot;&gt;
       &lt;mx:horizontalAxis&gt;
             &lt;mx:CategoryAxis categoryField=&quot;productName&quot; title=&quot;Product&quot;/&gt;
       &lt;/mx:horizontalAxis&gt;
       &lt;mx:verticalAxis&gt;
         &lt;mx:LinearAxis id=&quot;series1Axis&quot; title=&quot;Sales&quot;/&gt;
       &lt;/mx:verticalAxis&gt;
       &lt;mx:series&gt;
             &lt;mx:ColumnSeries yField=&quot;bottledSales&quot; displayName=&quot;Bottled Sales&quot; /&gt;
             &lt;mx:ColumnSeries yField=&quot;cannedSales&quot; displayName=&quot;Canned Sales&quot; /&gt;
         &lt;mx:LineSeries yField=&quot;total&quot; displayName=&quot;Total Sales&quot;/&gt;
       &lt;/mx:series&gt;
&lt;/mx:ColumnChart&gt;
[/sourcecode]</p>
<p>Oops….an exception occurs when we try to run the application</p>
<p><span style="color: #ff0000;">ReferenceError: Error #1056: Cannot create property offset on mx.charts.series.LineSeries.
mx.charts::ColumnChart/applySeriesSet()
[E:\dev\hero_private\frameworks\projects\datavisualization\src\mx\charts\ColumnChart.as:530]
mx.charts.chartClasses::CartesianChart/http://www.adobe.com/2006/flex/mx/internal::updateSeries()
[E:\dev\hero_private\frameworks\projects\datavisualization\src\mx\charts\chartClasses\CartesianChart.as:1042]…</span></p>
<p>As you can see when we try to run the above code, it gives a runtime error implying that the Chart Type ‘Stacked’ cannot be applied to Line Series. So, now the question is how to apply the stacked type to the column series separately, since there is <strong>no property named ‘Type’ in a Column Series</strong>??</p>
<p>Answer is to exploit the concept of <strong>ColumnSet</strong>. This is what Flex Charting API has to say about Column Set –</p>
<p>“<em><strong>ColumnSet </strong></em><em>is a grouping set that can be used to stack or cluster column series in any arbitrary chart. A ColumnSet encapsulates the same grouping behavior used in a ColumnChart, but can be used to assemble custom charts based on CartesianChart. ColumnSets can be used to cluster any chart element type that implements the <strong>IColumn interface</strong>. It can stack any chart element type that implements the IColumn and IStackable interfaces. Since <strong>ColumnSet itself implements the IColumn interface</strong>, you can use ColumnSets to cluster other ColumnSets to build more advanced custom charts.”</em></p>
<p>Summarizing the above statement, we can group the two column series in a column set and in this way we can apply same properties that a Column Chart persists like Chart Type which in our case is ‘stacked’. Similarly in Bar Charts, there is a concept of <strong>BarSet</strong>.
<div style="width: 100%;"><img class="aligncenter" title="ColumnSet" src="http://xebee.xebia.in/wp-content/uploads/2011/03/ColumnSet-300x253.jpg" alt="Exhibiting Column Set" width="300" height="253" /></div>
[sourcecode language="java"]
&lt;mx:series&gt;
    &lt;mx:ColumnSet type=&quot;stacked&quot;&gt;
       &lt;mx:ColumnSeries yField=&quot;bottledSales&quot; displayName=&quot;Bottled Sales&quot; /&gt;
       &lt;mx:ColumnSeries yField=&quot;cannedSales&quot; displayName=&quot;Canned Sales&quot; /&gt;
    &lt;/mx:ColumnSet&gt;
    &lt;mx:LineSeries yField=&quot;total&quot; displayName=&quot;Total Sales&quot;/&gt;
&lt;/mx:series&gt;
[/sourcecode]</p>
<p>The only change observed in the code is that now the two column series are wrapped into a column set and the <strong>type = “stacked”</strong> property is removed from the level of Column Chart &amp; is added to the Column Set, instead.</p>
<p>Now if we run our modified code, it runs successfully showing the desired results.</p>
<p>[kml_flashembed publishmethod="dynamic" fversion="10.1.0" useexpressinstall="true" movie="http://xebee.xebia.in/wp-content/uploads/2011/03/SingleColumnSet.swf" width="550" height="400" targetclass="flashmovie"]</p>
<p><a href="http://adobe.com/go/getflashplayer"><img src="http://www.adobe.com/images/shared/download_buttons/get_flash_player.gif" alt="Get Adobe Flash player" /></a></p>
<p>[/kml_flashembed]</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2011/03/SingleColumnSet.zip" target="_self">Download Source Code</a></p>
<p><strong>Example 2 – Composite Chart (Column &amp; Line) with multiple axes</strong></p>
<p><strong> </strong></p>
<p>Using the same problem as stated above, idea is to create the above mentioned composite chart with the Line Series now representing a separate &amp; distinct measure named ‘<strong>No. of Sales Persons Involved</strong>’ with a <strong>separate vertical axis</strong> for the same.</p>

## Comments

**[Nitin Khattar](#5605 "2011-06-01 12:31:35"):** @Mariya: Sorry for the late reply. I may not be right person to answer this as i have done charting in Flex not in Android. I'll ask my fellow colleague Yogesh to touch base with you as he has a clear idea on this.

**[Roopesh](#5604 "2011-06-01 11:28:53"):** Hey this is exactly what I am trying to do (composite chart with different axes) - only difference is there are four column series stacked with a separate line series. Now here's the wierd thing - when I load the entire thing with the data, it just doesn't load some of the data in the column series (for some x values, one or two series data is missing). What's more, If I refresh the page (the flex component reloads entirely in the same browser, it starts working properly. (Extremely wierd because right now the data is hardcoded as an object array - there is no dependency on any webservice or anything that can change!) Have you ever faced this kind of issue before?

**[Mariya](#5600 "2011-05-27 18:26:50"):** How to generate more than one chart in android... I am using afreechart jar file to generate the charts.... I want to display the charts one by one... I want to see that charts one by one using the scroll bar.... please guide me....

**[Nitin Khattar](#5608 "2011-06-02 08:11:28"):** @Roopesh: Haven't faced such an issue. If possible could you share the data as I tried out with four column series but couldn't find such a problem.

**[Mike J](#6052 "2011-10-25 02:22:02"):** this probably works on Flash Builder 4 or 4.5 but in Flex Builder 3.2 it does not. The code does not stack the bars you only see 1 bar and the line. There is a bug associated with this and they say to use the CartesianChart type.

**[vengatesh](#9272 "2012-07-30 11:36:50"):** great coding logic.

**[Nitin Khattar](#9273 "2012-07-30 11:46:04"):** Thanks Vengatesh

**[Sam](#7522 "2012-02-10 14:27:41"):** Thanks a lot for your blog. I resolved my issue with the help of your blog.

**[Nitin Khattar](#7523 "2012-02-10 14:38:11"):** Thanks Sam.

