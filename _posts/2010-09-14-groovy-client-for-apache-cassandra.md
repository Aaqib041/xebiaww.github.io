---
layout: post
header-img: img/default-blog-pic.jpg
author: rrawat
description: 
post_id: 4727
created: 2010/09/14 11:45:23
created_gmt: 2010/09/14 06:45:23
comment_status: open
---

# Groovy client for Apache Cassandra

I have been exploring [Apache Cassandra][1] recently for implementation of a project. Cassandra is a NoSQL (not only SQL) database open sourced by Facebook.

This blog shares a Groovy implementation of a Cassandra client which makes accessing Cassandra as simple as accessing a HashMap. The Groovy implementation is currently a wrapper on the basic [Thrift ][2]API. The map keys serve as the query language to perform slices and range queries.

Cassandra provides a [Thrift ][2]based interface which makes it accessible from different languages. It is similar to a WSDL file for Web Service which can be used to generate code clients in different languages.  There are two open source wrappers on Thrift API to access Cassandra:- 

  1. [Cassandra Java Client][3]
  2. [Hector][4]
Cassandra Java Client is too basic. Hector is a good option as it provides features like pooling and is JMX enabled.

I tried the Java thrift API and Hector but it looks too cumbersome and verbose if you wrote code in Groovy or another programming language. Following is the sample groovy test code compared to Thrift based [Java example.][5].

[sourcecode language="groovy"]
    
    
    void testSingleColumnGetAndSet() {
        k[&quot;User/a/user_id&quot;] = &quot;1&quot;;
        assert k[&quot;User/a/user_id&quot;] == &quot;1&quot;;
    }
    
    void testSlice() {
        k[&quot;User/b/1&quot;] = &quot;1&quot;;
        k[&quot;User/b/2&quot;] = &quot;2&quot;;
        k[&quot;User/b/3&quot;] = &quot;3&quot;;
    
        assert k[&quot;User/b/[1,2,3]&quot;] == [b:[&quot;1&quot;, &quot;2&quot;, &quot;3&quot;]];
        assert k[&quot;User/b/[1-2]&quot;] == [b:[&quot;1&quot;, &quot;2&quot;]];
        assert k[&quot;User/b/[3]&quot;] == [b:[&quot;3&quot;]];
    }
    
    void testRangeSlices() {
        k[&quot;User/d/1&quot;] = &quot;1&quot;;
        k[&quot;User/d/2&quot;] = &quot;2&quot;;
    
        k[&quot;User/e/1&quot;] = &quot;1&quot;;
        k[&quot;User/e/2&quot;] = &quot;2&quot;;
    
        assert k[&quot;User/[e-e]/[1,2,3]&quot;] == [e:[&quot;1&quot;,&quot;2&quot;]];
        assert k[&quot;User/[d-e]/[1,2,3]&quot;] == [d:[&quot;1&quot;,&quot;2&quot;],e:[&quot;1&quot;,&quot;2&quot;]];
    }
    

[/sourcecode]

object 'k' is the keyspace wrapper, 'User' the column family and keys and columns follow them in a '/' separated path.

Cassandra data model is simple with only complication of a SuperColumn as explained in the [blog][6] (the title of the post should give you an idea). 

  1. Keyspace: like a database schema
  2. ColumnFamily: Individual data spaces (User,Tweets)
  3. Key: A key in the ColumnFamily
  4. Column: A name,value and timestamp combination
  5. SuperColumn: A Column that can contain multiple columns (in a ColumnFamily you cannot mix super and 'normal' columns)
Following diagram tries to explain the data model. 
    
    
    ![Cassandra DataModel][7]

Now lets have a look at the Groovy code that accesses Cassandra.

**How it works:**

This is simply achieved by providing getAt and putAt in the Keyspace object which is the Groovy way of operator overriding.

[sourcecode language="groovy"] def getAt(String key) { def ctx = Selector.readContext(key) ctx["ks"] = ks; def ret = client.get( ctx); return ret }
    
    
    void putAt(String key, Object value) {
        def ctx = Selector.writeContext(key)
        ctx[&quot;ks&quot;] = ks;
        client.put( ctx, value.toString());
    }
    

[/sourcecode]

The 'selectors' (keys used in the subscript operator) are inspired from [JXPath][8] world and are used for simple range queries as well. Below is a sample of the type of selectors that could be used.

[sourcecode language="groovy"] void testIdentifySetRequest() { assert Selector.parse("cf/key/col", SelectorType.SET) == "set_slice_col"; }
    
    
    void testIdentifyGetRequest() {
        assert Selector.parse(&quot;cf/key/col&quot;, SelectorType.GET) == &quot;get_col&quot;;
    
        assert Selector.parse(&quot;cf/key/[col1,col2]&quot;, SelectorType.GET) == &quot;get_slice_col&quot;; // key slice
        assert Selector.parse(&quot;cf/key/[col1-col2]&quot;, SelectorType.GET) == &quot;get_slice_col&quot;;   // col range
        assert Selector.parse(&quot;cf/key/[col1-]&quot;, SelectorType.GET) == &quot;get_slice_col&quot;; // start to end
        assert Selector.parse(&quot;cf/key/[*]&quot;, SelectorType.GET) ==  &quot;get_slice_col&quot;; // all cols
    
        assert Selector.parse(&quot;cf/[key1-key2]/[col1-col2]&quot;,SelectorType.GET) == &quot;get_range_slice&quot;; // range
    }
    

[/sourcecode]

Selectors are implemented using regular expressions. Selectors parse the 'path' expression and provide it as an input to client. Following is a code snippet of the expressions used.

[sourcecode language="groovy"] def static final SET_REGEX = [ set_slice_col: ~/(\w+)\/(\w+)\/(\w+)/ ];
    
    
    def static final GET_REGEX = [
        get_col: ~/(\w+)\/(\w+)\/(\w+)/, //  column get
        get_slice_col: ~/(\w+)\/(\w+)\/((\[(\w+[,-]?)+\])|(\[\*\]))/, // give me all columns
        get_range_slice: ~ /(\w+)\/((\[(\w+[,-]?)+\])|(\[\*\]))\/((\[(\w+[,-]?)+\])|(\[\*\]))/,   // get range slice
        get_range_key: ~/(\w+)\/\[(\w+[,-]?)+\]/  // give me a key range
    ] as LinkedHashMap;
    

[/sourcecode]

Below is the client code (GHector?) and Category code that takes out the reusable Cassandra specific code.

[sourcecode language="groovy"] def put(ctx,value) { execute { Cassandra.Client client -> use (CassandraCategory) { client.insert ctx.ks, ctx.key, cpath(ctx), value.serialize(), timestamp, defConLevel } } }
    
    
    def execute(cmd) {
        TTransport tt = new TSocket(server, port);
        TProtocol tp =  new TBinaryProtocol(tt);
        Cassandra.Client c = new Cassandra.Client(tp);
        try {
            tt.open();
            cmd(c);
        } finally {
            tt.close();
        }
    }
    ...
    }
    // Category code
    class CassandraCategory {
    
    static cpath(ks, args) {
        ColumnPath cp = new ColumnPath(args[&quot;cf&quot;]);
        cp.setColumn(args[&quot;col&quot;].bytes);
        return cp;
    }
    
    static serialize(String s) {
        return s.getBytes(&quot;UTF-8&quot;);
    }
    ...
    }
    

[/sourcecode]

This approach could be useful for applications where we have fixed queries so the selectors could be cached. An application selecting large number of columns/keys dynamically would need an alternate approach (some sort of query API with filters).

This is still a work in progress (like results could have column names as keys as well... wait a second , it looks like JSON). In future I will be trying to implement the client using Hector instead of the raw Thrift API. Your comments are appreciated on how to make this better.

   [1]: http://cassandra.apache.org/ (Apache Cassandra)
   [2]: http://incubator.apache.org/thrift/ (Apache Thrift)
   [3]: http://code.google.com/p/cassandra-java-client/
   [4]: http://github.com/rantav/hector
   [5]: http://wiki.apache.org/cassandra/ThriftExamples#Java (Java example)
   [6]: http://arin.me/blog/wtf-is-a-supercolumn-cassandra-data-model
   [7]: http://xebee.xebia.in/wp-content/uploads/2010/09/DataModel2.png (DataModel)
   [8]: http://commons.apache.org/jxpath/ (JXPath)