---
schema_version: "1.0.0"
document_id: "492ecd4c49af504297ffffdf3a65b54d063ed6de8e4cbd4502526f7a1b9a3011"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/how-does-corgea-work"
published_at: "2023-12-15T00:00:00+00:00"
first_seen_at: "2026-07-21T15:02:54.375122+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:a8055ab65fe77c800b46979bfe88e9e1d03263bd3434b39dce1ce9d65b53f56e"
---

# How does Corgea work?

## Overview of Corgea


Corgea fixes vulnerable source code through[Corgea AI SAST](https://corgea.com/products/ai-sast) . It connects to SAST tools, such as Snyk and Semgrep, and writes code fixes and explanations using AI. Security engineers can issue pull requests for engineers to approve, which saves them 80% of the work, and accelerates patching cadence.


## How does it work?


Corgea connects to your existing SAST & SCA tools, and automatically writes code fixes for the reported vulnerabilities. Security teams can issue a pull request for the **fix with a single click** without disrupting any workflows. Engineers get the code fix for review, well written issue descriptions, and AI-generated fix explanations helping them understand the changes. For the workflow side, see[Corgea developer experience](https://corgea.com/products/developer-experience) .


Here’s a quick video from Ahmad, CEO at Corgea on[How does Corgea work?](https://www.loom.com/share/64cba8ad07a24964b3ffa6eacebc782d) .


## How did we achieve high quality fixes?


Currently, LLMs struggle at generalist coding tasks because of a wide range of possible variables it has to deal with. It has to “understand” the user’s request, and if that’s not good quality then it has to “assume” a lot, which yields a lot of wrong results.


Adam, one of the founding engineers on the team coined it well: LLMs don’t reason, they fuzz. We took several key decisions that helped in the LLM become much more deterministic. Firstly, what we’re doing is extremely domain specific: vulnerable code fixes in a limited number of programming languages. An SQL injection vulnerability in a Javascript app is the same regardless if you’re a payments company or a travel booking website. The second is that we have no user generated input into the LLM, which means it’s much more predictable for us and reproducible because input comes from existing vulnerability scanning tools. We can also create robust QA frameworks. There is no human input to describe the problem, which means we always have the same quality of input.


To illustrate the point, let’s put some of this to the test using some napkin math. Assume you’re serving 5,000 enterprises that ship on average 300 features a year in 5 different programming languages and each requires 30 lines of code changes across multiple files. You’ll have about 300m permutations the product needs to support, which is a nightmare!


Corgea needs to support very standardized vulnerabilities in code so no human input needed because it’s input is the scanners. Using the same napkin math, Corgea needs to support the top 100 vulnerabilities. Most vulnerability fixes require 1 - 2 line changes. Code base variations don’t matter here because the LLM doesn’t need to understand the whole codebase since the scope of change is so small. Let’s also assume a single company runs 5 different languages. So if we have 5,000 customers, it comes out to 500 permutations (100 issues x 5 different languages) because most fixes are the same across all customers.


Obviously, this is an oversimplification of the whole thing and it isn’t the full list of ingredients for our secret sauce.


### [​](https://docs.corgea.app/how_it_works#case-study-django-application-vulnerability) Case Study: Django Application Vulnerability


A practical example of Corgea’s capability is its handling of a specific vulnerability in a Django application. The issue, identified as CWE400 (uncontrolled resource consumption), was detected in the Django settings file because there was no rate limiting imposed on the[Django REST Framework](https://www.django-rest-framework.org/) . This allows an attacker to conduct a[denial of service](https://corgea.com/learn/denial-of-service-attacks) attack by bombarding the application with a lot of API requests.


To solve for this, Corgea’s AI accurately understood the context, being aware that Django was using a REST framework. It then generated a fix that:


-


Limited resources available to both anonymous and logged-in users.


-


Throttled incoming requests.


-


Provided an AI-generated explanation tailored to this specific fix.


## [​](https://docs.corgea.app/how_it_works#conclusion) Conclusion


Corgea represents a significant advancement in the field of cybersecurity. By leveraging AI to automate the process of fixing and explaining vulnerabilities, it not only enhances the security of applications but also allows engineers to focus on more critical tasks. With its intelligent integration, quality controls, and ability to handle complex vulnerabilities, Corgea is set to transform the way organizations approach cybersecurity.
