---
schema_version: "1.0.0"
document_id: "0745669d3cb85a5e2eb678cc6babc231e2ce7f0451c46abb3fb494976f249d1f"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-news-import-c7466264c659"
canonical_url: "https://www.usetusk.ai/resources/august-2025-changelog"
published_at: "2025-10-03T18:25:14.033+00:00"
first_seen_at: "2026-07-26T03:20:25.565537+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:1c7c3eb5dbe3f2f630edb857b3c5ef15eaa83dfe3847da5562d4a298acf6f5aa"
---

# August 2025 Changelog

## Release Notes


August 2025 brought improvements to the signal-to-noise ratio in Tusk’s tests and a renewed emphasis on detecting latent bugs with high-impact tests.


We expanded our **bug detection mode** by making its configurations default for all future organizations, and optimized the end-to-end user experience for finding edge cases.


To increase the value of Tusk's test suggestions, we improved how we score tests according to user-provided feedback. Tusk now also utilizes a mix of agentic grep and programmatic code search to gather deep context when iterating on tests.


Behind the scenes, we’ve been working on **Tusk Drift** , our next major product release that will change how API testing is done with AI. More details to come — here’s an early demo:


‍


‍


### Major Features


- **Agentic Self-Healing:** Tusk now utilizes agentic grep—where optimal—to gather code context when iterating on its tests. Fewer relevant tests will be excluded from Tusk's output due to test execution errors.
- **Optimized Bug Detection Mode** : Made PR check for generating only failing tests the default workflow for new organizations. Improved end-to-end user flow for quickly detecting bugs, while allowing users to override by using the PR/MR label to generate the full test suite. **‍**


### Testing Improvements


- **Better Learning From Feedback:** Improved relevance scoring for generated tests according to user-provided feedback on tests (i.e., thumbs up/down). Expect to see previously rejected test types be consistently excluded from Tusk's output.
- **Sandbox Stability:** Implemented robust error handling, timeouts, and retries for Tusk sandboxes to improve system stability


### DevEx Enhancements


- **CoverBot Cleanup:** CoverBot automatically closes its PR/MRs after 7 days by default. You have the ability to toggle this on and off. **‍**
- **Clearer Failure States:** Introduced new test generation retry logic and UI/UX that distinguishes between retry-able and terminal failures **‍**
- **Bulk Upload of Environment Variables:** Streamlined repo onboarding with a mass upload functionality for required environment variables
- **Faster Loading/Commits** : Repositories overview page now loads up to 5x faster. Committing only passing tests now happens without modal confirmation.
