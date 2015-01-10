---
layout: post
header-img: img/default-blog-pic.jpg
author: vhazratiblog
description: 
post_id: 322
created: 2007/11/05 20:16:01
created_gmt: 2007/11/05 18:16:01
comment_status: open
---

# Is Guice juicy enough?

<p>The following post talks about my first brush with Guice based on a couple of hours that I spent on it for the past few days. I would not get into burning fire of comparing Guice with Spring as there has already been a lot of debate on that. Personally in my view it is comparing apple with oranges. We can probably just make  comparison between how DI works on Spring and how it works on Guice but then we know that Spring is much more than DI, it is a feature full comprehensive stack.</p>
<p>So not getting into that path any further back to DI with Guice.</p>
<p>Guice wholly embraces annotations and generics. The idea is that annotations finally free you from error-prone, refactoring-adverse string identifiers and frees you up from the XML configuration hell. Guice injects constructors, fields and methods (any methods with any number of arguments, not just setters)</p>
<p>The injection process for Guice is a two step injection process.
1.Define the Bindings.
2.Inject the concrete implementations at the right places.</p>
<p>Let us look at how it is done...
<!--more-->
Take a simple scenario where you as a Client need to hire a Cab. And of course the Cab needs a Driver to drive it and Fuel to run on. So we would inject the Driver and Fuel in the Cab.</p>
<p>Let us start with 3 interfaces</p>
<pre lang="java">
public interface Driver {
    public String drive();
}
</pre>

<pre lang="java">
public interface Fuel {
    public void setOctaneLevel(int octane);
    public int getOctaneLevel();
}
</pre>

<pre lang="java">
public interface Cab {
    public void hire();
}
</pre>

<p>and the concrete implementation classes are</p>
<pre lang="java">
public class GoodDriver implements Driver {

    public String drive() {
        return " I drive well";
    }
</pre>

<pre lang="java">

public class HighOctaneFuel implements Fuel {

    public int octaneLevel;
    public void setOctaneLevel(int octane) {
        this.octaneLevel = octane;
    }
    public int getOctaneLevel() {
        return octaneLevel;
    }
}
</pre>

<p>So far so good. <strong>Now observe carefully the YellowCab implementation.</strong></p>
<pre lang="java">
public class YellowCab implements Cab {

    private Driver driver;
    private Fuel fuel;

    public void hire() {
        System.out.println("Cab hired with Driver who says : "+ driver.drive() + " and has fuel with octane : " + fuel.getOctaneLevel() );
    }

    @Inject
    public void injectDriver(Driver driver){
        this.driver = driver;
    }

    @Inject
    public void injectFuel(Fuel fuel){
        fuel.setOctaneLevel(90);
        this.fuel = fuel;
    }
}
</pre>

<p>We have specified <strong>@Inject</strong> annotation at 2 places where we want the Driver and Fuel concrete implementations to be injected. For injecting instances, we can use this Inject Annotation. This annotation can be used in a Constructor for a class, for a method or for a Field. Don't understand it yet? No worries, just go along the explanation would follow.</p>
<p>The client code looks like this</p>
<pre lang="java">
public class Client {
    public static void main(String[] args) {
        Injector injector = Guice.createInjector(new BasicModule());
        Cab cab = injector.getInstance(Cab.class);
        cab.hire();
    }
}
</pre>

<p>In the client we see some interesting Classes/Interfaces being used like <strong>Module, Injector and Guice</strong>.</p>
<p>Let us look at the client code line by line. First we have</p>
<p><em><strong>Injector injector = Guice.createInjector(new BasicModule());</strong></em></p>
<p><strong>Injectors</strong>
Injectors take care of creating and maintaining Objects that are used by the Clients. Injectors do maintain a set of Default Bindings from where they can take the Configuration information of creating and maintaining Relation-ship between Objects.</p>
<p>Here we are asking the Injector to be created on the basis of BasicModule, which leads to the question- What is a Module?</p>
<p><strong>Modules</strong>
Modules are objects which will maintain the set of Bindings. It is possible to have multiple Modules in an Application. Injectors, in turn, will interact will the Modules to get the possible Bindings. Module is represented by an interface with a method called Module.configure() which should be overridden by the Application to populate the Bindings. To simplify things, there is a class called AbstractModule which directly extends the Module interface. So Applications can depend on AbstractModule rather than Module.</p>
<p>In our example the BasicModule would look like</p>
<pre lang="java">
public class BasicModule extends AbstractModule{

    protected void configure() {
        bind(Driver.class).to(GoodDriver.class);
        bind(Fuel.class).to(HighOctaneFuel.class);
        bind(Cab.class).to(YellowCab.class);
    }
}
</pre>

<p>From here Guice knows which implementations to inject for which Interfaces.</p>
<p><strong>Guice</strong>
Guice is a class which Clients directly depends upon to interact with other Objects. The Relation-ship between Injector and the various modules is established through this class.</p>
<p>Once we get the Injector from Guice we can get the injected Cab object by calling
<em><strong>Cab cab = injector.getInstance(Cab.class);</strong></em></p>
<p>We get back a YellowCab because of the binding specified in the BasicModule as
<em><strong>bind(Cab.class).to(YellowCab.class);</strong></em></p>
<p>The driver and the fuel are injected into the Cab by Guice and we tell Guice to inject them by specifying the @Inject annotation in the YellowCab class.</p>
<p>Once you execute the client you should see this output</p>
<p><strong>“Cab hired with Driver who says :  I drive well and has fuel with octane : 90”</strong></p>
<p>OK you can rest a while now :) You have done your first example using Guice!</p>
<p>So far so good but what if I have multiple implementations for my interface? Hmm this is where it gets a bit tricky.</p>
<p>Say now we have a BadDriver which looks like this</p>
<pre lang="java">
public class BadDriver implements Driver {

    public String drive() {
        return " I drive bad!";
    }
}
</pre>

<p>Now how do we inject this in the cab say YellowCab?</p>
<p>For this you would have to create a Bad annotation file something like this</p>
<pre lang="java">
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD, ElementType.PARAMETER, ElementType.METHOD})
@BindingAnnotation
public @interface Bad {}
</pre>

<p>and add this line to the BasicModule
<pre lang="java">
...
    bind(Driver.class).annotatedWith(Bad.class).to(BadDriver.class);
...
</pre>
and now change the Injection in YellowCab to</p>
<pre lang="java">
...

    @Inject
    public void injectDriver(@Bad Driver driver){
        this.driver = driver;
    }
...
</pre>

<p>notice how <strong>@Bad</strong> is added besides the argument. This helps Guice understand that instead of the default Driver, a Bad Driver has to be injected. And now if you execute the client the output is
<strong>
“Cab hired with Driver who says :  I drive bad! and has fuel with octane : 90”</strong></p>
<p><strong>Conclusions </strong></p>
<p>This is a very simple injection example and does not deem to represent all the interesting features of Guice. For more information refer <a href="http://code.google.com/p/google-guice/">here </a></p>
<ul>
    <li>The main point that Guice tries to address is taking care of the XML configuration hell. But is this any better?
With XMLs at least I feel that all my configurations are in one place. Here they seem to be scattered all around with Annotations. If you really like to use annotations and want to use Spring you can use Spring JavaConfig.
Moreover having all configuration in one place acts as a good documentation which various tools can use to generate useful information.</li>
<li>As you saw adding multiple implementations for an interface is an issue and you need to have more files written for that, which does not sound good either.
Guice counters this issue by saying that you would never need multiple implementations for an interface for more than 10% of the code but with Spring you would have to write explicit wiring code 100% of the time. Well may be true but depends on your project needs I would say.
This issue becomes even more tricky when you want to inject multiple implementations  of an interface in a single class.</li>