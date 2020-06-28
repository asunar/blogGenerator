---
title: Trends/Predictions for the Software Industry
date: 2019-01-13 08:21:46
tags:
---
It is that time of the year when most people reflect on the past and make plans/predictions for the future. I have been having some conversations with my colleagues and wanted to put my thoughts on the two main trends dominating the software industry on _“paper”_.

## [](#Serverless-and-Remote-Work "Serverless and Remote Work")Serverless and Remote Work

Serverless software applications and architectures will dominate the software landscape in the coming years in my opinion. Serverless coupled with wider adoption of remote work arrangements may have further downstream effects on everything from real estate to social mobility in the developing world.

If software is eating the world, analyzing the trends that affect software construction may yield interesting insights.

## [](#Serverless "Serverless ")Serverless

Serverless software applications are usually

*   Event-driven ( someone clicked on a link/uploaded a file, it is 4am on a Tuesday i.e. code runs in response to an event )
*   Stateless ( don’t store any data )
*   Billed only for the milliseconds they run

It is called Serverless because someone else (like Amazon Web Services(AWS) or Microsoft Azure) runs and maintains the servers.

Why is this important? As an organization who uses/maintains software to run the business, think about the things one does **NOT** need to worry about :

*   Operating system updates ( ex. security updates, upgrade to Windows 16 etc. )
*   Network attack surface (no continuously running servers that may be prone to attacks)
*   Each process gets its own entire compute env (someone accidentally uploading a huge file should not affect any other users)
*   Paying for server/database licenses, system/server/network admin salaries  
    (if you happen to work in one of these areas and worry about being phased out read [this](https://www.susanjfowler.com/blog/2016/10/13/the-ops-identity-crisis))

In a nutshell, serverless will free businesses to spend more of their time and resources on improving the core business. On second thought, make that _smart businesses_; there will always be companies that are resistant to change.

> The avalanche has begun, it’s too late for the pebbles to vote. - Ambassador Kosh

If an organization can use an authentication service, data storage service, email service etc. that can scale automatically based on the need and pay for only what they use, why would they not do it? This will almost certainly be cheaper than paying software developer salaries to build these services in-house.

Given that most companies will want to do more with less, what will the already employed software engineers work on? Werner Vogels may have the answer here.

> In the future, all the code you ever write will be business logic. - Werner Vogels (Chief Technology Officer of Amazon)

<del>Coming soon</del> [Remote Work](../../../2019/01/15/Remote-Work).

