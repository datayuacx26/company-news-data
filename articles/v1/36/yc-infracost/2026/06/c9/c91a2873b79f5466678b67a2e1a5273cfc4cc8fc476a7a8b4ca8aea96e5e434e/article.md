---
schema_version: "1.0.0"
document_id: "c91a2873b79f5466678b67a2e1a5273cfc4cc8fc476a7a8b4ca8aea96e5e434e"
company_key: "yc-infracost"
company: "Infracost"
source_id: "yc-infracost-news-import-5e947f59e679"
canonical_url: "https://www.infracost.io/resources/blog/infracost-cli-v2-now-reads-cloudformation"
published_at: "2026-06-29T23:13:00+00:00"
first_seen_at: "2026-07-23T11:56:51.837513+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:93d9d812e47b5fa51d91e45b2399b9acbb646711518e3011e0d4a74aac5af062"
---

# Estimate CloudFormation costs before you deploy

# Infracost CLI v2 now reads CloudFormation


You wrote the template. You deployed the stack. The numbers looked fine.


Then the bill showed up four weeks later, and you're in a thread explaining why a staging environment quietly costs $1,800 a month. The gp2 volume that should've been gp3. The RDS instance two sizes too big "just to be safe." The NAT gateway nobody remembers adding.


None of that was a mistake when you wrote it. You just couldn't see the cost while the fix was still free.


If you run Terraform, Infracost has had your back for years: costs in your terminal, before you push. If your shop runs CloudFormation, you've been doing it the hard way. Eyeballing templates, guessing at instance pricing, pasting resources into a calculator one at a time.


That's over. **Infracost CLI v2 reads CloudFormation natively.**


## Point it at a directory. That's the setup.


No deployed stacks. No change sets. You point the CLI at your template directory and it tells you what those resources cost:


```text


```


```text


```


```text


```


It detects the type on its own, whether that's Terraform, Terragrunt, or CloudFormation. No flag, no config telling it which IaC you wrote. It reads the templates, figures out the resources, prices them against live cloud pricing, and runs your FinOps and tagging policies over the result.


Then you can slice the results without re-running anything:


```text
infracost inspect  --missing  -tag
```


```text
infracost inspect  --missing  -tag
```


```text
infracost inspect  --missing  -tag
```


Results are cached locally, so` inspect` comes back instantly.


## Using CDK? Synth first, then scan


CDK renders CloudFormation. An important point: the CLI won't run` cdk synth` for you.


Run synth yourself, then point Infracost at the output:


```text


```


```text


```


```text


```


For a hands-free option where Infracost *does* perform CDK synth for you, connect your repositories to Infracost Cloud. It detects and processes your CDK projects for you, synth included, with no manual step.


## For engineers, by engineers


The part we're most excited about has nothing to do with dashboards.


You don't need admin rights to the CI/CD pipeline. You don't need anyone to "roll out" anything. No meeting, no ticket. You install one binary and run one command against the templates on your laptop, before you open the PR.


It catches things that may surprise you. Outdated instance types; untagged volumes that trip your team’s policy; an innocuous one-line setting that turns $200/month into $900.


This is the workflow that made engineers fall for the Terraform CLI in the first place. Now it works on CloudFormation too. Close the laptop at 5, not 9.


## For the engineer rolling this out across teams


If you own the cost guardrails, here's what parity actually buys you.


Enterprise infra is never uniform. One business unit standardizes on Terraform. Another inherited a pile of CloudFormation stacks. A third went all-in on AWS-native tooling. Until now, that meant your cost policies covered half the org and stopped dead at the language boundary.


CLI v2 runs one engine, one set of pricing data, and one set of policies across all of it. A tagging rule you write once applies whether the resource was declared in HCL or YAML. You set the guardrail and the CLI enforces it everywhere you touch infrastructure: locally, in CI, in the editor. No second policy framework to babysit. No coverage gap to explain to finance.


That's enforcing the standard without living in meetings, which is the actual job.


## What's under the hood


CloudFormation support ships at full parity with our Terraform integration. Same resource coverage, same policies. Infracost prices over 1,100 resources across AWS, Azure, and Google Cloud, including usage-based ones like S3 and Lambda. No cloud credentials or secrets ever leave your machine; the CLI parses templates locally and only sends what it needs to look up a price.


And yes, it's v2, not 1.0. We skipped a number and rebuilt the CLI around` scan` and` inspect` , partly because the thing running it is more and more often an AI coding agent. Claude Code or Cursor calls Infracost as a subprocess to check its own work before it hands you a PR. Same cost data, now reachable by whatever's writing your infrastructure, human or not.


## Install it in the next two minutes


**macOS** (Homebrew):


```text


```


```text


```


```text


```


**Windows** (Chocolatey):


```text
choco   install   infracost
```


```text
choco   install   infracost
```


```text
choco   install   infracost
```


**Linux** (install script):


```text
curl    -fsSL   <https://raw.githubusercontent.com/infracost/cli/master/scripts/install.sh> |  sh
```


```text
```


```text
```


Then grab a free API key and wire everything up with one command:


```text


```


```text


```


```text


```


Now point it at any CloudFormation project you've already got and scan from the project root:


```text


```


```text


```


```text


```


It's free, and it parses your templates locally. No cloud credentials or secrets leave your machine. The first surprise it catches pays for the two minutes it took to install.


See what your next deploy costs before you deploy it.
