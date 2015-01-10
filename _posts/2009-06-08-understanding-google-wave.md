---
layout: post
header-img: img/default-blog-pic.jpg
author: sonnygill
description: 
post_id: 1898
created: 2009/06/08 10:51:42
created_gmt: 2009/06/08 09:51:42
comment_status: open
---

# Understanding Google Wave

(This blog post gives an overview of the architecture and technical concepts in Google Wave. If you are interested in how to use **Google Wave** in your applications, see [Developing with Google Wave][1])

At Google I/O 2009, Google unveiled Wave. Wave is a new way of thinking about online conversations.

Consider the following situations - 

  1. You write a blog post. Somebody comes around and posts a really insightful comment. Now, you or the comment author want to convert that comment into an independent blog post. 
  2. You have been having a long email discussion with a colleague. Now you would like to invite another colleague to the same. She will need to know the context of the discussion and how it evolved. 
  3. You email a few colleagues a draft of an article for review. They all email their comments back to you. A lot of them are suggesting the same changes without realizing that they have already been addressed. 

With the tools we are using today such as email, blogs, IM etc., all of the above will require some kind of tedious copy - paste, and manual tracking of the changes being made.

Is there a better way?

The **Google Wave** model tries to provide a better way  by doing away with the distinction between email, IM and other forms of online conversations. In this model, an email conversation, a blog post with its comments or an auction with its bids, are just online conversations with multiple participants. At the data model level, all you have is a wave, and you can look at it in many different ways.

Multiple participants can edit a Wave at the same time, and the wave client can show the changes in real time. The changes made to a Wave are stored as a series of operations. You can play the changes back, or revert the Wave to an earlier version. At the data model level, constituents of the Wave form a tree structure. You can take a particular node and spin it into an independent Wave.

Wave providers (servers) give out Wave accounts, and are responsible for sharing Waves with the clients, as well as with other Wave servers. Wave servers communicate with each other using **Google Wave Federation Protocol**, which is an extension of XMPP protocol. Wave servers use cryptographic signatures and certificates, in addition to transport level encryption provided by XMPP, to authenticate among themselves.

**Google Wave Operational Transformation**

A key concept at the center of Google Wave is **Operational Transformation** (OT).

![Operational Transformation][2]

* * *

Credit: [Wikipedia][3]

Many participants can edit a Wave at the same time, and they can see each other's changes as they are made. These attributes, concurrent modification and low latency updates, are implemented based on OT. OT is a framework for concurrency control. The document being edited is replicated at all sites. All editing actions are stored as operations that are propagated across all clients, local and remote. Concurrent edits are transformed by the server before being applied and the transformed operations are communicated to all clients. This is very similar to how version control systems like CVS manage merging of simultaneous edits of a document. 

From the **Google Wave Operational Transformation** whitepaper - 

> Wave OT modifies the basic theory of OT by requiring the client to wait for acknowledgment from the server before sending more operations. When a server acknowledges a client's operation, it means the server has transformed the client's operation, applied it to the server's copy of the Wavelet and broadcasted the transformed operation to all other connected clients. Whilst the client is waiting for the acknowledgment, it caches operations produced locally and sends them in bulk later.

For more technical details on these topics, see [Operational transformation][4], and [Google Wave Operational Transformation][5].

**Google Wave Data Model**

A Wave is made up of Wavelets. Each wavelet has a unique id within its wave. The Wavelet contains a list of participants and a set of documents. A document in a wavelet has a unique id within its wavelet. It is composed of an XML document, and a set of annotations. Some of these annotations specify the styling of the text content of the document. Concurrency control and operational transformation are applied at the level of Wavelets. A particular wave user may only have access to a subset of the wavelets in that wave.

The state of a wavelet is defined entirely by an ordered sequence of operations that have been applied to it. Wave clients send these operations to the server to communicate changes to the underlying document, and the server propagates these operations (after a transformation, if required) to other clients and other servers participating in the wave.

**Resources - ** [Google Wave Data Model and Client-Server protocol][6] [Google Wave Federation Architecture][7] [Operational transformation][4] [Wave Protocol forum][8] [Google Wave Developer Blog][9]

In the next blog post on this subject, we will look at different ways of [developing with Google Wave][10].

   [1]: http://blog.xebia.com/2009/06/08/understanding-google-wave
   [2]: http://xebee.xebia.in/wp-content/uploads/2009/06/basic_ot.png (Operational Transformation)
   [3]: http://en.wikipedia.org/wiki/File:Basicot.png (Wikipedia)
   [4]: http://en.wikipedia.org/wiki/Operational_transformation
   [5]: http://www.waveprotocol.org/whitepapers/operational-transform
   [6]: http://www.waveprotocol.org/whitepapers/internal-client-server-protocol
   [7]: http://www.waveprotocol.org/whitepapers/google-wave-architecture
   [8]: http://groups.google.com/group/wave-protocol/
   [9]: http://googlewavedev.blogspot.com/
   [10]: http://blog.xebia.com/2009/06/08/developing-with-google-wave