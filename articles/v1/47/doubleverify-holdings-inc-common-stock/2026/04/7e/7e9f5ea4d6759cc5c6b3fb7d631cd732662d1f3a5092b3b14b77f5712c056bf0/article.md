---
schema_version: "1.0.0"
document_id: "7e9f5ea4d6759cc5c6b3fb7d631cd732662d1f3a5092b3b14b77f5712c056bf0"
company_key: "doubleverify-holdings-inc-common-stock"
company: "DoubleVerify Holdings Inc."
source_id: "doubleverify-holdings-inc-common-stock-rss-f6469e95d005"
canonical_url: "https://medium.com/doubleverify-engineering/automation-first-qa-is-a-mindset-shift-not-a-framework-89f2d6967120"
published_at: "2026-04-19T12:15:12+00:00"
first_seen_at: "2026-07-20T23:17:33.321656+00:00"
fetched_at: "2026-07-28T22:15:51.462388+00:00"
content_hash: "sha256:9bc41393ac6cf11395cd8f7165a5fd2309d0f8bd7092068a15e7f04ad7eda29b"
---

# Automation-First QA Is a Mindset Shift, Not a Framework

Automation


Quality Assurance


Cursor


Testing


AI


# Automation-First QA Is a Mindset Shift, Not a Framework


[DV Engineering](https://medium.com/@dv-engineering?source=post_page---byline--89f2d6967120---------------------------------------)


7 min read


·


Apr 19, 2026


--


*Written By:*[Rashmi Konda](https://www.linkedin.com/in/rashmikonda/)


**Imagine cutting test creation time by 40–50%, accelerating release readiness and shipping with confidence before code even merges.**


That’s not a distant vision. It’s what my team has been building over the past few months. And it all started with a single shift in mindset: **automation-first QA.**


*“We used to cross the bridge one plank at a time. Now, the bridge builds itself as we walk.”*


Press enter or click to view image in full size


(Figure: Visualizing the bridge from design to automation)


For years, QA teams, including mine, built automation the old way: slowly, reactively and waiting for stability. But what we’ve learned is that to stay ahead, we need automation that starts at design and evolves alongside the product.


## Why Does QA Still Fall Behind?


Every QA engineer knows this story.


A new feature drops. Designs shift. Timelines tighten. Someone says, *“* We’ll automate once it stabilizes *.”*


But that day never comes.


By the time QA writes scripts, the product has already changed shape. And we end up maintaining fragile selectors more than we’re testing functionality.


It’s not a lack of tools holding us back — we have plenty. Playwright, Cypress and Selenium are powerful tools. The real blockers are **timing and mindset.**


We saw this exact pattern on one of our project migrations last quarter. We had a team committed to automating the new feature flows *after* the initial release. By the time we started writing test scripts, three major UI changes had already been deployed, and the underlying element IDs were shifting daily. We spent a significant amount of our automation time frantically maintaining fragile selectors and updating outdated tests, rather than writing coverage for new functionality. The automation suite was consistently failing, reducing the team’s confidence.


Automation has been treated as a *post-delivery task* instead of an *integral part of delivery.* We wanted to flip that model entirely.


## Turning Point: When Automation Started at Design Time


This approach focuses on **UI/web application automation** , where “design” refers specifically to **visual design artifacts** such as Figma or Miro screens.


Our breakthrough came from a simple idea: we can describe user intent clearly enough, and automation can start as soon as the first wireframe is ready.


That realization changed everything.


Now, instead of waiting for code, our automation begins with Figma/Miro, where design decisions already reflect product requirements.


At this stage, visual designs act as the most concrete, reviewable representation of intent. When available, we associate designs with higher-level artifacts, such as PRDs or tickets for traceability, but the automation itself is driven by what the UI actually enables and constrains.


### Step 1: Standardizing the Language: Labeled Designs


We aligned with designers to label interactive elements in Figma (e.g., ***btn_login, input_username, input_password, nav_dashboard*** ).


This took a little coaching at first, but once everyone saw it reduce back-and-forth during development, it quickly became part of our workflow.


This small step turned out to be game-changing.


Those labels became a universal language for design, QA and development.
No more guessing CSS selectors or hunting down element names. The labels enabled **AI tools to “see” user intent** and build stable selectors that wouldn’t break with every pixel tweak.


Press enter or click to view image in full size


### Step 2: AI Interpreting Design Intent


Once we exported our Figma screens, we fed them into **Cursor** using a vision-enabled model like GPT-4o. Cursor interprets the UI both visually and structurally, converting screenshots into a machine-readable JSON map as provided in the user prompt.


*“In reality, the JSON is more verbose — this is simplified for illustration.”*


```text
```{    "screens": [{"name": "Login"}],    "elements": [      {"type": "textbox", "label": "input_username"},      {"type": "textbox", "label": "input_password"},      {"type": "button", "label": "btn_login"}    ],    "flows": [{"from": "Login", "action": "btn_login", "to": "Dashboard"}]  }```
```


This is where the bridge began to form. AI transformed visual design into a functional structure.


LLM doesn’t “know” business intent on its own. What it infers is UI interaction intent, based on a combination of designer-provided labels, visual layout patterns, and explicit constraints we pass alongside the design as user prompts.


The flow entries in the JSON map aren’t discovered facts; they’re a structured record of intent, either explicitly provided (for example, “btn_login navigates to Dashboard”) or inferred in early drafts from common UI patterns. We use this intermediate JSON map not because it’s mandatory, but because it makes the pipeline reviewable, debuggable and easy to regenerate as designs change.


### Step 3: From Visuals to Behavior: Gherkin Scenarios


Next, the AI converted that JSON into human-readable Gherkin scenarios. This step was important because it aligned designers, QA, and developers around a shared understanding of behavior.


Example:


```text
``` Feature: Login    Scenario: Successful login      Given the user is on the login page      When they enter valid credentials      Then they should land on the dashboard    Scenario: Invalid password      Given the user is on the login page      When they enter an invalid password      Then an error message “Invalid credentials” should appear   ```
```


These weren’t just tests; they read like living documentation. Anyone on the team could read and validate them.


### Step 4: Instant Code Generation with Playwright


From there, we used Cursor to translate these Gherkin scenarios into clean, TypeScript-based, immediately executable Playwright test scripts.


```text
test('Successful login', async ({ page }) => {    await page.goto('/login');    await page.getByLabel('Username').fill('qa_user');    await page.getByLabel('Password').fill('password123');    await page.getByRole('button', { name: 'Login' }).click();    await expect(page).toHaveURL('/dashboard');  });
```


Our automation wasn’t playing catch-up; it was ready even before development finished.


### Step 5: Adaptive Testing: The Regeneration Loop


Change is constant. Designs evolve. Elements shift.


Traditionally, that has meant hours of rework. Not anymore.


When our design changed, we simply re-exported the new screens. Cursor reinterpreted them and generated updated test files, which we reviewed and merged.


We added a lightweight “healer” layer in our Playwright workflow. When a test fails, it analyzes the failure, proposes a patch, and reruns the affected test within defined guardrails. All changes are reviewed before being merged.


This alone removed hours of weekly selector-chasing.


### Step 6: Automating the Fix: Continuous Healing


We integrated this workflow into our CI/CD pipeline. When a test failed:


- The pipeline saved screenshots and DOM snapshots.
- The Healer Agent ran automatically, fixed selectors, and prepared a suggested patch.
- QA simply had to review and approve.


Press enter or click to view image in full size


6-step workflow


## Building Our Prompt Playbook


The real enabler behind this has been our **QA Prompt Playbook** .


It includes reusable templates that help Cursor understand screenshots, expand behaviors and generate consistent Playwright code. Each prompt follows a pattern we’ve refined through sprints, user flows and dozens of real test cases. They’re built to prevent hallucinations and ensure consistency.


### What’s Inside the Playbook:


- **Extraction Prompts** — How to turn a screenshot into a structured UI map
- **Scenario Prompts** — How to model behavior, validations, and edge cases
- **Automation Prompts** — How to generate stable, readable Playwright code
- **Regeneration Prompts** — How to update tests when designs change
- **Healer Prompts** — How to propose selector patches when tests drift


We keep the actual prompts versioned in our repo; they started simple and evolved based on where tests were failing or unclear. The process is now standardized, significantly accelerating the onboarding of new team members who can begin generating valuable tests quickly.


## The Results


After three sprints of adopting this workflow, here’s what we saw: nothing magical, just practical improvements that stacked up:


- **40–50% reduction in manual test authoring**


Not because AI wrote everything, but because early structure + Cursor removed a ton of repetition.


- **Test readiness moved ~30% earlier**


Scenarios were ready during design, allowing automation to start sooner rather than waiting for UI stability.


- **Validation cycles shortened by ~20–25%**


This led to fewer late surprises, clearer expectations and less back-and-forth across teams.


- **Flakiness went down — not to zero, but noticeably**


Consistent labels + better selector strategy + a small healer utility made tests break less often.


**And the biggest shift?**


QA stopped reacting and started shaping how features were built.


These numbers vary by team, but the direction was consistent.


## What Are We Building Next?


We’re starting to see workflows where LLMs don’t just generate artifacts but can safely interact with tools, and Playwright MCP is one example of this shift. The pipeline we’ve built focuses on generation. MCP will increasingly support execution, failure analysis, and targeted patch recommendations while keeping humans in the loop. This is a meaningful step forward from the old “script-and-maintain” model towards a more adaptive workflow. It allows QA to focus on real quality rather than repetitive fixes.


## Quality Is Not a Phase — It’s a Starting Point


Automation-first QA isn’t a toolchain, it’s a **mindset** .
It’s about shifting quality upstream to the moment ideas take form, not after the code lands.


So if you’ve ever said, *“* We’ll automate later, *” it* might be time to stop waiting.
