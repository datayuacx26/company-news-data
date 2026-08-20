---
schema_version: "1.0.0"
document_id: "39f2b55f42a50812378cefeecfa343abb0adea539ffec820df69384cb6b2e569"
company_key: "yc-workflow86"
company: "Workflow86"
source_id: "yc-workflow86-news-import-cb9e669ebbd5"
canonical_url: "https://www.workflow86.com/blog/delay-pause-module-in-make-com-how-to-pause-and-wait-in-a-workflow"
published_at: null
first_seen_at: "2026-07-22T20:30:21.795617+00:00"
fetched_at: "2026-07-28T22:07:07.393518+00:00"
content_hash: "sha256:409d87976e112c0839687c1d740d61507d8560715e3e7e6bf6d13f12e9d47d7d"
---

# Delay/Pause Module: how to pause and wait in a workflow in Make.com

The need to introduce a deliberate **pause** or **time delay** arises frequently, whether it's to accommodate external system processing times, respect API rate limits, or simply inject a more natural cadence into automated interactions. Users navigating **Make.com** (formerly Integromat) often seek a straightforward method to **stop** or **pause** their scenarios temporarily. While **Make.com** is undeniably a powerful automation platform, achieving a simple **time delay** can prove less intuitive than one might expect.


### Why Implement Time Delays?


A strategic **pause** serves several vital functions within automated processes. It's crucial for managing API rate limits, preventing your scenario from being blocked due to excessive requests. Delays are also essential when waiting for external processes, like report generation or data synchronization, ensuring subsequent steps only proceed when the required information is ready. Furthermore, introducing a **time delay** can simulate more human-like interaction patterns, particularly in communication workflows like email sequences, avoiding an unnatural, rapid-fire delivery. Pauses can also create windows for necessary manual intervention or review steps within a process, or simply allow systems a moment to stabilize, preventing potential race conditions between tightly coupled actions.


### Implementing Pauses in Make.com: The "Sleep" Tool


When searching for a dedicated "Wait" or " **Delay** " **module** within **Make.com** 's extensive library, users won't find an explicitly named component for this purpose. The primary mechanism available is the built-in " **Sleep** " tool.


Found under the "Tools" section in the scenario editor, the **Sleep** **module** allows configuration of a specific duration, measured in seconds, for which the scenario execution should halt. When a running scenario encounters this **module** , it simply stops for the designated period before moving to the next step.


For more information about adding delays and other workarounds, see this thread here: https://www.make.com/en/app-improvement-ideas/p/tools-sleep-more-than-300-seconds-and-less-than-a-second-milliseconds


## Common issues with delays/pauses in Make.com


While the **Sleep** tool does introduce a **pause** , its implementation carries significant considerations that limit its practicality for many common use cases.


#### Unsuitability for Long Delays


The Sleep module has a limit of 300 seconds (5 minutes) per module. This means to achieve a delay of 24 hours; you would need to repeat the sleep module 288 times.


For any substantial **delay** – minutes or hours – this approach becomes inefficient and potentially expensive, especially with numerous scenarios running concurrently. An active, sleeping scenario is still consuming resources. Consequently, using the **Sleep** tool for extended delays, such as waiting hours or days for a follow-up action or manual approval, is generally impractical and discouraged. It ties up platform resources and isn't architected for these longer waiting periods.


There is also no built-in handling of other time units like hours, days, weeks, months or business days.


#### Execution Halts and Resource Usage


The most critical aspect is that the **Sleep** tool halts the *entire execution* of that specific scenario run; it doesn't just **pause** a single branch but brings the whole instance to a temporary **stop** . This has implications for resource usage and cost, as **Make.com** 's pricing often involves operation counts and execution time. Time spent in the **sleep** state can count towards execution limits, keeping the scenario instance active.


#### Complexity of more advanced methods


Implementing conditional delays or pauses within different workflow branches using multiple **Sleep** modules can also add significant visual clutter and complexity to the scenario design.


Faced with these limitations, **Make.com** users often resort to more intricate workarounds for longer delays. These advanced techniques might involve splitting workflows across multiple scenarios, leveraging data stores or external databases (like Google Sheets) to track the status and required resume times, and using webhooks or complex scheduling configurations to re-initiate the process later.


While functional, these solutions dramatically increase the complexity, development effort, and ongoing maintenance burden of the automation.


### Will Make ever release a Delay/Pause module?


While a dedicated Delay/Pause module has been something Make users have requested since at least 2022, as of the publication of this article (March 2025) the module still has not been released. There is no indications on whether it will be released or when.


### A Streamlined Alternative: Dedicated Time Delays in Workflow86


Recognizing this common challenge, alternative platforms like Workflow86 have integrated efficient time delays as a core, dedicated feature. Workflow86 includes a specific " **Time Delay** " component, designed explicitly for pausing a workflow path for a defined period. Adding such a **delay** is intuitive: users drag the component onto the workflow canvas, connect it at the desired point of **pause** , and configure the duration – specifying seconds, minutes, hours, or even days, often using dynamic data from preceding steps.


For example, you can achieve the following delays in Workflow86:


1.


"Delay for 6 months"


2.


"Delay for 5 business days"


3.


"Delay until next Monday"


4.


"Delay until 12 March 2025 at 12:00PM"


You can also use placeholders to set these delay values dynamically, such as:


1.


"Delay for ${selected_days} days"


2.


"Delay until ${user_subscription_renewal)"


This can be used to create complex and powerful delay sequences, such as this one we developed for messaging new users at specific days after they have signed up:


### Advantages of Workflow86's Dedicated Delay Component


The advantages of this dedicated approach are clear.


#### Simplicity and Clarity


It offers superior simplicity and clarity; the purpose-built component makes the workflow logic immediately understandable, instantly signaling an intended **pause** .


#### No limits on delays: 12 minutes or 12 months


Need to wait for 5 minutes or 5 months? Time delays in Workflow86 can be scheduled for up to 12 months in advance meaning you can easily handle long running processes and workflows.


Workflow86's architecture handles these delays efficiently, meaning you can avoid the 45-minute total time limit in **Make.com** and the 300 second limit of individual **Sleep** **modules** , making it far more suitable for both short and long durations.


#### Purpose-Built Flexibility


Being purpose-built, it gracefully handles various **time delay** requirements without needing complex multi-scenario workarounds. This flexibility allows easy incorporation of delays into intricate branches and conditional logic without compromising the workflow's readability.


### Choosing Your Approach: A Decision Framework


When deciding how to implement a **pause** or **delay** , consider the required duration. Is it a brief **stop** of seconds or a few minutes, or a longer wait spanning hours or days? For short intervals, **Make.com** 's **Sleep** *might* suffice, but efficiency concerns remain. For long intervals, the **Sleep** tool is generally unsuitable, pushing users towards complex alternatives. Workflow86's dedicated **Time Delay** handles both efficiently.


Also, consider the frequency of the **delay** . High-frequency pauses exacerbate the potential inefficiencies of the **Sleep** approach in **Make.com** . Evaluate the workflow's complexity; managing delays with **Sleep** or workarounds in complex, branching logic adds significant overhead compared to a dedicated **module** . Finally, consider long-term maintenance. A dedicated component like Workflow86's **Time Delay** vastly improves readability and simplifies future updates compared to interpreting **Make.com** 's **Sleep** implementations or multi-scenario workarounds.


### Finding the Right Tool for Timed Pauses


While **Make.com** provides the " **Sleep** " tool to **stop** scenario execution temporarily, it acts more as a basic halt mechanism than a sophisticated **time delay** feature. Its limitations regarding operation consumption and suitability for longer waits often force users into complex, less intuitive solutions. For businesses requiring robust, clear, and efficient time delays, platforms offering dedicated components, like Workflow86's " **Time Delay** " **module** , present a compelling advantage. The ease of implementing and configuring a purpose-built **pause** streamlines design, enhances clarity, and efficiently manages delays of any length without the associated overheads found in **Make.com** .


If precise timing and efficient pausing are crucial for your automation success, investigating how a dedicated **delay** tool can simplify your design and optimize resource use is a worthwhile step.


**Ready to build workflows with intuitive, powerful time delays? Explore Workflow86 today and experience a simpler path to automation.**
