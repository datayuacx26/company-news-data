---
schema_version: "1.0.0"
document_id: "66ee6dab9460999500dd276b3a19eb9a0d5e11a296a2baebe52c7f561d65b352"
company_key: "yc-scrimba"
company: "Scrimba"
source_id: "yc-scrimba-news-import-2d86273fd3f5"
canonical_url: "https://scrimba.com/articles/how-to-test-react-apps-2026/"
published_at: "2026-07-28T11:31:59+00:00"
first_seen_at: "2026-07-28T19:22:09.547057+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:511797a5f7d97c60160b575232423934167166e7e34f97118873c3eb359e69ea"
---

# How to Test React Apps [2026]

React testing in 2026 means **Vitest** as the test runner, **React Testing Library** for rendering and querying components, **user-event** for interactions, and **Mock Service Worker** for the network. Playwright covers the handful of end-to-end tests on top. You test what a user can observe, never what the component stores internally.


Most React tutorials, including Scrimba's own free[Learn React](https://scrimba.com/learn-react-c0e?ref=scrimba.com) , stop at "it works on my screen." That is the honest gap. Testing is the step between building React apps and being trusted with someone else's React codebase.


This guide runs from setup to mocking, hooks, and the failure patterns that make a suite worse than no tests at all. Every version number is dated and sourced.


## Why Test a React App at All, and What Is Worth Testing?


React tests exist so you can change code tomorrow without breaking things you forgot about. They document behavior, catch regressions, and make refactoring safe.


Tests rarely find the bug you would otherwise ship. They earn their keep on the *second* change, when a component you wrote three months ago gets a new prop and you need to know whether the old paths still work.


Kent C. Dodds' **Testing Trophy** is the most useful shape for deciding where effort goes ([Write tests. Not too many. Mostly integration.](https://kentcdodds.com/blog/write-tests?ref=scrimba.com) ). Four layers, bottom to top:


1. **Static analysis** - TypeScript and ESLint, catching typos before a test runs.
2. **Unit tests** - one function, one behavior. Fast, low confidence on their own.
3. **Integration tests** - several units together. For React, a component with its real children.
4. **End-to-end tests** - the whole app in a real browser. Most confidence, slowest, most brittle.


> Write tests. Not too many. Mostly integration.


The trophy is fat in the middle on purpose. Component tests sit in that integration band, which is why they are the ones to write first. Verifying a button in isolation proves little if the form around it never calls the handler.


So what deserves a test? Behavior a user can observe: conditional rendering, form validation, loading and error states, and anything with a bug report attached to it. The static layer is not optional either. Scrimba's[TypeScript guide for JavaScript developers](https://scrimba.com/articles/how-to-learn-typescript-a-guide-for-javascript-developers-2026/) covers the base of the trophy, and types eliminate a class of tests you would otherwise hand-write.


## The 2026 React Testing Toolchain


The modern stack is a runner, a rendering library, and a network mock, each chosen independently.


Tool What it is for When to reach for it


Vitest Test runner and assertion library Default for anything built with Vite


Jest Test runner and assertion library Legacy Create React App projects and React Native


React Testing Library Renders components, queries the DOM Every React component test


user-event Simulates real user interactions Clicks, typing, keyboard, form entry


MSW Intercepts network requests Any component that fetches data


Vitest Browser Mode Component tests in a real browser When jsdom starts lying to you


Playwright End-to-end tests across real browsers Critical user journeys, kept few


**Vitest 4.1.10** is current, requiring Node 20 or later and Vite 6 through 8 ([npm registry](https://registry.npmjs.org/vitest/latest?ref=scrimba.com) ). It reads your existing` vite.config` by default, so aliases and plugins already work ([Vitest guide](https://vitest.dev/guide/?ref=scrimba.com) ).


The Vitest versus Jest question is largely settled for new React projects. In the npm week of July 14 to July 20, 2026,` vitest` recorded **79,327,217** downloads against Jest's **43,836,989** ([npm downloads API](https://api.npmjs.org/downloads/point/last-week/vitest?ref=scrimba.com) ). Jest is still correct for React Native, which Vitest does not support, and for older Create React App codebases. Vitest deliberately mirrors Jest's API, so switching later is mostly find and replace.


React Testing Library is at **16.3.2** and supports React 18 and 19 ([npm registry](https://registry.npmjs.org/@testing-library/react/latest?ref=scrimba.com) ). React itself is at 19.2.7. At 47.6 million weekly downloads, RTL is the assumed default in nearly every React codebase you will join.


## Setting Up Your First React Test


Setup is one install and one config file. Vitest supplies the runner, jsdom supplies a fake browser, React Testing Library supplies` render` and` screen` .


```text
npm i -D vitest jsdom @testing-library/react @testing-library/dom @testing-library/user-event @testing-library/jest-dom


```


React Testing Library v16 requires` @testing-library/dom` as an explicit peer dependency, which is a common first-run failure.


```text
// vitest.config.ts
import { defineConfig } from "vitest/config";
import react from "@vitejs/plugin-react";


export default defineConfig({
plugins: [react()],
test: {
environment: "jsdom",
globals: true,
setupFiles: "./vitest.setup.ts",
},
});


```


Two settings carry weight.` environment: "jsdom"` swaps Vitest's default Node environment for a simulated browser with` window` and` document` ([Vitest environments](https://vitest.dev/guide/environment?ref=scrimba.com) ).` globals: true` is what makes React Testing Library's automatic cleanup run, because cleanup hooks into a global` afterEach` ([RTL setup](https://testing-library.com/docs/react-testing-library/setup?ref=scrimba.com) ). Skip it and state leaks between tests in ways that are miserable to debug.


The setup file adds the jest-dom matchers, and a` // @vitest-environment jsdom` docblock overrides the environment per file:


```text
// vitest.setup.ts
import "@testing-library/jest-dom/vitest";


```


Now the first test:


```text
import { render, screen } from "@testing-library/react";
import Greeting from "./Greeting";


test("renders a greeting", () => {
render(<Greeting name="Ada" />);
expect(screen.getByRole("heading", { name: /hello, ada/i })).toBeInTheDocument();
});


```


That shape is *arrange, act, assert* , and it is the same in every JavaScript test runner. Scrimba's[Introduction to Unit Testing](https://scrimba.com/introduction-to-unit-testing-c01i?ref=scrimba.com) , 86 minutes with Dylan C. Israel, drills exactly that vocabulary: the three A's, grouping with` describe` , setup with` beforeEach` , spies, mocks, and matchers. It teaches with Jasmine rather than Vitest, so treat it as the concepts course, not the React one.


## How Do You Test Components, Props, and State?


You render the component, query the DOM the way a user would find things, and assert on what appears. Props go in, rendered output comes out.


React Testing Library ranks its queries by how closely they resemble real usage ([query priority](https://testing-library.com/docs/queries/about/?ref=scrimba.com) ). In order:` getByRole` ,` getByLabelText` ,` getByPlaceholderText` ,` getByText` ,` getByDisplayValue` , then` getByAltText` and` getByTitle` . Last is` getByTestId` , which the docs describe as only for cases where you cannot match by role or text.


> The more your tests resemble the way your software is used, the more confidence they can give you.


That line from the[React Testing Library docs](https://testing-library.com/docs/react-testing-library/intro/?ref=scrimba.com) is the entire design philosophy in one sentence.


Three prefixes cover every situation:


- ` getBy...` throws if the element is missing. Use it for things that should be there now.
- ` queryBy...` returns` null` instead of throwing. Use it *only* to assert absence.
- ` findBy...` returns a promise that resolves when the element appears. Use it for anything async.


Testing props is direct. State is where people go wrong, because the temptation is to reach for the state value itself:


```text
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import Counter from "./Counter";


test("increments the count when clicked", async () => {
const user = userEvent.setup();
render(<Counter initial={0} />);


expect(screen.getByText("Count: 0")).toBeInTheDocument();
await user.click(screen.getByRole("button", { name: /increment/i }));
expect(screen.getByText("Count: 1")).toBeInTheDocument();
});


```


Nothing there knows the component uses` useState` . Swap it for` useReducer` tomorrow and the test still passes, which is exactly the property you want.


## Testing User Interaction with user-event


Call` userEvent.setup()` before rendering, then await every interaction. user-event dispatches the full sequence of events a real browser fires, not a single synthetic one.


The difference from` fireEvent` is not cosmetic. Typing one character involves focus, keydown, keypress, input, keyup, and selection handling.` fireEvent` dispatches one event and moves on. user-event runs the whole sequence and adds visibility and interactability checks, so it refuses to click something a user could not click ([user-event docs](https://testing-library.com/docs/user-event/intro?ref=scrimba.com) ).


```text
test("submits the form", async () => {
const user = userEvent.setup();
const onSubmit = vi.fn();
render(<SignupForm onSubmit={onSubmit} />);


await user.type(screen.getByLabelText(/email/i), " [email protected]  ");
await user.selectOptions(screen.getByLabelText(/plan/i), "pro");
await user.click(screen.getByRole("button", { name: /sign up/i }));


expect(onSubmit).toHaveBeenCalledWith({ email: " [email protected]  ", plan: "pro" });
});


```


Every interaction is asynchronous. Forgetting an` await` is the single most common cause of tests that pass locally and fail in CI.


## Testing Async Behavior and Data Fetching


Use` findBy` queries to wait for elements that appear after a promise resolves. Save` waitFor` for assertions that are not element lookups.


A component that fetches has three states worth testing: loading, success, and failure.


```text
test("shows the user after loading", async () => {
render(<UserProfile id="abc-123" />);


expect(screen.getByText(/loading/i)).toBeInTheDocument();
expect(await screen.findByRole("heading", { name: /john maverick/i })).toBeInTheDocument();
expect(screen.queryByText(/loading/i)).not.toBeInTheDocument();
});


```


Prefer` await screen.findByX(...)` over wrapping a` getBy` in` waitFor` ; the built-in retry gives better error messages. Put only assertions inside a` waitFor` callback, never side effects, since it runs repeatedly until it passes.


That test still needs a fake server.


## Mocking APIs with MSW


Mock Service Worker intercepts requests at the network layer, so your component keeps calling` fetch` exactly as it does in production. Nothing in it changes.


Vitest recommends it by name in its own docs: *"We recommend Mock Service Worker to accomplish this. It allows you to mock http, WebSocket and GraphQL network requests, and is framework agnostic"* ([mocking requests](https://vitest.dev/guide/mocking/requests?ref=scrimba.com) ). At 19.2 million weekly downloads, it is the ecosystem's default answer.


Handlers describe a response per endpoint ([MSW quick start](https://mswjs.io/docs/quick-start?ref=scrimba.com) ):


```text
// src/mocks/handlers.ts
import { http, HttpResponse } from "msw";


export const handlers = [
http.get("https://api.example.com/user", () => {
return HttpResponse.json({ id: "abc-123", firstName: "John", lastName: "Maverick" });
}),
];


```


Then wire the server into the test lifecycle:


```text
// vitest.setup.ts
import { setupServer } from "msw/node";
import { handlers } from "./src/mocks/handlers";


const server = setupServer(...handlers);


beforeAll(() => server.listen({ onUnhandledRequest: "error" }));
afterEach(() => server.resetHandlers());
afterAll(() => server.close());


```


Set` onUnhandledRequest: "error"` deliberately. It fails the test when a request has no matching handler, surfacing the fetch you forgot instead of quietly hitting the real network. For a single test needing a different response, override at runtime with` server.use(...)` .


The alternative, stubbing` global.fetch` with` vi.fn()` , couples every test to the client you use today. Move from` fetch` to Axios and the MSW tests keep passing while the stubs all break.


## How Do You Test Custom Hooks?


Use` renderHook` , which mounts the hook inside a throwaway component and returns` result` ,` rerender` , and` unmount` . Read the current value from` result.current` .


```text
import { renderHook, act } from "@testing-library/react";
import { useCounter } from "./useCounter";


test("increments", () => {
const { result } = renderHook(() => useCounter(0));
act(() => result.current.increment());
expect(result.current.count).toBe(1);
});


```


The` act()` wrapper is required here because you are calling a state updater from outside React's callstack. This is one of the few places you should write` act` yourself ([fixing act warnings](https://kentcdodds.com/blog/fix-the-not-wrapped-in-act-warning?ref=scrimba.com) ).


The RTL docs treat` renderHook` as a tool for library authors and suggest preferring` render` with a small test component, because that produces *more readable and robust tests* ([RTL API](https://testing-library.com/docs/react-testing-library/api/?ref=scrimba.com) ). For a hook used by one component, test the component.


## Common Failure Patterns, and What Not to Test


Most bad React suites fail the same six ways, each costing maintenance time and returning no confidence.


1. **Testing implementation details.** Kent C. Dodds defines these as *"things which users of your code will not typically use, see, or even know about"* ([testing implementation details](https://kentcdodds.com/blog/testing-implementation-details?ref=scrimba.com) ). Testing internal state produces both failure modes at once: false negatives, where renaming a variable breaks a working component's tests, and false positives, where an unwired click handler ships green.
2. **Brittle selectors.**` container.querySelector(".btn-primary")` breaks the moment someone renames a class. Query by role and accessible name instead, which catches accessibility regressions for free.
3. **Act warnings.** These appear when a state update happens outside React's callstack, usually from an unawaited async operation. The fix is an assertion that waits for the result, not a stray` act()` wrapper. RTL's async utilities already wrap` act` internally.
4. **Over-mocking.** Mock the network, not your own modules. A test where every dependency is a stub verifies that your mocks work.
5. **Snapshot sprawl.** Large snapshots get regenerated on failure without anyone reading the diff. Assert on the text or attribute that matters.
6. **Coverage as a target.** Vitest ships v8 coverage via` npm i -D @vitest/coverage-v8` and` vitest run --coverage` ([coverage docs](https://vitest.dev/guide/coverage?ref=scrimba.com) ). Use it to find files nobody has tested. A team chasing 100% writes tests for getters.


Some things are not yours to test: third-party libraries, which ship their own suites; CSS, which jsdom does not render; React itself; and static markup with no logic in it.


## Where End-to-End Tests Fit


End-to-end tests drive a real browser through a real app. Playwright is the standard choice, covering Chromium, WebKit, and Firefox from one API ([Playwright docs](https://playwright.dev/docs/intro?ref=scrimba.com) ):


```text
npm init playwright@latest


```


Keep the count small and reserve them for journeys where failure is unacceptable: sign up, log in, check out. They are slow and they break for reasons unrelated to your code.


There is a middle option now. **Vitest Browser Mode** runs component tests in a real browser instead of jsdom, with Playwright or WebdriverIO as the provider ([browser mode](https://vitest.dev/guide/browser/?ref=scrimba.com) ). It earns its keep when jsdom's approximations of layout, focus, or scrolling start producing results a real browser would not.


## Where Scrimba Fits


Scrimba's[Testing in React](https://scrimba.com/testing-in-react-c01lu4ca7m?ref=scrimba.com) is the direct match for this article: 56 minutes across 12 lessons with Shant Dashjian, covering how to write and run tests with Vitest-style tooling and React Testing Library, user interactions and async behavior, mocking with Mock Service Worker, code coverage, accessibility-first testing, and test-driven development. It works against a real project rather than contrived examples and assumes you already know React. It does not cover Cypress or Playwright, backend testing, or CI setup.


The prerequisite is React itself. The free[Learn React](https://scrimba.com/learn-react-c0e?ref=scrimba.com) course runs 15.1 hours with Bob Ziroll across JSX, props, state, and effects, with two capstone projects and a completion certificate on the free tier. It deliberately does not cover testing, which is what makes the pairing work. Scrimba's[guide to learning React](https://scrimba.com/articles/how-to-learn-react/) sets the order, its[React course roundup](https://scrimba.com/articles/best-react-courses-and-tutorials-compared-2026/) compares the options, and its[React project ideas](https://scrimba.com/articles/best-react-projects-for-beginners/) give you something worth testing.


Both testing courses sit behind Pro, at $24.50 per month on the annual plan ($294 per year), with student pricing and location-based discounts available. A portfolio project with a passing test suite is a different interview conversation, as Scrimba's guide to[landing your first developer job](https://scrimba.com/articles/how-to-land-your-first-developer-job/) covers in more depth.


## Frequently Asked Questions


### Should I use Vitest or Jest for React in 2026?


Use Vitest for any new React project built with Vite. It reads your existing Vite config, runs faster, and uses a Jest-compatible API. Choose Jest for React Native, which Vitest does not support, and for older Create React App codebases.


### What is React Testing Library used for?


React Testing Library renders React components into a real DOM and provides queries for finding elements the way a user would, by role, label, or visible text. It is not a test runner. You pair it with Vitest or Jest, which supply assertions and mocking.


### How do I test a React component that fetches data?


Intercept the request with Mock Service Worker rather than stubbing fetch. Define a handler for the endpoint, start the server in beforeAll, and reset handlers after each test. Assert the loading state first, then use an await findBy query for the resolved content.


### Do I need to test every component?


No. Test components with logic: conditional rendering, form validation, async states, and anything with a past bug. Presentational components that only render props rarely justify a test. Coverage tools help you find untested files, but the number itself is a poor target.


### How long does it take to learn React testing?


You can write a first passing component test in an afternoon. Reaching the point where you mock APIs, test hooks, and avoid brittle selectors takes a few weeks of practice. Scrimba's Testing in React course covers that ground in 56 minutes of guided lessons.


## Key Takeaways


- The 2026 React testing stack is Vitest 4.1.10 as the runner, React Testing Library 16.3.2 for rendering, user-event for interactions, and MSW for the network.
- Vitest passed Jest in npm downloads, 79.3 million to 43.8 million in the week of July 14 to July 20, 2026. Jest remains required for React Native.
- Component tests sit in the integration band of the Testing Trophy, where most testing effort belongs.
- Query by role and accessible name first.` getByTestId` is the last resort, not the default.
- Call` userEvent.setup()` before rendering and await every interaction. Use` findBy` for async elements, and` queryBy` only to assert absence.
- Mock at the network layer with MSW, and set` onUnhandledRequest: "error"` so a missing handler fails loudly.
- Testing implementation details produces brittle tests that break on refactors and pass on real bugs. Do not test third-party libraries, styling, or React itself.
- Scrimba's Testing in React runs 56 minutes with Shant Dashjian on Pro, and the free Learn React course covers the fundamentals it assumes.


## Sources


- React Testing Library. "Intro." 2026.[https://testing-library.com/docs/react-testing-library/intro/](https://testing-library.com/docs/react-testing-library/intro/?ref=scrimba.com)
- React Testing Library. "About Queries." 2026.[https://testing-library.com/docs/queries/about/](https://testing-library.com/docs/queries/about/?ref=scrimba.com)
- React Testing Library. "API." 2026.[https://testing-library.com/docs/react-testing-library/api/](https://testing-library.com/docs/react-testing-library/api/?ref=scrimba.com)
- React Testing Library. "Setup." 2026.[https://testing-library.com/docs/react-testing-library/setup](https://testing-library.com/docs/react-testing-library/setup?ref=scrimba.com)
- Testing Library. "user-event Intro." 2026.[https://testing-library.com/docs/user-event/intro](https://testing-library.com/docs/user-event/intro?ref=scrimba.com)
- npm. "@testing-library/react registry metadata." 2026.[https://registry.npmjs.org/@testing-library/react/latest](https://registry.npmjs.org/@testing-library/react/latest?ref=scrimba.com)
- Vitest. "Getting Started." 2026.[https://vitest.dev/guide/](https://vitest.dev/guide/?ref=scrimba.com)
- Vitest. "Test Environment." 2026.[https://vitest.dev/guide/environment](https://vitest.dev/guide/environment?ref=scrimba.com)
- Vitest. "Mocking Requests." 2026.[https://vitest.dev/guide/mocking/requests](https://vitest.dev/guide/mocking/requests?ref=scrimba.com)
- Vitest. "Coverage." 2026.[https://vitest.dev/guide/coverage](https://vitest.dev/guide/coverage?ref=scrimba.com)
- Vitest. "Browser Mode." 2026.[https://vitest.dev/guide/browser/](https://vitest.dev/guide/browser/?ref=scrimba.com)
- npm. "vitest registry metadata." 2026.[https://registry.npmjs.org/vitest/latest](https://registry.npmjs.org/vitest/latest?ref=scrimba.com)
- npm. "Weekly download counts." 2026.[https://api.npmjs.org/downloads/point/last-week/vitest](https://api.npmjs.org/downloads/point/last-week/vitest?ref=scrimba.com)
- Mock Service Worker. "Quick Start." 2026.[https://mswjs.io/docs/quick-start](https://mswjs.io/docs/quick-start?ref=scrimba.com)
- Playwright. "Installation." 2026.[https://playwright.dev/docs/intro](https://playwright.dev/docs/intro?ref=scrimba.com)
- Kent C. Dodds. "Write tests. Not too many. Mostly integration." 2016.[https://kentcdodds.com/blog/write-tests](https://kentcdodds.com/blog/write-tests?ref=scrimba.com)
- Kent C. Dodds. "Testing Implementation Details." 2020.[https://kentcdodds.com/blog/testing-implementation-details](https://kentcdodds.com/blog/testing-implementation-details?ref=scrimba.com)
- Kent C. Dodds. "Fix the not wrapped in act(...) warning." 2020.[https://kentcdodds.com/blog/fix-the-not-wrapped-in-act-warning](https://kentcdodds.com/blog/fix-the-not-wrapped-in-act-warning?ref=scrimba.com)
