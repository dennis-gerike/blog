---
title: Discover Quality
description: This blog discusses ways to do quality assurance in the modern software engineering world.
---

Software engineering is hard.
Typing code is the easy part.
Doing it in a way, so that the code is stable, readable, maintainable, testable, performant, and is actually doing what the stakeholder expects it to do, is the difficult part.

Achieving good software is only in parts a technical problem.
It is important to make sure that everyone involved has understood the requirements and that everyone has the **same** understanding of them.
In general, that requires multiple discussions and therefore good communication skills.
At first glance, this sounds like the job profile of a product owner (or manager), but it is the responsibility of the whole team (Dev, PO, QA, Stakeholder, etc) to achieve this shared understanding.

My weapons of choice are **BDD**, **Cypress** and **Jira/Xray**. 
BDD gives us the tools to create the shared understanding and to extract a reliable list of acceptance criteria. 
With Cypress it is very easy to convert those ACs to automated end-2-end tests. 
And with the Jira plugin Xray we get a powerful test management tool, which directly connects the tests with the requirements, giving us a high level of transparency.

In this blog I will share my experiences with those tools and frameworks.
There will be some hands-on articles and tutorials on how to use them correctly. 
And there will be articles, that are more on the theoretical side, in which I discuss the ideas and concepts behind quality assurance in the modern software engineering world. 
You can start your [reading here]({{< ref "/posts" >}}).
