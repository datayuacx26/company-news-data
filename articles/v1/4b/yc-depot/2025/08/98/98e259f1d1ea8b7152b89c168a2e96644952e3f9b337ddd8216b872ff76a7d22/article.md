---
schema_version: "1.0.0"
document_id: "98e259f1d1ea8b7152b89c168a2e96644952e3f9b337ddd8216b872ff76a7d22"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/monorepos-worth-the-hype"
published_at: "2025-08-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:4278614f347391c8b86fce0cda75dfafb844e4132415b183c27bcfe7571bd8e9"
---

# Monorepos: Worth the hype?

## The monorepo


A monorepo is a version control strategy where all of an organization’s code lives in a single repository. Instead of maintaining separate repositories for each service, everything from backend APIs to frontend applications to shared utilities all exists in one Git repository. Google stores billions of lines of code in a single repository. Meta follows the same approach. Microsoft uses one for Windows development.


### The traditional approach: polyrepos


The traditional approach is, you guessed it, the polyrepo approach. Each service, library, or application gets its own repository. Your API server has one repository, your web frontend another, your mobile application a third. Each repository maintains its own CI/CD pipelines, dependency management, and release cycles. This is how most organizations start and what GitHub’s organizational model naturally encourages. Personally, this is how I was taught to approach software development.


### So, what’s different about a monorepo?


Monorepos are interesting in that you store **all** the code for a project in a single repository. This means your 500k line backend service sits next to your React frontend, your mobile app, your CLI tools. In a polyrepo setup, each of these would have its own commit history, contributors, and CI config. Want to see what changed across your entire system last week? That’s five separate` git log` commands, five different pull request pages. In a monorepo, it’s one linear history. You can trace a feature from the API change that started it to the frontend that displays it to the mobile app that consumes it, all in adjacent commits.


### Why it matters


The fundamental difference is the boundaries between services. In a polyrepo world, repository walls enforce architectural decisions. You can’t just import another service’s internal code, you go through its published API. That friction has benefits, it forces you to think about interfaces and contracts. Monorepos remove that friction entirely. Need that validation function from the billing service? Just import it. Want to reuse that database model from the user service? It’s right there. This makes development faster, but it also makes it easier to create the kind of tangled dependencies that turn codebases into unmaintainable nightmares.


## Worth the hype?


### Advantages of the monorepo


The real problems with polyrepos arise when you try to make changes across multiple services or libraries. Let’s say, for the sake of argument, you’re[building an egress filter for GHA runners](https://depot.dev/blog/now-available-egress-filtering-for-github-actions-runners) . You’d need to make changes to the software on the runner, to the API the runner queries to know the filter state, and to the frontend app that configures the filter. In a polyrepo world, you’d have to make three separate PRs, one for each service, and ensure that they’re merged and deployed in the right order. If not, you could end up with a broken system. With a monorepo, you can make all the changes in one PR, ensuring that everything is in sync and deployed together.


It also makes it significantly easier to follow the development of a new feature. Instead of reviewing several PRs across multiple repositories, you can see all the changes in one place. You can also see the history of a project or feature more easily. If I’m tracking down a performance regression, I can easily checkout a single commit and run performance tests on a snapshotted state of an entire project, rather than having to checkout multiple repositories at different commits to get the same state. Alternatively, if I want to know what changes were made to develop a feature, again I can just look at a few PRs in a single repo, instead of tracking changes in multiple.


Onboarding new engineers is also much simpler. In a polyrepo setup, day one looks like this: clone five repositories, install three different dependency managers, and set up environment variables for each service to talk to the others. With a monorepo, it’s one clone, one set of build tools, one README. Want to understand how the authentication flow works? The entire path from frontend login button to backend JWT validation to database query is right there in the same repository. No hunting through GitHub organizations trying to find where the auth service lives.


### Disadvantages of the monorepo


The sad truth? In engineering, there are no perfect solutions, and monorepos come with their own set of trade-offs. Everything that makes monorepos powerful at small scale becomes a liability at large scale. Git wasn’t designed for multi-gigabyte repositories. Your IDE wasn’t built to index millions of lines of code. Older CI systems weren’t meant to run every test on every commit, you’ll be waiting 40 minutes for CI to finish running. The architectural boundaries that polyrepos enforce keep your services from becoming a distributed monolith where everything depends on everything else.


The performance problems start innocuously. Your` git status` takes 3 seconds instead of instant. No big deal. Then` git checkout` starts taking 30 seconds. Your team discovers` git sparse-checkout` and starts maintaining lists of directories they actually need. Someone writes a wiki page about “Git Performance Best Practices” that includes gems like “avoid using git log on the repository root.” Then the project grows, and the monorepo becomes a liability. The same benefits that made it easy to onboard new developers now make it hard to manage the codebase. That same ease of cross-service changes now leads to massive PRs, tight coupling, and the kind of spaghetti code no one wants to touch.


Take[Microsoft’s Windows repository](https://devblogs.microsoft.com/bharry/the-largest-git-repo-on-the-planet/) as an extreme example. The Windows repo contains 3.5 million files and a modest 300GB. A fresh clone without any custom tooling would take hours. The performance got so bad they had to invent an entirely new virtual filesystem (VFS for Git, now called Scalar) just to make basic git operations usable. Even with these specialized tools, developers need custom configuration and training just to work with the codebase.


IDEs suffer too, with language servers crawling through indexing and even simple searches timing out unless carefully scoped. Search operations that used to be instant now require deliberate targeting to avoid overwhelming the system.


The CI/CD story is just as brutal. Every commit to your monorepo can trigger every CI job. Change a README in the documentation folder? That’s 45 minutes of backend tests, frontend builds, and integration suites you get to patiently wait for. Teams can implement path filtering (“only run tests if files in /backend changed”) but now you’re maintaining a complex web of dependencies. Even if it’s done perfectly, it still eats precious developer time.


Mundane issues can also become major annoyances. Merge conflicts in monorepos aren’t just annoying, they’re painful. In a polyrepo, the backend team merges their API changes, the frontend team handles theirs separately. In a monorepo, both teams are competing for the same merge queue. Now you have to resolve conflicts in code you’ve never seen, in a language you might not even know.


## Decision Guidelines


### When Monorepos Make Sense


Monorepos work when you have tightly coupled services that genuinely need to evolve together. If you’re building a SaaS product where the API, web app, and mobile app share data models and business logic, the monorepo keeps everything in sync. One PR updates the API contract and all its consumers.


Small to medium teams building a single product benefit most. When you have 5-50 engineers all working toward the same goal, the simplified coordination outweighs the tooling overhead. Everyone sees everyone else’s changes. Code reviews span the full stack. New engineers get the entire system from one` git clone` .


### When to Stick with Polyrepos


Distributed teams across time zones struggle with monorepos. When your American team member’s evening push breaks the build for your Hungarian team member’s morning deploy, coordination becomes a lot more difficult.


Multiple languages or tech stacks favor polyrepos. Your Go microservices don’t benefit from sitting next to your React Native mobile app. They use different build tools, different dependency managers, and different deployment pipelines. The monorepo forces you to maintain a build system that understands both, for no actual benefit. Your Go developers never touch the mobile code. Your mobile developers never touch the backend. Why make them deal with each other’s tooling?


## Fin


Monorepos trade architectural friction for speed and visibility. When they work, they really work. But they do not scale on good intentions alone. Without the right tooling, the benefits collapse under their own weight.


[Depot gives your monorepo](https://depot.dev/blog/how-to-use-depot-in-monorepos) fast builds, smart caching, and infrastructure that keeps up.


## FAQ


What is a monorepo?


A monorepo is a version control approach where all of an organization's code lives in a single Git repository. Backend services, frontend applications, and shared tools all sit side by side in one place.


What are the main benefits of using a monorepo?


Monorepos make it easier to coordinate changes across multiple services. You can track features across the entire stack, review all related changes in one pull request, and onboard new engineers with a single clone.


What are the common problems with monorepos?


As monorepos grow, git operations slow down, CI pipelines get longer, and IDEs struggle to keep up. Without the right infrastructure, the advantages disappear and the entire system becomes harder to manage.


When is a monorepo a good idea?


Monorepos work well when teams are small and services are tightly connected. If your product shares data models and business logic across systems, keeping everything in one repository can speed up development and simplify coordination.


How does Depot support monorepos?


Depot gives monorepos fast builds, smart caching, and isolated resources. Whether you are building multiple Docker images or running a large codebase, Depot provides the performance infrastructure to keep your workflows efficient.


## Related posts


- [How to use Depot to build Docker images in your monorepo](https://depot.dev/blog/how-to-use-depot-in-monorepos)
- [Developer Experience: Past, Present & Future](https://depot.dev/blog/devex-past-present-future)
- [Self-hosted GitHub Actions runners aren't free](https://depot.dev/blog/self-hosting-github-actions)
- [Faster GitHub Actions with Depot](https://depot.dev/blog/faster-github-actions)
- [The best CI provider for fast Docker builds](https://depot.dev/blog/best-ci-for-docker)


Billy Batista


Software Engineer at Depot
