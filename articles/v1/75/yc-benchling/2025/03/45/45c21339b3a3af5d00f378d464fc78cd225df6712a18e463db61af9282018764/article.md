---
schema_version: "1.0.0"
document_id: "45c21339b3a3af5d00f378d464fc78cd225df6712a18e463db61af9282018764"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/ai-agents-for-scientists"
published_at: "2025-03-25T07:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:d481a61cd5aeb1db321141c0b5b488fda36cf8ff638f9990e0c875c49bba3b23"
---

# Why I quit my job to build AI agents for scientists

I spent Christmas afternoon experimenting with o1, OpenAI’s first reasoning model. It wasn’t how I had intended to spend my holiday break. As the technical co-founder at Benchling, I had gotten so tired of people asking me, in the eloquent words of one board member, “WTF are we doing with AI?”


As both a scientist and an engineer by training, I’m a combination of short-term skeptical and long-term optimistic. While I was annoyed at the question, I saw the benchmarks of o1 on PhD-level science questions and had to try it out. And what I saw that afternoon completely floored me. AI had gone from barely being able to summarize a lab presentation to reverse-engineering a complex analysis and interpreting new scientific datasets with ease. This wasn’t incremental — the game had changed overnight.


Now, I live and breathe technology. I studied computer science. I live in San Francisco, where I'm surrounded by AI startups and self-driving cars. I have been personally experimenting with AI for two years. And I was still caught off guard. So how can I expect scientists, who have full-time jobs doing science, to keep up with breakthroughs like this?


My priorities changed overnight. When I returned to work in January, I stepped away from my team and existing responsibilities over our 600-person organization. Instead, I dedicated myself full-time to coding side-by-side with a team of engineers building AI assistants for scientists.


There is so much magic for scientists that is locked away in LLMs, but it’s going to take a lot more than just giving a scientist ChatGPT. LLMs need context to be magical, and scientific context has so much nuance and complexity. You need to be a scientist to understand what context is needed and an engineer to know how to supply it, the exact intersection of problems that Benchling is built to solve.


Beyond building, it’s crucial to share what we’re learning to help others who, like me, are trying to keep up with how fast AI is changing. That’s why I’m starting a series of posts on what AI can do for scientists, and how that’s shaping what we’re building at Benchling.


## Data entry assistant


We believe the first big impact of AI agents in science will be automating so much of the toil that happens in the lab and outside it: capturing and analyzing data, reviewing and verifying experiments, aggregating and drafting reports, and much more.


Today, we’re excited to announce the Data entry assistant in closed beta. So much of the data that scientists work with comes in messy, poorly structured formats: PDFs from CROs, CoAs from reagent vendors, spreadsheets with legacy internal data, and more. Structuring this data is critical to make it easier to search and analyze. But no one wants to do it manually — it can take hours even for a single dataset and still be error-prone.


This is fundamentally a translation problem, which LLMs are perfectly suited for. But tackling it requires solving some complex engineering challenges. How do you provide enough context on the data you want to extract? How do you break up large files into manageable chunks? How do you verify the results?


We spent months tackling these challenges and iterating on our approach with customers. Our Data entry assistant is beautifully simple for a scientist — upload your file, get your results — but orchestrates a complex set of LLM calls behind the scenes to develop a plan, process the inputs piece-by-piece, and verify the outputs across multiple models. You can see it in action in the short demo video below.


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## Lessons learned


-


**Build at the edge of what the models can do:** We got excited about building this assistant because it barely worked in our initial testing last year. Careful engineering of the overall pipeline and individual prompts was critical, but recent model releases (particularly Claude 3.7) also significantly improved accuracy. We’re eagerly awaiting Llama 4 later this year to further improve cross-model verification.


-


**Create scientifically relevant evals:** Taking real-life examples and converting them into evals that could test the accuracy and speed of our approach proved extremely useful. The density of scientific data, the importance of numerical precision, and idiosyncrasies in data structures particularly from CROs (horizontally stacking vertical tables in a Word file??) were difficult to find in generic LLM benchmarks.


-


**Context leads to magic:** The Data entry assistant feels the most magical when it’s not just structuring data, but also using context from your notebook entry to do additional translation (e.g. automatically converting the animal the CRO calls “C1” to your actual ID “BNCH157”). We thought about the context a human needs to perform this task, and found a way to provide it without burdening the user.


## What’s next?


Automating toil is the obvious place to start, but it’s just the first step. As the models improve, we will go beyond automation to actually improving experiment design and even generating novel hypotheses.


Imagine a future where every scientist has an AI assistant that leverages the full corpus of internal and external knowledge to recommend and improve their experiments. This is not a future where AI replaces scientists. It is one where AI assists scientists at every step and small teams of generalists can do the work of hundreds of specialists.


I’ve gone back to some of our earliest users — academics, cutting-edge biotechs, and change agents in large pharma — to share[what’s now possible with AI](https://www.benchling.com/ai) and brainstorm on what’s next. If you're a scientist curious about where to start, our[AI Resources Hub](https://www.benchling.com/ai/ai-for-science) is the best place to explore practical AI use cases, prompts, and stories from scientists already working with AI. If you’d like to stay up-to-date on what I’m hearing and building, follow me on[LinkedIn](https://www.linkedin.com/in/ashu-singhal-1b58a18/) . You can also jump on the waitlist for the Data entry assistant[here](https://www.benchling.com/data-entry-assistant-waitlist) .
