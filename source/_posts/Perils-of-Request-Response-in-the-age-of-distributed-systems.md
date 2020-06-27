---
title: Perils of Request/Response in the age of distributed systems
date: 2020-06-26 18:40:22
tags:
---

This is the first of -hopefully- a series of posts to document my learnings after watching Udi Dahan's [Advanced Distributed Systems Design](https://learn.particular.net/courses/adsd-online) course.

I reviewed the videos multiple times and took detailed notes to really internalize the principles. These are the kind of technology agnostic principles that have been around for decades and should be relevant for the years to come as opposed the intricacies of the JavaScript framework du jour.

Let's set the stage by stating our end goal.

> Design and build performant, maintainable and resilient software systems where adding the 200th feature is as fast and easy as adding the first.

While it looks like a common goal that is chased by many software teams, achieving it surprisingly difficult in the age of agile. Typical software project starts with -if you're lucky- with a meeting with business stakeholder. People draw boxes on the whiteboard and off you go, time to start coding.