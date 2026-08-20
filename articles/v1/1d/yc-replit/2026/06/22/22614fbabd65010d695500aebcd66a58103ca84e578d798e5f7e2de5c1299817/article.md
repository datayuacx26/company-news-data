---
schema_version: "1.0.0"
document_id: "22614fbabd65010d695500aebcd66a58103ca84e578d798e5f7e2de5c1299817"
company_key: "yc-replit"
company: "Replit"
source_id: "yc-replit-news-import-9d99ff8f4466"
canonical_url: "https://replit.com/blog/package-firewall"
published_at: "2026-06-09T21:52:19.327+00:00"
first_seen_at: "2026-07-23T23:34:32.089077+00:00"
fetched_at: "2026-07-28T21:44:22.050751+00:00"
content_hash: "sha256:bb7df0ae973fbb273f283c32b9fcd0e4e1ba3ea9fbe452b2de6fca16fbd59252"
---

# Package Firewall: Blocking 8,000+ malicious packages daily

Replit already scans your projects for vulnerable dependencies, and audits your dependencies before you publish. But risk shows up earlier than that, while you are developing, the moment a malicious package gets installed.


Today we're launching **Package Firewall** , in partnership with **Socket (a software supply-chain security company)** . Package Firewall blocks malicious and compromised packages from ever being installed into your app, even while you are building. This network-level security protection eliminates any window for malware to be installed into your project. It's on by default for every builder, with nothing to set up.


Since rolling out a week ago, Package Firewall has been blocking around **8,000 packages per day** across builders on Replit. Over the course of a year, millions of vulnerable package installs will be blocked, leaving builders and their users safer:


> *“Replit is helping define how the next generation of software gets built, with AI agents working alongside developers to create and ship applications faster. That makes install-time security more important than ever. By partnering with Replit, Socket is enabling builders to move quickly while keeping malicious packages out of the development workflow and stopping supply chain attacks before a single line of malicious code runs."* — **Feross Aboukhadijeh, CEO, Socket**


## How it works


When you or the Agent run an install command like` npm install` or` pip install` to install a new dependency, the request passes through Package Firewall. If the package is clean, the install runs as usual and you won't notice a thing. If Socket has flagged the package as malicious or compromised, the install is blocked before any code reaches your environment.


When something is blocked, you get a clear message about what was stopped and why, with a link to Socket's findings for that package. The Agent sees the same signal, so it can suggest a safe alternative, like fixing a misspelled package name, or hand the decision back to you.


Blocking packages at install time is critical for a particular class of vulnerabilities, called malware. Most supply-chain tools only scan dependencies at publish time, long after a package has already been installed into your project. For ordinary vulnerabilities that may be fine, but malware is different: it does its damage the instant it's installed, stealing secrets, opening backdoors, or exfiltrating your environment variables. By the time a publish-time scan flags it, the attack has already run.


## What we're seeing in blocked packages


The packages blocked most often are not obscure, they are the workhorses of modern codebases: form-data, protobufjs, handlebars, fast-xml-parser, jspdf, grpc-go, pgx. The flagged versions carry verified critical and high severity vulnerabilities spanning remote code execution, path traversal, authorization bypass, prototype pollution, and denial of service.


The overwhelming majority of these advisories were disclosed in 2025 and 2026, after the training cutoff of the models recommending them. LLMs confidently suggest dependency versions that were genuinely known-good in their training data but have since been disclosed as vulnerable. In the most extreme case we saw a model reaching for a package that doesn't exist as a real dependency, and resolved to a malware placeholder, hallucinating a package that should never be installed at all.


These ways bad packages can be installed, even by LLMs, have names in the field:


- **Typosquats** : a malicious package with a name one keystroke away from a real one (` reqeusts` instead of` requests` ). You explicitly ask for a download of the wrong package.
- **Slopsquats:** LLMs sometimes hallucinate a package name that doesn't exist; attackers register that exact name and wait for the next agent to install it.
- **Stale recommendations:** a real package with a disclosed vulnerability, that the AI recommends because it was known-good before its training cutoff.


## This is just the start


Package Firewall is one part of **Replit Auto-Protect** , the set of protections we turn on by default for every organization, alongside Security Agent and Security Center.


Moving forward, we'll keep investing in tooling that protects builders by default, right as they're developing their apps. This complements the protections we already offer before you publish, like Security Agent and our pre-publish security review.
