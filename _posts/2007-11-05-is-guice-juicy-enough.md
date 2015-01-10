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

The following post talks about my first brush with Guice based on a couple of hours that I spent on it for the past few days. I would not get into burning fire of comparing Guice with Spring as there has already been a lot of debate on that. Personally in my view it is comparing apple with oranges. We can probably just make comparison between how DI works on Spring and how it works on Guice but then we know that Spring is much more than DI, it is a feature full comprehensive stack.

So not getting into that path any further back to DI with Guice.

Guice wholly embraces annotations and generics. The idea is that annotations finally free you from error-prone, refactoring-adverse string identifiers and frees you up from the XML configuration hell. Guice injects constructors, fields and methods (any methods with any number of arguments, not just setters)

The injection process for Guice is a two step injection process. 1.Define the Bindings. 2.Inject the concrete implementations at the right places.

Let us look at how it is done...  Take a simple scenario where you as a Client need to hire a Cab. And of course the Cab needs a Driver to drive it and Fuel to run on. So we would inject the Driver and Fuel in the Cab.

Let us start with 3 interfaces
    
    
    public interface Driver {
        public String drive();
    }
    
    
    
    public interface Fuel {
        public void setOctaneLevel(int octane);
        public int getOctaneLevel();
    }
    
    
    
    public interface Cab {
        public void hire();
    }
    

and the concrete implementation classes are
    
    
    public class GoodDriver implements Driver {
    
        public String drive() {
            return " I drive well";
        }
    
    
    
    
    public class HighOctaneFuel implements Fuel {
    
        public int octaneLevel;
        public void setOctaneLevel(int octane) {
            this.octaneLevel = octane;
        }
        public int getOctaneLevel() {
            return octaneLevel;
        }
    }
    

So far so good. **Now observe carefully the YellowCab implementation.**
    
    
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
    

We have specified **@Inject** annotation at 2 places where we want the Driver and Fuel concrete implementations to be injected. For injecting instances, we can use this Inject Annotation. This annotation can be used in a Constructor for a class, for a method or for a Field. Don't understand it yet? No worries, just go along the explanation would follow.

The client code looks like this
    
    
    public class Client {
        public static void main(String[] args) {
            Injector injector = Guice.createInjector(new BasicModule());
            Cab cab = injector.getInstance(Cab.class);
            cab.hire();
        }
    }
    

In the client we see some interesting Classes/Interfaces being used like **Module, Injector and Guice**.

Let us look at the client code line by line. First we have

_**Injector injector = Guice.createInjector(new BasicModule());**_

**Injectors** Injectors take care of creating and maintaining Objects that are used by the Clients. Injectors do maintain a set of Default Bindings from where they can take the Configuration information of creating and maintaining Relation-ship between Objects.

Here we are asking the Injector to be created on the basis of BasicModule, which leads to the question- What is a Module?

**Modules** Modules are objects which will maintain the set of Bindings. It is possible to have multiple Modules in an Application. Injectors, in turn, will interact will the Modules to get the possible Bindings. Module is represented by an interface with a method called Module.configure() which should be overridden by the Application to populate the Bindings. To simplify things, there is a class called AbstractModule which directly extends the Module interface. So Applications can depend on AbstractModule rather than Module.

In our example the BasicModule would look like
    
    
    public class BasicModule extends AbstractModule{
    
        protected void configure() {
            bind(Driver.class).to(GoodDriver.class);
            bind(Fuel.class).to(HighOctaneFuel.class);
            bind(Cab.class).to(YellowCab.class);
        }
    }
    

From here Guice knows which implementations to inject for which Interfaces.

**Guice** Guice is a class which Clients directly depends upon to interact with other Objects. The Relation-ship between Injector and the various modules is established through this class.

Once we get the Injector from Guice we can get the injected Cab object by calling _**Cab cab = injector.getInstance(Cab.class);**_

We get back a YellowCab because of the binding specified in the BasicModule as _**bind(Cab.class).to(YellowCab.class);**_

The driver and the fuel are injected into the Cab by Guice and we tell Guice to inject them by specifying the @Inject annotation in the YellowCab class.

Once you execute the client you should see this output

**“Cab hired with Driver who says : I drive well and has fuel with octane : 90”**

OK you can rest a while now :) You have done your first example using Guice!

So far so good but what if I have multiple implementations for my interface? Hmm this is where it gets a bit tricky.

Say now we have a BadDriver which looks like this
    
    
    public class BadDriver implements Driver {
    
        public String drive() {
            return " I drive bad!";
        }
    }
    

Now how do we inject this in the cab say YellowCab?

For this you would have to create a Bad annotation file something like this
    
    
    @Retention(RetentionPolicy.RUNTIME)
    @Target({ElementType.FIELD, ElementType.PARAMETER, ElementType.METHOD})
    @BindingAnnotation
    public @interface Bad {}
    

and add this line to the BasicModule 
    
    
    ...
        bind(Driver.class).annotatedWith(Bad.class).to(BadDriver.class);
    ...
    

and now change the Injection in YellowCab to
    
    
    ...
    
        @Inject
        public void injectDriver(@Bad Driver driver){
            this.driver = driver;
        }
    ...
    

notice how **@Bad** is added besides the argument. This helps Guice understand that instead of the default Driver, a Bad Driver has to be injected. And now if you execute the client the output is ** “Cab hired with Driver who says : I drive bad! and has fuel with octane : 90”**

**Conclusions **

This is a very simple injection example and does not deem to represent all the interesting features of Guice. For more information refer [here ][1]

  * The main point that Guice tries to address is taking care of the XML configuration hell. But is this any better? With XMLs at least I feel that all my configurations are in one place. Here they seem to be scattered all around with Annotations. If you really like to use annotations and want to use Spring you can use Spring JavaConfig. Moreover having all configuration in one place acts as a good documentation which various tools can use to generate useful information.
  * As you saw adding multiple implementations for an interface is an issue and you need to have more files written for that, which does not sound good either. Guice counters this issue by saying that you would never need multiple implementations for an interface for more than 10% of the code but with Spring you would have to write explicit wiring code 100% of the time. Well may be true but depends on your project needs I would say. This issue becomes even more tricky when you want to inject multiple implementations of an interface in a single class.

   [1]: http://code.google.com/p/google-guice/