---
title: Who wants what and why? The original meaning of a user story
description: User stories sometimes become really big and cause more confusion than being helpful. How can we get back to the roots and focus on the important parts?  
tags: [ "User Story", "Jira", "Agile", "Connextra" ]
date: 2024-01-31
---

We have all seen them. 
Gigantic _Jira_ user stories, filled with extensive specification, a truckload of screens for every conceivable device type and a list of things that are supposed to be acceptance criteria. 
But, regardless of all the available information, in the end the programmed solution deviates significantly from what the stakeholder really wanted. 
How is this possible? 
Why does this happen? 
How can we do better?

<!--more-->

Originally, the idea of a user story was to describe a feature request in a compact and informal way.
This short description was meant to be used as a discussion starter.
In the _BDD_ world this would be the beginning of the discovery phase.
User stories were not meant as requirement documents.
Yes, we need good requirements, but they are not the starting point, they are the result of the discovery phase.
Here, the stakeholder presents and discusses the new feature with everyone involved - that can be Devs, Ops, PO, QA, UI/UX, and so on.
The first goal is to create a shared understanding.
Before any requirements document is written and before even one line of code is created, everyone needs to understand what we are trying to achieve with this new feature and why.
When this is clear, the next step is to check, if this feature request makes sense in general, if it is doable with the existing resources and knowhow, if there is maybe an easier way to achieve it, etc.
Now, we are in a situation were everyone has the same understanding.
We can start fleshing out the ticket and add details.
The developers can have refinement and planning meetings to specify the technical details and estimate the effort.
And QA has enough information to write down the (end-2-end) test cases.

A good method to write a good user story is to ask the question: _"Who wants what and why?"_ 
This template is also known as the _Connextra_ format.
```plain
As a <Role>
I want to be able to <Feature>
So, I can <Reason>
```

Unfortunately, Jira has "burned" the word _user story_.
They use it to describe one of their ticket types (epic, user story, task, bug).
In their [documentation](https://www.atlassian.com/agile/project-management/user-stories) they correctly define what a user story should be, but nobody uses them that way.
In reality tickets are treated as a container to hold every piece of information that is available for a specific feature request, but there is no dedicated field to put the user story itself.


## ✅ Who?

Depending on the perspective, the "who" can be two different things.
It can either be the _person_ (or department) that is _requesting_ this feature, or it can be the _end user_ that will be working with it.

In the first case, knowing the "who" can help the team to better understand the background (or the implications) of the request.
_"Oh, the SEO team wants this feature. Then we have to think about A, B and C."_
As a bonus, this automatically gives us the contact person (or team or department) in case of open questions.

The literature often recommends the other way - to write user stories from the perspective of the end user.
But, this can sometimes be misleading or just not very helpful.
Take the following example:
```plain
As a customer
I want to have free shipping
So I can save money
``` 
We are a business, not a charity.
How does it help us that the customer saves money?
That can only be a side effect, but not the real reason behind the feature request.
As a developer or QA person I don't really understand what problem we want to solve with "free shipping", and therefore I don't know in which direction I should focus my testing or coding efforts.
Also, who can I ask?
I can only hope to find my contact person somewhere in the ticket.

In many cases the user is not really the one that wants something.
It is more like the stakeholder wants to enable the user to do something. 
A small, but important difference.

Let's adjust the example:
```plain
As a business person
I want to offer free shipping
So the customer can save money
``` 

This solves the contact person problem, but the money saving part is still not convincing or helpful.
Let's go one step further and adjust also the "why":
```plain
As a business person
I want to offer the customer free shipping
So our average conversion rate increases
```

Now, we have a concrete reason, which makes it more tangible (and even measurable). 
We also know, that the feature request comes from a business perspective, not from IT, not from SEO, not from UI/UX.
This makes it easier to get into a discussion and into discovery - which was the whole point of user stories in the first place.


## ✅ What?

In general, the "what" is the easiest part of a user story.
It contains the feature request itself, e.g. _"adding a newsletter subscription box to the start page"_ or _"having free shipping for every purchase above 50€"_.
Even if the stakeholder did not provide the information directly, it can usually be extracted from the ticket description or the ticket title afterwards.

The important part is to keep the "what" in the user story short, but accurate.
E.g. the following example is a bit too verbose:
```plain
As a business person
I want the user to pay 0€ for shipping for purchases higher than 50€
So our average conversion rate increases
```

At first glance it can be hard for the reader to deduce how the 0€ and the 50€ are connected.
What are we trying to achieve here?
A better version would be:
```plain
As a business person
I want to offer the customer free shipping
So our average conversion rate increases
```

This is much easier to understand, because we all know the concept of free shipping.
The constraint regarding the 50€ threshold can be ignored here.
The logic will be much more complicated anyway.
Is it 50€ including or excluding tax? 
How do vouchers influence the threshold? 
Does the rule apply for express delivery, too? 
Etc.

As stated before, the user story is meant as a discussion starter.
It is not supposed to give the answers to all questions.


## ✅ Why?

No matter in which way a user story is written, the "why" is most of the time missing or exists only rudimentary.
But this is the most important part of a user story.
As we have already seen in the "who" part above this can massively improve the reader's understanding of the feature request.

Example with a missing "why":
```plain
As a business person
I want to offer the customer free shipping
```

This gives the team a rough idea what we want to do.
But without the "why" we lose guidance information.
We don't know which problem we want to solve with this feature.
It is difficult to ask "good" questions, because the scope is so huge.

Much better:
```plain
As a business person
I want to offer the customer free shipping
So our average conversion rate increases
```

Now, that the scope is massively narrowed down it is much easier to come up with proper questions.
E.g. _"What is our current conversion rate?"_,
_"Can we even measure it?"_,
_"What number would be a success?"_,
_"Will this be a permanent feature or is there a chance that we need to roll back after a few months?"_

The questions are now directly targeted towards the stakeholder.
Without the "why" it is difficult to come up with questions at all, or they go in every possible direction (which the stakeholder will not be able to answer).

Another version could be:
```plain
As a business person
I want to offer the customer free shipping in an A/B test
So we can test our assumption that the average conversion rate increases
```

In this case the team automatically knows that they don't have to create a fully fleshed-out solution.
A prototype or a quick'n'dirty solution might be sufficient to test the stakeholder's hypothesis.
This small change in the user story will also massively change the type of questions coming from the team members.
E.g. _"How do we decide which customers get the free shipping?"_,
_"Can we test two different implementations for the same features with our existing test tools?"_,
_"Will we mess up any of our statistics with this A/B split?"_

The "why" guides the development.
Do we only need a prototype or is this planned as a big scale rollout?
Do we get away with a quick'n'dirty solution or will this be a multi-sprint endeavor?
Again, the most important job of a user story is to get the conversation started.
The "why" helps with pushing it in the right direction.


## ❌ How?

The "how" should definitely not be part of the user story.
The stakeholder can of course add implementation ideas or suggestions to the ticket, but they should not be treated as requirements. 
The stakeholder cannot decide what programming language or framework to use, or if a microservice or a monolithic solution makes more sense here.
They can also not decide how a new database field should be called, or if one is needed at all. 
This is all developer territory and therefore their responsibility.

Bad example:
```plain
As a business person
I want to have a field "user_accepted_all_cookies" in the table "users"
So I know if we can track the user or not
```

This is called "programming by remote control".
The stakeholder micromanages the devs and uses them as puppets to produce code.
This is the worst possible option and an anti-pattern in modern software development.
The devs will stop thinking, the code quality gets worse and the stakeholders will become the bottleneck.

Better:
```plain
As a business person
I want to know if the user accepted the cookies
So I know if we can track the user or not
```

Now, we are completely avoiding the implementation details.
As soon as the devs have understood the request they can come up with a solution on their own -
one that fits into the existing architecture and does not mess up the code base.
The stakeholder's job is only to be able to explain the "what" and the "why".

The "how" is the result of the discovery phase and the dev's refinement sessions.
The "how" might even change while development.
It makes no sense to have a stakeholder design a solution upfront.
The chances that this solution is either bad, produces problems or just doesn't work is too high.
The "how" can only be the end result, not the starting point.


## Conclusion

As a stakeholder a good starting point for a new feature request is the question "Who wants what and why?". 
_How_ this feature can or should be implemented is not relevant at this stage.
The goal is to achieve a shared understanding across the whole team, first.
Then, the team is able to ask good questions, which in turn results in better requirements.
Better requirements lead to fewer misunderstandings, better code quality and better tests.


## Links

* https://www.atlassian.com/agile/project-management/user-stories
* https://www.mountaingoatsoftware.com/agile/user-stories
* https://www.adaptavist.com/blog/how-to-create-an-effective-jira-user-story
* https://www.agilealliance.org/glossary/user-story-template/
