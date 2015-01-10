---
layout: post
header-img: img/default-blog-pic.jpg
author: nkhattar
description: 
post_id: 8143
created: 2011/03/29 09:51:25
created_gmt: 2011/03/29 04:51:25
comment_status: open
---

# Flex: Creating Complex Charts using Multiple Categories

<p>In my last <a href=" http://xebee.xebia.in/2011/03/24/flex-composite-charts-with-multiple-series-axes/" target="_blank">Blog</a> on Flex Charting components, we talked about how to create <strong>Composite charts</strong> in Flex using <em>multiple chart series, multiple data series and multiple axes</em>. We also discussed the common problem faced while clubbing different chart series within a single chart. Then we explored the concept of <strong>ColumnSet</strong> in Column Charts &amp; similarly <strong>BarSet</strong> in Bar Charts. Also we learned about the significance of having multiple axes (vertical or horizontal) in a chart and how we can visualize indirectly related data with each other.</p>
<p>So now let’s raise the complexity to the next level and discuss how to create Complex Charts in Flex, and also showcase an example on creating such charts, this time using multiple categories.<!--more--></p>
<p>Creating Complex charts accounts for some <strong>important considerations</strong> that one should keep in mind:
<ol>
    <li><strong>Format</strong> (or Syntax) of the Data Collection (Arrays,      XML, Objects, etc.)</li>
    <li><strong>Parsing </strong>the data collection into suitable Chart      Data collection</li>
    <li><strong>Configuring</strong> Specific Chart Type with its specific      properties &amp; behavior</li>
</ol>
The <strong>first &amp; second </strong>points deal with the way we store the data collection in the form of XMLs or Remote Objects; and the way we parse it before sending to the chart. This might look like a minor detail to fuss over, but trust me, it’s important to tackle this beforehand; otherwise the chart would represent the same in a wholly unexpected graphical format.</p>
<p>The <strong>third </strong>point is undoubtedly the most important of above all. This deals with configuring the required chart type with its distinct properties &amp; controls and that too in the right manner. The properties may include vertical axis, horizontal axis, series, chart type, etc.</p>
<p>Ok, let’s start exploring this concept with the help of an example:</p>
<p><strong>Example – Column Chart with multiple categories &amp; chart series</strong></p>
<p>Let’s use the problem statement as used in the previous <a href="http://xebee.xebia.in/2011/03/24/flex-composite-charts-with-multiple-series-axes/#compositeChartEg1" target="_blank">blog</a> and redefine it a bit:</p>
<p>As a sales manager, I want to visualize the Data for the Sales (Bottle &amp; Canned) of different types of Soft Drinks (Coke, Pepsi &amp; Sprite) in different metropolitan Cities (Delhi, Mumbai &amp; Bangalore) in the form of a Column Chart where Soft Drink is the primary category, City is the secondary category, and Bottled Sales &amp; Canned Sales are the two series. In a nutshell, we are looking at <strong>City Sales (Bottled &amp; Canned), Product wise</strong>.</p>
<p>From the above mentioned statement, we could easily make out some basic facts that ‘product’ as soft drink will serve as the horizontal axis, ‘bottled / canned sales’ will serve as the vertical axis. But the question is <span style="color: #ff0000;">Where to accommodate an extra category, i.e. ‘<strong>city</strong>’?</span></p>
<p>The chart should appear like this:
<div style="width: 100%; align: center;"><img class="aligncenter size-full wp-image-8155" title="MultipleCategoryChart" src="http://xebee.xebia.in/wp-content/uploads/2011/03/MultipleCategoryChart.bmp" alt="Complex Chart with Multiple=" /></div>
What we know so far is that we can have <strong>only one category axis</strong> as the horizontal axis of a chart and can have multiple chart series which can be grouped accordingly but having another Category Axis is altogether a new and a bit unclear requirement.</p>
<p>But, again the answer to this question is <strong>ColumnSet</strong>.  If you remember, Definition of Column Set as per the Flex Charting API has something to add in this respect:</p>
<p>“<em>ColumnSet is a grouping set that can be used to stack or cluster column series in any arbitrary chart. A ColumnSet encapsulates the same grouping behavior used in a ColumnChart, but can be used to assemble custom charts based on CartesianChart. ColumnSets can be used to cluster any chart element type that implements the IColumn<strong> </strong>interface. It can stack any chart element type that implements the IColumn and IStackable interfaces. </em><strong><em>Since ColumnSet itself implements the IColumn interface, you can use ColumnSets to cluster other ColumnSets to build more advanced custom charts.</em></strong><em>”</em></p>
<p>Summarizing the ‘Golden Rule’ above, we need to <strong>make a collection of such Column sets for every distinct City grouped within a Parent Column Set for the product</strong>. Each City Column Set will be of type ‘<strong>Stacked’</strong> and Product Column Set will be of type ‘<strong>Clustered’</strong>.
And we will get our chart created in the desired manner.
<div style="width: 100%; align: center;"><img class="aligncenter size-full wp-image-8156" title="MultipleCategoryColumnSet" src="http://xebee.xebia.in/wp-content/uploads/2011/03/MultipleCategoryColumnSet.png" alt="Multiple Category Column Set Representation" width="546" height="402" /></div>
So let’s solve this problem taking into consideration the above mentioned approach for creating complex charts:
<ul>
    <li>Data Collection Format – I am using a simple xml as the data collection</li>
</ul>
[sourcecode lang="xml"]
&lt;chart&gt;
   &lt;chartData product=&quot;Coke&quot; city=&quot;Delhi&quot; bottledSales=&quot;500&quot; cannedSales=&quot;400&quot;/&gt;
   &lt;chartData product=&quot;Coke&quot; city=&quot;Mumbai&quot; bottledSales=&quot;800&quot; cannedSales=&quot;600&quot;/&gt;
   &lt;chartData product=&quot;Coke&quot; city=&quot;Banglore&quot; bottledSales=&quot;1200&quot; cannedSales=&quot;700&quot;/&gt;
   &lt;chartData product=&quot;Pepsi&quot; city=&quot;Delhi&quot; bottledSales=&quot;700&quot; cannedSales=&quot;1000&quot;/&gt;
   &lt;chartData product=&quot;Pepsi&quot; city=&quot;Mumbai&quot; bottledSales=&quot;500&quot; cannedSales=&quot;800&quot;/&gt;
   &lt;chartData product=&quot;Pepsi&quot; city=&quot;Banglore&quot; bottledSales=&quot;800&quot; cannedSales=&quot;300&quot;/&gt;
   &lt;chartData product=&quot;Sprite&quot; city=&quot;Delhi&quot; bottledSales=&quot;1200&quot; cannedSales=&quot;800&quot;/&gt;
   &lt;chartData product=&quot;Sprite&quot; city=&quot;Mumbai&quot; bottledSales=&quot;500&quot; cannedSales=&quot;1000&quot;/&gt;
   &lt;chartData product=&quot;Sprite&quot; city=&quot;Banglore&quot; bottledSales=&quot;1200&quot; cannedSales=&quot;500&quot;/&gt;
&lt;/chart&gt;
[/sourcecode]
<ul>
    <li><span style="font-family: Georgia, 'Times New Roman', 'Bitstream Charter', Times, serif; line-height: 19px; white-space: normal;">Correctly Parsing the Data collection into      Suitable Chart Data –</span></li>
</ul>
[sourcecode lang="javascript"]
public function set chartData(chart:Object):void
{
   var chartDataCollection:ArrayCollection = new ArrayCollection(chart.chartData);
   var chartDataCollection:ArrayCollection = event.result.chart.chartData;
   for each(var chartDataObject:Object in chartDataCollection)
   {
     if(!categoryOneDataprovider.contains(chartDataObject[ChartEnum.CATEGORY_ONE]))
        {categoryOneDataprovider.addItem(chartDataObject[ChartEnum.CATEGORY_ONE]);}
     if(!categoryTwoCollection.contains(chartDataObject[ChartEnum.CATEGORY_TWO]))
        {categoryTwoCollection.addItem(chartDataObject[ChartEnum.CATEGORY_TWO]);}
   }
   for each(var categoryTwoDataValue:String in categoryTwoCollection)
   {
     var categoryTwoDataCollection:ArrayCollection = new ArrayCollection();
     for each(var chartDataVO:Object in chartDataCollection)
     {
         if(chartDataVO[ChartEnum.CATEGORY_TWO] == categoryTwoDataValue)
           {categoryTwoDataCollection.addItem(chartDataVO);}
     }
     categoryTwoDataProvider.addItem(categoryTwoDataCollection);
   }
}
[/sourcecode]
<ul>
    <li>Configuring Column Chart with its specific controls &amp; properties like categories, series &amp; axes –</li>
</ul></p>

## Comments

**[surajit das](#5696 "2011-07-08 16:21:27"):** Nice explanation you have given above on Flex charting.However,can you also do a series of such trainings on creating custom chart types.I mean,I have a client who wants to represent data in form of a test tube filled with water.The test tubes will act as a bar charts and as we provide data to flex,the level of water in the set of test tubes must change in the chart.The test tube with bubbling water movie is a swf file,which is to be used as a chart in my case basically.Can you give some clues. Surajit Surajitdas01@gmail.com

**[Nitin Khattar](#5699 "2011-07-08 21:19:28"):** @Surajit: Thanks for your comment. I understood your usecase of showing test tubes in place of bars in a bar chart, and dynamically updating its level based on the data fed into the chart. But, for this you cannot make use of Default Flex Charting components. For building such custom charts, you need to programatically create chart using actionscript. This includes creating axis renderers and embedding test tube swf as series of the chart, using Loaders along with passing data into it which in turn will effect the level of liquid in the test tube.

**[Nitin Khattar](#6416 "2011-12-19 09:57:20"):** @win : One can easily add labels for every datablock via css. There is a style property in flex called 'labelPosition' that you can set to 'inside, outside, callout, or insideWithCallout'. This property is available for only Bar series, Column Series & Pie Series. You can also customize alignment, rotation & text of the datalabels. Refer to this link for any details. http://help.adobe.com/en_US/flex/using/WS2db454920e96a9e51e63e3d11c0bf69084-7c67.html In case of any queries, feel free to drop me a line

**[JZTexas](#5994 "2011-10-07 21:31:03"):** Nitin, Thank you for the very well done tutorial. You saved me a LOT of hours trying to figure this one out on my own.

**[win](#6401 "2011-12-16 06:45:46"):** how to show datalabel

**[Verma](#7907 "2012-03-13 12:22:53"):** Thanks a lot NItin, This is exactly what I am working on currently and it has saved me a lot of time. If you can just give me a hint on How to have extra horizontal labels like for cities (Delhi, Mumbai and Banglore) over the top of existing labels of Coke, pepsi and sprite. Thanks...

