---
schema_version: "1.0.0"
document_id: "a505007da30b603f301d8d240969d2a7d0b2c0d57e74e64b6b3e620122d5e21e"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-news-import-c7466264c659"
canonical_url: "https://www.usetusk.ai/resources/real-api-traffic-ai-generated-tests-evaluation"
published_at: "2026-03-16T05:28:15.863+00:00"
first_seen_at: "2026-07-26T03:20:25.565537+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:f8b3e69b5cd859926f041729415ebbc02e8880928418271f68e15ff41b00e050"
---

# Does Real API Traffic Make AI-Generated Tests Better? An A/B Evaluation of Tusk Drift

## **Summary**


We evaluated whether giving an AI coding agent access to real recorded API traffic improves the quality of generated unit tests. Using an A/B methodology, we prompted Claude Sonnet to write tests for a Node.js order processing API, once with access to Tusk Drift MCP (which provided recorded traces including HTTP requests, Postgres queries, and Redis calls), and once without. A separate LLM judge compared the outputs across five dimensions. Over 3 runs of 6 questions (18 total evaluations, 54 LLM invocations), the Tusk-augmented agent produced the preferred test suite in 67% of evaluations, won mock data quality in 100% of cases, and won test correctness in 56%. The strongest signal was that recorded traffic prevents "subtle wrongness" (e.g., wrong HTTP status codes, error body shapes, field names), which could lead to bugs that make tests pass while real integrations fail. The weakest area for Tusk-augmented output was error scenarios not captured in recorded traffic, where the non-Drift agent sometimes produced broader coverage.


## **1. Introduction**


### **1.1 Mock Data in AI-Generated Tests**


When AI coding agents generate unit tests for code that integrates with external APIs, they must fabricate mock data such as request payloads, response bodies, error formats, HTTP status codes. Without access to real API behavior, these mocks are guesses informed by TypeScript interfaces, SDK documentation, and code patterns. This works well enough for structure, but often produces subtly wrong values: placeholder IDs where UUIDs are expected, HTTP 200 where the real API returns 201, or simplified error envelopes that don't match what the API actually sends.


These subtly wrong mocks are worse than obviously wrong ones. They pass against the code as written, but miss bugs that would show up in production. If the code under test validates a UUID format, checks a specific HTTP status code, or parses an error response body, the invented mock data won't catch the problem.


### **1.2 Tusk Drift**


Tusk Drift is an observability tool that records API traffic from running applications: HTTP requests and responses, database queries, and other spans. It captures full request/response bodies with their actual field names, data types, status codes, and nested structures.


Tusk Drift exposes this data through an MCP (Model Context Protocol) server, making it directly accessible to AI coding agents. For example, when an agent needs to mock an order checkout response for a test, it can query Drift for a real recorded response instead of guessing.


### **1.3 Goal of Evaluation**


We designed an A/B evaluation to answer one question: Does access to real recorded API traffic, via Tusk Drift MCP, produce better AI-generated unit tests compared to an agent working from source code alone?


We measured "better" across five dimensions: mock data quality, response shape accuracy, test coverage, test correctness, and overall production-readiness.


## **2. Methodology**


### **2.1 System Under Test**


The evaluation target is drift-demo-order-api, a Node.js/Express order processing API with five external integrations. See the following table:


System Under Test — Integrations Integration Service Source File Reason for Selection


Payments Lemon Squeezy` src/services/payment.ts` A less-documented payment API means response shapes are harder to guess (webhook payloads especially)


Shipping rates Shippo` src/services/shipping.ts` Has complex nested responses like rates, parcels, addresses, carrier-specific fields


Email confirmations Resend` src/services/email.ts` Simple API but error responses and webhook payloads have non-obvious shapes


Order persistence PostgreSQL` src/db/` Real SQL queries with joins, transactions. Claude can guess simple queries but not exact schema/column names.


Order caching Redis` src/cache/redis.ts` Value is in the access pattern more so than the response shape


The \`createOrder\` function orchestrates all five services sequentially: insert order into Postgres, create a Lemon Squeezy checkout, get Shippo shipping rates, send a Resend confirmation email, and cache the result in Redis.


Tusk Drift had recorded 14 spans each for Lemon Squeezy, Shippo, and Resend API calls, plus 55 Postgres query spans and 30 Redis operation spans. All included full request/response payloads from real usage of the application.


### **2.2 Evaluation Questions**


We wrote six test-writing prompts covering different parts of the codebase, each targeting areas where external API mock data quality matters:


1. Payment API — Write unit tests for \`createCheckout\` covering the Lemon Squeezy checkout creation flow, including request payload structure and response handling.
2. Shipping — Write tests for \`getShippingRates\` verifying Shippo shipment creation and rate selection logic, including handling of multiple carriers and service levels.
3. Full order flow — Write unit tests for \`createOrder\` covering the full order creation flow across all external service calls (Postgres, Lemon Squeezy, Shippo, Resend, Redis).
4. Webhook handling — Write tests for the Lemon Squeezy webhook handler that processes payment completion callbacks, covering the webhook payload structure and order status updates.
5. Error handling — Write tests for how \`createOrder\` handles failures from external services (Lemon Squeezy, Shippo, Resend), including realistic error response payloads.
6. Email confirmation — Write tests for the order confirmation email step, covering the Resend API call including email content structure and how the confirmation ID is persisted back to the order.


All questions require test-writing (not descriptive answers) so the judge can evaluate concrete artifacts. Questions 4 and 5 specifically test Drift's value for webhook payloads and error responses, which are areas where recorded traffic may or may not help.


### **2.3 A/B Protocol**


For each question, there were three LLM invocations. They are as follows in chronological sequence:


**Test generation without Drift MCP.** Claude received the question with access to codebase tools (Read, Grep, Glob) but no access to Tusk Drift. The system prompt described the codebase architecture and explicitly noted no Drift access was available. The \`--strict-mcp-config\` flag ensured Drift tools were completely unavailable.


**Test generation with Drift MCP.** Claude received the identical question with access to both codebase tools and Tusk Drift MCP tools. The system prompt described the available Drift tools and encouraged their use for realistic mock data and API schema understanding.


**LLM-as-judge.** A separate Claude Sonnet instance received both responses (labeled only as "without" and "with" Drift) and was asked to do a blind comparison of them across five dimensions, with access to neither Drift nor codebase tools. The judge was instructed to be honest and specific.


### **2.4 Judging Criteria**


The judge evaluated each pair across five dimensions:


1. Mock Data Quality: Are mock payloads realistic? Do they match what the real API actually sends or returns, including webhook payloads and error responses? Are data types correct?
2. Response Shape Accuracy: Does the mock data structure match the actual API schema? Are there extra or missing fields? Would either mock cause issues if the code accesses fields not in the mock?
3. Test Coverage: Do both versions test the same scenarios? Are there edge cases one handles that the other doesn't? Would one set of tests catch bugs the other would miss?
4. Test Correctness: Would the tests actually pass if run against the real code? Are assertions correct and meaningful?
5. Overall Assessment: Which response would you trust more for production? What specific value did the recorded traffic data add or not add?


### **2.5 Statistical Design**


The full evaluation was run 3 times (runs) to reduce variance from LLM non-determinism. This produced 18 total comparisons (3 runs x 6 questions), requiring 54 LLM invocations (18 without-Drift + 18 with-Drift + 18 judge comparisons). All runs used Claude Sonnet with default temperature.


## **3. Results**


### **3.1 Quantitative Summary**


Overall Winner Per Evaluation (n=18) Winner Count Percentage


Drift MCP 12 67%


Non-Drift 2 11%


Tie / Mixed 4 22%


Winner by Dimension Across All 18 Evaluations Dimension Drift Wins Non-Drift Wins Tie


Mock Data Quality 18 0 0


Response Shape Accuracy 6 5 7


Test Coverage 7 7 4


Test Correctness 10 5 3


Overall Assessment 12 2 4


Win Rate by Question Type (Across 3 Runs) Question Drift Wins Non-Drift Wins Tie


Q1: Payment (Lemon Squeezy) 3 0 0


Q2: Shipping (Shippo) 2 1 0


Q3: Full order flow 1 0 2


Q4: Webhook handling 3 0 0


Q5: Error handling 1 1 1


Q6: Email (Resend) 2 0 1


### **3.2 Qualitative Findings**


### Strengths


**Mock data realism (100% win rate).** Every judge across all 18 evaluations noted that Drift-augmented tests used more realistic fixture data. Specific examples where the differences in realism were clear included Lemon Squeezy checkout URLs with query parameters (versus bare URLs without any signature structure), Shippo service level tokens, and UUIDs matching actual API formats versus placeholder strings like "checkout-xyz" or "email-123".


**Correctness of API-specific details.** Drift-augmented tests consistently got HTTP-level details for integrations right where the non-Drift agent hallucinated. The former was the winner on “Test Correctness” 55.6% of the time, compared to the latter’s 27.8%. This applied to status codes and error names.Tests for Lemon Squeezy returned HTTP 201 (Created) for checkout creation while the non-Drift agent incorrectly assumed 200 (OK) in multiple runs. In the case of Shippo, tests returned field-level error objects like \`{ address_to: { zip: \[...\] } }\`, while the non-Drift agent used Django-style non_field_errors. Resend SDK error names would use specific taxonomy (e.g., rate_limit_exceeded, validation_error) whereas the non-Drift agent used generic HTTP descriptions as a best guess.


**Fewer broken tests.** This is a downstream advantage of the above two points. In Run 3 Q4 (webhook handling), the non-Drift agent used a fake Lemon Squeezy order ID ("ls-order-456") that directly caused two test assertions to fail against the real code. The idempotency key format and the custom_data lookup behavior were both wrong because the invented ID didn't match what the code expected. The Drift agent, grounded in real data, used a real numeric ID ("234567") and got both assertions right.


**Tighter behavioral assertions.** Drift-augmented tests more frequently asserted on exact values rather than loose substring checks. This increases the likelihood of catching edge cases that occur in the real world. Specific examples that are significant include asserting on the Shippo order status, verifying city/state/zip rendered together versus a generic \`toContain("5")\`which matches anywhere in HTML, and complete INSERT parameter arrays with \`JSON.stringify(input.items)\` instead of skipping serialized parameters.


### Limitations


**Error handling and failure scenarios (Q5).** This was Drift's weakest question, producing the only Non-Drift win. In Run 2, the non-Drift agent generated nearly twice the test cases (18 vs 10). Error scenarios like network timeouts, DNS failures, connection refused didn't appear in recorded successful traffic, and the Drift agent didn't extrapolate these failure modes. The non-Drift agent, reasoning purely from code structure, produced broader failure coverage.


**TypeScript type fidelity.** Recorded traffic diverged at times from the codebase's own TypeScript interfaces. The Drift agent faithfully reproduced the real response for better or worse, introducing a type mismatch the non-Drift agent avoided by following the declared interface. Similarly, Drift-augmented tests sometimes used incomplete fixtures cast with \`as unknown as X\`, while non-Drift tests built complete typed objects.


**Test architecture.** Mock setup quality varied independently of traffic data access. to use \`jest.spyOn\` vs direct assignment and how to structure \`beforeEach\` — . In Run 2 Question 2, the non-Drift agent properly mocked the config module while the Drift agent omitted it, meaning the Drift tests wouldn't actually run. These are Jest knowledge issues, not data issues.


**Edge cases derived from code reasoning.** Float rounding edge cases, field omission tests, and cache-ordering invariants all came from careful reading of the source code. Recorded traffic provided no signal for these purely implementation-logic tests.


### **Recurring Pattern**


The judge in 12 of 18 evaluations explicitly stated that the best test suite would combine elements of both responses. The typical recommendation was to take Tusk Drift's realistic fixtures and grounded assertions, add the non-Drift agent's broader edge case coverage and cleaner TypeScript conformance. Neither approach alone produced the optimal result.


## **4. Discussion**


### **4.1 Mock Data Quality**


The 100% win rate on mock data quality might seem cosmetic. After all, if both test suites exercise the same code paths, does it matter whether the checkout ID is a real UUID or "checkout-xyz"? The evaluation, however, surfaced three scenarios where it concretely matters.


The first is status code sensitivity **.** If \`createCheckout\` ever adds a 201 response check (a reasonable defensive measure), only Drift-augmented tests would catch a regression. The non-Drift tests, using status 200, would pass against broken code.


Second is ID format validation. The Run 3 webhook test failure demonstrates this directly: using a fake ID format ("ls-order-456") produced a wrong idempotency key, causing two assertions to fail against the real implementation. Real data prevents this class of error.


Third is error body parsing. If error handling code inspects the error response body (in the case of showing a user-friendly message), Drift's accurate error envelopes would catch format mismatches that invented error shapes would miss.


### **4.2 Failure Mode Gap**


Tusk Drift's biggest weakness is error/failure scenario coverage. Recorded traffic from a working application captures the happy path and whatever errors happened to occur. It does not capture:


- Network-level failures (ECONNREFUSED, ETIMEDOUT, ENOTFOUND)
- Rate limiting responses (429) unless they occurred in production
- Service outages (503) unless observed
- Malformed input edge cases


Recorded traffic complements code-level reasoning about failure modes; it doesn't replace it. An ideal agent would use Drift data to ground its happy-path mocks in reality, then reason from code structure to cover failure scenarios.


### **4.3 Type Fidelity Tension**


Real API traffic sometimes contradicts the codebase's own TypeScript interfaces. Lemon Squeezy's jsonapi.version is typed as string in the code but comes back as an integer in real traffic. Shippo's zone field is typed as string but comes back as \`null\` in test mode.


Should mock data match the declared types (preventing TypeScript compilation errors) or match real API behavior (catching runtime bugs)? There's no clean answer, but the discrepancy itself is useful since it tells developers their type definitions are wrong and need updating.


## **5. Conclusion**


Access to real recorded API traffic via Tusk Drift MCP produced meaningfully better AI-generated tests in 67% of evaluations, with a 100% advantage on mock data realism and a 56% advantage on test correctness. The primary mechanism: recorded traffic prevents "subtle wrongness." This is the class of bugs where tests pass against invented mock data but fail against real API behavior. Wrong HTTP status codes, wrong error body formats, wrong ID formats, and wrong field names are all caught by grounding mocks in reality.


The approach has clear limitations. Error scenarios not captured in recorded traffic remain a gap, and the non-Drift agent sometimes produced broader failure-mode coverage by reasoning from code structure alone. TypeScript type fidelity can also be complicated when real API behavior diverges from declared interfaces.


The practical recommendation from this evaluation: use recorded traffic data to ground mock fixtures in reality, then layer on code-derived edge cases and failure scenarios. Neither approach alone produces optimal tests, albeitrecorded traffic eliminates an entire category of false confidence that pure code reasoning cannot.


---


## **Appendix**


**Model** : claude-sonnet-4-20250514 across all three stages, i.e., test generation without Drift, test generation with Drift, and judging of outputs


**Tooling** : Custom bash script (eval.sh) orchestrating Claude Code CLI invocations with \`--print\` mode, \`--strict-mcp-config\` for the non-Drift condition, and \`--dangerously-skip-permissions\` for automated execution


**Pre-recorded traffic** : 14 Lemon Squeezy spans, 14 Shippo spans, 14 Resend spans, 55 Postgres spans, 30 Redis spans. All include the full request/response payloads from the Order Processing API service in Tusk Drift


**Codebase** : Open-source[demo repo](https://github.com/Use-Tusk/drift-demo-order-api) using Node.js/Express, TypeScript, ~200 lines of core order processing logic across 5 service files. All files for the Drift MCP eval can be found in this public[GitHub directory](https://github.com/Use-Tusk/drift-demo-order-api/tree/main/drift-eval)
