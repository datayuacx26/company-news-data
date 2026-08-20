---
schema_version: "1.0.0"
document_id: "3658241e7fe9f8e8ebcc2edd6e3c5e24ae585b93255abc7be95b309d7a653cd1"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/15-years-of-duct-tape-why-iac-adoption-stalled-at-30"
published_at: "2025-10-06T00:00:00+00:00"
first_seen_at: "2026-07-27T21:32:22.131944+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:a809f80af00a08ad7615ddf765e68b4d02b75809e343e5d1d4db1917b476b210"
---

# 15 Years of Duct Tape: Why IaC Adoption Stalled at 30%

**Stop me if you’ve heard this one.**


We all started in the same place—whether you were the first infra hire at an enterprise or the founding engineer in a startup. You pick up Terraform—or maybe OpenTofu—and write some infrastructure. It feels great. You push it, it runs. You’re managing stuff in AWS!!! Infrastructure as code FTW!


Then your team grows. You add someone. State management gets messy, so you stick it in version control. Maybe you split up your environments: prod, dev. You attempt to follow all the “best practices”—Terraform all over. A few months later, it’s a mess. The blast radius is too big. You try to untangle it. You break it into modules. You make a few for prod, a few for dev, and a couple one-offs. Now you’ve got 17 Git repos.


Eventually it’s 100.


You’ve got your first customer asking about SOC2! Now you need compliance. You wedge in security and compliance scanners into a bunch of CI/CD pipelines because that’s what you already had lying around. It worked for app code, so we figured—why not? Everyone’s doing infrastructure as “code,” right?


Might as well run it through the CI.


Now devs start showing up. They want to use your Terraform modules. But you never really set up versioning. So now you’re tracking down who’s using what. You’re sending them YAML over Slack. You’re asking them to copy-paste OPA policies. They don’t.


So you do it for them.


Some modules bump Terraform versions. Some don’t. AWS providers move forward. You’re grepping through repos, patching pipelines, making updates manually. You’re still managing infrastructure—but now it’s through duct tape and damn near having to cattle-prod teams to get them to pay attention to *your debt.*


And then you get drift. This is all because you have code, and maybe even modules, but no interface…


New developers go straight into the AWS console. The Terraform is confusing and it takes forever to get a response in ServiceNow. They click around. They build what they need. And now it’s out of sync with the code. Sometimes they write the code later and import the state—usually you do it for them. Eventually, you just take away production access altogether.


So now you’re doing GitOps. But not because it’s great—because it’s the only interface we know, but that doesn’t make it the best choice.


You tell devs: go to GitHub. Open a PR. Ask in Slack if you don’t know what value to set for a variable.


But let’s be honest—you’re the one writing the Terraform now. They ping you. You write it. Now you’re looking for someone to review a PR, the devs don’t really care, so you’re pinging other ops people that are lying around with Mai Tais because they’re ***not*** absolutely overworked and behind on 10 shifting initiatives … *oh wait, they are.*


You buy a portal.


Now devs click a button and it runs a workflow. But underneath that nice-looking UI? Same deal: 100 repos, CI/CD pipelines, duct tape, fragility — and that’s *your product* now.


Yay, the DevOps team’s work is managing pipelines. You’ve made it!!!


---


And here’s the wild part.


We’ve now driven devs out of the editor, out of Terraform, and out of Git. And we’re still the ones doing all the work.


And now the industry starts chasing “better tooling.” People say “Terraform is hard.” So we get Pulumi. We get Crossplane. We get Winglang.


All of them make tiny tweaks to the developer experience. A little sugar. A new syntax. A different way of doing things. But underneath, they’re still standing on the same foundations—Terraform’s providers, the same resource model, the same assumptions.


They are treating a symptom as the problem.


The problem isn’t HCL. It’s not JSON. It’s not YAML.


Without that missing layer—policy, constraints, audit, a separation of roles—we’re just repackaging the same pain in different syntax. Still making every dev pretend to be an infra engineer, instead of giving them a safe interface to use what’s already been built.


**The problem is the workflow.**


It’s the pain of the process. It’s the time it consumes. It’s the burden of knowledge we’ve thrown onto engineers who just want to get their job done.


We’re asking them to understand cloud internals. To make decisions about compliance, IAM, encryption, backups, failover, cost.


IaC’s syntax is choking its adoption. It’s the copy-pasta workflows—the friction, the ritual, the process for its own sake—and we still leave developers stuck with the hardest part: guessing the right values for operating production.


---


**CI/CD was never the right tool for this job.**


It wasn’t the right tool for infrastructure. It wasn’t the right tool for Terraform. But we used it anyway. Not because it worked well—because it was there. It was already set up. It was familiar, it worked ( *if you could ignore all the ugly warts devs were already complaining about when CI/CD’ing their software* ).


We talk all the time about “using the right tool for the job” in software, but we rarely follow through. Maybe 20-30% of the time, we actually pick the right tool (I asked ChatGPT). The rest of the time, we use what’s available. What’s old. What’s convenient. Or we chase the shiny new bauble.


That’s what happened here.


CI/CD works great for apps—web services, mobile apps, embedded systems—stuff that needs to be built and shipped and hosted. But managing cloud infrastructure declaratively? That’s not the same type of workload.


CI/CD produces a software asset to run, that is the value. Infrastructure in CI/CD, the execution is the value, not the output.


It’s ephemeral. It’s short-lived. It’s just … making API calls.


We took a system designed to deliver services that respond to API calls, and used it to make API calls.


That’s a completely different kind of job.


That was the original sin of modern infrastructure management. The Cloud showed up. Infrastructure-as-code became real. And we just reached for whatever was lying around—CI/CD—and said: “Good enough.”


So here we are. 15 years in. And we’ve still only got around 30% IaC adoption.


Not 30% mastery. Not 30% success.


Just 30% of people who’ve started to try. More recent surveys say maturity may be as low as 13%


And the whole time, we’ve been duct-taping together tools that were never built for this.


We don’t need a new language.


We need a new model.


The joy in software development is solving a hard problem and making the solution accessible to a layman. That’s the entire SaaS market summed up in a sentence that’ll twist some neckbeard’s panties on hacker news.


Where is that joy in infrastructure management?


Where’s the opportunity for DevOps to design experiences for developers?


It’s not handing them YAML. It’s not telling them to figure out the cloud themselves.


**Ops (DevOps, SREs, Platform) chose the tool and the process for developers.**


And that was never the spirit of DevOps.


Devs told us they wanted something different. That it didn’t feel good to do the work this way.


They found easier paths—and we responded by putting blockers in their way.


And then we wonder: Why is there still animosity between teams in most orgs? Are they not culturing hard enough?!?


We’ve taken what has made the SaaS model successful, making hard problems accessible to laymans, and we’ve shunned it in DevOps.


Now everyone’s saying you should be doing “platform as a product.” And I agree—100%.


But building a platform as a product on top of a shaky foundation? That’s just another Band-Aid.


If we’re serious about infrastructure as a product—if we want to make it accessible to engineers—we need to rethink the entire foundation we’ve patched together for the last 15 years.


---


*Some* folks say the answer is to throw out IaC entirely. But that’s tossing the baby out with the bathwater.


Infrastructure as code is still the language of DevOps. It’s how we describe intent, enforce policy, and track state. It works for us, it just doesn’t work for our engineers. Hell, it’s our only medium to communicate infrastructure with our AI overlords that are coming for our jobs. What’s broken isn’t the code—it’s everything we’ve bolted onto it. CI/CD pipelines. Snowflake workflows. Tribal glue scripts nobody wants to own.


What we need isn’t a new language.


We need a new way to **deliver** infrastructure, the physical-in-our-minds, tangible-in-concept, cloud resources that we’ve skeuomorph’d into a … a fucking github action‽


What if infrastructure could carry the ops knowledge with it—security, compliance, runbooks, dependencies—instead of leaving teams to reinvent the wheel every time? Instead of duct-taping pipelines, the orchestration could live inside the architecture itself.


That’s what we’ve built at Massdriver: bundles of IaC that don’t just describe resources, but actually know how to connect, what they depend on, and how to safely hand off their outputs. That means a Terraform module and a Helm chart can talk directly—no Rube Goldberg chain of CI jobs, no brittle YAML rituals, no copy/pasta.


A new foundation for how infrastructure gets delivered: pipelineLESS, with guided, proactive compliance around configuration values—the thousands of security, reliability, and cost decisions that pile up across teams and keep shifting under your feet as standards, policies, and cloud defaults change.


We talk a lot about using the right tool for the job.


But when CI/CD didn’t scale for infrastructure, we didn’t replace it—we hacked around it. When devs found workarounds, we did our best Steve Jobs and told them they were holding it wrong.


For 15 years, we’ve shaped our workflows around pipelines instead of asking what the work really demands.


It’s time to stop bending our process to fit the tool and start building tools that fit the work.


---


If you’ve struggled to scale IaC adoption through brittle pipelines—or watched your developers turn into junior DevOps engineers just to ship infra—you’re not alone.


There is a better way.


**Aside:** While I was drafting this, I tossed out a[shitpost](https://www.linkedin.com/feed/update/urn:li:activity:7353531187979563009/?commentUrn=urn%3Ali%3Acomment%3A%28activity%3A7353531187979563009%2C7359410561874325506%29&dashCommentUrn=urn%3Ali%3Afsd_comment%3A%287359410561874325506%2Curn%3Ali%3Aactivity%3A7353531187979563009%29) on LinkedIn asking what the “D” in CI/CD stands for. I was hoping to spark a genuine debate about *delivery vs. deployment* —but the overwhelming gut reaction was jokes. Disaster. Disappointment. Dogs. (Okay, fair enough.)


It’s not a scientific sample, obviously, but it says something: we’ve built workflows people don’t take seriously anymore. Instead of sparking curiosity about better models, the instinct is mockery. Maybe that’s because, deep down, everyone knows the current system is broken—and we’ve been duct-taping it for 15 years.
