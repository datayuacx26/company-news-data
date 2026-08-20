---
schema_version: "1.0.0"
document_id: "303cd080bbb367758c9b8fff5f14af87c8ee38b9072712c2e61945ee09aa5112"
company_key: "yc-trigger-dev"
company: "Trigger.dev"
source_id: "yc-trigger-dev-news-import-c59968a07222"
canonical_url: "https://trigger.dev/blog/cursor-rules"
published_at: "2025-03-27T11:00:00+00:00"
first_seen_at: "2026-07-22T17:13:09.827889+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:1d30838eaa8bb56ef316af75a0f9755cf22ef838c1896c4a8b9f557a34a722b1"
---

# How to write great Cursor Rules

A Cursor Rules file is essentially a set of instructions that helps the AI understand the patterns, best practices, and constraints relevant to your specific project or framework. By creating a well-crafted rules file, you can dramatically improve the quality and consistency of AI-generated code and reduce the amount of manual corrections needed afterward.


If you want to jump ahead and just check out the Cursor Rules file we wrote to make creating Trigger.dev tasks easier, you can download it from our[GitHub repo](https://github.com/triggerdotdev/trigger.dev/blob/main/.cursor/rules/writing-tasks.mdc) and read our guide on[installing and using it](https://trigger.dev/changelog/cursor-rules-writing-tasks) . Keep reading to learn how we wrote the rules as well as 10 tips for writing your own.


## Video walkthrough


Check out all 10 tips in this 6-minute walkthrough.


- 00:00 - Intro
- 00:16 - How to install a Cursor Rules file
- 00:36 - Creating and configuring a new Cursor Rules file
- 02:25 - How we wrote the rules for writing Trigger.dev tasks
- 04:37 - Testing the rules


## 10 tips for writing a Cursor Rules file


### 1. Choose the right rule type for your use case


When creating a new Cursor Rules file, make sure you set the Rule Type options at the top of the file. The "Always" option will apply your rules regardless of the prompt, making it ideal for framework or language-specific guidelines. "Auto Attached" applies rules when files match specific patterns, perfect for monorepos or targeted file types. "Agent Requested" intelligently applies rules based on user intent, while "Manual" requires explicit attachment of the file to the AI context.


### 2. Start with a high-level overview


Begin your rules file with a clear, high-level description of what the AI should be trying to achieve. This provides valuable context and sets clear expectations for the AI to follow, resulting in more accurate code generation from the start.


### 3. Specify essential code elements


Include anything you consider essential for the AI to include in its generated code. This might be specific SDK versions, import statements, error handling patterns, or documentation requirements that ensure the generated code follows your project standards.


### 4. Explicitly mark deprecated patterns


Include examples of deprecated code patterns and explicitly instruct the AI *not* to use them. Then provide the correct, up-to-date patterns with clear instructions to use only these. Use strong, direct language to emphasize the importance of avoiding deprecated patterns.


### 5. Create example patterns where possible


When creating example code patterns, use proper markdown formatting with clear headings, add detailed descriptions of each function or code example, include helpful code comments, and ensure each snippet is specific to the use case without extraneous code. Include as many examples as needed to fully cover your use cases.


### 6. Include verification steps


At the end of your rules file, include verification steps that the AI must perform to validate its work. These should be specific checks that either pass or fail. Explain the consequences of verification failures, such as code that won't run properly, deploy successfully, or integrate correctly with your system.


### 7. Organize rules by category or feature


Group your rules logically by category, feature, or functionality. This makes the rules file easier to navigate and update, and helps the AI understand the relationships between different patterns and examples. For complex projects, consider using clear markdown headings to separate different sections or even many rules files to cover different parts of your project.


### 8. Test your rules file thoroughly


Test your rules file with a variety of prompts, including edge cases and intentionally problematic requests. This helps identify areas where the rules might be misinterpreted or insufficient. Pay special attention to how the AI handles requests for deprecated functionality or ambiguous instructions.


### 9. Include common pitfalls and solutions


Document common mistakes or pitfalls that developers encounter, along with their solutions. This helps the AI generate code that avoids these issues from the start. For example, if certain API combinations frequently cause problems, explicitly note this and provide the correct pattern to use instead.


### 10. Keep your rules file updated


Always keep your rules file in sync with any changes to your project, framework, or API. Outdated rules can lead to the AI generating deprecated code patterns or missing new features, creating additional work for developers who need to correct the generated code.


## Additional insights from our experience


When creating our Cursor Rules for Trigger.dev tasks, we found several patterns that significantly improved the quality of AI-generated code:


**Be explicit about SDK versions** : We noticed that explicitly specifying the SDK version in our rules helped prevent the AI from generating code using older versions or mixing syntax from different versions.


**Include real-world examples** : The most effective rules included real-world examples that demonstrated complete task implementations, not just isolated code snippets. This gave the AI a better understanding of how different components fit together.


**Address edge cases** : We included examples of handling edge cases like error handling, retries, and idempotency. This resulted in more robust generated code that considered failure scenarios from the start.


**Explain the "Why" behind patterns** : When appropriate, we included brief explanations of why certain patterns were preferred. This context helped the AI make better decisions when adapting examples to specific requests.


## Conclusion


Creating a well-structured Cursor Rules file has dramatically improved our productivity when writing Trigger.dev tasks and the time invested in creating these rules has already paid off. Whether you're building developer tools, working with a complex framework, or just trying to maintain consistency across a large codebase, creating your own Cursor Rules file can be an extremely valuable exercise.


If you're working with Trigger.dev, we encourage you to try our rules file and see the difference it makes in your development workflow.


## Next steps


- [Get started with Trigger.dev](https://cloud.trigger.dev/)
- Install the Cursor Rules by following[this guide](https://trigger.dev/changelog/cursor-rules-writing-tasks/)
- Check out[our docs](https://trigger.dev/docs/introduction)
- Join the[Discord community](https://trigger.dev/discord)
