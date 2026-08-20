---
schema_version: "1.0.0"
document_id: "2158cf994c745335bf044c95f62e6ddac436acaf5a2f18b4b4b39671fcabece9"
company_key: "yc-momentic"
company: "Momentic"
source_id: "yc-momentic-news-import-348aec23cbaf"
canonical_url: "https://momentic.ai/blog/truth-driven-development"
published_at: "2025-12-29T00:00:00+00:00"
first_seen_at: "2026-07-22T04:45:46.439360+00:00"
fetched_at: "2026-08-19T18:58:31.052183+00:00"
content_hash: "sha256:19cc7d83f7f9df3b8979dbf654877c9d45715c3edd066d362dc3dd8aa991d512"
---

# Truth-Driven Development

AI-assisted code generation breaks the key assumption engineering workflows were built on.


That assumption is code is scarce. Code is now abundant. When code becomes abundant, verification becomes the bottleneck. And when verification becomes the bottleneck, the most valuable artifact in a codebase stops being the code.


[It becomes the truth](https://momentic.ai/blog/the-test-is-the-truth) .


This is the premise of Truth-Driven Development (TrDD): define what must be true, then let implementations come and go.


## AI makes code easy, and easy changes everything


Software development *was* constrained by the cost of writing code. That constraint forced focus and tradeoffs. It forced people to think carefully about what they built because the build itself was expensive.


Now you can produce code faster than humans can review it. You can get Claude or Codex or Devin to generate multiple "reasonable" implementations of the same feature. You can change code constantly without feeling any cost, until something breaks.


But this generates variance. Different structures, abstractions, assumptions, and edge cases. That variance is not inherently bad, but it's fatal if you don't have a stable definition of correctness. If you can regenerate the implementation at will, you need something else that stays fixed.


The old answer was TDD. TDD's original promise was straightforward:


1. Write a failing test (red).
2. Write code to make it pass (green).
3. Refactor safely (refactor).


This intent was solid. Tests act as a forcing function for clarity, and a safety harness for change. But in practice, TDD was slow, brittle, overly coupled to implementation details, and too reliant on developer discipline. So TDD became polarizing. Some loved it. Many abandoned it.[DHH declared it dead in 2014](https://momentic.ai/blog/test-driven-development) , and most of the industry moved on.


Moving on was kind of OK, because testing could afford to be reactive. The rate of change was slow enough for reactive to work. Testing didn't disappear; it just got demoted. It became a second-class citizen in the development workflow: something you added after the fact, something QA handled, something that lived downstream of the "real" engineering work. Code was the artifact. Tests were the chore.


That can no longer work. Velocity is too high to manually verify. So we have to shift back to treating testing as the primary constraint on what gets built, which forces a question most teams haven't had to answer clearly:


**What is your system of truth when code can change faster than you can inspect it?**


## What is TrDD?


Truth-Driven Development treats behavior-level tests as the primary artifacts of development and code as disposable output that must satisfy them. The truth comes before the implementation.


Here’s an example. You're building authenticated access to a billing page. Traditional workflow: write the auth logic, build the billing UI, then maybe add some tests afterward.


TrDD inverts this. Start by defining the behaviors that must be true:


- "A valid user can sign in and reach the dashboard."
- "A logged-out user cannot access /billing; they are redirected to /login."
- "After login, the user can access /billing and sees their plan and invoices."


These aren't test cases. *They're truths* . They describe what should happen, not how. No selectors, no internal architecture. Just outcomes.


Now the flow is:


- Run truth.
- It fails. /billing is reachable while logged out. That's your red state. You now have an unambiguous definition of "not done."
- Prompt Claude to fix it. It enforces an auth guard and redirects unauthenticated users.
- Re-run truth.
- The redirect passes, but billing content fails because invoices aren't wired up.
- Prompt again. Claude completes the implementation: billing data call, empty-state handling.
- Re-run truth. All behaviors pass. Green.


Now refactor freely. Let AI reorganize routes, restructure state, and rename components. It doesn't matter what the implementation looks like. The only thing that matters is whether truth holds. Truth stays stable because it validates behavior, not DOM structure or internal architecture. A refactor that changes every file but preserves behavior passes. A one-line change that breaks the auth redirect fails.


Release is allowed only if the truth suite passes in CI. Not "someone reviewed the PR." Not "QA spot-checked it." The system ships when truth is satisfied. Six months later, someone rewrites the auth system. If that rewrite breaks the redirect invariant, the failure surfaces in CI before users see it. That's the difference between "we have tests" and "we have truth."


We can generalize to this:


1. **Define truth** (behavior contracts).
2. **Use AI to generate implementation.**
3. **Verify against truth.**
4. **Refactor or regenerate freely** , because truth remains stable.


## Isn’t this just TDD, with a lowercase r to seem cool?


No. Two differences matter.


First, abstraction level.


- TDD as practiced was usually unit-level. Test this function. Mock that dependency. Assert this return value. Those tests were tightly coupled to implementation, which made them brittle and refactoring painful.
- TrDD operates at the behavior level. It doesn't care how you implement the auth guard. It cares whether a logged-out user can reach /billing.


Second, the role tests play. TDD was a developer discipline: a practice you adopted because it led to better design. You could skip it. Many did. TrDD is an operational necessity. When AI generates code faster than you can read it, you don't have the option of "we'll add tests later." Truth is the only thing standing between you and plausible wrongness at scale. Truth is how you keep the system anchored.


TDD said: write tests first, and your code will be better.


TrDD says: define truth first, or you won't know if your code is right at all.


## Engineering becomes truth design


TrDD does not demand that teams become purists, but does ask teams to accept a new reality:


- Code can and will change constantly.
- AI can and will generate plausible wrongness.
- The only way to move fast without breaking everything is to define correctness as executable truth.


TrDD changes what testing means. Testing stops being a phase you move through or a role you hire for. It becomes the governing constraint on the entire system. The test suite is the contract. CI enforces it. Nothing ships unless truth is satisfied.


This is not "more testing." It's testing as infrastructure. Just as you wouldn't ship without version control, you don't ship without truth passing.


It also changes where humans add value. When AI handles implementation, the human contribution moves upstream: specifying correct behavior, defining invariants, deciding what matters enough to encode as truth.


The differentiator is no longer "can you write the code." It's "can you define what must be true, clearly and completely." That's a different skill. It requires precision about behavior, not just familiarity with syntax.


## Build with truth


If you're building with AI, or planning to, you don't need more code generation. You need a better definition of correctness.


Momentic helps teams adopt Truth-Driven Development by turning product behaviors into stable, executable truth that keeps up with change.


If you want to see what TrDD looks like in your own app,[we'd love to show you](https://momentic.ai/) .
