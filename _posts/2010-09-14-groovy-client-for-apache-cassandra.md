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

<p>I have been exploring <a title="Apache Cassandra" href="http://cassandra.apache.org/" target="_self">Apache Cassandra</a> recently for implementation of a project. Cassandra is a NoSQL (not only SQL) database open sourced by Facebook.</p>
<p>This blog shares a Groovy implementation of a Cassandra client which makes accessing Cassandra as simple as accessing a HashMap. The Groovy implementation is currently a wrapper on the basic <a title="Apache Thrift" href="http://incubator.apache.org/thrift/" target="_self">Thrift </a>API. The map keys serve as the query language to perform slices and range queries.<!--more--></p>
<p>Cassandra provides a <a title="Apache Thrift" href="http://incubator.apache.org/thrift/" target="_self">Thrift </a>based interface which makes it accessible from different languages. It is similar to a WSDL file for Web Service which can be used to generate code clients in different languages.  There are two open source wrappers on Thrift API to access Cassandra:-
<ol>
    <li><a href="http://code.google.com/p/cassandra-java-client/">Cassandra Java Client</a></li>
    <li><a href="http://github.com/rantav/hector">Hector</a></li>
</ol>
Cassandra Java Client is too basic. Hector is a good option as it provides features like pooling and is JMX enabled.</p>
<p>I tried the Java thrift API and Hector but it looks too cumbersome and verbose if you wrote code in Groovy or another programming language. Following is the sample groovy test code compared to Thrift based <a title="Java example" href="http://wiki.apache.org/cassandra/ThriftExamples#Java" target="_blank">Java example.</a>.</p>
<p>[sourcecode language="groovy"]</p>
<pre><code>void testSingleColumnGetAndSet() {
    k[&amp;quot;User/a/user_id&amp;quot;] = &amp;quot;1&amp;quot;;
    assert k[&amp;quot;User/a/user_id&amp;quot;] == &amp;quot;1&amp;quot;;
}

void testSlice() {
    k[&amp;quot;User/b/1&amp;quot;] = &amp;quot;1&amp;quot;;
    k[&amp;quot;User/b/2&amp;quot;] = &amp;quot;2&amp;quot;;
    k[&amp;quot;User/b/3&amp;quot;] = &amp;quot;3&amp;quot;;

    assert k[&amp;quot;User/b/[1,2,3]&amp;quot;] == [b:[&amp;quot;1&amp;quot;, &amp;quot;2&amp;quot;, &amp;quot;3&amp;quot;]];
    assert k[&amp;quot;User/b/[1-2]&amp;quot;] == [b:[&amp;quot;1&amp;quot;, &amp;quot;2&amp;quot;]];
    assert k[&amp;quot;User/b/[3]&amp;quot;] == [b:[&amp;quot;3&amp;quot;]];
}

void testRangeSlices() {
    k[&amp;quot;User/d/1&amp;quot;] = &amp;quot;1&amp;quot;;
    k[&amp;quot;User/d/2&amp;quot;] = &amp;quot;2&amp;quot;;

    k[&amp;quot;User/e/1&amp;quot;] = &amp;quot;1&amp;quot;;
    k[&amp;quot;User/e/2&amp;quot;] = &amp;quot;2&amp;quot;;

    assert k[&amp;quot;User/[e-e]/[1,2,3]&amp;quot;] == [e:[&amp;quot;1&amp;quot;,&amp;quot;2&amp;quot;]];
    assert k[&amp;quot;User/[d-e]/[1,2,3]&amp;quot;] == [d:[&amp;quot;1&amp;quot;,&amp;quot;2&amp;quot;],e:[&amp;quot;1&amp;quot;,&amp;quot;2&amp;quot;]];
}
</code></pre>
<p>[/sourcecode]</p>
<p>object 'k' is the keyspace wrapper, 'User' the column family and keys and columns follow them in a '/' separated path.</p>
<p>Cassandra data model is simple with only complication of a SuperColumn as explained in the <a href="http://arin.me/blog/wtf-is-a-supercolumn-cassandra-data-model">blog</a> (the title of the post should give you an idea).
<ol>
    <li>Keyspace: like a database schema</li>
    <li>ColumnFamily: Individual data spaces (User,Tweets)</li>
    <li>Key: A key in the ColumnFamily</li>
    <li>Column: A name,value and timestamp combination</li>
    <li>SuperColumn: A Column that can contain multiple columns (in a ColumnFamily you cannot mix super and 'normal' columns)</li>
</ol>
Following diagram tries to explain the data model.
<pre><a rel="attachment wp-att-4781" href="http://xebee.xebia.in/2010/09/14/groovy-client-for-apache-cassandra/datamodel-3/"><img class="alignnone size-full wp-image-4781" title="DataModel" src="http://xebee.xebia.in/wp-content/uploads/2010/09/DataModel2.png" alt="Cassandra DataModel" width="572" height="324" /></a></pre>
Now lets have a look at the Groovy code that accesses Cassandra.</p>
<p><strong>How it works:</strong></p>
<p>This is simply achieved by providing getAt and putAt in the Keyspace object which is the Groovy way of operator overriding.</p>
<p>[sourcecode language="groovy"]
def getAt(String key) {
        def ctx = Selector.readContext(key)
        ctx[&quot;ks&quot;] = ks;
        def ret = client.get( ctx);
        return ret
    }</p>
<pre><code>void putAt(String key, Object value) {
    def ctx = Selector.writeContext(key)
    ctx[&amp;quot;ks&amp;quot;] = ks;
    client.put( ctx, value.toString());
}
</code></pre>
<p>[/sourcecode]</p>
<p>The 'selectors' (keys used in the subscript operator) are inspired from <a title="JXPath" href="http://commons.apache.org/jxpath/" target="_self">JXPath</a> world and are used for simple range queries as well. Below is a sample of the type of selectors that could be used.</p>
<p>[sourcecode language="groovy"]
    void testIdentifySetRequest() {
        assert Selector.parse(&quot;cf/key/col&quot;, SelectorType.SET) == &quot;set_slice_col&quot;;
    }</p>
<pre><code>void testIdentifyGetRequest() {
    assert Selector.parse(&amp;quot;cf/key/col&amp;quot;, SelectorType.GET) == &amp;quot;get_col&amp;quot;;

    assert Selector.parse(&amp;quot;cf/key/[col1,col2]&amp;quot;, SelectorType.GET) == &amp;quot;get_slice_col&amp;quot;; // key slice
    assert Selector.parse(&amp;quot;cf/key/[col1-col2]&amp;quot;, SelectorType.GET) == &amp;quot;get_slice_col&amp;quot;;   // col range
    assert Selector.parse(&amp;quot;cf/key/[col1-]&amp;quot;, SelectorType.GET) == &amp;quot;get_slice_col&amp;quot;; // start to end
    assert Selector.parse(&amp;quot;cf/key/[*]&amp;quot;, SelectorType.GET) ==  &amp;quot;get_slice_col&amp;quot;; // all cols

    assert Selector.parse(&amp;quot;cf/[key1-key2]/[col1-col2]&amp;quot;,SelectorType.GET) == &amp;quot;get_range_slice&amp;quot;; // range
}
</code></pre>
<p>[/sourcecode]</p>
<p>Selectors are implemented using regular expressions. Selectors parse the 'path' expression and provide it as an input to client. Following is a code snippet of the expressions used.</p>
<p>[sourcecode language="groovy"]
    def static final SET_REGEX = [
            set_slice_col: ~/(\w+)\/(\w+)\/(\w+)/
        ];</p>
<pre><code>def static final GET_REGEX = [
    get_col: ~/(\w+)\/(\w+)\/(\w+)/, //  column get
    get_slice_col: ~/(\w+)\/(\w+)\/((\[(\w+[,-]?)+\])|(\[\*\]))/, // give me all columns
    get_range_slice: ~ /(\w+)\/((\[(\w+[,-]?)+\])|(\[\*\]))\/((\[(\w+[,-]?)+\])|(\[\*\]))/,   // get range slice
    get_range_key: ~/(\w+)\/\[(\w+[,-]?)+\]/  // give me a key range
] as LinkedHashMap;
</code></pre>
<p>[/sourcecode]</p>
<p>Below is the client code (GHector?) and Category code that takes out the reusable Cassandra specific code.</p>
<p>[sourcecode language="groovy"]
    def put(ctx,value) {
        execute { Cassandra.Client client -&gt;
            use (CassandraCategory) {
                client.insert ctx.ks, ctx.key, cpath(ctx), value.serialize(), timestamp, defConLevel
            }
        }
    }</p>
<pre><code>def execute(cmd) {
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
    ColumnPath cp = new ColumnPath(args[&amp;quot;cf&amp;quot;]);
    cp.setColumn(args[&amp;quot;col&amp;quot;].bytes);
    return cp;
}

static serialize(String s) {
    return s.getBytes(&amp;quot;UTF-8&amp;quot;);
}
...
}
</code></pre>
<p>[/sourcecode]</p>
<p>This approach could be useful for applications where we have fixed queries so the selectors could be cached. An application selecting large number of columns/keys dynamically would need an alternate approach (some sort of query API with filters).</p>
<p>This is still a work in progress (like results could have column names as keys as well... wait a second , it looks like JSON). In future I will be trying to implement the client using Hector instead of the raw Thrift API. Your comments are appreciated on how to make this better.</p>