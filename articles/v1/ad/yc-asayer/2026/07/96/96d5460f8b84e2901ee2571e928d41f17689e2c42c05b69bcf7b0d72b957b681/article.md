---
schema_version: "1.0.0"
document_id: "96d5460f8b84e2901ee2571e928d41f17689e2c42c05b69bcf7b0d72b957b681"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/integration-vs-end-to-end-tests/"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T20:09:38.230352+00:00"
fetched_at: "2026-07-31T20:09:39.894782+00:00"
content_hash: "sha256:31ee44e4222fe588a15f8bc1f34d438731c5c72b8dd17a1014bc95a271465185"
---

# Integration Tests vs End-to-End Tests

Integration tests verify that *your own* components and modules work together — they run in CI in milliseconds and mock external systems; end-to-end tests drive the whole running app through the real UI like a user, run against a deployed or staging build, and mock nothing. That single distinction resolves most of the confusion, but it hides a second problem specific to frontend work: the word “integration” means something different to a JavaScript developer than it does to a backend QA engineer, and the line between “integration” and “E2E” in the browser is genuinely fuzzy.


This article draws that line in web-stack terms. It gives you crisp definitions with concrete examples, a comparison across the criteria that actually decide which to write, where each sits relative to unit tests, the JavaScript tools that map to each (Vitest, Jest, Testing Library, MSW, Playwright, Cypress), and a decisive recommendation on how to split your testing budget — not the “use both” hedge.


## Key Takeaways


- Integration tests render your own modules together with the network mocked and run in CI in milliseconds; end-to-end tests drive the deployed app in a real browser and mock nothing.
- In a JavaScript app, “integration” usually means Vitest or Jest plus Testing Library and MSW; “end-to-end” usually means Playwright or Cypress against your real URL.
- The frontend integration/E2E boundary is a spectrum, not a wall — where you draw it is a budget decision, not a law.
- Write mostly integration tests for the best confidence-per-cost, keep a small suite of three-to-five E2E tests on your highest-value journeys, and don’t try to E2E everything.
- A production bug is, by definition, a journey your tests never asserted, which makes a session replay of a real failure the fastest path to the missing test.


## Integration tests vs end-to-end tests at a glance


The two test types differ on every axis that matters for budgeting: how much they cover, how fast they run, where they run, what they fake, how often they break for the wrong reason, and which bugs only they can catch.


Criterion Integration test End-to-end (E2E) test


**Scope** Several of your own modules together (e.g., a component + its data layer) The whole running app, front to back


**Speed** Milliseconds to low seconds Seconds to minutes per test


**Where it runs** Locally and in CI, in Node or jsdom Against a deployed or staging build, in a real browser


**What it mocks** External systems — the network, third-party APIs Nothing (or as little as possible)


**Flakiness** Low — no real network, no real browser timing Higher — depends on the whole system staying still


**Cost / maintenance** Lower to write and keep green Higher; suites rot as the app changes


**Uniquely catches** Broken API contracts, mis-shaped responses, state wiring Auth, redirects, env config, third-party scripts, cross-page state


## What an integration test is, with a web example


An integration test renders a tree of your own components together and asserts their combined behavior with the network stubbed out, verifying that the units you already unit-tested actually cooperate. It does not boot a browser and it does not hit a real server.


A concrete example: a` LoginForm` component wired to an authentication hook and a profile fetch. The integration test renders the form, fills the fields, submits, and asserts that the welcome state appears — but the` POST /api/login` and` GET /api/profile` calls are intercepted and answered by a mock. You are testing the wiring between the form, the hook, the request layer, and the rendered result. You are *not* testing your real backend, your auth provider, or your CDN.


This is the layer where contract bugs surface: the component expects` user.name` but the handler returns` user.fullName` , a 401 path never re-renders the error banner, or a loading flag never clears. Integration tests catch the bugs that live *between* your modules — broken API contracts, mis-shaped responses, state wiring — without the cost of a real deployment.


## What an end-to-end test is, with a web example


An end-to-end test drives your actual deployed application in a real browser, exactly as a user would, and asserts on what the user sees — with nothing mocked. It exercises the real server, the real database, the real auth flow, real redirects, and any third-party scripts the page loads.


The canonical example is a user logging in and checking out: navigate to the live URL, type credentials, submit, land on the dashboard, add an item to the cart, complete payment, and confirm the order. Every layer participates — frontend bundle, network, API, database, session cookies, payment integration.


E2E tests catch the bugs that only exist in a real deployment: authentication and redirects, environment configuration, third-party scripts, content-security-policy violations, and cross-page state that survives a navigation. None of those reproduce when the network is mocked, which is exactly why E2E is worth its higher cost for the journeys that earn revenue.


## Where they sit: the pyramid and the trophy


Both test types sit above unit tests, which check a single function or component in isolation. The classic **testing pyramid** prescribes many unit tests at the base, fewer integration tests in the middle, and very few slow E2E tests at the top. It is a sound default for keeping a suite fast.


The modern counterpoint is the **testing trophy** , popularized by Kent C. Dodds, which fattens the integration layer because integration tests give the best confidence-per-cost for web applications — close to real usage, but without the flakiness and runtime of full E2E. The reasoning rests on Testing Library’s guiding principle that[the more your tests resemble the way your software is used, the more confidence they can give you](https://testing-library.com/docs/guiding-principles) . An integration test that renders a real component tree and asserts user-visible behavior resembles real usage far more than a unit test of an isolated reducer — and it does so in milliseconds. Treat the trophy as a widely adopted modern view for web apps, not a universal replacement for the pyramid.


## The fuzzy frontend line


In frontend work, the integration/E2E boundary is a spectrum, not a wall: a Testing Library test that renders a component subtree is “integration,” a Playwright test that loads your real URL is “E2E,” and where you draw the line is a budget decision, not a law. That fuzzy boundary is the single biggest source of confusion, because the same word means different things in different rooms.


Backend QA literature splits integration testing into *bottom-up* , *top-down* , and *big-bang* strategies — that taxonomy is real but it is server-integration jargon, and it rarely helps someone working in Vitest and Playwright. Ignore it. What matters for a web developer is the practical mapping: “integration” almost always means *rendering a component tree with the network mocked* , and “E2E” almost always means *driving the deployed app in a real browser* . The gray zone in the middle — say, a Playwright test pointed at a local dev server with MSW intercepting the network — is fine; just decide deliberately which side of the budget it comes from.


## The JS toolchain for each, with code


In a JavaScript app, an “integration test” usually means rendering a component tree and asserting its behavior with the network mocked — typically[Vitest](https://vitest.dev/) or[Jest](https://jestjs.io/) plus[Testing Library](https://testing-library.com/) and[MSW](https://mswjs.io/docs/) — while an “end-to-end test” means driving the deployed app in a real browser with[Playwright](https://playwright.dev/docs/intro) or[Cypress](https://docs.cypress.io/) .


Mock Service Worker (MSW) is the key piece that makes integration tests realistic without a real backend. It intercepts requests at the network layer in both environments — via the Service Worker API in the browser (` setupWorker` ), and via low-level request interception in Node (` setupServer` , which your Vitest or Jest tests use and which needs no service-worker file). Because interception happens at the boundary rather than by patching` fetch` , the same handlers power your integration tests, your dev server, and even your Playwright runs; the docs put it directly: “Imagine using the same API mocks during development, integration and end-to-end testing, and then in your Storybook.”


Here is the same login-and-fetch-profile feature as an integration test. Note the MSW v2` http` +` HttpResponse` API, which replaced the v1` res` /` ctx` pattern:


```text
// login.integration.test.tsx — Vitest 4 + @testing-library/react 16 + MSW 2
import   {   render  ,   screen   }   from   '  @testing-library/react  '
import   userEvent   from   '  @testing-library/user-event  '
import   {   http  ,   HttpResponse   }   from   '  msw  '
import   {   setupServer   }   from   '  msw/node  '
import   {   LoginForm   }   from   '  ./LoginForm  '


const   server   =   setupServer  (
http  .  post  (  '  /api/login  '  , ()   =>
HttpResponse  .  json  ({   token  :   '  abc  '   })
),
http  .  get  (  '  /api/profile  '  , ()   =>
HttpResponse  .  json  ({   name  :   '  Ada  '   })
)
)


beforeAll  (()   =>   server  .  listen  ())
afterEach  (()   =>   server  .  resetHandlers  ())
afterAll  (()   =>   server  .  close  ())


test  (  '  logs in and shows the profile name  '  ,   async   ()   =>   {
render  (  <  LoginForm   />  )
await   userEvent  .  type  (  screen  .  getByLabelText  (  /  email  /  i  ),   '  ada@example.com  '  )
await   userEvent  .  type  (  screen  .  getByLabelText  (  /  password  /  i  ),   '  hunter2  '  )
await   userEvent  .  click  (  screen  .  getByRole  (  '  button  '  , {   name  :   /  sign in  /  i   }))
expect  (  await   screen  .  findByText  (  /  welcome, ada  /  i  )).  toBeInTheDocument  ()
})
```


No browser launches, no server runs, and the network is fully controlled — so this test is fast and deterministic. The same feature as an E2E test runs against the deployed app and mocks nothing. Playwright drives Chromium, Firefox, and WebKit with one API, with auto-waiting and web-first assertions:


```text
// login.e2e.spec.ts — Playwright 1.6x
import   {   test  ,   expect   }   from   '  @playwright/test  '


test  (  '  user logs in and sees their profile  '  ,   async   ({   page   })   =>   {
await   page  .  goto  (  '  /login  '  )
await   page  .  getByLabel  (  '  Email  '  ).  fill  (  '  ada@example.com  '  )
await   page  .  getByLabel  (  '  Password  '  ).  fill  (  '  hunter2  '  )
await   page  .  getByRole  (  '  button  '  , {   name  :   '  Sign in  '   }).  click  ()
await   expect  (
page  .  getByRole  (  '  heading  '  , {   name  :   /  welcome, ada  /  i   })
).  toBeVisible  ()
})
```


The assertions look similar; what differs is everything underneath. The Playwright spec proves the real auth endpoint, redirect, and session cookie work in a real browser — and it pays for that proof in runtime and flakiness. One practical limit worth knowing: because async React Server Components are new,[Jest does not currently support them and Next.js recommends E2E tests for async components](https://nextjs.org/docs/app/guides/testing/jest) — a concrete case where the integration layer can’t yet reach and E2E earns its place.


## A decision rule by scenario


Pick the test type from the bug you are trying to prevent, not from habit. The rule:


- **New module interaction or a frontend-backend contract** → integration test. Wiring a component to a new endpoint, a new reducer to a new selector, a form to a validation layer: render the tree, mock the network with MSW, assert the behavior.
- **A critical user journey across the whole stack** → one E2E test. Login, signup, checkout, the one flow that, if broken, costs money or trust.
- **Everything else** → lean on integration, and do not E2E it.


The practical recommendation for a web team: write mostly integration tests for confidence-per-cost, keep a small suite of three-to-five end-to-end tests on your highest-value journeys, and don’t try to E2E everything. An E2E test for a tooltip is a maintenance liability; an integration test for it is cheap insurance.


## Why E2E suites rot, and how to keep them stable


End-to-end suites rot because every test depends on the entire system staying still; the fix is to keep them few, assert on user-visible outcomes rather than implementation details, and delete the ones that no longer protect revenue. A renamed test ID, a slower staging deploy, a flaky third-party widget, or a shifted redirect can turn a passing suite red without any real regression — and a suite that cries wolf gets ignored. Concretely:


- prefer role- and label-based selectors over brittle CSS paths;
- rely on Playwright’s auto-waiting instead of fixed sleeps;
- isolate test data; and
- cap the count so the suite stays fast enough to trust.


No suite covers the journeys you didn’t imagine, though — and a production bug is, by definition, a path your integration and E2E tests never asserted. Session replay earns its keep precisely here: a replay of a real failure shows the exact untested journey that broke — the DOM state, the network calls, and the console errors at the moment it failed. Session replays of login and checkout flows frequently reveal failure modes that no local test exercised, like an environment-specific redirect or a third-party script blocked by CSP. The craft move is to turn that reproduction into the missing test: a journey-level break becomes a new Playwright spec, a component or contract break becomes a new Testing Library + MSW integration test, and the same bug can’t regress.


## Conclusion


The split that holds up in production is mostly integration, a few high-value E2E, and unit tests underneath for pure logic — integration because it buys the most confidence per second of runtime, E2E because some bugs only exist in a real deployed browser. Start by writing integration tests with Vitest or Jest, Testing Library, and MSW for your next feature’s module boundaries, reserve Playwright for the handful of journeys you can’t afford to break, and let real production failures tell you which test you forgot to write.


## FAQs


Can a single tool like Playwright or Cypress write both integration and end-to-end tests?


Yes, but the distinction comes from how you scope the test, not the tool. Playwright and Cypress can mount a component in isolation with the network mocked, which is effectively an integration test, or load your deployed URL and exercise the whole stack, which is end-to-end. Component testing modes exist in both, while Vitest or Jest with Testing Library remains the more common integration path in the JavaScript ecosystem.


Do I need the mockServiceWorker.js file for MSW in Vitest or Jest integration tests?


No. The mockServiceWorker.js file is only required for the browser path using setupWorker, which relies on the Service Worker API. Node-based integration tests run through setupServer, which intercepts requests using low-level class extension rather than a service worker, so no generated file is needed. You only generate the worker script when mocking requests in a real browser, such as during local development or browser-based test runs.


When should I write an end-to-end test instead of an integration test?


Write an end-to-end test when the bug you are preventing only exists in a real deployment — authentication, redirects, environment configuration, session cookies, third-party scripts, content-security-policy violations, or cross-page state. These never reproduce when the network is mocked. For everything that lives between your own modules, such as broken API contracts, mis-shaped responses, or state wiring, an integration test is faster, cheaper, and less flaky. Reserve end-to-end tests for your highest-value revenue journeys.


Why do my end-to-end tests pass locally but fail in CI?


End-to-end suites depend on the entire system staying still, so differences between local and CI environments break them without any real regression. Common causes include fixed sleeps instead of auto-waiting, brittle CSS selectors that shift between builds, slower CI deploy timing, shared or non-isolated test data, and flaky third-party widgets. Prefer role- and label-based selectors, rely on Playwright's auto-waiting, isolate test data per run, and keep the suite small enough to stay reliable.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
