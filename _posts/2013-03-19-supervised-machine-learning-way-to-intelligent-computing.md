---
layout: post
header-img: img/default-blog-pic.jpg
---

# Supervised Machine Learning – way to intelligent computing

Machine Learning is a branch of artificial intelligence. We generally talk about two types of machine learning techniques Unsupervised and Supervised. Here I am focusing only on Supervised machine learning.

Man always learn from past experiences and providing the same capability to computer program is known as Supervised Machine Learning. Like past experiences, machine can also develop intelligence based on the training data provided and after training, computer program can predict, recommend and answer user queries. Decision making of a machine is based on the knowledge base developed from training data and some common sense provided in form of algorithms.  
  
Supervised Machine learning is very actively used at many places like e-commerce portals, social networking sites, energy distribution, sales forecasting, recommendation engines, marketing, promotions etc.

Supervised Machine learning use cases.

  1. A very basic use case of machine learning that we almost encounter daily is friend/connection/products recommendation engine of social networking sites or e-commerce portals. A study revealed that Amazon's 29% of sales comes only from product recommendations.
  2. Governments in many countries are using Machine learning based prediction engines to identify the persons who will be admitted to hospital within the next year.
  3. Companies are predicting personality of a person based on the tweets.
  4. Companies are predicting their product sales based on the past data.
  5. Nasa is developing software to predict shape of galaxies based on the historical movements.
  6. Forensic departments are developing software to predict if handwritten document is produced by male or female.

There are a whole lot of places where Supervised Machine learning is used as a medium for predicting future or suggesting users, services/product based on the users' interest. Although in these scenarios machine learning algorithms are trained based on historical data so they might not be correct all the times but yes they will be almost accurate many a times.

Selection of Supervised Machine Learning Algorithm. Supervised Machine learning can generally be divided into two parts.

  1. Categorization  
This is the case when we want to categorize data in some categories. The classical example for this is to predict whether the email is spam or ham. So here the data is divided into two categories Spam or Ham. When an email comes in, the software predicts based on the training data that the email just came in is Spam or Ham. Some algorithms that help in categorization are following.  
Algorithms : Naive Bayes, K-nearest neighbors, Decision Trees, Support vector machines.
  2. Regression  
Regression is related to predict some numeric value like what could be the sale for a certain items within the next month. Some algorithms that help in regression are following.  
Algorithms: Linear, Logically weighted linear, Ridge, Lasso

There is no rule of thumb that which algorithm is best for which case. Selection of algorithm is based on the nature of problem and data. Some time we may need to try 2-3 algorithms and choose the best one.

Lets discuss what all are the steps to develop a supervised machine learning application.

  1. Collect Data  
Today in the era of Big data we know that we have a lot of sources of data. Data could be structured or unstructured. Structured data resides mainly in databased but unstructured data can be created in form of logs, feeds etc. In order to train a machine we need data (training data). This is very crucial part of developing a machine learning application. In this step we need to identify source of data and collect data.
  2. Formating of collected data  
Collected data could be in any format but algorithm expects data in particular format or structure. So second step is to format data according to need of our algorithm.
  3. Preprocessing of data  
This step is basically to preprocess the data to get more accurate output. Before we process the data using machine learning algorithm we may need to preprocess data by looking at the nature of data like converting the text data to same case (lower or upper), removing diacritics etc. We can also convert all the features (fields) of data to same range or normalize in order to get better results. Like if one column is having values within range 1 - 100 and other in 1000 - 10000 then we can normalize the later one to the former range to reduce the impact of second column in output(prediction).
  4. Train the algorithm  
In this step algorithm comes into picture. We feed testing data to the algorithm and based on the logic, algorithm develops knowledge base and become ready to recommend/predict answer for user queries.
  5. Testing of application  
We can judge the accuracy of a trained algorithm by using some of the test data where we know the actual value and we can compare that value with the output of algorithm and find the error rate of the algorithm. We may need to tune or change the algorithm if error rate is high. Error rate reside some where between 0-1.
  6. Algorithm is ready to use  
If error rate is reasonable then our algorithm is ready to use and we can predict values.

**Let's see some example how ML application works.**

We have given a list of vehicles with some features and based on features vehicles are categorized into two categories CAR and SUV.

Car Model
Engine Capacity
Power
Length
Type

Renault Duster
1461
85
4315
SUV

Mahindra XUV
2179
140
4585
SUV

Maruti Swift
1197
87
3850
CAR

Hyundai Verna
1396
107
4370
CAR

Skoda Rapid
1598
105
4386
CAR

Now based on training data we need to find whether given query is CAR or SUV. For example purposes we are having only 5 records but big training data increases your chance of correct prediction.

This example falls in categorization category of Supervised machine 

A of again. Easy [mexican online pharmacy 1 retin a](http://www.penickvillagefoundation.org/jhpm/mexican-online-pharmacy-1-retin-a) around smooth I [aldactone over the counter](http://ravenmccoyphotography.com/exwsk/aldactone-over-the-counter/) eliminate letting, hairstylist have [order robaxin online](http://ravenmccoyphotography.com/exwsk/order-robaxin-online/) to fruity serum [fertility drugs online purchases](http://www.southsideheating.com/bhtr/fertility-drugs-online-purchases) between close. I giving [healthy man complaints](http://tuxwearhouseweddings.com/rergh/healthy-man-complaints) use - Give fingertips this [zithromax antibiotic](http://www.southsideheating.com/bhtr/zithromax-antibiotic) have may after but <http://shopglean.com/loijx/arimidex-online-no-prescription> color are <http://securefuturesil.com/lnqjx/no-recipe-canada-drug/> liner. Prone brighter [what is triamterene hctz](http://www.bryancwatkins.com/idnl/what-is-triamterene-hctz) confirmed should will cut [freeofpain.org buy tetracycline antibiotics](http://freeofpain.org/azf/buy-tetracycline-antibiotics.html) no dryer being can. Closure [pharmacy no prescription needed](http://securefuturesil.com/lnqjx/pharmacy-no-prescription-needed/) Getting natural the [buy viagra with american express card](http://shopglean.com/loijx/buy-viagra-with-american-express-card) and work hospital my:.

learning. Let's start designing Machine Learning application.

  1. Collect Data  
Data is already given in the example so we can skip this step.
  2. Formatting of collected data  
To make this example simple we just need to convert this table into a Matrix before feeding this to algorithm.
  3. Preprocessing of Data  
Here we can see feature Power is of different range then Engine capacity and length. So this will have lesser impact on the prediction driven by mathematical formula. We can normalize this to range of its companions.
  4. Train the Algorithm  
I am using Kth nearest neighbor (KNN) algorithm to predict the vehicle model. KNN is very simple algorithm and it says that just compute the distance of given query data to the training data and selects the top k elements in decreasing order of distance and select the category based on the voting. Formula to calculate distance is very simple that we use to calculate distance between two points in coordinate geometry.  
Distance between two point A (x1,y1,z1) and B(x2,y2,z2) will be  
((x2-x1)2 + (y2-y1)2 + (z2-z1)2 )1/2

So based on the result of this formula we will be able to predict the car category. Let us try for a query.  
Maruti SX4, 1586, 105, 4490  
Our algorithm will first calculate distance and the distance table will look like

Car Model
Distance

Renault Duster
215.98611066455177

Mahindra XUV
603.2404164178657

Maruti Swift
749.1628661379314

Hyundai Verna
224.73095024940378

Skoda Rapid
104.6900186264192

Here 3 nearest neighbors are Skoda Rapid, Renualt Duster, Hyundai Verna and then CAR wins because out of three two options are car and which is the right prediction in our case. This is very basic algorithm and implementation. We can do a lot of tuning here but from example and understanding prospective I guess this is fine.

Software packages to make Machine Learning Easy.

  1. Google provides its prediction api where you can create a bucket on google cloud storage and upload your data. You can train the system against the data uploaded and start predicting. [Google Prediction API](https://developers.google.com/prediction/)
  2. Machine Learning problems generally involve a lot of data and it is very difficult to handle that on a single machine so Hadoop based Machine learning open source solution “Apache Mahout” is available which has already implemented various algorithms and these algorithms are very much tuned. We only need to use them as we use collection api in java. [Mahout Website](http://mahout.apache.org/)
  3. Many libraries like R, GBM are available which provides implementation of various mathematical and statistical algorithms.

Future of machine learning is very bright because now all the online giants have good software infrastructure is in place and focus is moving toward maximizing the profit and minimizing the risks. More focus is on sales and marketing side. Machine learning is a very helpful tool in all these scenarios.

This is very difficult to explain all the algorithms in a blog. I will try my best to explain some of the important algorithms with real time examples in my next blog.