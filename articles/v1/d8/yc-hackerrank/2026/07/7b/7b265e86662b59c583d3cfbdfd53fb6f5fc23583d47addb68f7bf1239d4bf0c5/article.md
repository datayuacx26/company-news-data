---
schema_version: "1.0.0"
document_id: "7b265e86662b59c583d3cfbdfd53fb6f5fc23583d47addb68f7bf1239d4bf0c5"
company_key: "yc-hackerrank"
company: "HackerRank"
source_id: "yc-hackerrank-rss-01583abef605"
canonical_url: "https://www.hackerrank.com/blog/how-chakra-scores-an-interview/"
published_at: "2026-07-24T17:05:56+00:00"
first_seen_at: "2026-07-24T17:48:13.647123+00:00"
fetched_at: "2026-07-28T20:32:33.872616+00:00"
content_hash: "sha256:84301db9e841185b5f6788c75ddaca09df36af2ae2d289283a65ce6a0ebeeaaf"
---

# How HackerRank’s Chakra Scores an Interview

[Hiring Technical Talent](https://www.hackerrank.com/blog/category/hiring-technical-talent/)


# How HackerRank’s Chakra Scores an Interview


Written By Danielle Bechtel | July 24, 2026


### Does an AI interviewer make up scores?


No.[Chakra’s](http://chakra.sh/) scoring is evidence-anchored: every score traces back to a specific, verbatim moment in the interview transcript. If there’s no relevant moment, the expectation isn’t scored at all. It’s marked Not Assessed, not guessed at.


### How does Chakra actually run an interview?


Three separate AI agents, each with one job:


- **Interview Creator Agent** works with the recruiter upfront to turn a job description into a structured interview plan: sections, sample questions, and explicit expectations.
- **Interviewer Agent** conducts the live, voice-based interview, following that plan, probing for depth, and covering every section in the allotted time.
- **Reporter Agent** runs after the interview ends. It reads the full transcript against the plan and scores each expectation independently.


### How does the scoring methodology actually work?


Each expectation gets scored on a 4-point scale, anchored strictly to transcript evidence:


- **3 (Met):** concrete evidence of clarity, ownership, structured reasoning, specific examples
- **2 (Partially Met):** some relevant evidence, but lacking depth or specificity
- **1 (Not Met):** incorrect reasoning or only vague, theoretical answers
- **0 (Not Assessed):** no relevant transcript moment exists


Scores are normalized to a 0–5 scale for the recruiter-facing report. Nothing in the report is inferred beyond what’s in the transcript, which is what prevents hallucinated scores and keeps every rating traceable back to something the candidate actually said.
