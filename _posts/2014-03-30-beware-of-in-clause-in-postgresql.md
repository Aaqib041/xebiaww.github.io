---
layout: post
header-img: img/default-blog-pic.jpg
author: dbansal
description: 
post_id: 18204
created: 2014/03/30 15:43:50
created_gmt: 2014/03/30 10:43:50
comment_status: open
---

# Beware of IN-Clause in Postgresql

During my last assignment I was given a task to write an SQL query to  “kind of deduplicate” some records from a single table. Doesn’t it sound a very trivial task. After all, this is one of most frequently asked  questions from database subject. Let me explain you what the actual structure of the table looked like. Keeping the confidentiality of my project, I would simulate the problem through a general scenario (I am in no mood to spend some time on monster.com).

### Problem Statement

An airline firm, FlyHigh, keeps track of its customers in a table Customer in the database. The table looks like following: 
    
    
    Customer [id, ssn, name, gender,email, phone, fax]

  * id: Auto-generated primary key
  * ssn (indexed): Social Security Number (alphanumeric) - comes as part of datafile.
  * Rest are self-explanatory.
FlyHigh receives various new customers’ information in form of csv files from various airports around the world. These files are concatenated into a single file which is then onboarded to the database through some tool like DataCleaner.

Since the files are concatenated to single file, there can be a chance that few customers’ information is duplicated (which can be seen through SSN). So this table is now required to be deduplicated of any such records keeping only the most recent record entering the database i.e. having highest id value. Now sit back and write the query for finding the records to be deleted. (Deleting would require just to replace SELECT * with DELETE). 

### Basic Solution:

Simplest query, that you would most probably come up with, looks like: 
    
    
    SELECT * FROM customer
    WHERE id NOT IN (SELECT MAX(id)
        FROM customer
        GROUP BY ssn
        );

So I ran this query on Postgres with 20000 + 13 records (with 13 duplicates). Zapp !! It produced exactly these 13 records in 41 milliseconds. Lets see its query plan: 

> 
>     "Seq Scan on customer (cost=2124.76..2643.92 rows=10006 width=75)"
>      " Filter: (NOT (hashed SubPlan 1))"
>      " SubPlan 1"
>      " -> GroupAggregate (cost=0.00..2074.76 rows=20000 width=17)"
>      " -> Index Scan using ssn_idx on customer (cost=0.00..1774.69 rows=20013 width=17)"

So everything looks perfectly fine and performant. Now I changed the number of customer records from 20000 to 30000 (thanks to the recent sale in flying market). With same 13 duplicate records, I ran the query. 5 secs….. 15 secs….20 secs.. still no result...43.. 44.. 45...46 seconds. There it is. Postgres engine took 46 seconds to fetch these 13 seconds out of the pool of 30000 records against 41 milliseconds to fetch the same 13 records out of 20000 records. It is neither linear nor exponential nor any other form of mathematical relation. Lets see what the explain plan looks like (I know you are very curious just like I was): 

> 
>     "Seq Scan on customer (cost=0.00..53262636.66 rows=15006 width=75)"
>      " Filter: (NOT (SubPlan 1))"
>      " SubPlan 1"
>      " -> Materialize (cost=0.00..3474.25 rows=30000 width=17)"
>      " -> GroupAggregate (cost=0.00..3148.25 rows=30000 width=17)"
>      " -> Index Scan using ssn_idx on customer (cost=0.00..2698.19 rows=30013 width=17)"

The two plans look very similar. The only difference is that the query is now “Materializing” some batches of data and using a Subplan instead of a hashed subplan as in previous query’s explain.

The inner query is producing so much data that cannot be accommodated in a single batch. So the batches are swapped out to disks (i.e. Materialized). Because the hashed Subplan is not capable of handling batches of data, a subplan is used instead. Why so ? This [link][1] may be able explain it better. [Getting into the query optimizer’s strategies is beyond the scope of this blog]. Seq scan with a subplan is what we should try to avoid in this case.

But how ?

### Enhanced Solution:

Currently, in the above query, the subquery returns 30000 records. By any means, can we convert it from a NOT-IN clause to IN clause with the subquery that returns those 13 records instead of 30000 records. Lets try it. 

Lets have a look at the sample data as follows:

![Selection_005][2]

For that we need to filter out these records:

![Selection_002][3]

So we have to think of a way to get this last table of records through a query. Lets go step by step:

![Selection_006][4]

The unified single query looks like:
    
    
    SELECT *
    FROM customer
    WHERE id IN
      (SELECT all_duplicates.id id
      FROM
        (SELECT id,
          c.ssn
        FROM
          (SELECT ssn FROM customer GROUP BY ssn HAVING COUNT(1) >= 2
          ) duplicates,
          customer c
        WHERE c.ssn = duplicates.ssn
        ) all_duplicates,
        (SELECT MAX(c.id) maxid,
          c.ssn dsid
        FROM
          (SELECT ssn FROM customer GROUP BY ssn HAVING COUNT(1) >= 2
          ) duplicates,
          customer c
        WHERE c.ssn = duplicates.ssn
        GROUP BY c.ssn
        ) maxids
      WHERE all_duplicates.ssn = maxids.dsid
      AND maxids.maxid        != all_duplicates.id
      );

Lets hit the run button. Back to ZIPP!!  62 milliseconds.

There is no Seq scan with Subplan attached to it in the explain plan. I would not print out the full query plan for this query. You can check that on your own. Below is the comparison of numbers in two solutions that we just discussed.

![Selection_007][5]

### **Conclusion:**

IN-clause can dramatically increase the execution time if the subquery returns substantially large number of records. We should try to avoid using IN-clause as much possible, especially, when we do not have any control over the number of records returned by subquery. Also, as we saw, under most business cases it is evident that the records are skewed on one side. In these cases, we can use either IN or NOT-IN clause depending upon the direction of skewness.

   [1]: http://postgresql.1045698.n5.nabble.com/Why-will-hashed-SubPlan-not-use-multiple-batches-td5742450.html
   [2]: http://xebee.xebia.in/wp-content/uploads/2014/03/Selection_005.png
   [3]: http://xebee.xebia.in/wp-content/uploads/2014/03/Selection_002.png
   [4]: http://xebee.xebia.in/wp-content/uploads/2014/03/Selection_006.png
   [5]: http://xebee.xebia.in/wp-content/uploads/2014/03/Selection_007.png

## Comments

**[Gagan Juneja](#9483 "2014-05-01 20:43:10"):** Nice explanation !!

**[Deepak](#9484 "2014-05-01 23:04:41"):** Thanks :)

