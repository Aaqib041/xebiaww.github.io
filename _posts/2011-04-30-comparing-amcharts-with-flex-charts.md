---
layout: post
header-img: img/default-blog-pic.jpg
author: GauravS
description: 
post_id: 8757
created: 2011/04/30 23:59:33
created_gmt: 2011/04/30 18:59:33
comment_status: open
---

# Comparing AmCharts with Flex Charts

<p>In this blog, i will share my experiences with <a href="http://flex.amcharts.com/">AmCharts</a> (Third party API for Flex Charting).</p>
<p>The requirement was to show a dataset in two different views, first was a tabular view and second was the chart view. For tabular view we simply used the Flex datagrid. For showing the same dataset in charts, we first evaluated Flex charts included in the Flex SDK. (Here onwards, 'Flex Charts' would refer to the Flex Charting available in Flex SDK) It solved our purpose of showing the data, but somehow it was not appealing from a UI perspective. We all know that Flex Charts can be customized to any extent, but due to a time bound release, we were not able to implement it. <!--more--></p>
<p>While googling, i found many third party API for Flex Charts. But either they were complicated to use, or did not have attractive UI. I came across AmCharts, easy to use, great UI effects, customizable and easily came in client's budget.</p>
<p>To use AmCharts was pretty much simple. Below is the break down of the basic entities:
<ul>
    <li> Type of Chart: Our requirement was to show a column chart, so we used <a href="http://flex.amcharts.com/class_ref&lt;/li&gt; &lt;p&gt;erence/com/amcharts/AmSerialChart.html">AmSerialChart</a>. It contains the property 'dataProvider'. You can specify 'categoryField' here.</li>
    <li>CategoryAxis: This is the horizontal axis.</li>
    <li>ValueAxis: This is the verical axis.</li>
    <li>Graphs: An array containing items of type <a href="http://flex.amcharts.com/class_reference/com/amcharts/chartClasses/AmGraph.html">AmGraph</a>, which is the series for the charts. It contains the 'valueField' property.</li>
</ul></p>
<p>If you have used Flex Charts, then you can see that it is very similar to Flex Charts, but AmCharts do not extend Flex Charts. Instead they are built from scratch by extending the UIComponent class.</p>
<p>There were certain factors which made us to go with AmCharts.</p>
<p>a) <strong>3D Look &amp; Feel</strong>: It's good to have 3D looks with charts. Flex charts (included in Flex SDK) do not have 3D looks. There are certain ways which you can use to get the feel but no doubt they are time consuming. But with AmCharts, you get these all ready.</p>
<p>b) <strong>Effects</strong>: Charts with showing up with some effects is something which every customer would want. With AmCharts, you get these effects. Also, you can customize these effects. Flex Charts also provides you with effects, but the quality of effects is better and that too when you don't have explicitly write code for them.</p>
<p>c) <strong>Easy Integration</strong>: Though Flex Charts are the best under this category, but integrating AmCharts was not at all a difficult job. But yes, while googling i found many third party APIs which were difficult to use and integrate.</p>
<p>d) <strong>Cost</strong>: Seeing the quality and the time it saves, <a href="http://www.amcharts.com/buy">cost</a> of AmCharts are pretty reasonable.</p>
<p>We also faced a problem with AmCharts. Our requirement was to show axis labels (title of the axis). Unfortunately there is no direct way of doing it. So the only workaround was to show Flex default Label across the Category Axis and the Value Axis. So if you are stuck with it, don't waste your time by googling on it. Just go with the above solution.</p>
<p>So, if you also want to have good UI that too in a short span of time, do give a try to AmCharts.</li></p>
<p>Useful links:
<a href="http://flex.amcharts.com/files/getfile.php?filename=amcharts_flex_components_1.7.1.0.zip">Download</a>
<a href="http://flex.amcharts.com/docs">Documentation</a></p>