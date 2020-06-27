---
title: Perils of Request/Response in the age of distributed systems
date: 2020-06-26 18:40:22
tags: ADSD 
---

This is the first of -hopefully- a series of posts to document my learnings after watching Udi Dahan's [Advanced Distributed Systems Design](https://learn.particular.net/courses/adsd-online) course.

I reviewed the videos multiple times and took detailed notes to really internalize the principles. These are the kind of technology agnostic principles that have been around for decades and should be relevant for years to come as opposed to the intricacies of the JavaScript framework du jour.

Let's set the stage by stating our end goal.

> Design and build performant, maintainable and resilient software systems where adding the 200th feature is as fast and easy as adding the first.

We will start with reliability. Everyone knows that network is not reliable. However, we continue to write code similar to this.

```csharp
    var result = importantService.Process(data);
```

Udi Dahan used timeout exception to demonstrate the reliability issues with this code snippet.
1. Need to send some data to a remote service for a long running workflow.
2. Invoke a synchronous call and wait for response.
3. Get a timeout exception
 
 Here are some posibble scenarios

 * Data did not arrive at the server 
 * Data arrived, service did not complete processing before timeout.

 At this point client does not know what to do; this is important data needs to processed. 
 
 * Resend and risk duplicate processing? 
 * Write service code to handle deduplication? What if we don't own the service?
 * What do we tell the user? I know "An unexpected error occurred" :)

 The bottom line is:
 > If you're doing this on top of http, you're always going to be almost there. There's always going to be the scenario of this time I've done it. This time I finally solved all of the edge cases, and then some period of time later and production, some other thing will blow up and you'll discover oh except that one.       
 -Udi Dahan

Today I learned: It is very difficult to build reliable systems on top of http with request/response (remote procedure calling-RPC, REST, Web Services etc. )



