---
title: Request/Response Scalability
date: 2020-06-29 09:43:22
tags: ADSD
---
This is a part of a series of [posts](../../../../tags/ADSD/) to document my learnings after watching Udi Dahan's [Advanced Distributed Systems Design](https://learn.particular.net/courses/adsd-online) course.

It's time to analyze the scalability characteristics of synchronous request/response operations as in the case of making REST/gRPC/Web Services to call one or more (micro)services, databases.

Typical scenario where the server is running a language with a Garbage Collector to free up memory (Java, .NET languages)
* Client sends a request to the server
* Server receives the request
* Memory is allocated to get info out the request, stored on the heap
* Server sends a request to another microservice or database
* Server waits for a response for that request
* Garbage Collector (GC) comes (after some miliseconds) and asks: Are you done with that memory?
* Server: Nope, still waiting
* GC: No problem, marks memory as Gen1
* GC comes back (after some miliseconds) and asks: Done yet?
* Server: Nope, still waiting
* GC: No problem, marks memory as Gen2
* Now that memory is pinned. The only way to get it out of Gen2 is to do a global GC.Collect
* Response comes back but GC DOES NOT clear regular memory in Gen2, just the disposables that have gone out of scope.
* GC.Collect will freeze ALL THE THREADS while GC goes and cleans up the memory in Gen2.

I was one of those people who thought that async/await fixes this problem.

> No, it doesn't. That just means the thread gets out of the way so you can take on another request. That actually means you're gonna fill up faster and use more memory quicker and maybe fail quicker. It is not a solution. 
> -David Boike


Thanks to [ADSD](https://learn.particular.net/courses/adsd-online) course and to [David's Boike's presentation](https://www.youtube.com/watch?v=aE-p0cfwTVU), I now understand how under sufficient load, long running queries can exhaust all the memory on a web server. 

Today I learned: In addition to being [unreliable](../../../../2020/06/26/Request-Response-Reliability/) request/response style interaction does not scale well under load. 