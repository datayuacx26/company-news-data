---
schema_version: "1.0.0"
document_id: "74863724359a3c1f7533e406ba5b1305d85d2ef88cbd25a937aa17b15bdba0db"
company_key: "yc-blacksmith"
company: "Blacksmith"
source_id: "yc-blacksmith-news-import-a006191a9a76"
canonical_url: "https://www.blacksmith.sh/blog/stacked-diffs-can-be-expensive-why-you-should-do-it-more"
published_at: null
first_seen_at: "2026-07-23T03:46:48.319296+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:5722f265cec8489ee142ecade78e071c6fa9bae20bbe490cd8196710e7565dd3"
---

# Stacked Diffs can be expensive: Why you should do it more

There has been a recent uptick in online discourse about “stacking”. Some folks have been doing it since the dawn of time with nothing but git and their bare hands. Others are learning about it through newer dev tool startups like[Graphite](https://graphite.dev/) , that are focussed on making it the de-facto development workflow. Everyone, however, seems to be in general agreement that stacking is the way forward, so let us unpack why.


## What are Stacked Diffs?


Stacking is a git workflow where you break up large code changes into several, smaller pull requests that build on top of each other. Tinier building blocks mean that the changes can be tested, reviewed and merged in isolation, without blocking the development of dependant features. Developers can continuously work on their feature branch while chaining small, dependent changes known as stacked diffs. Here’s a simple visualization that should convince you why this is more efficient for developers.


Engineers at Facebook and Uber have been developing like this forever. Newer companies, like Cockroach Labs, are also enforcing similar best practices to maintain a high level of code quality. Dare I say it is becoming increasingly uncool to push out a 1000 line patch, as good as it feels.


## Benefits of Stacked Diffs


Not to belabour the point but the main advantages I have felt are:


- **Less context switching:** Once I submit a stacked diff I am able to immediately move on to the next one without worrying about the chain forming behind me. As and when the diffs are reviewed I can address the comments lower down the stack and then “re-stack” all the dependant diffs above it (more about this later). It may not seem much, but the forward momentum in such a workflow is invaluable once you give it a shot.
- **More thorough reviews:** Time and again I have noticed that folks (including me) are less likely to throughly review a pull request if it touches 17 files and hundreds of lines of code. Stacking obviates this problem by allowing reviewers to leave comments at every stage of the feature. Less bugs slip through the cracks and you’re not faced with refactoring your entire feature because of an opinionated comment!
- **Every commit is green:** Every stacked diff runs CI, and so you have to ensure it builds and passes all tests. This is my personal favourite because it makes future[git bisections](https://git-scm.com/docs/git-bisect) to find a flaky test or a performance regression infinitely easier.


## Stacking with git


Companies like Graphite and Aviator have made stacking a pleasant experience, and are building an entire ecosystem around this workflow. As a git purist I was reluctant to switchover for a long time, so let’s take a look at what it takes to stack in git.


For this example let’s say we want to:


- Write some endpoints for our backend service
- Add tracing to each endpoint
- Add metrics to each endpoint


A typical workflow would look like this


```text
// Create and switch to the base branch for endpoints
git checkout   -  b endpoints


// Write your endpoints
git commit   -  am   "routes: add backend endpoints"
git push origin endpoints


```


Let us stack two more branches on this base branch


```text
// Create and switch to a child branch for adding traces
git checkout -b traces


// Add tracing to endpoints
git commit -am   "routes: add tracing to endpoints"
git push origin traces


// Create and switch to a branch for adding metrics
git checkout -b metrics


// Add metrics to endpoints
git commit -am   "routes: add metrics to endpoints"
git push origin metrics
```


As is often the case, a review will ask you to rename your endpoints. This will mean updating the base branch` endpoints` leaving the` traces` and` metrics` branches on the older version of` endpoints` . This is where you will have to reach for` git rebase` to update all your upstream branches with the new base code.


```text
git checkout endpoints


// Address comments on parent branch
git commit -am   "routes: change /health endpoint to /up"
git push origin endpoints


// Manually rebase child branches on updated parent
git checkout traces
git rebase traces


// Rebase the second child and continue doing so up the stack
git checkout metrics
git rebase metrics
```


While this doesn’t seem terrible, in a real world scenario you will likely have longer chains of branches and your rebases will run into merge conflicts. The thought of having to manually rebase all your dependant branches as you address review comments lower in the stack, is a big deterrent for effectively using stacking.


## Stacking with Graphite


With a tool like Graphite the same workflow looks like this:


```text
// Start working on the branch you are currently on egs: main.
// This command will create a new branch with your changes off of main.
gt create -am   "routes: add backend endpoints"


// Add traces to endpoints.
// This command will create a child branch with the changes.
gt create -am   "routes: add tracing to endpoints"


// Add metrics to endpoints.
// This command will create a child branch with the changes.
gt create -am   "routes: add metrics to endpoints"


// Submit all 3 stacked diffs as independent PRs.
gt submit --stack
```


Now, when you have to rename certain endpoints in your first diff you would:


```text
// Checkout the stacked diff.
gt checkout pp--  06  -  14  -part_1


// Make changes to rename the endpoints.
// Restack all upstream branches with the changes to the endpoint names.
gt modify -a


// Push the updates to the respective pull requests.
gt submit
```


As you can see there is no manual iteration through all upstream changes to run` git rebase` .` gt modify` handles all the re-stacking automagically. When there are conflicts, Graphite will open up a familiar conflict resolution editor and prompt you on the CLI on how to resolve them in order to continue with the re-stacking. The automatic propagation of changes across the stack is one of the biggest reasons to give Graphite a try.


Compared to git, tools like Graphite have much more advanced coordination and state management across dependant branches. This allows them to offer commands such as` gt stack view` to visualize your branches and diffs or` gt sync` to pull an updated version of main and re-stack all your un-merged diffs. If you’re reading this and thinking that you don’t need these bells and whistles, I feel you, I was skeptical too; but once it becomes part of your workflow it is very hard to go back.


## Stacking can be expensive


At this point you’re probably convinced you should be stacking. One lesser spoken about consequence of stacking is that you will burn more minutes (and money) running CI. Many smaller pull requests, naturally means that there will be more CI runs. The gotcha is that when you re-stack or rebase a set of changes, all the upstream changes are going to be updated and will rerun CI even though there has been no logical change in the diffs that are up the stack. Here’s a graph of one of our customer’s GitHub Action spend as soon as they started using stacked diffs


The last thing you want is to deter your engineers from stacking because of these growing costs. There are a couple of solutions I have come across so far:


- **Graphite CI Optimizer:** With Graphite you can configure your GitHub Action workflows to selectively run on some[portion of your stack](https://graphite.dev/docs/stacking-and-ci#reducing-ci-runs-for-github-actions) . Your changes are merged from the bottom up and so you can configure your CI to only run on the base of the stack every time your re-stack. Graphite also offers per workflow configurations to run quicker CI checks on each stack, but the more expensive e2e ones just prior to merge.


- **Use Blacksmith:** With Blacksmith, your GitHub Actions are running up to twice as fast and at half the cost. So you can effectively double the number of CI runs without asking for an increased CI budget from finance. Integrating with Blacksmith is a one-line code change, and one that you should do as soon as you start encouraging stacking in your organization!


Reach out to us if you have any questions athello@blacksmith.sh , happy stacking!
