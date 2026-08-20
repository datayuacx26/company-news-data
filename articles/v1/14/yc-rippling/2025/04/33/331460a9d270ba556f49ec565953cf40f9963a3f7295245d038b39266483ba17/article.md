---
schema_version: "1.0.0"
document_id: "331460a9d270ba556f49ec565953cf40f9963a3f7295245d038b39266483ba17"
company_key: "yc-rippling"
company: "Rippling"
source_id: "yc-rippling-news-import-758faf74b02a"
canonical_url: "https://www.rippling.com/blog/boosting-workflow-velocity-gitspice"
published_at: "2025-04-30T00:00:00+00:00"
first_seen_at: "2026-07-22T11:59:36.706542+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:e11b13a384bd9d85bba8192e20d20d79ed3e84121f65754a0c694303d09032ef"
---

# Boosting Development Workflows with git-spice

For Rippling’s Go Experience team lead[Abhinav Gupta](https://www.linkedin.com/in/abhgup/) — three things proved to be true. One, large pull requests (PRs) were hard to review. Two, making one PR at a time slowed him down. And three, stacking PRs manually was not effective enough. With no available open-source offerings fitting his needs, he took the problem in his own hands.


Read Abhinav’s take on building git-spice and how it’s helped him and his team keep up the speed at Rippling.


---


## Problem 1: Large PRs are hard to review


It is sometimes difficult for large pull requests to get meaningful code reviews. Consider this scenario: ** In implementing a new feature, the author discovered an opportunity to[DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) up related code, so they took advantage of it with a refactor. While refactoring, they also found and fixed a bug. They post the resulting pull request for review, and it looks like this.


The reviewer opens the PR, is overwhelmed by the diff—there's just too much noise—so they skim it and press approve.


### *What went wrong*


Pull requests like this are a burden to review. Most reviewers faced with one will scroll through the diff and stamp it without looking at anything too closely. Those with stronger stomachs will dive in anyway, but find themselves sinking hours into it. This throws away one of the most effective tools we have as software engineers to share knowledge with our peers and ensure quality in our output. Beyond that, when something inevitably goes wrong after the PR is merged, it can be difficult to revert the offending change without also reverting other bundled changes.


## Solution 1: Make atomic PRs


It's not the size of the PR that's the problem, but how many things it does. Pull requests that do too many things— *non-atomic PRs* —are difficult to review. The overlapping changes create noise in the PR, making it easy for a reviewer to miss something important.


For the previous example, suppose the author instead created three separate PRs:


1.


Fix the bug first and include a regression test. The reviewer will look closely at the bug, reviewing the changes line by line.


2.


Refactor without changing behavior. The reviewer will be able to consider the architectural implications of the refactor without concerning themselves with each of the hundreds of lines being moved.


3.


Finally, add the new feature. The reviewer will have space to look at the feature by itself.


These would make more efficient use of the code review process. They would allow the reviewer to switch between micro- and macro-level analysis based on the kind of change in that PR. Therefore, to get meaningful code reviews, **pull requests must only do one thing each** .


## Problem 2: Atomic PRs slow me down


Submitting atomic PRs one at a time can negatively impact development velocity.


The author submits the PR, gets it reviewed, then they work on the next one, submit it, get it reviewed, and so on.


Consider the above scenario again: the author wants to fix a bug, refactor some code, and implement a new feature, and each change depends on the prior. If they work on these one at a time, the timeline for getting all this work reviewed might look similar to the following.


*Note, in this example, the refactor change required a follow-up change and review.*


The author can submit the next atomic pull request only after the previous one is reviewed and merged. If any pull request requires multiple review passes, it further delays the next one.


## Solution 2: Introduce pipelining


What if the author is able to submit PRs as soon as the corresponding change is ready, even if the work it depends on isn't merged yet? This would significantly collapse the timeline above, making more optimal use of the author and reviewer's time.


The author will submit pull requests as soon as they're ready, and spend less time waiting for reviews as they'll be working on the next change in parallel. The reviewer will spend less time waiting for changes, finding PRs in their queue when they finish reviewing another.


**Pull request stacking** is the practice of submitting pull requests against other open pull requests in order to build upon the unmerged work in them. It facilitates exactly such a workflow.


## Problem 3: Stacking pull requests is hard


Stacking pull requests manually is possible, but difficult and cumbersome. As a quick example, given a stack of branches locally, the[GitHub CLI](https://cli.github.com/) can be used to create stacked PRs.


However, anytime a change is made to one of these branches, all branches based on it must be rebased and re-submitted. For example, to update bugfix, the following commands are required.


Doing all of this manually severely limits the value of incorporating PR stacking into a developer's workflow.


I experimented with a few other tools and workflows to aid here, including[Stacked Git](https://stacked-git.github.io/) ,[git-branchless](https://github.com/arxanas/git-branchless) ,[ghstack](https://github.com/ezyang/ghstack) ,[spr](https://github.com/ejoffe/spr) , and[restack](https://github.com/abhinav/restack) , but none of the available open-source offerings quite fit my needs. I didn't want something this integral to my workflow to be closed-source or commercial.


## Solution 3: Build my own


Therefore, I decided to build git-spice ([https://abhinav.github.io/git-spice/](https://abhinav.github.io/git-spice/) ).


It facilitates a PR stacking workflow without being intrusive. It provides commands to manage and navigate a local stack of branches, submit and update pull requests from them, and handles the bulk of the rebasing overhead for you.


For example, to update the bugfix branch above with git-spice would take the form:


This post won't go into detail on how to use git-spice. Check out[git-spice's Getting Started](https://abhinav.github.io/git-spice/start/) to play with it.


## **How I Did It**


The initial public version of git-spice was crafted over a few months of evenings and weekends. It's not uncommon for me to put pet projects on pause after a few weeks. However, with git-spice, I was highly motivated to get it to a stable version—I really needed this itch scratched.


The first decision I had to make was which language I would write it in. It was tempting to use the project as an excuse to learn another language, but the fact remained that I wanted to use this tool as soon as possible. Therefore, to get it done sooner, I chose **my most productive language** : Go.


One of the most valuable things I did during its early development was to set up an **integration test harness** . Taking advantage of the[testscript package](https://pkg.go.dev/github.com/rogpeppe/go-internal/testscript) , the harness defines test cases as "test scripts," written in a shell-like language with access to a real file system, Git, and an almost-real git-spice binary. These tests are therefore able to verify end-to-end flows against fresh Git repositories. Anytime I added a feature or fixed a bug, I also added a test script. As of writing this, the project has[170 test scripts](https://github.com/abhinav/git-spice/tree/b970e7e921ba5e987738737e25b492a99e4d47c3/testdata/script) . This significantly increased my velocity during early development by ensuring things kept working without the burden of writing unit tests. At that stage of development, these were **more valuable than unit tests** because with the constant refactors, the units that would be tested were not well-defined. If I had relied more on unit tests, I would've had to constantly rewrite the unit tests as part of refactoring.


Testing also forced me to make architectural decisions that paid for themselves in spades. For example, the test harness ran git-spice from "outside"—as just another executable. These tests needed to test GitHub interactions without actually sending requests to GitHub. This forced me to **isolate external communications behind an abstraction** . Tests would provide a different implementation of this abstraction: one that talked to a local GitHub-like test server. This choice paid for itself soon after, when I had to switch to using GitHub's GraphQL API instead of REST: I could do so without changing the command implementations or tests, as it's still the same interface. This design further paid for itself after git-spice became publicly available, because an external contributor was able to implement **GitLab support** behind the **same abstraction** : *no changes to business logic* .


Lastly, two actions helped polish the usage experience and iron out early bugs: **dogfooding** and a **closed beta** . As soon as there was meaningful functionality, I started using git-spice to develop itself. Local stack management was implemented first, so after implementing the[commit create command](https://abhinav.github.io/git-spice/cli/reference/#gs-commit-create) , I started using that for committing. When I added support to create pull requests, I began using that right away. When the end-to-end functionality was reliable enough, I tagged an[alpha release](https://github.com/abhinav/git-spice/releases/tag/v0.1.0-alpha1) and began using it at work. A few alpha releases into development, I shared the project with friends who had been, until now, receiving development updates in a group chat. They began using it as well. All of this helped find a number of bugs and annoyances. Exposure to other eyes found blind spots in the UX and design that I didn't encounter as part of my own development flow.


Once I had implemented everything in my[public release milestone](https://github.com/abhinav/git-spice/milestone/1) , I published the[first stable release](https://github.com/abhinav/git-spice/releases/tag/v0.1.0) on July 21, 2024. At Rippling, a day after the public release, I posted a short message in our PR stacking enthusiasts Slack channel, and watched usage grow from there.


## Conclusion


Pull requests that do too many things hurt development velocity and are hard to review. Atomic pull requests are easy to review, but they reduce development velocity further. Atomic pull requests can be stacked on top of each other to buy back that lost development velocity—at the cost of a difficult development workflow. **git-spice** strikes a balance here by giving us all three: development velocity and reviewability with an easy-to-use workflow.


With git-spice or similar tooling, a single engineer or an entire team can reach a point where breaking complex changes into easy-to-review increments becomes a habit, because the overhead of managing the stack is nearly non-existent. This, in turn, makes better use of the code review process, allowing developers to teach and learn during reviews, and helps them produce output at an overall higher quality.


If you're curious about git-spice, check out:[https://abhinav.github.io/git-spice/start/](https://abhinav.github.io/git-spice/start/) .


Schedule a demo with Rippling today


[See Rippling](https://www.rippling.com/request-demo)


Disclaimer


Rippling and its affiliates do not provide tax, accounting, or legal advice. This material has been prepared for informational purposes only, and is not intended to provide or be relied on for tax, accounting, or legal advice. You should consult your own tax, accounting, and legal advisors before engaging in any related activities or transactions.
