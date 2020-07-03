---
title: Request/Response Performance
tags: ADSD
date: 2020-07-02 20:40:51
---

This is a part of a series of [posts](../../../../tags/ADSD/) to document my learnings after watching Udi Dahan's [Advanced Distributed Systems Design](https://learn.particular.net/courses/adsd-online) course.

We will focus on performance aspects of request/response interactions. Latency and bandwidth are two important concepts that we need to understand when discussing performance.

>Latency: Time to cross the network in one direction.

Remember that latency does not include any processing time (serialization/deserialization of the payload). Furthermore, looking at the scaled latency chart will help us understand relative cost of remote calls.

[![](/images/scaled_latency.png)]()


```csharp
    var importantService = new ImportantService();
    var result = importantService.Process(data);
```

Assuming that we're making a remote call across the US, we can conclude that -relatively speaking- if the first line takes 1 second, the second line will take 4 years. Let that sink in for a moment.

>Bandwidth: the capacity for data transfer of an electronic communications system

Bandwidth is not usually measured. Most developers are not even aware of the bandwidth that is available for their applications in production. Hence the fallacy: Bandwidth is unlimited. Traditionally, the amount of data processed by the software systems grew much faster than bandwidth capacity. The network will obviously affect the performance of your application. 

#### Example: Gigabit internet= 128mb/sec
#### Available bandwidth = 128 - 60(TCP cost) - serialization_cost(bigger object graph, xml => higher cost)

Today I learned: Making remote calls can be extremely slow compared to to the rest of the application code. We can safely assume that this is going to hold true for some time.