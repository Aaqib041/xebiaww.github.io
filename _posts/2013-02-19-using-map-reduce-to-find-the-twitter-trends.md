---
layout: post
header-img: img/default-blog-pic.jpg
author: anirudh.xebia
description: 
post_id: 15672
created: 2013/02/19 14:15:28
created_gmt: 2013/02/19 09:15:28
comment_status: open
---

# Using Map Reduce to find the twitter trends

Few weeks back while preparing for our presentation for [agileNCR ][1]2013, Sanchit and I started working on an interesting problem statement to solve using MapReduce.

We thought of applying MapReduce algorithm to find the trends in Twitter.

A Tweet in a twitter can have hashTags (#helloTwitter) and a certain hashTag used most number of times in tweets globally is said to have highest trend. More details can be found [here][2].

This data is huge and also keeps on increasing, so processing it in traditional manner would not be possible.

Hence we would require [hadoop ][3]to help us solve this problem.

Twiiter uses Cassandra to store the data in key-value format. Lets assume for simplicity that the key value pair for tweet data looks something like this < twitterHandle,Tweet >.

So, in order to find the top n trends in a given snapshot, we would need to: 1\. Process all Tweets and parse out tokens with HashTags. 2\. Count all the hashTags. 3\. Find out top n hashtags by sorting them.

So, the input data for our Mapper could be a flat file generated out of Values of the Key-Value of <twitterHandle,Tweet>. It would look something like this :

[sourcecode language="java"] I love #agileNCR. Attenindg sesssion on #hadoop. ..... .... [/sourcecode]

click [here ][4]to see the sample input file.

**Assumption: ** As one tweet can be of maximum 140 characters we can store the data in such a format that each line is new tweet.

**Step 1: Mapper** Mapper while tokenising would collect only the tokens which start by #.

[sourcecode language="java"] @Override public void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException { String line = value.toString(); StringTokenizer tokenizer = new StringTokenizer(line); while (tokenizer.hasMoreTokens()) { String token = tokenizer.nextToken(); if(token.startsWith("#")){ word.set(token.toLowerCase()); // Context here is like a multi set which allocates value "one" for key "word". context.write(word, one); } } [/sourcecode] 

**Step 2: Combiner** Combiner would combine all the same hashtags together.

**Step3: Reducer**

[sourcecode language="java"] @Override protected void reduce(Text key, Iterable&lt;IntWritable&gt; values, Context context) throws IOException, InterruptedException { int sum = 0; for (IntWritable val : values) { sum += val.get(); } context.write(key, new IntWritable(sum)); } </pre> [/sourcecode] 

Reducer will generate the output something like this :

[sourcecode]

# agileNCR 21

# hello 4

# xomato 88

# zo 36

[/sourcecode]

The problem with this output is that it is sorted by the key values, as in the mapping phase the shuffle and sort step sorts them alphabeticaly on the basis of keys.

To get the desired out of sorting it on the basis of number of occurances of each hashTag, we would need them to be sorted on the basis of values. So we decided to pass this output to second Map-Reduce job which will swap the key and the value and then perform sort.

Hence : **Step4: Mapper 2**

Tokenise the input and put 2nd token(the number) as key and 1st token (hashtag) as value. While mapping it will shuffle and sort on the basis of key. However, the sorting of the keys by default is in ascending order and we would not get out desired list. So, we would need to use a Comparator. We would need to use LongWritable.ReverseComparator.

[sourcecode language="java"] @Override public void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {
    
    
        String line = value.toString(); // agilencr 4
        StringTokenizer tokenizer = new StringTokenizer(line);
        while (tokenizer.hasMoreTokens()) {
    
         String token = tokenizer.nextToken();
    
        // Context here is like a multi set which allocates value &quot;one&quot; for key &quot;word&quot;.
    
         context.write(new LongWritable(Long.parseLong(tokenizer.nextToken().toString())), new Text(token));
    
        }
    }
    

[/sourcecode]

**Step5: Reducer 2** In reducer we will swap back the result again.

[sourcecode language="java"] @Override public void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {
    
    
        String line = value.toString(); // agilencr 4
        StringTokenizer tokenizer = new StringTokenizer(line);
        while (tokenizer.hasMoreTokens()) {
    
            String token = tokenizer.nextToken();
    
        // Context here is like a multi set which allocates value &quot;one&quot; for key &quot;word&quot;.
    
            context.write(new LongWritable(Long.parseLong(tokenizer.nextToken().toString())),
                 new Text(token));
    
        }
    }
    

[/sourcecode]

So that we get the desired output like this:

[sourcecode]

# xomato 88

# zo 36

# agileNCR 21

# hello 4

[/sourcecode]

It might not be the best way but its quite useful to understand MapReduce and Comparator. The complete source code can be downloaded from [here ][5].

   [1]: http://agilencr.xebia.in/
   [2]: https://support.twitter.com/articles/101125-faqs-about-twitter-s-trends
   [3]: http://hadoop.apache.org/
   [4]: https://github.com/anirudh83/xke_hadoop/blob/master/input/test.jpda
   [5]: https://github.com/anirudh83/xke_hadoop/blob/master/src/com/example/trendfinder

## Comments

**[Vijay Rawat](#9314 "2013-03-13 13:36:35"):** Hey Anirudh, Good blog. Nice example.

