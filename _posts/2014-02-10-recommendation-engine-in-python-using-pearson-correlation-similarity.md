---
layout: post
header-img: img/default-blog-pic.jpg
author: gagrawal
description: 
post_id: 17931
created: 2014/02/10 11:39:55
created_gmt: 2014/02/10 06:39:55
comment_status: open
---

# Recommendation Engine in Python using Pearson Correlation Similarity

Recommendation Engine is a tool with which an application can recommend items to it’s users. There are various strategies to develop a recommendation engine depending upon the use case, but “Collaborative Filtering” is the most popular and widely used technique. With collaborative filtering, an application can find people with similar taste and can look at things they like and combine them to create a ranked list of suggestions which is known as user based recommendation. Or can also find items which are similar to each other and then suggest the items to users based on their past purchases which is known as item based recommendation. The first step in this technique is to find users with similar tastes or items which share similarity. There are various similarity models like Cosine Similarity, Pearson Correlation Similarity, Euclidean Distance Similarity etc. which can be used to find similarity between users or items. In this blog post I am going to discuss an example of how one can develop a basic recommendation engine in Python using Pearson Correlation Similarity.

**>> MovieLens Data**

There are ample sample data sets available on net to do experiments. I will be using MovieLens data set available from <http://grouplens.org/datasets> which contains movies and ratings given by users to these movies. Using this dataset, I will write a python program to give movie recommendations to users. There are two datasets available. I will be using 100,000 dataset in this blog post. After you download and unzip the dataset there are two files u.item and u.data which contain following data 

  * **u.item** – Contains a list of movie IDs, titles and other details which we are not much concerned with. 
    * e.g. 
      * 1|Toy Story (1995)|01-Jan-1995|..
      * 2|GoldenEye (1995)|01-Jan-1995|..
      * 3|Four Rooms (1995)|01-Jan-1995|..
  *  **u.data** – Contains actual ratings given by user to these movies in the format user ID, movie ID, rating and timestamp. 
    * e.g. 
      * 196         242         3              881250949
      * 186         302         3              891717742
      * 22           377         1              878887116
With this data set available, let’s first write a function in Python which will parse movie and user ratings file and using Pearson Correlation Similarity find similar movies.

**>> Python Program - movieRecommendations.py**

Create a python file movieRecommendations.py and write loadMoviesRatings function which is responsible for parsing movie and user ratings file and creating a dictionary:

[sourcecode language="python"] def loadMovieRatings(): movies={} for line in open("data/ml-100k/u.item"): (id,movie)=line.split("|")[0:2] movies[id]=movie movieRatings={} for line in open("data/ml-100k/u.data"): (userId, movieId, rating, ts) = line.split("\t") movieRatings.setdefault(userId,{}) movieRatings[userId][movies[movieId]]=float(rating) return movieRatings

[/sourcecode]

Now let’s execute this function on python command line to see what is the output.

[sourcecode language="python"] >>>import movieRecommendations >>>movieRatings=movieRecommendations.loadMovieRatings() >>>movieRatings [/sourcecode]

This returns a dictionary in following format where key is user ID and value is movie name and rating given by respective user:

[sourcecode language="python"] { 1 : { 'Doom Generation, The (1995)': 2.0, 'Twelve Monkeys (1995)': 4.0, 'Mask, The (1994)': 4.0, 'To Wong Foo, Thanks for Everything! Julie Newmar (1995)': 3.0 … }, 2 : { 'River Wild, The (1994)': 3.0, 'Sabrina (1995)': 3.0, 'Evita (1996)': 3.0, 'Richard III (1995)': 2.0, 'Postino, Il (1994)': 4.0, 'Donnie Brasco (1997)': 4.0 … } } [/sourcecode]

**>> Finding users with similar taste : Pearson Correlation Similarity**

Once we have above data structure we can now write a function to find users with similar taste. We will use Pearson correlation similarity in this case. The correlation coefficient is a measure of how well two sets of data fit on a straight line. It tends to give better result in situations where data is not normalized. In this case it is possible that some users rate movies more harshly as compared to other users. However it doesn’t mean they are not similar. Pearson correlation score normalizes such data and finds out similar users.

[sourcecode language="python"] def calculatePearsonSimilarity(movieRatings,u1,u2): movies={} for movie in movieRatings[u1]: if movie in movieRatings[u2]: movies[movie]=1 noOfMovies=len(movies)
    
    
        if noOfMovies==0: return 0
        sum1=sum([movieRatings[u1][movie] for movie in movies])
        sum2=sum([movieRatings[u2][movie] for movie in movies])
    
        sum1Sq=sum([pow(movieRatings[u1][movie],2) for movie in movies])
        sum2Sq=sum([pow(movieRatings[u2][movie],2) for movie in movies])
    
        pSum=sum([movieRatings[u1][movie]*movieRatings[u2][movie] for movie in movies])
    
        # Calculate Pearson score
        num=pSum-(sum1*sum2/noOfMovies)
        den=sqrt((sum1Sq-pow(sum1,2)/noOfMovies)*(sum2Sq-pow(sum2,2)/noOfMovies))
        if den==0: return 0
        r=num/den
        return r
    

[/sourcecode]

Above function returns value between -1 and 1. A value of 1 means two users have exactly same ratings for every movie. Let’s now try this function with 2 users and see how similar they are –

[sourcecode language="python"] >>>import imp >>>imp.reload(movieRecommendations) >>>movieRecommendations.calculatePearsonSimilarity(movieRatings, ‘1’,’2’) 0.16084123285437085 [/sourcecode]

Here the output is 0.16 which means user ‘1’ and ‘2’ are not very similar in terms of their ratings for movies.

**>> Weighted Similarity Score**

Now let’s say we want movie recommendations for user with id ‘43’.  What we need is to find top n users who have given similar ratings as compared to user ‘43’. And then find the movies which they have rated good and user ‘43’ has still not rated, so that we can recommend the same to user ‘43’. However it is possible that top n users might have given different ratings to same movies that user ’43’ has not watched. Also it is possible that not all users would have watched the same movie. So we need to give weighted score to movies by considering ratings from all top n users and rank them to give correct recommendation to user ‘43’.

Following table illustrates how this can be done. First column contains user IDs and second column contains their similarity with user ‘43’. Now suppose that there are 3 movies i.e. Start Kid, Visitors, The (Visisteurs, Les) and Saint of Fort Washington that user ‘43’ has not rated. We can multiply their respective ratings given by users in first column  with their similarity with user ‘43’ as in second column and then take total of this product and divide by sum of all similarities. This gives us weighted score for all 3 movies as mentioned in last row “Total/Similarity Total” 

**Users**
**Similarity**
**Star Kid (1979)**