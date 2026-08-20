---
schema_version: "1.0.0"
document_id: "cef8a4492f69c49188eef981b9a966914afa6076aa77dfa46fa7500447201892"
company_key: "weave-communications-inc-common-stock"
company: "Weave Communications Inc."
source_id: "weave-communications-inc-common-stock-rss-cb397ff18858"
canonical_url: "https://engineering.getweave.com/post/weave-devs-dont-do/"
published_at: "2022-06-27T19:27:31+00:00"
first_seen_at: "2026-07-20T23:19:36.973215+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:fa3ae1115f8d5445bf27a926124944f6f9346b635576db893a15c3a22c4e92ec"
---

# What Devs Don't Have to Do at Weave

[My Todo List](https://www.flickr.com/photos/40198520@N00/6562689119) by[JenGallardo](https://www.flickr.com/photos/40198520@N00) is licensed under[CC BY-NC-ND 2.0](https://creativecommons.org/licenses/by-nd-nc/2.0/jp)


# What Devs Don’t Have to Do at Weave


When I joined Weave last year, I got lots of questions from friends about my new job and the work I was doing on the DevX (Platform Engineering) team.


Weave as a company is pretty easy to explain: “if your dentist (or vet, or eye doctor) texts you reminders, good chance they’re a Weave customer”. Of course, there’s a lot more than reminder texts to[what we do](https://www.getweave.com/weave-works/) for small businesses at Weave, along with a lot of interesting engineering challenges.


But when talking to my developer friends about what I do, I was surprised how hard it was to explain. Without experience working with a team like DevX, they had a hard time grokking what I meant by “it’s Platform Engineering - infrastructure stuff, kind of like DevOps or SRE, but with more coding, and more focused on tooling for developers.”


I found it easier to explain by telling them what developers *don’t* have to do at Weave - because of our DevX team - and how that makes dev work so much more productive and fun.


## Don’t have to take forever to get set up


I had a job once where setting up as a new dev was called “the gauntlet”. It involved a long, always-outdated set of instructions to install the software stack in several Linux VM’s, and many CLI commands to get it all working together. It took at least a day to get through it - if you didn’t run into problems.


At Weave, getting set up as a new developer is nothing like a gauntlet - it’s pretty simple and quick:


- Install Go (back-end devs) or Node (front-end devs)
- Install Weave’s developer tool (which we affectionately call` bart` )
- Install your favorite editor or IDE


Everything else runs in Weave’s[Cloud Native](https://www.cncf.io/about/faq/) infrastructure, so it doesn’t take long to be ready to code!


## Wait to launch a new service or deploy code


That gauntlet didn’t end at setting up my environment. There, and at other places I’ve worked, deploying a new Web site or backend service meant waiting for Ops to configure things, or setting things up myself. If I was lucky, I “only” had to create Helm charts or configure cloud services.


[Clock Work Man](https://www.flickr.com/photos/18090920@N07/4360987794) by[Sean MacEntee](https://www.flickr.com/photos/18090920@N07) , licensed under[CC BY 2.0](https://creativecommons.org/licenses/by/2.0/?ref=openverse)


Even small things like code changes could drag out - a production code fix could mean (waiting for) deploys into multiple environments for QA, acceptance testing, and testing in a staging environment, before getting on the schedule for a production deployment.


At Weave, devs don’t wait for anything to set up a new service or deploy updates.


Creating a new service is simple enough that new devs do it as part of their on-boarding, in maybe an hour. They use` bart` to take care of a lot of the work:


1. Run` bart init` to create a new project with scaffolding.
2. Add custom settings like the service named to Weave’s custom YAML file (called WAMLTM)
3. Code up a simple HTTP or gRPC handler
4. Create a Pull Request, which triggers the build and test pipeline
5. Run` bart deploy` (or click Deploy in bart’s browser UI) to start the new service running in the dev or production environment


A code update is just as easy, though many services automatically deploy (even into production) after a Pull Request is approved and merged. Devs decide with their teams how often to deploy, and many teams deploy multiple times each day.


The deploy process is fast, so it makes back-end work feel more like the quick iterations of front-end work (which I’ve wished for as a long-time back-end developer). In a typical day working on DevX services, I might make and deploy changes many times - sometimes several times in just a few hours! \[ Note: front-end updates are just as simple and quick. \]


## 🚫 Be a Kubernetes / Helm / Terraform expert to get things done


Weave runs on modern Cloud Native infrastructure, using Kubernetes and other cloud services.


Which is all fine and good, but often “cloud native” means devs have to learn Helm charts or Terraform and a bunch of Kubernetes commands before deploying code. And when a deploy fails, or a service doesn’t work as expected, it means more digging into cloud stuff, or finding someone who already knows those things.


[Close up of computer coding](https://www.rawpixel.com/image/432918/free-photo-image-computer-language-datum-php) by Markus Spiske, licensed under[CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0)


Weave developers don’t write Helm charts or Terraform, and there are many devs who’ve never had to run` kubectl` to get their job done. Most day-to-day interaction with infrastructure is done through the WAMLTM and` bart` .


While devs don’t *have* to use Cloud Native tools, they aren’t prevented from using them or from having access to most of the internals of our infrastructure. There’s nothing stopping devs from using` kubectl` if they find it more efficient or need to get to Kubernetes internals (well, those that are safe to access).


And though you don’t *have* to be a cloud guru, there are many of them at Weave, and developers are encouraged (and given time) to become experts on the technologies we use.


## 🛑 Figure out how to debug services locally


Debugging can get pretty complicated with cloud-based microservices. Sometimes a bug in one service is caused by interactions with other services, or only happens in a specific environment.


In the past, debugging these problems meant I’d have to set up Docker Compose, Kubernetes, or VM’s to run services locally, or figure out how to set up port-forwards and other complicated things to reach remote services.


At Weave, local debugging is simple:` bart run` sets up local and remote dependencies somewhat “automagically” using configuration from a YAML file and data from our infrastructure. It even (securely) manages any secrets that are needed by the service.


## ⛔ Set up monitoring and alerting services


[Recent Purchase: Megaphone](https://www.flickr.com/photos/51035734296@N01/2575986601) by[LarimdaME](https://www.flickr.com/photos/51035734296@N01) , licensed under[CC BY-NC 2.0](https://creativecommons.org/licenses/by-nc/2.0)


A developer’s job doesn’t end when coding is done - running a service reliably is always the bigger challenge. It involves identifying user expectations, knowing when the service is not working, and fixing bugs or improving performance.


Detecting service problems is not trivial - it covers 3 chapters in the[SRE book](https://sre.google/sre-book/table-of-contents/) . The service has to be instrumented to capture metrics, traces and logs, and those have to be collected and compared to the expected behavior. That usually means setting up and learning how to use products like Prometheus, Grafana, Jaeger, AlertManager, and a logging platform. It’s a lot of work.


When I create a new service at Weave, most of this is already done for me. My service automatically gets its own metrics dashboard page. Traces are automatically collected for HTTP, gRPC, NSQ and database requests, and it’s easy to add custom traces and custom metrics.


Alerts are also simple to set up in the WAMLTM, and feed into our existing alerting infrastructure. For example, a Slack alert for HTTP errors looks like this:


```text
monitoring  :
alertMethods  :
-  slack
rules  :
-  alert  :     Error Rate (http)
expr  :     |
sum(rate(http_timer_count{code!="200", code!="404"}[10m])) / sum(rate(http_timer_count[10m])) > .10
annotations  :
summary  :     Error rate over threshold
labels  :
severity  :     critical


```


Weave uses[SLOs (Service Level Objectives)](https://sre.google/sre-book/service-level-objectives/) for analyzing metrics and triggering alerts - this Utah Go Meetup[presentation](https://www.youtube.com/watch?v=sfwnr5K8oCs&t=5s) explains how.


## ❌ Search (or wait) for expert help


Modern development is complex, involving microservices, CI/CD pipelines, Kubernetes and other Cloud Native technologies. It can be dizzying and overwhelming.


[Pair Programming](https://www.flickr.com/photos/80288388@N00/16146549307) by[haslo](https://www.flickr.com/photos/80288388@N00) , licensed under[CC BY-NC-ND 2.0](https://creativecommons.org/licenses/by-nd-nc/2.0/jp)


There’s nothing more frustrating than getting stuck trying to figure out why a CI workflow is failing, or how to configure an infrastructure feature, or why a shared library is breaking your service - you’re just trying to get a feature out the door!


At other places I’ve worked, those kinds of problems meant trying to track down another dev who knew how to solve the issue *and* had the time to help, or bugging someone in Ops or SRE who usually had more important priorities.


One of my favorite things about the DevX team at Weave is that we help developers be more productive - including helping when they get stuck. We have a Slack channel for devs to reach us, an active knowledge base (conveniently accessible through` bart search` ), and regular sessions of “DevX School” covering all sorts of dev-related topics.


Sometimes we just need to point someone in the right direction, other times we have to dig into the infrastructure or pair on a code problem. Whatever the problem might be, Weave developers don’t have to worry about finding help.


## Don’t have to work in a certain way


As an engineering team grows, often there is a push to have teams work in the same way to keep things manageable. Teams may have to adopt the same Agile processes, or synchronize sprints and release schedules with everyone else.


Weave is a large organization, but teams still decide how they get their work done: how long their sprints are, which Agile methodology they follow, the meetings they need, how they coordinate with other teams, and how they release and support their code.


Some teams do a release on every PR merge, others may have a more-formal, less-frequent release process. Teams that are often in the office may have a lot of informal communication, where teams that mostly work remote might rely more on Slack and scheduled meetings.


Team autonomy is an important part of Weave’s culture, and our modern architecture and infrastructure support that, even as we grow.


## What DevX does, so Weave developers don’t have to


So, that’s how I explain what I do as a DevX engineer at Weave: my team makes it easier for developers to do more of what they want to do - coding, problem-solving, and building things that make customers happy - because of the things they *don’t* have to do anymore.


Check out[The State of Platform Engineering at Weave](https://engineering.getweave.com/post/platform-engineering-at-weave/) if you’d like to know more about how we do that.
