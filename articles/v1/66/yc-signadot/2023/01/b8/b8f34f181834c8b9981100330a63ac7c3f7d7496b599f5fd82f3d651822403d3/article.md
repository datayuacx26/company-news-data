---
schema_version: "1.0.0"
document_id: "b8f34f181834c8b9981100330a63ac7c3f7d7496b599f5fd82f3d651822403d3"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/new-paradigm-for-end-to-end-feature-testing-in-microservices/"
published_at: "2023-01-25T11:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:7d588e27f1edbfea387e487d302c6a496aa46a18cafb40062f405111be13ea46"
---

# Introducing a New Paradigm for End-to-End Feature Testing In Microservices

Until recently, Signadot’s testing platform has focused largely on automated integration testing in Kubernetes and DevOps UX. We’re now launching capability to support “Feature Testing” to better meet the End-to-End testing needs of app devs.


This new feature testing capability can be used to test any changes to one or more microservices end to end. It can be used by a single developer working on a feature or across teams and developers working on different microservices. Developers can use the Signadot platform to test features end to end, as well as preview those features with stakeholders early in the development lifecycle. This capability also allows multiple developers to effectively collaborate and test product features end-to-end that touch multiple services.


Here, I’ll describe how this new testing capability came about and explain why it promises to be a powerful new feature testing paradigm.


## Where We Started


Signadot is a Kubernetes-based platform that enables testing in a production-like environment much earlier in the development lifecycle. Teams building microservices-based applications can quickly spin up[lightweight, ephemeral environments called Sandboxes](https://www.signadot.com/docs/concepts/sandbox) .


Signadot supports a large number of Sandboxes on a single Kubernetes cluster and scales to hundreds of developers and services. Benefits include the ability to test in isolation before merging code without impacting other developers, test without having to maintain mocks of dependent services, and test against the latest version of all services. Fast feedback loops give developers the information they need to iterate quickly.


As mentioned, our initial focus was on integration testing and automation. The Signadot solution scales testing by creating one environment for every pull request and enabling integration testing in a production-like environment.


Over time, we heard from developers seeking a developer-centric testing capability for new feature development. And that got us thinking.


## What We Heard from Developers


Developers have traditionally performed parallel feature testing by time sharing staging environments or cloning new environments for every feature. As we have discussed, time sharing an environment creates a bottleneck and creating a new environment for every feature is expensive. Each environment can cost up to tens of thousands of dollars per month. In addition, each environment could take 20-30 minutes to spin up.


We’ve received a lot of input from developers seeking a sandbox solution for end-to-end, collaborative feature development and testing. Three requests in particular rose to the fore.


### Developers Want to Test Features from the Front End


First, developers want to be able to test new features from the front end no matter where they’re making a change. While they want to test end to end before merging code, they don’t currently have access to a real environment before a code merge. Instead, they must wait for slow CI/CD pipelines to run, after which time code is deployed to a staging environment. This testing occurs too late in the game, and it’s slowing rapid development and iteration.


While developers aren’t always excited about writing automated tests, they are excited about the possibility of testing new features holistically in a real environment and quickly shipping those features to production.


### Developers Want Fast Feedback


Second, developers want fast feedback. Let’s say one developer (or team) works on one set of microservices and another developer (or team) works on a different set of microservices. Developers want to collaborate with each other on feature development across API boundaries much quicker and much earlier in the development lifecycle.


This iterative collaboration model is especially useful when dependencies exist between services. For example, when one service is consuming the API of another service, ongoing testing helps determine if API or other changes are needed. The more microservices that a team depends on and the more API dependencies that exist, the more complex the testing process. Waiting to perform testing in the staging environment slows this process considerably. The faster the feedback, the better.


### Developers Want to Share Previews with Stakeholders


Finally, developers want to be able to share early previews of their work with product managers, QA teams, UX and UI designers, and other stakeholders to gain critical feedback early and often.


## Back to the Drawing Board


We took all this valuable developer feedback to heart and went back to the drawing board. Our goal was to expand our focus and emphasize feature testing with the developer experience in mind, enabling them to quickly and easily test new features in a real environment and test much earlier — before a code merge.


Using Signadot’s existing capability set, DevOps can set up a single sandbox that is deployed from a single pull request. Developers, then, can perform feature testing by determining which sandboxes need to be part of the feature and linking the appropriate sandboxes together through sandbox label matching.


Essentially, each sandbox has a set of labels associated with it. Each label is usually associated with a particular feature name (e.g auth-mfa). Developers specify the label values when the sandbox is created. A GitHub pull request, in turn, can have a set of labels, and developers can match GitHub pull request labels including branch names to the sandbox labels.


Developers can then create a route group that links different sandboxes together. By using dynamic binding, sandboxes and route groups can be created independently and then dynamically matched at any time.


The last thing developers need to specify in the route group is the endpoint that can be used to test the feature. An endpoint is typically a frontend service like a web UI or a frontend API through which testing occurs. Once the route group is specified, this endpoint can be used to test that feature from end to end. Signadot takes care of the routing configuration for requests to go through all the sandboxes contained in the route group.


## End-to-End Feature Testing Is a Game Changer for Developers


By expanding our initial integration testing and automation focus, we are now able to better address the needs — and specific requests — of app developers. This powerful, developer-focused feature testing paradigm will enable engineering teams to collaborate better, give and receive fast feedback, and move faster and more efficiently.


Signadot Sandboxes support new feature testing at a fraction of the cost of cloned environments, and they spin up in seconds. Signadot’s new feature testing capability supports fast iteration throughout the development lifecycle for the following use cases:


- Performing pre-merge, end-to-end testing in a real environment
- Testing backend changes from the front end
- Collaborating with other developers across microservices API boundaries
- Enabling stakeholders to experience and weigh in on features much earlier in the development lifecycle.


[Sign up for free](https://www.signadot.com/pricing/) and give us your feedback! You can also check out our[Guide on Feature Testing](https://www.signadot.com/docs/guides/set-up-pr-sandboxes) on our website and join our[Slack channel](https://signadotcommunity.slack.com/join/shared_invite/zt-1estxm8pv-qfiaNfiFFCaW~eUlXsVoEQ#/shared-invite/email) for faster communication and collaboration.


‍
