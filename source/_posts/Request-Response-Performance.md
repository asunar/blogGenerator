---
title: Request/Response Performance
date: 2020-06-28 09:43:22
tags: ADSD
---

It's time to analyze the performance characteristics of synchronous request/response operations as in the case of making REST/gRPC/Web Services to call one or more (micro)services.

Typical scenario where the server is running a language with a Garbage Collector to free up memory (Java, .NET languages)
* Client sends a request to the server
* Server receives the request
* Memory is allocated to get info out the request, stored on the heap
* Server sends a request to another microservice or database
* Server is waiting for a response for that request
* Garbage Collector (GC) comes (after some miliseconds) and asks: Are you done with that memory?
* Server: Nope, still waiting
* GC: No problem, marks memory as Gen1
* GC comes back (after some miliseconds) and asks: Done yet?
* Server: Nope, still waiting
* GC: No problem, marks memory as Gen2
* Now that memory is pinned. The only way to get it out of Gen2 is to do a global GC.Collect
* GC.Collect will freeze ALL THE THREADS while GC goes and cleans up the memory in Gen2.