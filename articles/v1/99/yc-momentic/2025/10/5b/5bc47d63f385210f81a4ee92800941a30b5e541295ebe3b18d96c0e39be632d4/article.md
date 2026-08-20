---
schema_version: "1.0.0"
document_id: "5bc47d63f385210f81a4ee92800941a30b5e541295ebe3b18d96c0e39be632d4"
company_key: "yc-momentic"
company: "Momentic"
source_id: "yc-momentic-news-import-348aec23cbaf"
canonical_url: "https://momentic.ai/blog/playwright-locators-guide"
published_at: "2025-10-22T00:00:00+00:00"
first_seen_at: "2026-07-22T04:45:46.439360+00:00"
fetched_at: "2026-08-19T18:58:31.052183+00:00"
content_hash: "sha256:d7f01d1a371de5c475025b489d93a41bb3277a72b72c28ea34ec26daf4adb4bb"
---

# Playwright Locators Guide: getByRole, getByText, getByLabel, CSS & XPath

Flakiness is the silent killer of test confidence. A test suite that passes one moment and fails the next destroys trust.


At the heart of most test instability lies poor element selection. Your test clicks a button using div.btn-primary, then fails when someone updates the CSS. It finds an element by its position in the DOM, then breaks when a new feature adds content above it. It waits exactly 2 seconds for data to load, then times out when the API is slow. Every UI update becomes a cascade of test failures, turning maintenance into a full-time job.


Playwright locators address these fundamental problems through a different approach: auto-waiting that eliminates timing issues, strict matching that prevents ambiguous selections, and user-facing selectors that remain stable across implementation changes. Instead of brittle CSS selectors and XPath queries, Playwright encourages you to find elements the way users do, by their role, text, and labels.


This guide will explore the entire spectrum of Playwright locators, from the highly recommended user-facing selectors like getByRole to the necessary fallbacks of CSS and XPath, empowering you to build a test automation suite that is not just functional but truly reliable.


## Playwright Locator Strategy: Think Like a User, Not the DOM


Traditional test frameworks force you to think like a computer: find an element by its position in the DOM, its CSS class, or its internal ID. But applications are constantly in flux. Classes change with styling updates. DOM structures shift with framework upgrades. IDs get regenerated in each build. If your tests are coupled tightly to any of these, any UI update triggers cascading failures.


The most common sources of locator flakiness include:


- **Timing issues** : Elements that aren't ready when you try to interact with them. Modern web apps are dynamic, with components rendering asynchronously, data loading from APIs, and animations changing element positions. A button might exist in the DOM but be disabled until data loads. A modal might be present but still sliding into view.
- **Ambiguous selections** : Multiple elements matching your selector. This is especially problematic in lists, grids, and repeated components. Your test might pass locally, where there's one "Delete" button, but fail in staging, where there are twenty.
- **Implementation coupling** : Selectors tied to implementation details rather than user-visible behavior. When you select by div.btn-primary-new-style, you're testing the implementation, not the user experience. Every refactor becomes a test maintenance burden.
- **Dynamic content** : Elements that change based on data, user state, or time. A dashboard that shows different widgets based on user permissions. A form that reveals fields progressively. Content that updates in real-time.


Playwright addresses these fundamental issues with its modern, opinionated approach to element selection. Unlike traditional methods that query the DOM directly, Playwright locators represent a paradigm shift in how we write automated tests.


### Playwright Auto-Waiting Explained


One of the most common sources of test flakiness is timing. When apps are dynamic, with elements rendered, hydrated, and enabled asynchronously, a traditional test script might try to click a button before it's interactive, leading to failure.


Playwright's auto-waiting mechanism solves this. When you perform an action on a locator, Playwright automatically performs a series of checks before executing the action. Playwright auto-waits so actions run only when the locator resolves to exactly one visible element, stable, receives events, and is enabled (for fill, also editable).


This is done automatically for every action, with a configurable timeout. This means your test code becomes cleaner and more representative of user intent. You simply state, "click the submit button," and Playwright handles the necessary waiting to ensure the button is ready.


The impact on CI/CD pipeline stability is significant. Tests that previously required complex wait strategies or arbitrary sleep statements now work reliably across different environments and load conditions.


### Strict Locators in Playwright: Prevent Ambiguous Clicks and Hidden Flakes


If a locator resolves to more than one element when you try to perform an action, Playwright will throw an error. This is a deliberate design choice to prevent ambiguous and potentially erroneous tests.


Consider a page with multiple "Delete" buttons. In many frameworks, clicking would just select the first one, potentially deleting the wrong item. Playwright forces you to be explicit:


‍


This strict-by-default behavior forces you to write more specific and unambiguous selectors. While you can opt out by using methods like first(), last(), or nth(), the default encourages best practices. This philosophy aligns with the principle that a test should be a precise specification of behavior.


### User-Facing Playwright Locators: Roles, Text, and Labels for Stable Tests


Playwright recommends a specific hierarchy for choosing locators, prioritizing those that are most resilient to change. At the top of this hierarchy are user-facing locators. These selectors find elements the way a user would, based on what they see and interact with on the screen, such as text, labels, and accessibility roles.


This approach decouples tests from the underlying DOM structure, making them far more robust against code refactoring. When you select a button by its visible text or ARIA role, your test continues to work regardless of whether the development team changes from <button> to <a> tags, modifies CSS classes, or restructures the component hierarchy.


Basing tests on implementation details like CSS classes or complex XPath is fragile. A developer might change a div to a section or refactor CSS class names, breaking tests without altering the user's experience. User-facing locators are immune to such changes. As usability experts emphasize, designing for accessibility and user experience often leads to more robust and maintainable systems, a principle that holds true for test automation.


## Playwright Locator Ladder: Best-Practice Order


Choose your locators in this priority order for maximum resilience:


Priority Locator Type Example When to Use


1 Role + Name getByRole('button', { name: 'Submit' }) Interactive elements with ARIA roles


2 Text getByText('Welcome back') Visible text content


3 Label getByLabel('Email address') Form inputs with labels


4 Placeholder/Alt/Title getByPlaceholder('Enter email') When labels aren't available


5 TestId getByTestId('cart.add') Escape hatch for ambiguous elements


6 CSS page.locator('.submit-btn') Structural selection when needed


7 XPath page.locator('xpath=//div\[3\]') Last resort for complex DOM traversal


Each level down the ladder increases fragility, but sometimes becomes necessary. Start at the top and work down only when required.


### Why getByRole Is Best: ARIA Roles, Accessible Names, and Resilient Tests


The getByRole locator is the most powerful and recommended locator. It finds elements based on their ARIA role, which defines the element's purpose. This aligns your tests directly with accessibility best practices. Screen readers and other assistive technologies use these roles to understand the page, and so can your tests.


Using getByRole makes your tests more readable and resilient. The accessible name can be derived from the element's content, an aria-label, or an associated <label> tag. For a complete list of roles, you can refer to the WAI-ARIA specifications.


The power of role-based selection extends beyond simple elements. Complex widgets like tabs, menus, and dialogs all have specific ARIA roles that make them easily targetable:


### When to Use getByText and getByLabel (and When Not To)


getByText and getByLabel are your next best options when role-based selection isn't suitable:


These locators excel when dealing with content that users read directly or form fields with proper label associations. They maintain the principle of testing what users see while being more flexible than role-based selection for non-interactive content.


## Playwright Locator Examples: Buttons, Forms, Cards, Tables, Modals, Virtual Lists


### Buttons and forms (icon-only, label associations)


**Icon-only button without accessible text:**


**Form with associated labels:**


### Cards & tables (scoped chains, has patterns)


**Product cards with repeated buttons:**


**Data table with row selection:**


### Modals, dialogs, and toasts (focus traps, titles)


### Virtualized lists & pagination (row scoping + has)


## Advanced Playwright Locator Patterns: filter(), has, Chaining, .or()/.and()


Playwright's API is designed to be composable, allowing you to chain and filter locators to pinpoint the exact element you need with confidence and clarity.


### Chaining locators for scoped searches


Chaining is the most common and intuitive advanced technique. You can call a locator method on another locator object to narrow down the search scope. This is extremely useful for finding an element within a specific container, such as a card, form, or table row.


### Filtering with filter() and has


The .filter() method allows you to refine a set of locators that match a specific criterion. This is particularly useful when dealing with lists or tables where you need to select an item based on some of its content.


The has filter allows for interacting with complex data grids and tables, a common challenge in enterprise application testing.


### Combining with .or() and .and()


For complex scenarios, you can combine locators with logical operators:


## Fallback Locators: getByTestId, CSS Selectors, and XPath—When to Use Each


While user-facing locators should be your first choice, there are situations where they are not practical or possible. For these cases, Playwright provides powerful, albeit more brittle, alternatives.


### The pragmatic escape hatch: getByTestId()


Sometimes, an element is difficult to select using user-facing attributes. It might not have a clear role, text, or label. For instance, a container <div> used for layout or a close button represented only by an 'X' icon font. In these scenarios, the best practice is to ask developers to add a dedicated test attribute to the element.


Test IDs are part of your application's public API. They should be stable, semantic, and follow a naming convention (e.g., cart.add, user.profile.edit). This concept is championed by testing experts who argue it's one of the best ways to create resilient UI tests. You can configure the attribute Playwright looks for if your team uses a different convention (e.g., data-cy).


### CSS locators: a familiar tool


CSS selectors are a familiar tool for any web developer and can be used in Playwright when other options are exhausted. They are good for selecting elements based on IDs, classes, or specific attribute values.


The main danger with CSS locators is tying tests to styling. A class like .red-text or .pull-right is purely presentational and likely to change. If you must use CSS, prefer structurally significant classes or, even better, data attributes.


### XPath locators: the last resort


XPath (XML Path Language) is the most powerful and most brittle locator strategy. It allows you to traverse the entire DOM tree with complex expressions, selecting elements based on their position, parents, siblings, or ancestors. This power comes at a high cost of readability and maintainability.


XPath is a recipe for a flaky test. A minor change to the page's structure will break it instantly. Think of XPath as a surgical tool for emergencies, not a hammer for everyday use.


## Tools for Finding Great Locators: Codegen, Inspector, Trace Viewer


Playwright provides excellent tools to help you find the best locators:


### 1. Codegen (starting point)


Run npx playwright codegen <your-url> to launch a browser where you can interact with your site. Playwright will record your actions and automatically generate the code, often suggesting the best user-facing locator for each interaction.


### 2. Inspector (debug mode)


When a test is running in debug mode, the Playwright Inspector opens. You can use its "Pick Locator" feature to hover over elements on the page and see the recommended locator in real-time. This is an invaluable tool for debugging and writing new tests.


### 3.[Trace Viewer](https://momentic.ai/blog/the-ultimate-guide-to-playwright-trace-viewer-master-time-travel-debugging) (post-mortem analysis)


Playwright can record a trace of your test execution, capturing screenshots, network activity, and console logs at each step. When a test fails in CI, download the trace to see exactly which locator failed and the DOM state at that moment.


Start with Codegen for initial capture, refine with Inspector during development, and debug with Trace Viewer when CI fails.


## Converting Brittle Selectors to Reliable Playwright Locators


The difference between a test suite that builds confidence and one that destroys it comes down to how you select elements. Every div.btn-primary you write is technical debt. Every XPath expression is a future 3 am debugging session. But every getByRole('button', { name: 'Submit' }) is an investment in stability.


The journey from flaky to reliable tests isn't about writing more assertions or adding more waits. It's about fundamentally changing how you think about element selection. When you stop asking "How can I grab this element?" and start asking "How would a user identify this?", your tests transform from brittle implementation checks into resilient behavior specifications.


This shift has compound effects. Your tests become documentation, showing new developers how the application should behave. Code reviews get faster because tests clearly express intent. Refactoring becomes safer because tests verify outcomes, not implementation details. Most importantly, you stop dreading the test suite and start trusting it.
