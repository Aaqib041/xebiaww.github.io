---
layout: post
header-img: img/default-blog-pic.jpg
author: Shruti Khattar
description: 
post_id: 11222
created: 2012/02/02 14:34:46
created_gmt: 2012/02/02 09:34:46
comment_status: open
---

# Opportunities and Challenges in Social Web Data Mining

Social Web Analysis focuses on the social, structural,and behavioral aspects of the social network. It is a media which produces up to one third of new Web content. Because the content always grows, is highly competitive and undergoes constant metamorphosis which is  studied by sociologists, physiologists and anthropologists to understand network evolution, belief formation, friendship formation, etc. These relations and structural amalgamation influence the behavior and performance of almost all the real world entities. In this blog, I will be taking stock of mining the 'BIG DATA' 

** **

**THE NEW BUZZWORDS FOR CONTENT GENERATION**

![][1]

** **

**_Opportunities _**

Managing the immeasurable data is not easy. But because of its affluence in features and patterns it gives rise to, and a way towards infinite scope of exploring, analyzing and channelizing the output in the desired direction. For example, users can then run Google MapReduce jobs to analyze this data for insight and factors such as new relationships. We can use Google Prediction API is based on pattern matching and recognition approach to generate and filter data. Given a set of data examples to train against, you can create applications that can perform the following tasks:

  * Given a user's past viewing habits, predict what other movies or products a user might like.
  * Categorize emails or blogs as spam or non-spam.
  * Analyze posted comments about your product to determine whether they have a positive or negative tone.
  * Guess how much a user might spend on a given day, given his spending history.
  * Give recommendations on purchases based on his previous transactions

Step 1 :  **Upload training data ** to your Google Cloud Storage account in the bucket space.  This should ideally be in a .txt file or as JSON/GSON. This can be updated as and when needed.

Step 2: **Train the system **TrainingModel's insert method will take the Training data as an input file and analyze it. This is an asynchronous process, so you'll have to query the server periodically to check the status of the training session. Training must be complete before you can start to send queries.

There are 3 ways to interact with Google API:

  * Google API REST client libraries
  * Apps Script : A JavaScript cloud scripting language
  * APIs Explorer : An interactive tool that lets you easily try out Google APIs right from your browser

Step 3:  Now query the model and it will return the results based on the best guess at the language of your phrase. Each result has a likelihood score associated with it.

Lets dig into the social networking sites. The example of Facebook would be something we all can relate to. The profile of an individual, can provide two types of data. The first one is profile data and the other one is transactional data which can probably be educed from the wall posts, status messages, the communication with friends, the articles they have gone through or read, the apps and the games they have been involved in, the content that they have shared/liked, the pages they have subscribed to, monitoring their activities in timeline and likewise can be used to give meaningful and concrete information to assess the individual s characteristics and traits that can be leveraged for both personal and business gains.

Blog mining can explore the networking element of blogs, such as the use of hyperlinks between blogs and to outside sources of information. The literary aspect of blogging probes into the personal traits of an individual like nationality, political affiliation, thinking orientation and likewise based on his style. In my opinion, blogs are an important source of sentiment extraction as one is free to give words to their thoughts without sticking to a particular syntactical or semantic boundaries. The nature of posts, the comments that follow, can help in identifying and classifying the like minded people and also in reasoning their interpretations. Ranking the blogs on basis of in-and-out links or number of likes can be used to help determine their significance in the 'BlogSphere'. 

**_Challenges_**

Listed below are the weaknesses or limitations for social web mining.

_Finding a needle from the haystack_ : The social web data is heterogeneous and lacks structure. A Web page typically contains a mixture of many kinds of information. The Web is dynamic and the content changes constantly. The arena of social networking data is highly fragmented. 

_No standards for information retrieval : _There are no predefined ways to fetch information or guaranteeing the authenticity of data. 

_Privacy policies and settings_: Media scrutiny, and easy data access to social networking sites is pushing users to designate their profile as private. While this certainly improves privacy, but is a challenge to data mining. 

_Spam_ has become a serious problem in blogs and social media, users and to systems that harvest, index and analyze generated content. Two forms of spam are common in blogs. Spam Blogs, or splogs where the entire blog and hosted posts are machine generated, and spam comments where authentic posts feature machine generated comments. They bring a new set of challenges for blog analytics. Similarly, the information derived from social networking sites is not something which can be relied upon completely.

**_Conclusion_**

The above mentioned, was the SWOT analysis of social web mining from my point of view. Its versatility will always intrigue researchers and give scope for innovations and algorithms for data mining. But the fact  remains that the Web is a virtual society. It is not only about data, information and services, but also about interactions among people, organizations and automatic systems. They cannot be mapped to actual human responses. However, how close or far it is from the reality will always be a subject of discussion.

   [1]: http://xebee.xebia.in/wp-content/uploads/2012/01/css-social-media-icons.png (css-social-media-icons)

## Comments

**[Shilpa](#7395 "2012-02-06 15:55:43"):** Very informative...good intellectual work ...would be awaiting more blogs like this :)

**[Ishwinder Singh Bedi](#7383 "2012-02-05 18:54:02"):** Informative piece.Good work.Hoping to see more blogs from you.

**[Siddhartha](#7396 "2012-02-06 16:00:53"):** informative work Ma'am.looking forward to some more of your blogs.

**[Neha Vidhani](#7297 "2012-02-02 15:30:17"):** Very well written ..

**[Navin](#7302 "2012-02-02 16:27:01"):** Thanks for sharing… was really helpful to do a Jump start..

**[Priyanka Malik](#7304 "2012-02-02 17:02:03"):** Very nicely compiled..!!

**[Nitin](#7306 "2012-02-02 17:31:44"):** Hmm...Good collection of knowledge. Thanks for sharing!!! :)

**[Gaurav Chuahan](#7307 "2012-02-02 17:46:43"):** Nice One Shruti... Great work.. Keep it Up Dear...

**[Varun Phanda](#7310 "2012-02-02 20:44:51"):** This is a good piece of information. Thanks for sharing. :)

**[Parul](#7312 "2012-02-02 21:05:53"):** Good work Shruti :)

**[Sagar](#7314 "2012-02-02 23:07:35"):** A true insight. Highly intellectual work.

**[Prachi Nagpal](#7330 "2012-02-03 10:12:45"):** Quite Informative...Keep up the good work!! :)

**[sachin jangid](#7331 "2012-02-03 10:22:24"):** very well written and a very good work. It opens thinking dimension about where our social information could be used.

**[Anshika](#7360 "2012-02-04 15:25:57"):** hey, very well done Shruti... keep this great work goin on.. All the very best !! :-)

**[garima valecha](#7337 "2012-02-03 16:52:30"):** Good job...keep it up.

**[Kanica](#7401 "2012-02-06 19:11:14"):** That was very informative. Looking forward to some more information on this in your coming articles. Keep up the good work.

**[Shobhit](#7435 "2012-02-07 14:57:45"):** Nicely written. Good job ! :)

**[Pooja](#7451 "2012-02-07 20:55:44"):** Nice work Shruti!!

