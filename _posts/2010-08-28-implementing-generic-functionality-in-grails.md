---
layout: post
header-img: img/default-blog-pic.jpg
author: sunil.inteti
description: 
post_id: 4593
created: 2010/08/28 16:24:00
created_gmt: 2010/08/28 11:24:00
comment_status: open
---

# Implementing Generic functionality  in Grails

In web applications we have functionalities like auto suggestions , rating objects, adding to favourites etc which repeat themselves in an application and across applications. So it demands to think about implementing a generic way of rating and adding to user favourites etc and follow **DRY** principle. Recently in one of my Grails projects we implemented these functionalities in a generic way so that they are reusable and easy to use. Here in this blog, specifically lets look at how we can implement generic auto suggest functionality in Grails. This idea can be extended for other functionalities as well.

Jquery already provides [auto suggest][1] functionality. The question here is making functionality generic so that next time we have to use that its a breeze to use. Lets create a tag which can add the necessary jquery code for a text field to have auto suggest capability.

**HTML code :** [sourcecode language="html"] <g:textField name="city" id="city" value="" /> <g:autoSuggest autoCompleteForId="city" searchObject="com.xebia.Address" searchProperty="city"/> [/sourcecode]

For searchObject property we must give the fully qualified class name of the domain object. Here we have Address as the domain objects which has a property city in it. The above tag is handled by the following closure in the Taglibrary.

**Autocomplete Tag Library:**

[sourcecode language="java"] def autoSuggest = { attrs , body -> def searchObject = attrs.searchObject def searchProperty = attrs.searchProperty def htmlId = attrs.autoCompleteForId def width = attrs.width def contextName = grailsApplication.config.grails.serverURL def actionName = attrs.action ? attrs.action : 'autoSuggest' def controllerName = attrs.controller ? attrs.controller : 'autocomplete' 
    
    
        String serviceUrl =  createLink(controller:controllerName, action:actionName , absolute:true)
    
        importScriptsAndCSS(out, contextName, pageScope);
    
        out &lt;&lt; &quot;&lt;script&gt;  jQuery(function() {&quot;
        out &lt;&lt;    &quot;\$(\&quot;#&quot;+ htmlId+ &quot;\&quot;).autocomplete({ &quot;  
        out &lt;&lt;              &quot; serviceUrl: \'&quot;+ serviceUrl +&quot;\',&quot;
        out &lt;&lt;        '''
                        minChars:1,
                        maxHeight:400,
                        zIndex: 9999,
                        deferRequestBy: 0,
                     '''
        if(width) {
            out &lt;&lt;    &quot;width:&quot;+width+&quot;,&quot;
        }
    
        out &lt;&lt;    &quot;params: {searchObject:\'&quot;+searchObject+&quot;\',searchProperty:\'&quot;+searchProperty  + &quot;\'}, &quot;
    
        out &lt;&lt; '''
                        noCache: false
                    });
            });
             &lt;/script&gt; 
        '''
    
    }
    

[/sourcecode]

The method importScriptsAndCSS(out, contextName, pageScope) as the name suggests, adds the necessary css and javascript needed only once per page. The auto suggest tag that we use in tandem with a text field captures the property that we need suggestions for on an domain object and adds necessary jquery code to hit a generic AutoSuggestController which will return the matching options in JSON format to jquery. Now lets look at the generic AutoSuggestController code.

**AutoSuggestController**

[sourcecode language="java"] def autoSuggest = { log.debug("Inside the auto complete controller") def searchObject = params.searchObject def searchProperty = params.searchProperty def queryValue = params.query
    
    
        def searchCriteria = grailsApplication.getClassForName(searchObject).createCriteria()
        log.debug &quot;query value is ${queryValue}&quot;
        def results = searchCriteria.list{
            projections {
                groupProperty(searchProperty)
            }
            ilike(searchProperty, &quot;%${queryValue}%&quot;)
            maxResults(5)
        }
        log.debug(&quot;Auto suggest Results  : &quot;+results)
        render createJSONForResults(results, queryValue , searchProperty)
    }
    

[/sourcecode]

The above code snippet is the action that will be accessed by the jquery which also passes the search Property and Search Object to the controller. Dynamically we determine the domain object by this line of code **grailsApplication.getClassForName(searchObject).createCriteria()**

We get the domain object from the fully qualified class name and then create a criteria on the grails domain object. In this criteria we attempt to get the list of objects for which the searchProperty(city) matches. Once we get the list of objects we have to convert the list into JSON object format, so that that is passed back to the jquery to render the auto suggestions.

**JSON format can be achieved like this**

[sourcecode language="java"] private String createJSONForResults(def results, String queryValue, String searchProperty) { def resultsArray = results.toArray() StringBuilder resultsJSONObject = new StringBuilder("{ query : \'${queryValue}\' , suggestions: [") if(resultsArray.length > 0) { for(int i=0;i<resultsArray.length-1;i++) { String currentProperty = resultsArray[i] currentProperty = "'" \+ currentProperty + "'," resultsJSONObject.append(currentProperty) }
    
    
            def lastResult = resultsArray[resultsArray.length-1]
            String lastElement = &quot;'&quot; + lastResult + &quot;']}&quot;
            resultsJSONObject.append(lastElement)
            return resultsJSONObject.toString()
        } else {
            resultsJSONObject.append(&quot;]}&quot;)
        }
    }
    

[/sourcecode]

This piece of functionality can be used across the Grails applications and can be resued heavily in a single application as well following the DRY principle.

**Conclusion** Following the same way of using a taglibrary and generic controller we can implement the other functionalities like rating objects, using adding favourites and many more using Grails dynamism.

   [1]: http://docs.jquery.com/Plugins/Autocomplete

## Comments

**[omarji](#359 "2010-08-30 19:31:15"):** Cool, I would be interested to know how to manage not writing the scripts and css more than once using importScriptsAndCSS() Note: you could use the elvis operator for the ternary ops. if you like :) def controllerName = attrs.controller ?: 'autocomplete'

**[Sunil Prakash Inteti](#858 "2010-09-06 09:40:14"):** omarji, Here is the way that you could use for importing css once. [sourcecode lang="java"] private void importScriptsAndCSS(def out,String contextName,def pageScope ){ def executedOnceAutocomplete try { executedOnceAutocomplete = pageScope.executedOnceAutocomplete } catch(MissingPropertyException b) { //ignore } if(!executedOnceAutocomplete) { out << '<link rel=\"stylesheet\" href=\"' out << contextName out << '/css/autocomplete/autocomplete.css\" /> ' out << '<script src="' out << contextName out << '/js/jquery/jquery.autocomplete.js" ></script>' pageScope.executedOnceAutocomplete = true; } } [/sourcecode]

