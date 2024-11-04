---
title: An Introduction to Peer Reviews
date: 2023-04-13 15:25:11
tags: [Code quality, Reviews]
categories: [Software development]
---

A peer review is both a functional and a code review performed between two or more people (from now on, the “reviewers”).

The developer who completes a user story (from now on, the “creator”) shows his work to other teammates to get feedback about it, before showing it to the final customer.

The **review should**:

- be performed in a similar environment (if not identical) to the production environment. A development environment with the same configuration (but probably fewer performance resources) as the production environment.
- check that all the requirements that the user story has have been met (and even situations not specified initially).
- check that edge cases are covered (the help of people not involved in the user story development helps at this point, adding a fresh view)
- check the code complies with the quality standards and the coding style of the project/company.

But **the review is not**:

- an exam (there will probably always be something to improve, just deal with it).
- an evaluation of a teammate (we are reviewing the code and the functionality a developer did, assuming he did his best, so we are not doubting the professionalism of the developer).

## Goals of the review

Peer reviews help development teams in some ways.

They improve the quality of the functionality we deliver to our customers. The fresh view of other people will ensure we deliver a more stable product.

They help ensure that the code base is coherent through time. Of course, linters will help you at this point, but there are places they do not reach.

The peer review sessions help share development knowledge between the teammates. In fact, the help is in both directions: people reviewing can learn from the work done by the reviewed one, and vice-versa, the user being reviewed will get new ideas for improving the code and, therefore, his knowledge about software development.

A session like this, in which functionality and source code are shown, spreads product and project knowledge. The more people who are aware of the stories being developed, and how they are thought of and implemented, the fewer dependencies on single individuals, which removes knowledge silos and protects the project from people leaving the company.

## And pull requests?

I know what you are thinking:

> — Doesn’t a pull request solve the code review part?
> — Yes and no.

Peer reviews have **the following advantages**:

- They offer more context. The creator will explain the decisions taken and the why, which is not always clear with a pull request.
- A call or even a meeting in a room makes easier communication: seeing the face of the person who developed the code helps reviewers to empathize with him, and the creator will feel criticism more constructive than reading it in a Github pull request, for example.

Peer reviews don’t replace pull requests. Simply put, a pull request may allow some checks, but a deep review should be done here.

But peer reviews are not perfect. **They are not asynchronous**, which makes it complicated for teams that are throughout the world and in different time zones. They require that all people involved are at the same time in the same meeting, with the development environment available for making the review (which probably requires synchronization with other reviews that may occur at the same time).

## Some bits of advice

If you are the reviewer:

- Please, show respect to the creator. He has done his best and has put a lot of effort into what you are seeing.
- Remember that the goal is to improve the value delivered to customers, not show that you know more than him.
- Be open to learning new things from what you see in the review.
- Explain and justify your opinion. Having ’N’ years of experience is not an argument (it has never been).
- Review the code, not the developer. It should be obvious but don’t judge the person reviewed.

If you are being reviewed (not you, your user story):

- Prepare the review beforehand, to make the review the fastest possible.
- Have it clear what you want to show and how.
- Write down the pieces of code in which you especially want to receive feedback.
- If the functional review requires some data, please ensure that it is added before the review.
- Be open to comments and suggestions. You are not being judged personally, it’s the code that is being reviewed.
- Don’t take the reviewers’ opinions as a final truth. Try to understand their opinions and, if you don’t understand them, ask for clarification or examples. If you feel that you still miss something, please ask for resources to improve your knowledge. Also, defend your position with arguments (if you guess that some aspect of the review will require clarification, please prepare the arguments beforehand).
- Don’t be afraid to be reviewed by people with less experience (even juniors). New points of view always help, and you just don’t know who will come up with the great idea that will make the day.
- Be open to learning, but also to teaching. Surely your user story has something to teach the rest of the team (about the project, about software development…).

## Who should be involved in a peer review?

Ideally, anyone on the same team is a candidate for the peer review, but we need to take into account several aspects.

People in the review should have the context of the application. Making a review with people who have just arrived at the project may not be a good idea, as the user story may be perfect by itself, but break the coherence with other parts of the application. It is interesting to involve people, when possible, who know the application in detail, but… on the other hand, it would be great to also have people who are not involved directly in the project. A fresh view and a different set of opinions may add a lot of value to the review. As you see, finding equilibrium is not trivial.

Try to make a mixture of different levels of experience in the review. This will avoid making a review between people who have just started to code, which could result in starting with not the best practices.

Two people (the creator and a reviewer) may not always be enough. Involve a third person if possible. If one of the reviewers has just started the project, add a third person with more experience, and vice-versa, if the review is between experienced people, involve a person with less experience, to disseminate knowledge.

## Conclusions

Peer reviews are a great tool to guarantee, both the quality of the functionality we deliver to our customers and the quality and coherence of the code we add to our codebase.

They also help to share the knowledge (about the application and software development) between the members of the team (and if you involve people from other teams, between teams).

Finally, they augment the collectivity sense of the code.
