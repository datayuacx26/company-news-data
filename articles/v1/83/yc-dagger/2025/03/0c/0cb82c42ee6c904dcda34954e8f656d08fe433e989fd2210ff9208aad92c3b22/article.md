---
schema_version: "1.0.0"
document_id: "0cb82c42ee6c904dcda34954e8f656d08fe433e989fd2210ff9208aad92c3b22"
company_key: "yc-dagger"
company: "Dagger"
source_id: "yc-dagger-news-import-d3f1ddf31a06"
canonical_url: "https://dagger.io/blog/automating-cypress-tests/"
published_at: "2025-03-11T00:00:00+00:00"
first_seen_at: "2026-07-21T15:47:35.732544+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:2ad68b5b251f92ef5fd7df9636977bbdcd565460649c8bf6ef4cc5d38b7f5ad5"
---

# Automating Cypress Tests with AI: How Dagger Powers LLM-Generated End-to-End Tests

Writing end-to-end tests is a necessary but often tedious task for developers. What if an AI agent could generate those tests for you based on a GitHub diff? That’s exactly what Rebecca and Jeremy set out to solve with Cypress Test Writer, an AI-powered test generator built using Dagger.


#### The Problem: Writing Tests for Every Code Change


Every time a developer makes a change to their code, they need to write new tests to verify that change. This is especially true for end-to-end tests, which ensure an application behaves correctly from a user’s perspective. Manually writing these tests is time-consuming, repetitive, and, let’s be honest - not the most exciting part of the job.


Rebecca wanted a solution that could:


- Automatically detect changes in a GitHub repository.
- Generate Cypress tests using an LLM (Large Language Model).
- Run those tests to verify they work.


In short, she needed an AI-powered intern that could handle Cypress test writing for her.


#### The Solution: AI-Generated Tests with Dagger


Using Dagger, Rebecca and Jeremy built a workflow where an LLM:


1. Detects code changes by analyzing GitHub diffs.
2. Generates Cypress end-to-end tests in TypeScript.
3. Runs the tests inside a controlled environment.
4. Validates the results, ensuring the new tests function as expected.


```text
// CypressWorkspace has:
// a container sandbox: cypress/included:14.0.3
// only 4 functions: [write,read,look,test]
dag.llm().withCypressWorkspace...withPrompt...
```


#### How It Works


The Cypress Test Writer repository defines a function called CypressTestUpdate(), which:


- Clones the repository and identifies changes.
- Passes relevant context to the LLM.
- Writes new Cypress test files.
- Runs those tests in a containerized environment.


All of this is orchestrated using Dagger, allowing the AI agent to work in an ephemeral, reproducible environment - just like a real developer writing tests locally.


#### Demo: Watching the AI Write Tests


In a live demo, the agent successfully created and executed a new Cypress test:


- It detected a button style change in the application.
- It generated a Cypress test to verify that the button had the correct green styling.
- It ran both the old and new tests to ensure they passed.


Of course, like any engineer, the AI sometimes made mistakes. But rather than burning through unnecessary API calls, the pipeline was designed to fail fast and retry efficiently.


When the test generation succeeded, it produced a (basic) fully functional Cypress test:


```text
describe('Button Style Test', () => {
it('should have a green button', () => {
cy.visit('/');
cy.get('button').should('have.class', 'green-button');
});
});
```


This test, if deemed good-enough, could then be added to the repository, automating a task that would normally take manual effort. You can watch the full demo here:


#### Why This Matters


This project isn’t just a cool demo - it highlights a powerful shift in software development:


- AI-powered development workflows: LLMs can generate and validate code, making developers more efficient.
- Reproducible, containerized environments: Dagger ensures that test generation and execution happen in isolated, predictable environments.
- Automation for repetitive tasks: Instead of spending hours writing tests manually, developers can focus on building features while AI handles the grunt work.


#### Get Started


Want to try it yourself? Check out the[Cypress Test Writer repo](https://github.com/jpadams/cypress-test-writer) and start automating your end-to-end tests today, and build your own agent using the[Dagger Agent Quickstart](https://docs.dagger.io/ai-agents) .


Have questions? Join our[Discord](https://discord.com/invite/dagger-io) ! We’re happy to help.
