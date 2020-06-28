---
title: Works on my machine
date: 2020-06-28 08:43:44
tags:
---
This is a part of a series of posts to document my learnings after watching Udi Dahan's [Advanced Distributed Systems Design](https://learn.particular.net/courses/adsd-online) course.


In the previous [post](../../../2020/06/26/Perils-of-Request-Response-in-the-age-of-distributed-systems), we demonstrated the unreliability of request/response.

If such a trivial example can show how unreliable request/response *could* be, why do we continue to write similar type of code?

The problem is request/response code may work in production without any issues for a long time. However, if and when the conditions are right we may receive a bug report about lost data and/or duplicate processing etc. 

* Unit tests pass
* Integration tests pass
* End to End test pass 
(If any of these had failed, we would not have deployed to production anyways right?)

At this point, we may attempt to reproduce the issue manually.
Most of the developers have some sort of local setup where we start up each dependent service on our machine and proceed with debugging. The services involved may be logically separate but they are not physically separate (all running on the same machine - more on this on another post). No dice, close the JIRA ticket with comment.

> Cannot reproduce.

Congratulations on joining the [Works on my machine](https://blog.codinghorror.com/the-works-on-my-machine-certification-program/) club.

[![Works on my machine](/images/works_on_my_machine.png)]()

