---
layout: post
header-img: img/default-blog-pic.jpg
---

# JSONPATH like Navigation in R for JSON Data

According to Wiki: 

> R is a free software programming language and software environment for statistical computing and graphics.

However, before any analysis, the input data should be restructured. If the input data is JSON, this restructring becomes very complex and time-consuming. But, if we get some mechanism to navigate through that JSON data using JSONPATH mechanism, it becomes, all the more easy to rearrange JSON data to other formats like table and arrays. Last week, we had an Innovation Day in our organization, rather project, to be more precise. Everyone was working, mostly in groups, on some or the other ideas. I was also working on an idea proposed by my teammate. When it came to technology, we chose to pick my current love of that time, R as it required some kind of statistical analysis.

Most of us should agree on the fact that the hardest part in any statistical analysis is data munging. That is, bringing the input data in analyzable format. It becomes even harder when the source data is in JSON. JSON is best supported by data transfer or communication. Rather, we should say, data communication or transfer is best supported by JSON ( or XML ? Ok I agree on both, no hard feelings !). But when it comes to Data Analysis, it is, definitely, not the best format. So the main obstacle that I had was to break this JSON data into separate rows and columns i.e. cells of a table or a matrix or a dataframe.

**JSONLITE:**

So, a nursery standard googling took me to R's package for JSON, **JSONLITE**: A smarter JSON encoder/decoder for R. This package is a fork of the RJSONIO package by Duncan Temple Lang. More information about this package can be found [here](http://cran.r-project.org/web/packages/jsonlite/index.html). It converts your JSON string into a dataframe or list and vice-versa.

## **Problem Scenario:**

I will try to simulate the problem using a dummy [JSON data file](http://78.46.48.103/sample/hourly_14.json.gz) that I found on <http://openweathermap.org/current#bulk>. Each line of this file contains a JSON record of the form: ![Selection_009](/wp-content/uploads/2014/07/Selection_009.png)

Each record here shows the weather information of a particular city for every three hours. Each element in the array “_Data_” contains the weather info for a particular timestamp (number of seconds from epoch). Various data fields in each element of “_Data_” consists of _temp, temp_min, temp_max, pressure, weather description_ (nested in “_Weather_” array), _clouds_ data etc.

## **Problem Statement:**

For all the cities in the file I had to get a list of _temp_min_ values at _dt_(timestamp) = “1394863200” (i.e. for datetime -> 2014-03-15 06:00:00). So the JSON structure has to be **drilled down and refined (filtered)** on the basis of a value that persists deep in the structure in an array.

So, we need to use some JSONPATH kind of mechanism to drill down this JSON and filter according to this value. But it is not available (atleast not readily) through any package in R.

## **Solution:**

First lets convert this data to an R object.

To convert the above data into an R object, use _fromJSON()_ method from JSONLITE package:
    
    
    allCities <- readLines("hourly_14.json")
    firstCity <- allCities[[1]]
    jsonData<- fromJSON(firstCity)

Now, we have jsonData object in R which contains all the information about the weather of first city in the file. We have to apply some JSONPATH like mechanism on this object to fetch out the precise information from the whole JSON object. JSONLITE, which is wonderful in creating the R object, however, does not provide any mechanism to go inside the document using JSONPATH. So it is the responsibility of R's code to dive into this JSON object to fetch the relevant information denoted by the JSONPATH.

You can use the following custom (read basic) method to get the relevant field by passing the json string and the JSONPATH. It first converts the jsonString into an object in R and then applies the JSONPATH like navigation to that object using jsonPath (the second argument) .
    
    
    drillDownJSON <- function (jsonString, jsonPath) {
      jsonData <- fromJSON(jsonString, flatten = T)
      out <- tryCatch({
        for(ele in jsonPath) {
          if (class(ele) == "list") {
            jsonData <- jsonData[[unlist(ele)]]
          }
          else {
            jsonData <- subset(jsonData, eval(parse(text=ele)))
          }
        }
      unname(jsonData)
      }, error = function(cond) {
        print(cond)
        return (NA_character_)
      })
      if (is.null(out))
        return (NA_character_)
      else return (out)
    }

Here: _jsonString -> character : the JSON document_ _jsonPath -> list of (list and strings) : this argument specifies the JSONPATH to search for._ The caveat here is that the second argument should be passed in a specific format viz.,_ _

  * a list with each element either a list of strings or a single string. 
  * if it is a list of strings( i.e. the list of json keys) then it will be used to navigate into the document 
  * if it is a string then it should be a condition which selects one element from array.
  * obviously the order of elements in the list should be according to the json structure.

In our example, to get the _temp_min_ values at _dt_(timestamp) = “1394863200” (i.e. for datetime -> 2014-03-15 06:00:00), we would pass the second argument as:
    
    
    list(list("data"),"dt == 1394863200", list("main","temp_min"))

It says:

  * From root of the document, go inside "_data_" field which is an array.
  * Select element from the array which has _dt_'s value as 1394863200.
  * Now in that go inside  field "_main"_.
  * At last go inside "_temp_min"_ and return its value.
Call to this method for our specific example would be: 
    
    
    drillDownJSON(jsonString = firstCity, jsonPath = list(list("data"),"dt == 1394863200", list("main","temp_min")))

So it returns the _temp_min_ for _firstCity_ at time: “2014-03-15 06:00:00”. To get the list of these values for all the cities, we can use _sapply _function of R: 
    
    
    Y <- sapply(x, drillDownJSON, list(list("data"),"dt == 1394863200", list("main","temp_min")))
    temp_min_list <- unname(y)

In _temp_min_list_ we have the list of the _temp_min_ values for all the cities. 

## **Conclusion**

Using JSONLITE, we can convert a JSON data to an R object and wrap it to write a custom method like above to easily mock a JSONPATH like interface which you can use for other JSON objects. The above method is a very basic implementation which can be extended further to have more drill down and navigational features.