---
layout: post
header-img: img/default-blog-pic.jpg
author: usinha
description: 
post_id: 15629
created: 2013/01/10 14:03:38
created_gmt: 2013/01/10 09:03:38
comment_status: open
---

# Testing Stories and Behaviours (BDT)

<p>As promised in my <a href="http://xebee.xebia.in/2012/10/31/mobile-app-automation-testing-2/">previous post</a>, This post introduces a tool for Behaviour Driven Testing.</p>
<p>Agile Teams can now choose from a wide variety of tools for AUATs or Automated User Acceptance Test. Thus a lot of quite different concepts show up. <a href="http://fitnesse.org/">FitNesse</a> for example is using an integrated Wiki to organise testcases, while the <a href="http://code.google.com/p/robotframework/">Robot Framework</a> is using <i>keyword</i>-driven test development. These are examples of two well-established tools in this area.</p>
<p>While <a href="http://jbehave.org">JBehave</a> JBehave is a framework for Behaviour Driven Development (BDD). BDD is an evolution of test-driven development (TDD) and acceptance-test driven design, and is intended to make these practices more accessible and intuitive to newcomers and experts alike. It shifts the vocabulary from being test-based to behaviour-based, and positions itself as a design philosophy.</p>
<p>The post tries to the cover following :-</p>
<p><b>Maven Configuration:</b> Explains the required configuration for Maven to implement and execute tests using <i>JBehave</i>.</p>
<p><b>Stories:</b> Explains how stories are written in <i>JBehave</i> using the BDD-format which are basically the test cases.</p>
<p><b>Implementation:</b> Explains mapping textual steps to Java methods via candidate step. The scenario writer needs to only provide annotated methods that match, by regex patterns, the textual steps.</p>
<p><b>Reporting:</b> Showing the results of a test execution with <i>JBehave</i>.</p>
<p><b>Integration:</b> Maven, description of testcases and implementation of test functionality are glued together using certain naming and directory conventions.</p>
<!--more-->

<p><span style="text-decoration: underline"><strong>Maven Configuration</strong></span></p>
<p>The Maven configuration (POM) is sufficient to start with the development of Acceptance Tests with <i>JBehave</i>. Tests can be compiled, started and even artifacts required for the reporting can be downloaded.</p>
<p>You can download the POM from <a href="https://github.com/jbehave/jbehave-tutorial/blob/master/pom.xml">here</a>.</p>
<p>As said with this configuration all required Jar files are downloaded automatically and the tests can be started using the command: “mvn clean install”. Of course we need some testcases and implementation of test functionality for this to work.</p>
<p><span style="text-decoration: underline"><strong>Test Cases </strong></span></p>
<p>BDD revolves around the concept of a <b>Story</b>, which represents an <b>automatically executable increment of business functionality</b>. At its core a Story comprises of one or more<b>Scenarios</b>, each of which represents a concrete example of the behaviour of the system. Each Scenario comprises of a number of executable steps.</p>
<p><b>Given</b>, <b>When</b> and <b>Then</b> are also called <b>BDD Keywords</b>
<table border="0">
<tbody>
<tr>
<td><i>Given:</i></td>
<td></td>
<td>a static precondition</td>
</tr>
<tr>
<td><i>And:</i></td>
<td></td>
<td>another static precondition</td>
</tr>
<tr>
<td><i>And:</i></td>
<td></td>
<td>…</td>
</tr>
<tr>
<td><i>When:</i></td>
<td></td>
<td>tested behaviour</td>
</tr>
<tr>
<td><i>And:</i></td>
<td></td>
<td>more tested behaviour</td>
</tr>
<tr>
<td><i>And:</i></td>
<td></td>
<td>…</td>
</tr>
<tr>
<td><i>Then:</i></td>
<td></td>
<td>the result of the tested behaviour under the given preconditions</td>
</tr>
<tr>
<td><i>And:</i></td>
<td></td>
<td>…</td>
</tr>
</tbody>
</table>
<i>JBehave</i> aims to provide an <b>automatable</b> description of the behaviour of the system, from the point of view of the stakeholders. As such, it is essential that the language used in the description reflects the language used by the business users. The language and its grammar represents the shared understanding of the behaviour between the business users and the development team. And as any language it evolves with the evolution of this shared understanding. BDD provides the grammar, but the language is agreed between the business and the team.</p>
<p>The emphasis should be placed on concepts relevant to the business, shielding away issues of technical implementation. A good rule of thumb is that if details of the technical implementation can be changed without affecting the overall behaviour, as visible by the business user, then it should not appear in the scenario.
<h3>Write a textual story</h3>
Create a textual story file with a name that expresses the behaviour to verify, e.g. <b>Add_Item_In_Cart.story</b> and define steps in it:
<pre class="lang:default decode:true">Narrative: </p>
<p>In order to show the basic cart functionality
As a user
I want to add and remove items from the cart</p>
<p>Scenario: Item can be added to cart</p>
<p>Given that the cart is empty
When I search for an item
And an item is added to the cart
Then the cart contains that item</pre>
<h3>Map steps to Java methods</h3>
Define your <b>AddItemsCartSteps</b> class, a simple POJO, which will contain the Java methods that are mapped to the textual steps. The methods need to annotated with one of the JBehave annotations and the annotated value should contain a regex pattern that matches the textual step:
<pre class="lang:default decode:true">package org.jbehave.tutorials.etsy.steps;</p>
<p>import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.equalTo;</p>
<p>import org.hamcrest.Matchers;
import org.jbehave.core.annotations.Alias;
import org.jbehave.core.annotations.Composite;
import org.jbehave.core.annotations.Given;
import org.jbehave.core.annotations.Named;
import org.jbehave.core.annotations.Then;
import org.jbehave.core.annotations.When;</p>
<p>public class AddItemsCartSteps {</p>
<p>@Given("I am shopping for a $thing in $section on Etsy.com")
    public void shoppingForSomethingOnEtsyDotCom(String thing, String section) {
        home.go(section);
        home.search(thing);
    }</p>
<p>@When("I want to browse through a treasury gallery")
    @Composite(steps = { "When I want to buy something from etsy.com", "When I want to browse the treasury",
            "When I choose the first treasury gallery" })
    public void browseToFirstTreasuryGallery() {
    }</p>
<p>@Then("there are search results")
    @Alias("results will be displayed in the gallery")
    public void thereAreSearchResults() {
        assertThat(searchResults.resultsFound(), Matchers.greaterThan(0));
    }</pre>
<h3>Configure a Java Embeddable class</h3>
Define your Embeddable class which provides the link between the JBehave's executor framework (called Embedder) and the textual stories. The simplest configuration is a one-to-one mapping between a Java class and a textual story file. So we'll extend JUnitStory and give it a name that can be (conventionally) mapped to the textual story filename, e.g.<b>AddItemsCart</b><b>.java.</b></p>
<p>Obviously it is very helpful being able to execute the tests directly from an IDE (e. g. Eclipse). This is working with <i>JBehave </i>very easily thanks to this unimposing method from the above shown class:
<pre> @Override
    @Test
    public void run() {
        try {
            super.run();
        } catch (Throwable e) {
            e.printStackTrace();
        }
    }</pre>
<span style="text-decoration: underline"><strong>Reporting</strong></span>
<div>•Reporting is an essential element of BDD as it allows to monitor the outcome of the stories that have been run. At the heart of JBehave's reporting is the StoryReporter, to which events are reported as they occur. Currently, the story reporters supported are:</div>
<div></div>
<div><strong>•ConsoleOutput:</strong> A text-based console output</div>
<div><strong>•IdeOnlyConsoleOutput:</strong> A text-based console output, which only reports events when running in IDEs (Eclipse and IDEA supported).</div>
<div><strong>•TxtOutput:</strong> A text-based file output</div>
<div><strong>•HtmlOutput:</strong> An HTML file output</div></p>