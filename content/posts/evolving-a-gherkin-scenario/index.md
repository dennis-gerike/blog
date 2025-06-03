---
title: Evolving a Gherkin scenario
description: Being either too abstract or too concrete, both can make it harder to understand Gherkin scenarios. This article shows how they can be evolved to find the right balance.
tags: [ "Gherkin", "Gherkin Scenario", "BDD", "Magic Numbers" ]
date: 2025-06-03
---

Even a well written Gherkin scenario can still be improved. 
Have a look at the following example.
It describes a customer who wants to buy alcohol in an online shop.

```Gherkin
Given the customer is an adult
When the customer adds a bottle of whisky to the cart
Then the checkout process should allow the customer to buy the whisky
```

✅ The scenario is short. 
Seb Rose and Gaspar Nagy recommend five steps or fewer. [[1]](#resources)

✅ It is easy to read and to understand, therefore supporting the _shared understanding_ aspect of BDD (Behavior-driven Development). [[2]](#resources) 

✅ It describes the intended behavior, not the (incidental) details - a vital element to "good" Gherkins.
For example, it does not describe how the whisky is added to the cart or how the checkout process is initiated. [[3]](#resources)

✅ It doesn't mix different rules or acceptance criteria.
Everything that is not essential to the scenario is ignored.
Things like availability (`Given the article is in stock`) or address information (`Given the user has provided a valid address`) are important aspects, but not here.
This supports the "focus" rule in the BRIEF pattern. [[4]](#resources)

So, objectively this is already a good Gherkin scenario.
It checks many boxes.
There is nothing obviously wrong with it.
But, we can still do better.

<!--more-->

## Round 1

There is one issue with this scenario that could cause some trouble along the development process. 
In most countries `adult` means 18 years and older, but not everywhere. 

Let's assume the programmer comes from Germany. 
Here, the age limit is indeed 18 years.
Therefore, she will intuitively implement that number as the minimum age. 
When the QA person starts testing he will be confused, because his 19-year-old test user can successfully buy the liquor.
In his world that should not be allowed.
As a Japanese he expects 20 years to be the legal limit. 
When discussing the problem with the team it becomes clear that both assumptions were wrong. 
Because it is a Canadian online shop and the product owner is also from Canada, adult "obviously" means 19 years. 

Granted, this is a contrived situation. 
In the plannings, refinements or other meetings this question would have popped up (hopefully).
But nonetheless, this insight needs to be addressed in the Gherkin scenario somehow.

In this specific case the better solution would have been to make the age limit explicit.

```Gherkin
Given the customer is at least 19 years old
When the customer adds a bottle of whisky to the cart
Then the checkout process should allow the customer to buy the whisky
```

The benefit is that everyone involved now works with the same number. 
The developer implements the 19. 
The tester tests the 19. 
The product owner communicates the 19 to the stakeholders, etc. 

Important to note here, changing the Gherkin sentence did not change the context.
The scenario still describes the same business logic.
Nothing more, nothing less, just a bit different.

## Round 2

But we have a new problem now.
With the `19` we introduced a `magic number`. 
When a new colleague joins the team and sees this scenario, they might not understand why there is a 19.
Do I need to be 19 to use the shop?
Do I need to be 19 to buy the alcohol?
What happens if I am younger than 19?

So, on the one hand we don't want to have information that is too `abstract`.
On the other hand having `concrete` numbers is also bad.
What to do?
We somehow need both information.

```Gherkin
Given the customer is of legal drinking age (at least 19 years old)
When the customer adds a bottle of whisky to the cart
Then the checkout process should allow the customer to buy the whisky
```

This gives a concrete example and also helps the reader to understand the reason behind this magic number.

## Round 3

The intention is good, but now the first sentence gets a bit convoluted.
The previous version was easier to read.

Besides, a Gherkin scenario is meant to act as an example for a business rule. 
By including `legal drinking age` we added contextual information. 
Yes, we also want to communicate the intent, but directly within the Gherkin sentences is not the right place.
A better location for that would be the `scenario title`.

```Gherkin
Scenario: Alcohol can be bought when the customer is of legal drinking age
  Given the customer is at least 19 years old
  When the customer adds a bottle of whisky to the cart
  Then the checkout process should allow the customer to buy the whisky
```

Switching back to the Gherkin sentence with the _magic number_ works now, because the `magic` is explained directly one line above.

Why is this better?
When using _Xray_ or _Zephyr_ or any other test management tool the scenario title will be seen in every Gherkin listing.
It will appear in the test repository, the test plans, the test executions, and so on.
The user stories will have a section showing all attached scenarios.
And when searching for tickets they will show up, too.

In general, the reader is not interested in the details when glancing over those lists.
They either want to get an overview, or they want to find a specific scenario (e.g. everything related to age-restricted items in the shop).
The reader cannot be expected to open each scenario to find the relevant information.
Putting the context information in the title makes it much easier.

## Round 4

This iteration already looks good. 
But, as a tester we might be tempted to go one step further.
We might want to test for multiple cases, like `18`, `19` and `20` year old customers. 

```Gherkin
Scenario: Alcohol can be bought when the customer is of legal drinking age
  
  Given the customer is <age> years old
  When the customer adds a bottle of whisky to the cart
  Then the checkout process should <allow?> the customer to buy the whisky

  Examples:
    | age | allow?   |
    | 18  | disallow |
    | 19  | allow    |
    | 20  | allow    |
```

Using "boundary value analysis" is a good testing technique, but Gherkin scenarios are _not_ tests.
They are meant to describe the system's _behavior_, the business logic. 
That we can use them afterward to create automated tests is just a nice side effect. 
A better place for those test cases are the developer tests or the manual tests.

Regardless, this gives us an idea for the final iteration.

## Final Round

Instead of using an example table the better approach is to split the scenario up into `happy path` and `unhappy path`.

---
```Gherkin
Scenario: Alcohol can be bought when the customer is of legal drinking age
  Given the customer is at least 19 years old
  When the customer adds a bottle of whisky to the cart
  Then the checkout process should allow the customer to buy the whisky
```
---
```Gherkin
Scenario: Alcohol CANNOT be bought when the customer is not of legal drinking age
  Given the customer is younger than 19 years
  When the customer adds a bottle of whisky to the cart
  Then the checkout process should NOT allow the customer to buy the whisky
```
---

At first glance this seems a bit redundant (90% is identical), but this design has a few advantages.
It answers the question from above: _"What happens if I am younger than 19?"_.
Also, this is still a `specification` and did not accidentally turn into a `test`.
And we still have the separation of concrete data in the Gherkin and the explaining information in the title.

Another advantage of the split is the gained flexibility to describe both cases individually.
It is just a minor difference, but the first Given-sentence is formulated slightly different than its counterpart - `...is at least...` versus `...is younger than...`.
When going back to round 4, we had to squeeze both cases into one sentence: `...is <age> years old...`.
Granted, that was an easy one. 
But there are often situations where describing both cases in the same sentence become difficult.
Or the flow changes so much that it is just not possible.
 
## Conclusion

So, what is the takeaway? 

**tl;dr** When using `magic numbers` put the concrete values in the Gherkin sentences, but give context information in the scenario title.
Both information is needed.
Doing it the other way around (or omitting one of them) will make it harder for the reader and might provoke misunderstandings.

A very similar example would be the VAT (value-added tax). 

```Gherkin
When the customer adds a bottle of whisky to the cart
Then the cart should apply the correct tax rate to each article
```

This is good _starting point_ for a Gherkin scenario, but it has the same issues as discussed throughout this article.
Only this time it is not the `Given`, but the `Then` sentence that is too abstract.

## Resources

* [1] Seb Rose and Gaspar Nagy, 2021, _Formulation: Document examples with Given/When/Then_
* [2] Behaviour-Driven Development, https://cucumber.io/docs/bdd/
* [3] Writing better Gherkin, https://cucumber.io/docs/bdd/better-gherkin/
* [4] Keep your scenarios BRIEF, 2019-09-05, Seb Rose, https://cucumber.io/blog/bdd/keep-your-scenarios-brief/
