---
schema_version: "1.0.0"
document_id: "f831f2eb5a0d4fa7fbe8ea654ee62027b20e0fe5a9eb64267cc9563e5873c1be"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-news-import-c7466264c659"
canonical_url: "https://www.usetusk.ai/resources/the-case-for-comment-driven-development"
published_at: "2025-10-03T18:25:44.192+00:00"
first_seen_at: "2026-07-26T03:20:25.565537+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:43d6537811e2118d70b7f6f602796b7d78427484d1a48029a1fff3731b9dbcbc"
---

# The Case for Comment-Driven Development

The comments are dead. Long live the comments.


When I first started coding over a decade ago, one principle that stuck with me was Steve McConnell’s principle that good code is its own documentation.


This has served me well over the years on teams that prize engineering excellence. Not to mention the sense of satisfaction you get in reaching the platonic ideal that is having self-documenting code.


Which is why, not too long ago, my gut reaction when reviewing heavily-commented pull requests was: “Wow, this software engineer is not confident enough to let their code speak for itself.”


> Good code is its own best documentation. As you're about to add a comment, ask yourself, "How can I improve the code so that this comment isn't needed?' Improve the code and then document it to make it even clearer.
> - Steve McConnell, Code Complete


With LLMs, I’ve done a 180 on this.


Software engineers who are early adopters already spend most of their time reviewing code as opposed to writing code. This new habit will be the norm in a year.


Which then raises a new question: **who's responsible for the comments?**


The short answer is *you* , the engineer. As an AI-first engineer, one of the most valuable things you can do is writing and maintaining better comments. I call this approach comment-driven development.


## **The Comment Problem**


Before I get into explaining comment-driven development, let’s first dive into why comments are relevant to pair programming with AI.


Here's what we know so far about LLMs and their relationship with comments:


- **LLMs use comments to "think"** - When generating code, AI models often write comments first to plan their approach, similar to how developers sketch out pseudocode before coding.
- **LLM-generated code is often over-commented** - AI has a tendency to explain ad nauseam. It will document its every step when writing code, creating verbose documentation.
- **Comments and docstrings can be hallucinated** - Just like code, AI can generate comments and docstrings that don't accurately describe what the code actually does.


The comment problem is best summarized by this simple example that tripped up Claude 3.7 Sonnet:


```text
const   LIMIT =   10  ;


/**
* Returns if current value is over limit.
*/
const   isOverLimit =   () =>   {
const   current =   await   getCurrent();
return   current >= LIMIT;
}
```


‍


The issue here is that the LLM-generated comment says "over limit" but the code it generated checks for "greater than or equal to."


This subtle discrepancy then caused the AI to misinterpret the function's behavior in subsequent interactions. When the comment is inaccurate, it doesn't just confuse humans. it confuses future AI assistants too.


### **Other Points of Concern**


**Technical Debt**


LLM-generated comments often describe the "what" rather than the "why." They tell you that a loop iterates through an array, not why you chose this one approach or why you're handling a specific edge case. Over time, these obvious comments become noise that makes code harder to digest.


**Comment Drift**


AI-generated comments are particularly susceptible to becoming stale. When you modify LLM-generated code (whether manually or automatically), these verbose explanatory comments sometimes don't get updated, creating a growing disconnect between intent and implementation.


**Loss of Context**


When refactoring code, LLMs have a tendency to omit or overwrite existing comments. Those carefully crafted "why" comments (say, explaining business logic quirks, third-party API gotchas, or performance considerations) can disappear when AI assists with refactors.


## **Applying Comment Driven Development**


Despite this, it isn’t in your best interest to “ban” your coding agents from leaving comments. AI-generated comments are helpful for getting code out the door fast.


Given that LLMs are next token predictors, comments serve as helpful thinking prompts for the coding agent (at runtime) as it's generating code.


Pre-existing comments also help coding agents retrieve context more accurately when generating code on top of its past changes, particularly if modifying a file it previously created. Additionally, natural language explanations of code make it easier for human engineers to pick up context when reviewing a PR, speeding up mean time to merge.


The solution then is making sure you’re pair programming with your coding agents as if you’re a senior engineer managing a team of book-smart junior engineers.


This means carefully reviewing the generated comments that come with the code, and iterating on them locally. Don't be afraid to be verbose if the comments provide high-signal context for posterity. Some things to include:


- **The "why" behind decisions** - Business requirements, performance considerations, security implications
- **Edge case explanations** - Why certain inputs are handled in specific ways
- **Integration notes** - How this code interacts with third-party services or legacy systems
- **Maintenance warnings** - Dependencies between components, update procedures, TODOs


### **Tactical Advice**


The biggest blocker to adopting AI coding agents in this way is the mindset shift for a seasoned engineer who is used to hand-crafting code.


You have to embrace that LLMs are going to have preferences that don’t always align with the core principles of Clean Code. And to move fast, you’re going to have to be comfortable with a level of verbosity that you perhaps wouldn't normally like.


With that said, some basic principles of software design still apply. Here are some tactical advice to consider as you’re applying comment-driven development.


#### **1) Descriptive Function Names**


This is one area where you want to reduce the number of comments. Opt for descriptive function names over a comment for the function if it's purely behavior related.


Instead of:


```text
// Calculates user discount
const   calc =   (  user, amount  ) =>   { ... }
```


Write:


```text
const   calculateMembershipDiscountForPurchaseAmount =   (  user, amount  ) =>   { ... }
```


‍


This might seem clunky but longer, detailed names help both human readers and AI agents understand intent without relying on comments that might become outdated.


#### **2) Focus on Method Documentation**


Write well-written docstrings for methods. This serves multiple purposes:


- They help language servers provide better autocomplete
- They guide AI assistants toward correct usage patterns
- They force you to think clearly about the function's contract


#### **3) Cross-Reference Comments**


When duplicating logic across components (sometimes necessary during transitions), add explicit comments:


```text
// IMPORTANT: If updating this validation logic,
// also update the similar logic in UserProfile.tsx
const   validateEmail =   (  email  ) =>   { ... }
```


‍


This allows coding agents to gather additional context from files or functions mentioned in the comment when it is performing agentic grep.


#### **4) Two-Layer Documentation**


Think of your codebase as having two types of documentation:


- **Comments are the "why"** - Context that AI can't infer from the code alone
- **Unit tests are the "what"** - Specifications that AI can generate and maintain


Tools like[Tusk](https://www.usetusk.ai/?utm_source=tuskblog&utm_campaign=cdd-post) and[Cursor](https://cursor.com/?utm_source=tuskblog&utm_campaign=cdd-post) help with the unit test generation. Both products also benefit from the "why" comments because they provide business logic that can't be gleaned from the code itself.


#### **5) Comment Quality Gate**


Create a PR check for the below workflow that requires acknowledgement before merging a PR that predominantly contains AI-generated code:


1. Read and understand the generated code completely
2. Remove redundant comments that just restate what the code does
3. Add context comments that explain business logic and call out dependencies
4. Verify existing comments weren't accidentally removed or modified
5. Update related documentation in markdown files if the change affects other components


## **Final Note**


Mathematicians today don’t try to outcompete a calculator at doing complex calculations. Similarly, as a software engineer, you’re not here to compete with AI coding agents at creating *X* amount of lines of code in *Y* amount of time.


The value you’re providing now is in what the AI can’t do, which is providing the business context and long-term planning that don’t exist in model weights. The engineers who will become senior engineers in this landscape will be those who embrace this mindset shift.


This starts with comment-driven development. If you maintain comments in a way that makes your codebase easily comprehensible, you will move 3x faster than the engineer who swears by self-documenting code and declines a majority of AI-generated code with comments.


Just a year ago, a subset of developers swore that coding agents could never do meaningful work in brown-field codebases. Today, Cursor Agent and Claude Code have broken into the mainstream.


The gap now remains in how you utilize these coding agents to their full potential.


Code comprehension is more important than ever. That's why thoughtful, human-vetted comments are table stakes for any AI-native engineering team.
