---
layout: post
header-img: img/default-blog-pic.jpg
author: Gagan
description: 
post_id: 18284
created: 2014/04/30 09:58:51
created_gmt: 2014/04/30 04:58:51
comment_status: open
---

# Run Predictive Machine Learning algorithms on Hadoop without even knowing Mapreduce.

<p>Data Scientists are very much familiar with working on tools like R, SAS etc. for them writing or converting algorithms into mapreduce is bit difficult. There are libraries such as Mahout is available which provides mapreduce implementation of many algorithms. you can not run your algorithm directly on a hadoop cluster. Before that you need to create a Data Model based on data and decide the values for some tweaking parameters and changing these parameters multiple time in hadoop job and running again and again is bit pain for a Data scientist, for a java developer it could be a fun. Data scientists can do Data modeling or model training in SAS/R very easily and efficiently.</p>
<!--more-->

<p>Cascading comes up with a solution where Data scientists can design their data model in R and then they can export it into a PMML ( what is this? we will see in a moment) file and run cascading job which will run this algorithm over hadoop cluster. We need to write here minimal code. We need not to change the job if we are changing data model even if we are trying with different algorithm.</p>
<p>Let’s understand some of the terms we used above.
<ul>
<li><b>PMML</b> Predictive Model Markup Language is an XML-based file format developed by the Data Mining Group to provide a way for applications to describe and exchange models produced by data mining and machine learning algorithms [Wikipedia].
</li>
<li>
<b>Cascading</b> is a software abstraction layer for Apache Hadoop. Cascading is used to create and execute complex data processing workflows on a Hadoop cluster using any JVM-based language (Java, JRuby, Clojure, etc.), hiding the underlying complexity of MapReduce jobs. It is open source and available under the Apache License [Wikipedia].
</li>
<li><b>Cascading Pattern</b> is an extension to Cascading that provides various machine learning scoring algorithms and a utility for translating Predictive Model Markup Language (PMML) documents into applications on Apache Hadoop. Now you can deploy predictive models on to Hadoop or utilize the Cascading Pattern Java API to deploy your models or sophisticated ensembles [cascading.org]
</li></p>
</ul>

<p>Here PMML helps us in defining data models with the help of any good predictive analytics tools and feed that PMML file to cascading job and you are done.</p>
<p>Let’s understand this with the help of very simple example.</p>
<p>Let’s say I want to run simple linear regression algorithm on iris dataset. iris is very popular dataset if you are not aware of this please read <a href="http://archive.ics.uci.edu/ml/datasets/Iris" title="Iris data set" target="_blank">here</a>.</p>
<p><b>Steps</b>
<ol>
<li>Let’s say I want to predict sepal length based on the other parameters.</p>
<p>[code]</p>
<p>data(iris)
str(iris)
liearModel&lt;-lm(Sepal.Length~.,data=iris)
library(pmml)
irisPMML&lt;-pmml(liearModel)
saveXML(irisPMML, &quot;irisPMML.xml&quot;)</p>
<p>[/code]
</li>
<li>Now pmml file is available in the working directory of R. You can check your working directory using following command
[code]
getwd()
[/code]
</li></p>
<li>Create a new java project using maven as a build tool and add following dependencies.
[code]
&lt;repositories&gt;
        &lt;repository&gt;
            &lt;id&gt;conjars.org&lt;/id&gt;
            &lt;url&gt;http://conjars.org/repo&lt;/url&gt;
        &lt;/repository&gt;
    &lt;/repositories&gt;
    &lt;dependencies&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;cascading&lt;/groupId&gt;
            &lt;artifactId&gt;cascading-core&lt;/artifactId&gt;
            &lt;version&gt;2.5.1&lt;/version&gt;
        &lt;/dependency&gt;

        &lt;dependency&gt;
            &lt;groupId&gt;cascading&lt;/groupId&gt;
            &lt;artifactId&gt;cascading-local&lt;/artifactId&gt;
            &lt;version&gt;2.5.1&lt;/version&gt;
        &lt;/dependency&gt;

        &lt;dependency&gt;
            &lt;groupId&gt;cascading&lt;/groupId&gt;
            &lt;artifactId&gt;cascading-hadoop&lt;/artifactId&gt;
            &lt;version&gt;2.5.1&lt;/version&gt;
        &lt;/dependency&gt;

        &lt;dependency&gt;
            &lt;groupId&gt;cascading&lt;/groupId&gt;
            &lt;artifactId&gt;cascading-xml&lt;/artifactId&gt;
            &lt;version&gt;2.5.1&lt;/version&gt;
        &lt;/dependency&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;cascading&lt;/groupId&gt;
            &lt;artifactId&gt;pattern-pmml&lt;/artifactId&gt;
            &lt;version&gt;1.0.0-wip-46&lt;/version&gt;
        &lt;/dependency&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;cascading&lt;/groupId&gt;
            &lt;artifactId&gt;pattern-core&lt;/artifactId&gt;
            &lt;version&gt;1.0.0-wip-46&lt;/version&gt;
        &lt;/dependency&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;cascading&lt;/groupId&gt;
            &lt;artifactId&gt;pattern-hadoop&lt;/artifactId&gt;
            &lt;version&gt;1.0.0-wip-46&lt;/version&gt;
        &lt;/dependency&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;org.apache.hadoop&lt;/groupId&gt;
            &lt;artifactId&gt;hadoop-core&lt;/artifactId&gt;
            &lt;version&gt;1.2.1&lt;/version&gt;
            &lt;scope&gt;provided&lt;/scope&gt;
        &lt;/dependency&gt;

[/code]
</li>

<li>
Let’s write cascading job which reads PMML file and run the algorithm defined in the pmml file on input data over hadoop cluster. 
[code]
public class RunLinearRegressionPMMLExportFromR {

    public static void main(String args[]) throws IOException, ParseException {

        //getting input and output paths
        String inputPath = args[0];
        String outputPath = args[1];

        // some cascading classpath setting stuff
        Properties properties = new Properties();
        AppProps.setApplicationJarClass(properties,
                RunLinearRegressionPMMLExportFromR.class);
        AppProps.setApplicationJarPath(
                properties,
                &quot;/home/gagan/cascading-pmml/target/cascading-pmml-1.0-SNAPSHOT-packed.jar&quot;);

        //getting pmml file name with path in job itself
        Options options = new Options();
        options.addOption(&quot;pmml&quot;, true, &quot;pmml file&quot;);
        CommandLineParser parser = new PosixParser();
        CommandLine cmd = parser.parse(options, args);

        if (!(cmd.hasOption(&quot;pmml&quot;))) {
            HelpFormatter formatter = new HelpFormatter();
            formatter.printHelp(&quot;RunLinearRegressionPMMLExportFromR.java&quot;,
                    options);
        }

        // pmml file with path
        String pmmlWithPath = cmd.getOptionValue(&quot;pmml&quot;);

        // creating cascading input/output Tap. Tap is a cascading terminology to define a source/sink.
        Tap inputTap = new Hfs(new TextDelimited(true, &quot;,&quot;, &quot;\&quot;&quot;), inputPath);
        Tap outputTap = new Hfs(new TextDelimited(true, &quot;,&quot;, &quot;\&quot;&quot;), outputPath);

        // generating flow definition 
        FlowDef pmmlFlowDef = FlowDef.flowDef().setName(&quot;pmml-flow&quot;)
                .addSource(&quot;input&quot;, inputTap).addSink(&quot;output&quot;, outputTap);

        // PmmlPlanner. This will read pmml file and create a plan how cascading is going to create map reduce jobs for you data model.
        PMMLPlanner pmmlPlanner = new PMMLPlanner().setPMMLInput(new File(
                pmmlWithPath));
        pmmlFlowDef.addAssemblyPlanner(pmmlPlanner);

        // creating the final flow.
        Flow flow = new HadoopFlowConnector(properties).connect(pmmlFlowDef);

        // We can write dot file to understand how cascading is writing the flow.
        flow.writeDOT(&quot;dot/flow.dot&quot;);

        //run and complete the flow.
        flow.complete();

    }
}

[/code]

I have written detailed comments in code to explain what is happening there.
</li>

<li>
Build the above code and run using following command.

[code]
hadoop jar &lt;Jar File&gt; &lt;fully qualified class name &gt; &lt;input path&gt; &lt;output path&gt; -pmml &lt;pmml file with path&gt;
[/code]
</li>

<li>After successfully running above hadoop job you can see find your predicted sepal lengths on the given iris input data.
</li>

<p></ol>
You can see the beauty of Cascading. We are able to run our linear regression algorithm over hadoop using mapreduce paradigm with a few lines of code.</p>
<p>Cascading Pattern following predictive modeling algorithms. 
<ul>
<li>Hierarchical Clustering</li>
<li>K-Means Clustering</li>
<li>Linear Regression</li>
<li>Logistic Regression</li>
<li>Random Forest Algorithm</li>
</ul>
Cascading team is working on providing support for following algorithms in their next release.</p>
<ul>
<li>Neural Network</li>
<li>Support Vector Machine</li>
</ul>