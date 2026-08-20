---
schema_version: "1.0.0"
document_id: "1a3fae4617696bdff1147275502d80d3e986f1ab7abd53105a5952eaf7be419b"
company_key: "yc-okteto"
company: "Okteto"
source_id: "yc-okteto-rss-a64bce3f80ea"
canonical_url: "https://www.okteto.com/blog/preview-environments-importance/"
published_at: "2021-09-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.770894+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:021785c1136c1a355a166bdc5b2cddc17969ba5160299315e872f369848a14de"
---

# Level Up Your Review Process With Preview Environments

# Level Up Your Review Process With Preview Environments


Reviewing code diffs has been the primary and often sole way of reviewing feature changes in software products. Now that more and more teams are using ephemeral environments, it allows for different review patterns. One of the more significant improvements in the review process has been the automated preview environment, one of Okteto's core features!


## What is a preview environment exactly?


A preview environment is a full deployment of your application that is automatically created alongside a pull request. The preview environment deployment is made from the branch being reviewed. Often the deployment of the preview environment uses the same or similar techniques as production environments.


## Traditional code review


During the code review process, you are likely evaluating the change in regards to:


- Code that is appropriately designed for the system
- Code that matches required style guidelines
- Overly complex code that requires simplification
- Behavior that functions as intended
- Tests that validate the expected behavior


If your team can consistently evaluate the code in all these regards, it helps maintain the codebase for a healthy product. However, it may not leave room for receiving the variety of feedback that a team with diverse roles can provide. Executing reviews on code diffs consistently can also be a struggle simply due to human nature and priorities. It is in these scenarios where the addition of a preview environment is valuable.


## Reviewing with a preview environment


The preview environment allows for a more holistic review when paired with traditional code review.


The additional review steps may include:


- Live review of visual changes
- Exercises deployment process before merging
- Manual and automated testing of interactions and dependency chains in a deployed environment
- Performance evaluation from the perspective of a user of your application


### Software interaction


Reviewing user interaction and visual elements is often mentioned as the sole reason for using preview environments. While the review of visual elements is a large benefit of having preview environments, it is not the only way they are helpful. Almost all code has some form of interaction with other code or technology not present in the code diff. Code changes can have a significant effect whenever it is a shared library or central backend service with several consumers. In such cases, deploying an environment, allowing hands-on experience and testing, helps teams engage all users affected by a change.


### Timing of feedback


Reviewing a deployed environment alongside the pull request is a big win! Reviewing a change requiring integration or visual feedback takes longer without a preview environment. The extra time needed is due to the possibility of multiple merges and deployment that it may take to iterate on feedback.


Shortening the feedback cycle helps for faster delivery and preservation of developer focus. A preview environment is created simultaneously with a pull request, so there's no waiting for a merge or staging deployment. You can address and make changes all within the PR utilizing the preview environment, and the flow is much more efficient.


Adding more review opportunities to the pull request ensures that changes get addressed before any merge takes place. When developing, it's inconvenient to re-visit a change after a merge has taken place because there's a sense of completeness when the request is closed. Handling more feedback earlier ensures a greater chance of not going back and making changes after moving on.


Reviewing all of the components at the same point in time allows for addressing most concerns in a single iteration. You aren't necessarily receiving less feedback, but you are receiving it at that critical point in time when you are most focused on reviewing the change.


Preview environment feedback also makes it more likely to get changes in before a merge to a critical branch. There is always some amount of risk involved when merging into a critical branch. The amount of risk depends on various levels of test coverage, the type of change and the overall health of the branch. Getting as much change and feedback done before the critical merge means that there will be less merging and each merge will be safer.


The successful deployment of a branch occurs before merging is an enhancement. How many times have you written your tests, received a critical review, only to find the automation can deploy the changes reviewed. The bare minimum value received from preview environments is eliminating post-merge hiccups due to automation!


### Include more of your team


If you are fortunate enough to work on a broad team with writers, designers, or product managers, the preview environment provides an opportunity for quick, mixed feedback. The entire team is now reviewing changes that include wording, graphics, or user interaction alongside the code diff. This diversification of immediate feedback will make for a more robust product and possibly a more cohesive team.


### Providing opportunities for collaborative ownership


A preview environment allows for a more fluid engagement from *all* team members. When the opportunity to engage is simplified, then a typical result is an increase in ownership.


Being part of the review process is engaging and promotes a sense of responsibility. The ability to engage often facilitates more consistent involvement with a project which leads to better understanding of changes for all involved.


## Preview environments in Okteto


I hope at this point that you have been convinced that preview environments can be a tremendous tool to add to your development and review processes. Now, how can you start using them?


If you are already an Okteto user then preview environments are already[available](https://www.okteto.com/docs/previews) for you! Go ahead and set up a repo and see how it works. We have a[video walkthrough](https://www.okteto.com/videos/how-to-set-up-preview-environments-with-okteto/) as well as[further reading](https://www.okteto.com/blog/preview-environments-for-kubernetes/) on why Kubernetes, in particular, is a good fit for preview environments.


If you aren't yet using Okteto now is a good time to check us out! Preview environments are just one of many ways in which Okteto assists with the development cycle, and it's[free to get started](https://www.okteto.com/signup/) .


If you have any questions about getting started with Okteto or preview environments, feel free to[book a call](https://www.okteto.com/contact) or reach out in our[community forum](https://community.okteto.com/) .


Jacob MacElroy


Engineering / Cyclist 🚴‍♂️


[View all posts](https://www.okteto.com/blog/authors/jacob-macelroy/)


[ephemeral-environments](https://www.okteto.com/blog/tags/ephemeral-environments/)


[preview-environments](https://www.okteto.com/blog/tags/preview-environments/)


[okteto](https://www.okteto.com/blog/tags/okteto/)


[development](https://www.okteto.com/blog/tags/development/)


[best-practices](https://www.okteto.com/blog/tags/best-practices/)


#### Share this:
