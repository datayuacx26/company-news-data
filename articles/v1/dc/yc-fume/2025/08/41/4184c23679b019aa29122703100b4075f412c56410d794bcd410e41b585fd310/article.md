---
schema_version: "1.0.0"
document_id: "4184c23679b019aa29122703100b4075f412c56410d794bcd410e41b585fd310"
company_key: "yc-fume"
company: "Fume"
source_id: "yc-fume-rss-5fd00bac91aa"
canonical_url: "https://www.fumedev.com/blog/ai-testing"
published_at: "2025-08-13T12:00:00+00:00"
first_seen_at: "2026-07-20T23:23:55.513253+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:ec86425492517155da6c0026179b3d096b86f7624eb6aaf06d46e1277e5254b6"
---

# What is AI Testing & Why Do We Need QA Agents Yesterday?

Slapping the word “AI” before any traditional business concept is the cool thing to do nowadays. However, I believe we desperately need true “AI testing,” and here’s why.


## What is AI Testing?


AI testing is the process of writing and maintaining software quality assurance tests using large language models. The level of involvement AI models have over the process depends on the type of testing you are aiming for and the type of tool you are using. Here, we will focus on writing browser tests at three different levels of autonomy: IDE-level Assist, AI-Aided Test Builders, and Fully Agentic Systems.


### IDE-Level Assist


This is the shallowest form of AI involvement. A general-purpose AI sits inside an IDE to help the QA engineer by suggesting snippets, fixing syntax, and automating simple debugging.


**Example Tools:**


- Cursor — The AI Code Editor
- Windsurf — Purpose-built IDE with AI agents
- Claude Code — A coding agent in your terminal (This one is a little different in UX but overall has the same capabilities for QA testing purposes)


**Pros:**


👍 Full control over the test code


👍 Fast feedback cycle. You can immediately run the code AI suggests and see the result


👍 Limitless customization (any framework or tools you desire — Cypress, Playwright, Selenium)


**Cons:**


👎 Limited productivity boost


👎 Still requires a dedicated QA engineer to use the tool


👎 No help with selectors and test design


👎 The tests are still as fragile as before


### AI-Aided Test Builders


This is the middle ground between manual coding and fully autonomous testing. You design a test scenario using a no-code test builder, step by step. AI helps with generating selectors and creating a no-code automation. These tools usually require you to run the tests within the same platform, without source code access.


**Example Tools:**


- Testim - AI-powered test recorder and builder
- Reflect - Record browser flows and build tests
- Mabl - Low-code AI-assisted test creation


**Pros:**


👍 Time savings in initial test creation


👍 No technical knowledge is required


👍 AI handles generating initial selectors and usually fixing broken ones automatically


**Cons:**


👎 Time-consuming, and therefore still requires a dedicated QA person


👎 Tests cannot handle major UI/UX changes or dynamic data within your app


👎 Limited control over underlying code structure/framework


👎 Vendor lock-in. You need to keep using the tool to run the tests.


---


### Fully Agentic Systems


This is the "self-driving" level of AI testing. You give the AI access to your staging or production environment and describe what you want tested via text or video. An AI agent learns the product and uses its own computer in the cloud to write tests for you, just like a human.


**Example Tools:**


- [Fume](https://fumedev.com/) - Fully autonomous browser test generation from product walkthroughs
- Devin - Generalist software development agent. Not focused on QA.
- testRigor - Plain English test creation with self-maintenance features (only supports text)


**Pros:**


👍 Minimal manual intervention after setup


👍 Can cover large portions of the app quickly


👍 Automatically adapts tests as the app evolves (in theory)


👍 Takes little to no time. Even PMs or developers can effortlessly use it.


👍 Full source code access


**Cons:**


👎 Longer wait times per test generation


👎 Still an emerging technology


## Why Do We Need AI Testing Yesterday?


With LLM-assisted coding on the rise, engineering teams output more and more code every day. Plus, this code goes through less revision than ever before, as sometimes even the engineer committing the code doesn’t read it carefully. Treating the “Accept All” button in Cursor like a cookie banner on a website is a real phenomenon among many engineers. Here is a mind-blowing statistic from the most popular AI IDE: *Cursor writes almost 1 billion lines of accepted code per day* .


To put it in perspective, this means at least 20–30% of the world’s code output is the result of a simple “Accept” click with little to no revision by the author.


This, of course, means a great productivity boost for a lot of teams, yet it would be naive to think it comes without consequences. If your quality assurance processes are not scaling in parallel with your code output, it means you are shipping more and more bugs to production.


The chart shows coding productivity growing exponentially—as it is right now (if you do not agree, I’d suggest you read[this great blog](https://blog.samaltman.com/three-observations) post from Sam Altman)—and testing scaling linearly with limited-autonomy tools like IDEs and no-code test builders.


I feel like we are either at or fast approaching the intersection point for the “coding” and “testing” lines on the chart—which means we will enter the “AI Slop Zone” very soon. This is where you see bugs popping up every day in production because you shipped something that seemed believable without testing. I promise you, writing “believable” code is what LLMs are the best at. You won’t know the changes break a second-degree dependency until you actually test it. That is why you need a testing method that scales as fast as agentic coding: Agentic Testing!


When the way we develop software changes by 180 degrees, we cannot sit still and expect to test it effectively with more or less the same methods. We need novel and 10x-easier methods to create extensive test suites and maintain them. I believe agents enable this in two ways.


### 1) Screen Recording → Tests


I believe the most natural way to describe a test is just recording a Loom video. It is straightforward and includes everything someone needs to know about what to test:


- Your screen showing how to use the application
- Your voice describing expected behavior and additional information


This is not surprising because this is how a lot of teams internally communicate application flows between humans. Finally, we have the technology to do the same for AI agents.


The video-to-test UX shrinks the time it takes to create a new test case by an order of magnitude compared to things like no-code builders or AI-assisted Playwright scripts. Plus, you can describe multiple test cases in a single video. The efficiency of information you can provide with a video is simply unmatched and is a true force multiplier for teams. An engineer who creates a new feature can record a Loom in under a few minutes and ensure it is now covered by tests, or a PM’s screen recording for engineers can be used to ensure the same bug never happens again.


### 2) Background Maintenance


What usually drives teams away from browser testing is not creating the tests; it’s maintaining them. It’s annoying to have a test break every time you make a change in your product. Yes, some AI-powered no-code test builders can handle simple selector changes, yet this is not enough. Imagine you add a new step to your onboarding process. All of the existing selectors are the same, but the test still breaks because it is missing the newly added step entirely.


Now take agentic testing: in the first run after the new onboarding step is added, the test code would fall back to a browser-using agent to complete it with no false-positive test failure. As the first run is saved and the system reports there is a broken test script, the same agent that wrote the Playwright code can rewrite it to match the new flow. In this case, you made changes in your app but never had to deal with fixing a test. This is the kind of hands-off QA system we need to handle the incoming wave of AI slop.


## Conclusion


We are rapidly going through a revolution in the way that we develop apps. This requires us to revolutionize the way we create and maintain tests too. I think this will happen through agents because we can communicate with them just like we communicate with a human QA engineer. You would never build a step-by-step schema for a QA engineer to succeed at your task. You would send them a Loom or a Slack message and expect them to figure it out. In the same way, you would expect a QA engineer to fix their own tests without you needing to intervene. Agents promise a world where we can expect all the same things from a machine—and it’s here now.


If you want to experience it yourself, we have a demo[here](https://app.fumedev.com/demo) for you :)
