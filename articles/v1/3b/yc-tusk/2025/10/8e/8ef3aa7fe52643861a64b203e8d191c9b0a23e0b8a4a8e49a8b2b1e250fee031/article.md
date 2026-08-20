---
schema_version: "1.0.0"
document_id: "8ef3aa7fe52643861a64b203e8d191c9b0a23e0b8a4a8e49a8b2b1e250fee031"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-news-import-c7466264c659"
canonical_url: "https://www.usetusk.ai/resources/july-2025-changelog"
published_at: "2025-10-03T18:30:13.058+00:00"
first_seen_at: "2026-07-26T03:20:25.565537+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:140b305585549a4ee2bb2aa3f5516a7cec0cd49f7d426ca4618d2b6fd7d84f5a"
---

# July 2025 Changelog

## Release Notes


In July, we introduced the ability for Tusk to maintain existing test files by addressing syntax errors, missing imports, and failing tests. We also boosted CoverBot's capabilities with coverage-guided test generation to ensure higher test coverage.


Enterprise customers with multiple squads can now configure directory-specific testing guidelines for different parts of their monorepos. They also enjoy self-serve access to Tusk usage analytics through our new[Analytics API](https://docs.usetusk.ai/analytics-api/overview) .


Every developer wants their tools to be fast. Which is why we've also introduced a bug detection mode for quickly surfacing only failing tests, made committing all passing tests instantaneous, and slashed test environment setup times by up to 90% for complex repos.


Enable "Only generate failing tests" for bug detection mode


### Major Features


- **‍** ‍ **AI Test Maintenance:** Tusk now intelligently self-heals errors in existing test files for modified symbols in a PR, reducing the occurrence of blocked deployments.
- ‍ **Coverage-Guided Test Generation:** CoverBot now uses code coverage feedback to automatically generate additional tests for uncovered lines, ensuring higher test coverage. **‍**


- **Accelerated Test Execution:** Introduced snapshot-based test execution environments that reduce setup time by up to 90% for complex repositories, resulting in faster test execution at scale.


- **Analytics API:** Access Tusk usage analytics via REST API endpoints for smooth integration with internal dashboards and reporting systems. **‍**


### Testing Improvements


- **Bug Detection Mode:** New testing mode that focuses exclusively on surfacing failing tests, providing targeted bug detection for teams not optimizing for coverage.
- **Faster Commits:** Committing or creating a PR with all Tusk passing tests selected now happens instantaneously.


- **Squad-Level Testing Guidelines:** Define different testing guidelines for subdirectories, allowing individual squads to maintain their own testing standards within the same repo.


### DevEx Enhancements


- ‍ **CoverBot UI:** Redesigned UI for scheduling and managing automated test backfills, including timezone-aware scheduling and improved configuration management. **‍**
- **Test Generation UI/UX:** Revamped empty and loading states in Tusk UI, made test-level feedback popover editable, among other polish items. **‍**
- **Trial Management:** Increased visibility of trial management features (e.g., banners/CTAs, billing page), making it easier for teams to manage their subscription status.
