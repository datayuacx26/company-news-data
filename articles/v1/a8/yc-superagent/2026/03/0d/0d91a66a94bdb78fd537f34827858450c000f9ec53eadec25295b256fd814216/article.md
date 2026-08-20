---
schema_version: "1.0.0"
document_id: "0d91a66a94bdb78fd537f34827858450c000f9ec53eadec25295b256fd814216"
company_key: "yc-superagent"
company: "Superagent"
source_id: "yc-superagent-news-import-3c33ed06ece7"
canonical_url: "https://www.superagent.sh/blog/frontier-models-miss-57-percent-of-threats"
published_at: "2026-03-24T12:00:00+00:00"
first_seen_at: "2026-07-24T02:47:34.464363+00:00"
fetched_at: "2026-07-28T22:19:21.440084+00:00"
content_hash: "sha256:f4db5aa6c815f0dce8f44bd98a4251cda691c67308d33374b63ec4d7bc20977b"
---

# Frontier models miss 57% of threats in agent context

Most agent builders assume the model itself will catch malicious content. We wanted to know if that's actually true.


We built[brin-bench](https://github.com/superagent-ai/brin-bench) to measure the gap. 485 real artifacts from[brin's](https://brin.sh/) scanning database, tested against Claude 4.6 Opus with a strong security system prompt. The model saw the same raw content a coding agent would encounter in production: packages, web pages, skills, repositories, contributors.


The model missed 57.3% of the threats brin had already identified. 106 out of 185 flagged artifacts went undetected.


The benchmark and full dataset are open-source at[github.com/superagent-ai/brin-bench](https://github.com/superagent-ai/brin-bench) .


## What we measured


Each artifact gets its full content (HTML, registry metadata, skill definitions, READMEs, contributor profiles) and a security-focused system prompt. The model classifies each artifact as safe or dangerous based on content alone. That's what a coding agent does today.


brin's own verdicts are the ground truth. We compare what the model flagged against what brin flagged, and count the misses.


Three metrics, reported overall and per category:


- **Model coverage** : of artifacts brin flagged, what percentage did the model also flag?
- **Model gap** : what the model missed, broken down by which brin signal drove the detection
- **False positive rate** : percentage of brin-safe artifacts the model incorrectly flagged


## Results by category


Category Brin flagged Model caught Model missed


Repositories 18 11.1% (2) 88.9% (16)


Packages 7 28.6% (2) 71.4% (5)


Web pages 50 30.0% (15) 70.0% (35)


Domains 50 34.0% (17) 66.0% (33)


Contributors 10 40.0% (4) 60.0% (6)


Skills 50 78.0% (39) 22.0% (11)


Skills had the lowest miss rate because the threats are content-visible: prompt injection sitting in plaintext. The model can read it and flag it.


Everything else depends on signals the model can't access.


## Results by signal type


brin scores artifacts across four dimensions. The model has zero visibility into three of them.


Signal type Brin flagged Model caught Model missed


Graph (dependency chains, cross-repo trust) 5 0% 100%


Identity (domain reputation, account age) 49 30.6% 69.4%


Behavior (install hooks, runtime patterns) 10 40.0% 60.0%


Content (what's in the artifact) 121 49.6% 50.4%


A model can only judge what's in front of it at one point in time. It can't check whether a domain was registered yesterday on a bulletproof host, whether a package was published two hours ago by a throwaway account, or whether a contributor went dormant for six months before suddenly submitting PRs to popular repos.


## Results by threat type


Threat Count Model missed


Blocklisted entities 22 100%


TLS failures (dead infrastructure) 4 100%


Install attacks 5 80.0%


Credential harvesting 5 80.0%


Encoded payloads 26 76.9%


Phishing 46 67.4%


Exfiltration 103 61.2%


Cloaking 8 50.0%


Typosquat 21 42.9%


Prompt injection 17 41.2%


100% of blocklisted entities passed undetected. 80% of credential harvesting. 77% of encoded payloads. These aren't exotic attacks. They're common, and they need external context to catch.


The false positive rate was 1.3% (4 out of 300 safe artifacts). The model isn't noisy. It just can't see enough.


## What this means if you build agents


If your security model is "the model will catch it," you have a 57% hole. The threats that get through are specifically the ones that require reputation data, behavioral history, and trust graphs to identify.


Coding agents fetch packages, load web pages, install skills, and read contributor profiles without reviewing any of it. Content-only classification catches about half the threats in the content itself, and nothing else.


The model spots a skill file with prompt injection. It does not spot a package published by a two-day-old account that mimics a popular library name. It does not spot a domain on a blocklist. It does not see that a contributor's activity graph looks like a sleeper account.


Those are the signals brin covers. Identity, behavior, content, and graph. The benchmark measures the cost of not having them.


## Limitations


This benchmark measures what you miss without brin. It does not measure what brin misses. Artifacts that both systems fail to detect are invisible by design. The contributor (10 flagged) and npm (7 flagged) categories have small flagged samples and should be read with that caveat.


## Methodology and data


The full methodology, per-threat breakdowns, raw results, and dataset are at[github.com/superagent-ai/brin-bench](https://github.com/superagent-ai/brin-bench) . The model path runs inside a[Shuru](https://github.com/superhq-ai/shuru) microVM for isolation, since the malicious artifacts in the dataset are real.


For background on what brin is and how it works:[Launching brin.sh](https://www.superagent.sh/blog/launching-brin-sh) .
