---
layout: post
header-img: img/default-blog-pic.jpg
---

# Testing Stories and Behaviours (BDT)

As promised in my [previous post](/2012/10/31/mobile-app-automation-testing-2/), This post introduces a tool for Behaviour Driven Testing. Agile Teams can now choose from a wide variety of tools for AUATs or Automated User Acceptance Test. Thus a lot of quite different concepts show up. [FitNesse](http://fitnesse.org/) for example is using an integrated Wiki to organise testcases, while the [Robot Framework](http://code.google.com/p/robotframework/) is using _keyword_-driven test development. These are examples of two well-established tools in this area. While [JBehave](http://jbehave.org) JBehave is a framework for Behaviour Driven Development (BDD). BDD is an evolution of test-driven development (TDD) and acceptance-test driven design, and is intended to make these practices more accessible and intuitive to newcomers and experts alike. It shifts the vocabulary from being test-based to behaviour-based, and positions itself as a design philosophy. The post tries to the cover following :- **Maven Configuration:** Explains the required configuration for Maven to implement and execute tests using _JBehave_. **Stories:** Explains how stories are written in _JBehave_ using the BDD-format which are basically the test cases. **Implementation:** Explains mapping textual steps to Java methods via candidate step. The scenario writer needs to only provide annotated methods that match, by regex patterns, the textual steps. **Reporting:** Showing the results of a test execution with _JBehave_. **Integration:** Maven, description of testcases and implementation of test functionality are glued together using certain naming and directory conventions.  **Maven Configuration** The Maven configuration (POM) is sufficient to start with the development of Acceptance Tests with _JBehave_. Tests can be compiled, started and even artifacts required for the reporting can be downloaded. You can download the POM from [here](https://github.com/jbehave/jbehave-tutorial/blob/master/pom.xml). As said with this configuration all required Jar files are downloaded automatically and the tests can be started using the command: “mvn clean install”. Of course we need some testcases and implementation of test functionality for this to work. **Test Cases ** BDD revolves around the concept of a **Story**, which represents an **automatically executable increment of business functionality**. At its core a Story comprises of one or more**Scenarios**, each of which represents a concrete example of the behaviour of the system. Each Scenario comprises of a number of executable steps. **Given**, **When** and **Then** are also called **BDD Keywords**

_Given:_
a static precondition

_And:_
another static precondition

_And:_
…

_When:_
tested behaviour

_And:_
more tested behaviour

_And:_
…

_Then:_
the result of the tested behaviour under the given preconditions

_And:_
…
_JBehave_ aims to provide an **automatable** description of the behaviour of the system, from the point of view of the stakeholders. As such, it is essential that the language used in the description reflects the language used by the business users. The language and its grammar represents the shared understanding of the behaviour between the business users and the development team. And as any language it evolves with the evolution of this shared understanding. BDD provides the grammar, but the language is agreed between the business and the team. The emphasis should be placed on concepts relevant to the business, shielding away issues of technical implementation. A good rule of thumb is that if details of the technical implementation can be changed without affecting the overall behaviour, as visible by the business user, then it should not appear in the scenario. 

### Write a textual story

Create a textual story file with a name that expresses the behaviour to verify, e.g. **Add_Item_In_Cart.story** and define steps in it: 
    
    
    Narrative: 
    
    In order to show the basic cart functionality
    As a user
    I want to add and remove items from the cart
    
    Scenario: Item can be added to cart
    
    Given that the cart is empty
    When I search for an item
    And an item is added to the cart
    Then the cart contains that item

### Map steps to Java methods

Define your **AddItemsCartSteps** class, a simple POJO, which will contain the Java methods that are mapped to the textual steps. The methods need to annotated with one of the JBehave annotations and the annotated value should contain a regex pattern that matches the textual step: 
    
    
    package org.jbehave.tutorials.etsy.steps;
    
    import static org.hamcrest.MatcherAssert.assertThat;
    import static org.hamcrest.Matchers.equalTo;
    
    import org.hamcrest.Matchers;
    import org.jbehave.core.annotations.Alias;
    import org.jbehave.core.annotations.Composite;
    import org.jbehave.core.annotations.Given;
    import org.jbehave.core.annotations.Named;
    import org.jbehave.core.annotations.Then;
    import org.jbehave.core.annotations.When;
    
    public class AddItemsCartSteps {
    
     @Given("I am shopping for a $thing in $section on Etsy.com")
        public void shoppingForSomethingOnEtsyDotCom(String thing, String section) {
            home.go(section);
            home.search(thing);
        }
    
     @When("I want to browse through a treasury gallery")
        @Composite(steps = { "When I want to buy something from etsy.com", "When I want to browse the treasury",
                "When I choose the first treasury gallery" })
        public void browseToFirstTreasuryGallery() {
        }
    
     @Then("there are search results")
        @Alias("results will be displayed in the gallery")
        public void thereAreSearchResults() {
            assertThat(searchResults.resultsFound(), Matchers.greaterThan(0));
        }

### Configure a Java Embeddable class

Define your Embeddable class which provides the link between the JBehave's executor framework (called Embedder) and the textual stories. The simplest configuration is a one-to-one mapping between a Java class and a textual story file. So we'll extend JUnitStory and give it a name that can be (conventionally) mapped to the textual story filename, e.g.**AddItemsCart****.java.** Obviously it is very helpful being able to execute the tests directly from an IDE (e. g. Eclipse). This is working with _JBehave _very easily thanks to this unimposing method from the above shown class: 
    
    
     @Override
        @Test
        public void run() {
            try {
                super.run();
            } catch (Throwable e) {
                e.printStackTrace();
            }
        }

**Reporting**

•Reporting is an essential element of BDD as it allows to monitor the outcome of the stories that have been run. At the heart of JBehave's reporting is the StoryReporter, to which events are reported as they occur. Currently, the story reporters supported are:

**•ConsoleOutput:** A text-based console output

**•IdeOnlyConsoleOutput:** A text-based console output, which only reports events when running in IDEs (Eclipse and IDEA supported).

**•TxtOutput:** A text-based file output

**•HtmlOutput:** An HTML file output