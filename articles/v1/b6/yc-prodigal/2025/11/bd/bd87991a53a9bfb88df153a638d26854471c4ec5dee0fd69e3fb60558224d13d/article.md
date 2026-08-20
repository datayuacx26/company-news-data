---
schema_version: "1.0.0"
document_id: "bd87991a53a9bfb88df153a638d26854471c4ec5dee0fd69e3fb60558224d13d"
company_key: "yc-prodigal"
company: "Prodigal"
source_id: "yc-prodigal-rss-445231af9883"
canonical_url: "https://www.prodigaltech.com/blog/building-bullet-proof-us-address-validation-at-prodigal"
published_at: "2025-11-27T09:59:30+00:00"
first_seen_at: "2026-07-25T19:50:44.288158+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:3c7c48b5e890d25946405080756dbc36f62899e602b58a9a3c49c21afb680f32"
---

# Designing a deterministic, low-latency architecture for U.S. address validation in payment systems - Engineering Blog

At Prodigal, our payments platform is designed to process thousands of consumer transactions daily. Each of these relies on accurate, processor-compliant addresses. Incorrect addresses cause transaction declines, generate unnecessary support overhead and pose compliance risks. To tackle this, we developed an incremental, test-driven address validation module that is both ultra-fast and highly reliable.


## **Why we built this**


Prodigal’s payment rails are mission-critical. Our goal was to ensure that every transaction included:


- Support for all 50 U.S. states, Washington D.C., territories, and military (APO/FPO) codes.
- Immediate identification of formatting errors before data reached payment processors.
- Clear, field-specific error messages suitable for frontend display.
- Sub-millisecond validation speed for seamless user experiences.


## **Our development philosophy**


We approached the challenge with key engineering principles:


Principle Implementation at Prodigal


Tests as Specifications Our Vitest suite, comprising 36 cases, defined correctness from the start. Nothing shipped until tests passed.


TDD-inspired (but flexible) Many tests were defined upfront, while additional scenarios emerged during implementation, guided by real-world use.


Incremental, Not Big Bang Features arrived in manageable increments (regex checks, ZIP validation, territories support), simplifying reviews and integrations.


Explicit Errors > Silent Fails Every validation failure generates user-friendly error messages that clients can parse and provide necessary UX to handle various scenarios.


Edge Performance Priority ZIP datasets preload at startup, ensuring constant-time lookups.


## **Incremental delivery**


We structured our development in clear milestones:


Milestone Key Outcomes Lessons Learned


M1 – Format Guardrails Validated basic ZIP patterns, state codes, and lengths. Instantly removed ~80% of invalid inputs.


M2 – Territory Branch Introduced specialized validation paths for U.S. territories. Recognized that one size does not fit all regions.


M3 – ZIP Existence Check Integrated a comprehensive, in-memory ZIP database (~40k entries). Balanced data accuracy against resource efficiency (memory footprint <2MB).


M4 – Unicode & Symbols Ensured robust handling of international and accented characters. Improved reliability for diverse user inputs.


M5 – Military Codes Added support for military postal addresses without ZIP checks. Addressed critical edge cases identified during QA.


Each milestone was completed only when associated tests were green, ensuring continuous confidence.


## **Technical highlights**


Our validator is built using TypeScript, with key functionalities encapsulated in these sub-functions:


- Comprehensive validation including ZIP existence.
- Territory-specific validation bypassing ZIP checks.


**Performance benchmark:**


- Approximately **0.35 ms per validation** tested over 1 million iterations.


## **Tests as living documentation**


Our test suite doubles as documentation, clearly outlining expected behavior:


✓ Validates ZIP+4 format.


✓ Invalidates lowercase state.


✓ Accepts accented city names.


✓ Rejects address lines exceeding 256 chars.


✓ Accepts territory ZIP codes absent from the main DB.


Every PR references these tests explicitly, fostering clarity and ease of onboarding: **“To understand the module, run the tests because they document every rule.”**


## **Lessons for future projects**


- **Tests as specs:** Precise test definitions keep implementation and documentation aligned.
- **Early consideration of edge cases:** Territories and military addresses should be prioritized early.
- **High-quality error messages:** Immediate dividends across frontend, support, and QA teams.
- **Incremental delivery:** Reduces risk and ensures that each iteration meets real-world needs.
- **Flexible TDD approach:** Flexibility ensures practical gains in both speed and quality. Not always writing “test firsts” but choosing consciously when test first or test after leads to better design and choose accordingly.


## **Final thoughts**


At Prodigal, our success wasn't driven by complex heuristics but by a commitment to clarity, speed, and reliability. By placing our tests at the heart of development, we delivered a fast, transparent, and verifiable address validation module, incrementally and confidently.
