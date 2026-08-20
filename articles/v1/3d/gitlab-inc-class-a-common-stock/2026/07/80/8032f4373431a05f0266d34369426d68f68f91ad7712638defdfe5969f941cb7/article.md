---
schema_version: "1.0.0"
document_id: "8032f4373431a05f0266d34369426d68f68f91ad7712638defdfe5969f941cb7"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/green-devops-carbon-measurement-cicd-pipeline/"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:07.155656+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:a79316ce60f05f871cbd37e26c2d01411cfa3153e5d9a3ae44d7e3af531a20c0"
---

# Green DevOps: Why carbon measurement belongs in your CI/CD pipeline

A typical software team runs hundreds of CI/CD jobs a day. Each one runs on compute and burns energy that doesn't show up in your pipeline logs, including its carbon impact. That invisibility is exactly the problem.


**You can't reduce what you don't measure.**


Eco CI and Carmen are third-party integrations that add a carbon lens to the pipelines you're already running in GitLab. Both are open source tools you can wire into any pipeline today. Here's why that's worth doing, and how to get started with Green DevOps.


## Why it matters


The compute footprint of a modern pipeline is growing. AI-assisted testing, code review, and pipeline automation all add jobs that didn't exist a few years ago, and each one carries an energy cost that never shows up in your pipeline metrics or your architecture diagram. Green DevOps is the practice of changing that. It means measuring emissions per pipeline run, per service, and per pod, and then using that data to make better engineering decisions.


## Where carbon shows up in your stack


Two of the most actionable layers for engineering teams are:


### Pipeline level


[Eco CI](https://docs.gitlab.com/ci/sustainability/eco_ci/) measures energy consumption and carbon emissions from your CI/CD jobs. It runs as lightweight bash scripts, with no separate servers or databases. You get per-job emission data, identify your most resource-intensive jobs, track trends over time, and display a carbon badge in your README.


For most teams, it's a good starting point. Your pipeline is already instrumented; Eco CI adds a carbon lens to it.


### Infrastructure and application levels


[Carmen](https://docs.gitlab.com/ci/sustainability/carmen/) (Carbon Measurement Engine) goes deeper. Built on the[Green Software Foundation Impact Framework](https://if.greensoftware.foundation/) , it measures emissions from virtual machines, pods, and individual application workloads running in Kubernetes. You get a per-component CSV report, broken down by operational carbon (energy use) and embodied carbon (hardware manufacturing and disposal), that you can feed into Grafana, FinOps dashboards, or your own tooling. Output fields include` EnergykWh` and` TotalCarbonGramsCO2eq` per component, so the data slots into existing dashboards without transformation.


Carmen is particularly powerful for answering questions like:


- Which service in our stack emits the most CO2?
- How does our API gateway compare to our data processing layer?


## Getting started in your pipeline


Both tools integrate directly into` .gitlab-ci.yml` . A Carmen job looks like this:


```text
carbon-report  :
image  :   python:3.12
before_script  :
# Install Carmen and the IF toolchain
-   apt-get update && apt-get install -y nodejs npm git lsb-release
-   git clone https://github.com/Green-Software-Foundation/if-carmen.git
-   npm install -g "@grnsft/ [email protected]  " "@grnsft/ [email protected]  " "@grnsft/ [email protected]  "
-   pip install --upgrade pip && pip install -e $CI_PROJECT_DIR/if-carmen
script  :
-   cd $CI_PROJECT_DIR/if-carmen/example-data && carbon-daemon
artifacts  :
paths  :
-   if-carmen/example-data/output/
expire_in  :   1 week


```


Run it, download the artifact, and you have your first carbon report.


## What it looks like in practice


Consider a team running hundreds of pipeline jobs a day. They add Eco CI in an afternoon, with a few lines in` .gitlab-ci.yml` and a README badge. Their first weekly report surfaces an unexpected finding: Their integration test suite accounts for a disproportionate share of total pipeline emissions, more than any other job type.


The culprit isn't the tests themselves, but the setup: Every run reinstalls its full dependency set from scratch. Caching the dependency layer can cut both test job runtime and its emission footprint. No new infrastructure, no architectural decisions. A one-line cache config.


Six weeks later, they run Carmen against their staging cluster and find a different kind of problem: A data processing service left over from a deprecated feature is still running, idle, consuming embodied and operational carbon for work that no longer happens. A ticket gets filed. The service is decommissioned.


Neither fix required a sustainability initiative, just visibility. Most carbon waste isn't intentional, it's invisible, and that's what Eco CI and Carmen are built to fix.


## Low effort, real payoff


This Green DevOps strategy is low effort because it slots into pipelines you already run. It also has real payoff because the data compounds, whether that shows up as a compliance baseline or a lower CI bill.


**Small numbers still build a baseline**
At the individual company level, your pipeline's emissions are probably a rounding error against global totals. But measurement isn't primarily about your share of the total. It's about building the data, tooling, and culture you'll need as emissions reporting expectations evolve. Teams measuring now will already have baselines and internal habits in place when that day comes, instead of starting from zero under pressure.


**It fits into pipelines you already have**
Eco CI is a handful of lines and a bash script. It doesn't spin up additional infrastructure or add meaningful latency. Carmen runs as a separate, non-blocking job. Neither is in your critical path. If your pipeline can run a linting check, it can run a carbon measurement.


**The payoff shows up in your FinOps numbers, too**
Carbon-efficient code is often also faster and cheaper code. Bloated pipelines burn developer time and cloud budget before they burn carbon. The same cache that reduces emissions can also help reduce your CI bill. Right-sizing runners is a FinOps win as much as a sustainability one, and you don't have to frame it as an environmental initiative to benefit from it.


## The bigger picture


Carbon-aware engineering is becoming a professional expectation, not a nice-to-have. Regulations like the[EU's Corporate Sustainability Reporting Directive (CSRD)](https://finance.ec.europa.eu/financial-markets/company-reporting-and-auditing/company-reporting/corporate-sustainability-reporting_en) require large companies to disclose emissions across their value chain, including cloud usage, and enterprise customers increasingly ask vendors about sustainability practices during procurement.


You don't need a mandate to start. You can add Eco CI to a single pipeline, then bring Carmen in for the infrastructure-level view.


## Learn more


- [CI/CD sustainability overview](https://docs.gitlab.com/ci/sustainability/)
- [Eco CI integration](https://docs.gitlab.com/ci/sustainability/eco_ci/)
- [Carmen integration](https://docs.gitlab.com/ci/sustainability/carmen/)
