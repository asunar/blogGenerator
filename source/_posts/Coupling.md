---
title: Coupling
date: 2020-07-10 07:53:28
tags: ADSD
---
This is a parts of a series of [posts](../../../../tags/ADSD/) to document my learnings after watching Udi Dahan's [Advanced Distributed Systems Design](https://learn.particular.net/courses/adsd-online) course.

Coupling is one of the fundamental concepts that affects the maintainability of software systems as discussed [before](../../../2020/07/09/Request-Response-to-Service-Boundaries/).  Coupling measures how dependent are the components of the system on one another. 

* Incoming coupling: who depends on you (aka afferent coupling)
* Outcoming coupling: who you depend on (aka efferent coupling) 

| Component   | Incoming | Outgoing | Comment                                                      | Example                             |
|:-:|:--------------:|:--------------:|--------------------------------------------------------------|-------------------------------------|
| A |        5       |        0       | Called by many, does not call others; not necessarily bad    | Logging library                     |
| B |        0       |        5       | Calls many others, not called by others; not necessarily bad | UI layer code, Controllers          |
| C |        2       |        2       | Calls many others and called by many; needs investigation    | Higher numbers: recipe for disaster |
| D |        0       |        0       | Does not call or get called by others                        | Useless code, better deleted        |