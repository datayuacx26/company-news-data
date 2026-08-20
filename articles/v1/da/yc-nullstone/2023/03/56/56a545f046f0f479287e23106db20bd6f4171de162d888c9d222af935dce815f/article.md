---
schema_version: "1.0.0"
document_id: "56a545f046f0f479287e23106db20bd6f4171de162d888c9d222af935dce815f"
company_key: "yc-nullstone"
company: "Nullstone"
source_id: "yc-nullstone-news-import-96ce084d16d8"
canonical_url: "https://www.nullstone.io/blog-posts/staging-is-broken"
published_at: "2023-03-09T00:00:00+00:00"
first_seen_at: "2026-07-27T04:02:46.708969+00:00"
fetched_at: "2026-08-10T01:11:39.788792+00:00"
content_hash: "sha256:214b902808ec98be5fd15bf1425609c180e1db3df9da66dfed8e07575ef0c0c3"
---

# Why is staging always broken?

## Introduction


If you’ve ever worked for a company at least five years old, you probably have that massive elephant you cannot eliminate. Everybody in the company complains about this elephant — it slows software teams from shipping that new feature, prohibits teams from trying out a technology that could significantly improve the product, or prevents sales from closing deals. Yet, it persists because it takes too much effort or knowledge or it’s too risky to address until the company has more revenue. (Isn’t that ironic — too dangerous until the company is more prominent)


The elephant for many software teams is their staging environment (and sometimes **environments**). The idea of a staging environment is simple: create a replica of production to vet changes, data, and infrastructure before release. However, several issues emerge that slow teams down and reduce release confidence. Let’s look at some of the problems with staging and what we can do to improve.


## What broke?


While working with many companies, we found three common patterns that remained untreated for extended periods. Let’s look at those patterns and how they negatively affect software teams’ ability to ship software.


### Who moved my cheese?


Staging is usually a shared environment. Developers, DevOps engineers, QA, and product managers use this environment to stage new features, test bug fixes, run regression tests, and demonstrate new functionality. Unfortunately, each role experiences a different version of the same nightmare: a change is made (usually for a good reason), but few on the team know what changed and why. Developers quickly make changes to make their feature work. DevOps engineers make a fix because the product keeps crashing. QA adds mountains of fake data to test edge cases. Product Managers create demo data to simulate real customer environments.


DevOps engineers are often interrupted routinely to fix a broken staging environment. After hours of diagnosis, DevOps engineers usually discover an incompatible setting or overrun database. If you’ve been in this position as a DevOps engineer, you can empathize with how frustrating this can feel.


To amplify the problem, a common reaction from some teams is “that’s why we have staging…so production doesn’t break”. While I admire an optimistic mindset, it’s missing the point. “Cheese-moving development” is wildly unpredictable, destructive to productivity, and destroys morale.


So why can’t teams communicate more? For small teams, talking more does fix many problems. Unfortunately, this additional chatter inevitably degrades over time and with larger groups. What was once valuable knowledge sharing becomes noise that is tuned out. Even if there is a sustained improvement, team members burn out putting out endless fires.


### “Real” Data


Data in the database can cause an environment to break because it is missing, corrupt, misleading, and incorrect. If the application is heavily data-driven, data becomes the lynchpin maintaining stability for developers, testers, and stakeholders. What appears as a bug may be a poorly maintained dataset.


We noticed a few trends from the teams we’ve talked to:


- More companies are storing data that is sensitive while consumers and businesses become more privacy-aware.


- Third-party data providers heavily drive fintech and analytics companies.


- With massive growth in AI/ML, companies are growing more reliant on training model data.


- Many startups rely heavily on third-party data sources; they develop their own internal data sources as they mature.


These trends point to the growing need for software teams to manage their staging environments.


1. Teams cannot copy sensitive production data without fuzzing sensitive datasets.


2. Creating and maintaining representative data is difficult and time-consuming, sometimes requiring engineers and product managers to collaborate routinely.


3. Teams must store and maintain a set of test/sandbox API credentials/tokens.


### What is deployed?


For most teams, production environments are highly predictable. For example, some teams will deploy to production on a particular day of the week, while others deploy when teams achieve a specific scope. However, staging is wildly different from production — teams deploy a convergence of features and bug fixes several times a day. We spoke with many groups, most of whom didn’t know what their teams deployed to their staging environment and how staging differs from production.


Suppose we expand our definition of “deployed” to include infrastructure (e.g., database configuration, app resources, third-party APIs, etc.). In that case, you can imagine how difficult it is to know what teams have deployed. Furthermore, if you use VMs, this manual configuration drifts over time as your team frantically makes changes to staging or production without synchronizing.


We have seen a general trend that teams using infrastructure-as-code (IaC) are better aware of deployments. However, IaC is not a silver bullet. Few teams are able to quickly identify what has changed over time with infrastructure and code. In addition, due to using several tools, teams accidentally scatter application code, infra code, build logs, deployment logs, and infrastructure logs requiring manual correlation.


## What can we do to improve?


\[We have learned from DORA that deployment frequency and change failure rate are critical indicators of high-performing software teams\](https://cloud.google.com/blog/products/devops-sre/using-the-four-keys-to-measure-your-devops-performance). If every deployment must go through a broken staging environment, how can we ship fast and be confident that we don’t introduce issues? Let’s look at practices many teams incorporate to accelerate product releases.


### Adopt infrastructure-as-code


If you haven’t already, adopt infrastructure-as-code. (We’ll explore which IaC is best in another post) Creating a Terraform script that stands up a VM where admins manually configure the box is insufficient. We’re shooting for the goal of being able to recreate an entire environment from scratch. By codifying the configuration, any engineer that joins the team has a clear definition of what it takes to run the environment.


This practice can feel daunting or overwhelming if you have an expansive architecture. Remember, we’re coding — it’s best to take an iterative approach just as you would with application code. Start with a low-risk, high-impact area to establish foundational patterns and practices. Then, in every sprint, commit to a budget of 10% to incorporate IaC. If you focus on the areas that routinely fail, you will have an immediate payback on the investment because you spend less time putting out fires.


### Establish a single source of truth


You should track your IaC in Git. Git tracks who made what changes to what IaC file (and, if your team is diligent, why developers made changes). More importantly, IaC needs a place to execute and an activity log of what changes were made by who. Once you have reached a base maturity for your IaC, every change *must* go through this process. If done well, making emergency fixes through your automation will be faster and safer.


If you are in a regulated industry, open access to this activity log and process to security testers and auditors. This transparency builds trust, and your team does not have to do extra work during routine audits.


### Invest in data


A product manager or technical lead should commit to building mock datasets or automation to import scrubbed production data. The team should consciously maintain this data by including this as acceptance criteria for initiatives. This responsibility usually falls on the product manager as it communicates the expected way for a user to interact with the system.


In our current ecosystem, creating good data provides more value than good tests (ideally, the data is in the tests too). Just like good testing, it takes initial effort that feels wasted. However, good data generates momentum for a team as the application grows in complexity. Instead of stalling feature development at a critical moment in a team’s growth, a software team can ship confidently.


### Make Repeatable


I worked on a team that adopted one of the first versions of Terraform. We enforced a rule: “Launch and destroy three times before trusting your IaC.” We made this rule to insulate ourselves from the volatile nature of cloud provider APIs and Terraform. Even though Terraform has added various reliability measures to the core engine and each Terraform provider, the rule has remained a philosophy that saves time and frustration for teams. IaC engines execute wildly different code paths based on the current state of infrastructure. For example, the execution path of creations is different from updates and destroys. Even more, an additional variable value during an update can result in an attempt to destroy parts of your infrastructure. Cloud provider APIs subject Terraform to hostile conditions because they are not 100% reliable.


Whether you adopt the specific policy we instituted or not, the critical mindset is to verify repeatability before shipping infrastructure changes that affect other team members. Here are some other techniques you can apply that will insulate your developers from infrastructure bugs by ensuring repeatability:


1. Provision infrastructure in a sandbox environment upon merge.


2. Build automated testing inside your delivery process that occurs before a merge of your infra code.


3. Enforce smaller Terraform modules that are heavily tested and reused.


### Simplify


Keep it simple. Our work as product teams is judged by how it serves customers. It will be challenging for your teams to simplify IaC because infrastructure communities push for practices that overcomplicate systems.


Many push \[DRY (Don’t repeat yourself) practices\](https://terragrunt.gruntwork.io/docs/features/keep-your-terraform-code-dry/) even though so many problems in software emerge from \[premature optimization\](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.103.6084&rep=rep1&type=pdf). Instead, let’s follow \[the WET (Write everything twice) method\](https://dev.to/wuz/stop-trying-to-be-so-dry-instead-write-everything-twice-wet-5g33). I’m sure you can empathize if you have ever rolled out infra changes late at night only to find out the following day that something that depended on that module broke.


Use smaller IaC modules: fewer variables, fewer resources, and less functionality. Unfortunately, \[some of the most popular Terraform modules on the official Terraform registry have over 175 variables.\](https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/latest?tab=inputs) This generalization introduces potential errors and regressions while slowing down Terraform plan/apply.


## Conclusion


Staging environments can be a significant source of frustration and inefficiency for software teams. However, by adopting best practices such as infrastructure-as-code, establishing a single source of truth, investing in data, ensuring repeatability, and simplifying processes, teams can improve their staging environments and accelerate product releases. In addition, by reducing the time and effort required to manage staging environments, teams can focus on delivering value to customers and achieving their business goals.


‍
