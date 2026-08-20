---
schema_version: "1.0.0"
document_id: "719af3ef349e880f44c94335adc81c2027f67c40689b4762293628d747c1a178"
company_key: "yc-stably-ai"
company: "Stably AI"
source_id: "yc-stably-ai-news-import-4d9fcc9f146f"
canonical_url: "https://www.stably.ai/blog/build-an-sre-agent-with-stably"
published_at: "2026-01-26T00:00:00+00:00"
first_seen_at: "2026-07-22T14:42:49.723725+00:00"
fetched_at: "2026-07-28T22:23:07.215004+00:00"
content_hash: "sha256:eb05cc37d8cfb76b8b821c81bcf931ab1c14c2d08a9f86924e0ac1c0ff864b55"
---

# Build an Autonomous SRE Agent with Stably

At 3am, your pager goes off. Latency spike. Error rate climbing. You drag yourself out of bed, SSH into production, stare at logs, form a hypothesis, push a fix, pray it works.


What if that entire workflow could run autonomously—while you sleep?


## The Dream: Closed-Loop Incident Response


The pieces are already here. You have observability platforms firing alerts. You have AI coding agents that can read code, understand context, and write fixes. What's missing is **validation** —a way for the agent to know if its fix actually works before declaring victory.


That's where Stably comes in.


Here's the architecture: take any alert from your observability stack, feed it to a coding agent (Claude Code, Cursor, Devin, or your own), and use Stably as the **confidence layer** . The agent doesn't just push a fix and hope. It spins up a test environment, runs regression tests, creates new tests for the specific failure, fixes any broken tests, and loops until it's as confident as possible that the issue is resolved.


## The Loop: Debug → Test → Fix → Repeat


### 1. Ingest the Alert


Your observability platform (Datadog, PagerDuty, Sentry, whatever) fires a webhook. The payload contains everything the agent needs: error message, stack trace, affected endpoint, timestamp, severity.


```text
// Example: Parse incoming alert
const alert = {
error: "TypeError: Cannot read property 'user' of undefined",
endpoint: "/api/checkout",
stackTrace: "...",
severity: "critical"
};


```


### 2. Let the Agent Investigate


Your coding agent (we'll use "stably" as a generic term for any AI coding agent) reads the alert, pulls the relevant code, and forms a hypothesis. It might grep through logs, read recent commits, or trace the call stack. The key insight: **the agent doesn't need to be right the first time** —it just needs to iterate.


### 3. Validate with Stably


Here's where it gets interesting. The agent runs` npx stably test` to execute your existing test suite against the proposed fix:


```text
npx stably test


```


This does two things:


- **Runs all Playwright tests** against your application
- **Reports results to Stably** for AI-powered analysis


If tests pass, great. If tests fail, the agent now has concrete feedback—not just a vague alert, but specific assertions that broke.


### 4. Auto-Fix Broken Tests


Sometimes the fix is correct, but the tests are outdated. Stably handles this with` npx stably fix` :


```text
npx stably fix


```


This command:


- **Analyzes failing tests** using AI
- **Proposes fixes** for broken locators, outdated assertions, or flaky selectors
- **Works automatically in CI** without needing manual run IDs


The agent can accept these fixes, re-run tests, and continue iterating.


### 5. Generate New Tests for the Bug


If the original bug wasn't covered by existing tests (and it often isn't—that's why it shipped), the agent creates a new test. Using Stably's SDK, this is just Playwright with AI superpowers:


```text
import { test, expect } from '@stablyai/playwright-test';


test('checkout handles missing user gracefully', async ({ page }) => {
// Navigate to checkout without auth
await page.goto('/api/checkout');


// AI assertion: describe what "correct" looks like
await expect(page).toMatchScreenshotPrompt(
'Should display login prompt, not crash'
);
});


```


The test gets added to the suite. Future regressions are caught automatically.


### 6. Loop Until Confident


The agent keeps iterating:


1. Push fix to preview environment
2. Run` npx stably test`
3. If failures, run` npx stably fix` or refine the code fix
4. Repeat until all tests pass


Only when the full test suite passes—including the new regression test—does the agent consider the incident resolved.


## Why This Works


### Deterministic Validation


The agent isn't guessing. Every iteration produces concrete pass/fail signals.` stably test` gives you the same results every time, so the agent can reason about cause and effect.


### AI-Native Feedback Loop


Stably's auto-fix isn't just string replacement. It uses AI to understand *why* a test failed and propose semantically correct fixes. This means the agent can trust the suggestions and iterate faster.


### Full Playwright Compatibility


There's no vendor lock-in. Every test is a standard Playwright test. You can run` npx playwright test` directly if you want. The agent works with your existing test infrastructure, not against it.


### Parallel Execution at Scale


When the agent needs to validate a fix, it can spin up 100+ parallel tests on Stably Cloud. What would take 20 minutes locally runs in under a minute. Faster feedback means faster resolution.


## The Practical Setup


Here's how to wire this up today:


**1. Configure Stably in your repo:**


```text
npx stably init


```


**2. Add the Stably Reporter to` playwright.config.ts` :**


```text
reporter: [['@stablyai/playwright/reporter']],


```


**3. Set environment variables:**


```text
export STABLY_API_KEY=your_api_key
export STABLY_PROJECT_ID=your_project_id


```


**4. Give your agent the commands:**


- ` npx stably test` — Run tests, report results
- ` npx stably fix` — AI-powered test repair


That's it. Your agent now has a validation layer. It can propose fixes, test them, and iterate—all without human intervention.


## The Future of On-Call


We're not replacing SREs. We're giving them superpowers. The agent handles the 3am grunt work: investigating, hypothesizing, testing, iterating. By morning, you wake up to a resolved incident and a pull request ready for review.


The pager still goes off. But now something else answers.


## Get Started


Ready to build your own SRE agent? Start with the[Stably CLI Quickstart](https://docs.stably.ai/) and wire up your first automated validation loop. The infrastructure is ready—you just need to connect the pieces.
