---
schema_version: "1.0.0"
document_id: "666a4ecb35353bcf5e365461fef9e4e6bc9523454b6165866e05942eb4e9adeb"
company_key: "yc-howdy-com"
company: "Howdy.com"
source_id: "yc-howdy-com-news-import-7de6c48bfb23"
canonical_url: "https://www.howdylatam.com/blog/test-driven-development-senior-engineers"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-25T08:44:52.452861+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:0a7d1141a34dd07948f38577a6527e65195f09f96020f434b81e10616bfc216f"
---

# Test-Driven Development Isn't Dead. It's Just Not What the Book Promised

You've been in this meeting. Someone on the team proposes doing test driven development properly this sprint: write the failing test, write the minimum code to pass it, refactor, repeat. Red, green, refactor. Everyone nods. Two weeks later the whiteboard diagram is gone, and nobody mentions it again.


That cycle isn't a myth. Kent Beck's book describes a specific, disciplined practice: you write a test before any production code exists, you watch it fail for the right reason, you write the smallest amount of code that makes it pass, and only then do you clean up the design. Done as written, it's a design technique as much as a testing technique. The tests are a side effect. The real product is a codebase shaped incrementally by constant, tight feedback.


Where the dogma gets applied badly is in treating that loop as a universal law rather than a tool with a specific job. Strict test driven development assumes you're building something from a blank file, that requirements are stable enough to encode as an assertion, and that you have the time to let the design emerge one red bar at a time. Most senior engineers spend very little of their career in that situation.


## **Where Strict Test Driven Development Breaks in Real Production Teams**


Three things kill the textbook version on contact with a real job.


The first is legacy code. You don't get a blank file. You get a 4,000-line service class with no seams, written by someone who left the company three years ago, and a ticket that says "fix the null pointer in checkout." Michael Feathers wrote an entire book about this problem because Beck's book doesn't address it: you can't write a test first for code that already exists and has no dependency injection, no interfaces, and side effects wired directly into a database call. The actual first step here is writing characterization tests, tests that pin down what the code currently does, not what it should do. That's test-driven in spirit but it's not the red-green-refactor loop. It's closer to reverse engineering with a safety net.


The second is delivery pressure. Strict TDD asks you to write a test for a feature whose shape might change twice before lunch. When product is still deciding whether a field is required or optional, writing an exhaustive first test suite for it is a bet you'll lose. Senior engineers who insist on full test-first discipline in that situation usually end up throwing away half their tests when the requirements shift, which is exactly the kind of waste TDD is supposed to prevent.


The third is the sheer volume of code that carries no meaningful logic. A getter. A DTO mapper. A React component that takes a user prop and renders a name and an avatar. Writing a failing test first for <UserBadge user={user} /> before writing the component buys you nothing. There's no branch, no calculation, no state transition to get wrong. You're testing that JSX renders what you told it to render. That's not rigor, it's ceremony.


## **The Subset of Test Driven Development That Actually Survives**


Here's the part that doesn't get talked about enough: the engineers who ship reliable systems under real deadlines are not the ones who dropped TDD. They're the ones who narrowed it.


What survives is test-first for anything with actual decision logic: branching, math, state machines, anything where a wrong output is hard to notice by eye and expensive to catch late. What gets dropped is test-first for structural code: wiring, glue, pass-through components, configuration objects. The senior version of TDD isn't "test everything before you write it." It's "know which ten percent of your code is where bugs actually hide, and write that ten percent test-first."


This isn't a compromise that waters down Beck's idea. It's closer to what experienced teams were already doing before "TDD" became a certification-friendly buzzword: write the test first when the code is genuinely hard to get right on the first try, and write the test after (or not at all, for the truly trivial cases) when it isn't.


## **A Concrete Line: Pricing Logic vs. a React Prop Pass-Through**


Concrete example, because "business logic" is vague until you see it next to something that isn't.


Say you're building a pricing calculator for a subscription product: base price, a proration rule for mid-cycle upgrades, a regional tax multiplier, and a discount stack that has to apply in a specific order because the finance team says so. Write the test first here. Write it("applies percentage discount before flat discount when both are present") before you write a line of the calculation. You will get the discount ordering wrong on the first attempt. Everyone does. The test catches it in three seconds instead of in a support ticket from a customer who got charged the wrong amount four months from now. This is exactly the case Beck's book was written for: a rule complex enough that holding it entirely in your head while writing code is a losing bet.


Now take the <UserBadge user={user} /> component from earlier. It receives a user object and renders a name, an avatar, and an online-status dot. There's no rule to get wrong. A snapshot test or a quick visual check after the fact tells you everything a test-first cycle would have told you, at a fraction of the ceremony. Forcing a red-green-refactor loop onto that component doesn't make the codebase safer. It makes the pull request longer and the reviewer's eyes glaze over.


The line isn't "backend versus frontend" or "hard versus easy to write." It's "does getting this wrong cost you something you can't easily see at a glance." A pricing calculation, a rate limiter's edge cases, a state machine for an order's lifecycle, a date-boundary calculation across time zones: all of these earn a test-first approach. A component that hands props to markup does not.


## **What This Looks Like Week to Week**


In practice, this means your test suite is lopsided on purpose. The pricing module, the auth token refresh logic, the retry-with-backoff code, the CSV parser that has to handle three vendors' slightly different formats: those have tests written before or alongside the implementation, and they're specific about edge cases, not just happy paths. The controller that calls three services and returns their combined result, the config loader, the component library: those get tests, but written after, mostly to lock in behavior once it's stable, and nobody loses sleep over the ordering.


This also changes how you review code. A pull request that adds a discount rule with no test attached is a real finding, worth blocking on. A pull request that adds a new prop to a display component with no test attached is not something you should be commenting on, and if your team's process treats both the same way, that process is optimizing for a metric (coverage percentage) instead of the thing coverage was supposed to protect (confidence in the code that's actually risky).


The discipline that matters isn't "write tests first, always." It's knowing, before you write a line, whether the thing you're about to build is the kind of thing you can get subtly wrong. If it is, slow down and write the test first. If it isn't, write the code and move on. That judgment call is the actual skill. The red-green-refactor ritual is just one way of exercising it.


## **Where This Leaves the Argument**


Test driven development, as a strict practice, isn't something most production teams do, and it isn't something most production teams should do everywhere in the codebase. That's not a failure of the idea. It's a mismatch between a technique designed for one kind of problem, code whose correctness is hard to verify by inspection, and the reality that most of a codebase isn't that kind of problem. Applying it uniformly is why teams abandon it two weeks after the whiteboard session: they were paying the full cost of the discipline on code that never needed it.


The version that survives contact with a real deadline is narrower and, frankly, more useful than the book's version: test-first for the calculations, the state transitions, and the edge cases that keep you up at night, and normal engineering judgment for everything else. That's not TDD lite. It's TDD aimed at the part of the job where it actually pays for itself.
