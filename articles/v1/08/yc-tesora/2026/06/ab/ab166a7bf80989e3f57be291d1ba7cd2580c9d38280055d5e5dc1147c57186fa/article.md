---
schema_version: "1.0.0"
document_id: "ab166a7bf80989e3f57be291d1ba7cd2580c9d38280055d5e5dc1147c57186fa"
company_key: "yc-tesora"
company: "Tesora"
source_id: "yc-tesora-rss-4d7d570f276c"
canonical_url: "https://tesoraai.substack.com/p/without-the-scaffolding-even-the"
published_at: "2026-06-19T14:45:42+00:00"
first_seen_at: "2026-08-10T03:58:35.856576+00:00"
fetched_at: "2026-08-10T03:58:36.939902+00:00"
content_hash: "sha256:13438f433b603e73ca61f270f33017a535f1bf693f30fd8adc1525ea1ca5ca68"
---

# Without the scaffolding, even the best AI slows actuaries down

# Without the scaffolding, even the best AI slows actuaries down


### AI has been shown to slow down expert work, which many actuaries experience. New research shows how to remedy these slowdowns.


[AI for Actuaries @Tesora.ai](https://substack.com/@aiforactuariestesoraai)


and[Philo Bishay](https://substack.com/@philobishay)


Jun 19, 2026


[A 2025 randomized trial by METR](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/) , a nonprofit AI research group, recorded what happened when sixteen experienced developers worked in their own codebases, with and without AI. Before starting, the developers were sanguine, predicting AI would speed them up by twenty-four percent. Their perceptions remained positive throughout the experience, believing they had delivered twenty percent gains. Reality was a harsh gut check. They were actually nineteen percent slower:


Source: 2025 randomized trial by METR, https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/


These slowdown observations were not limited to coding. Soon enough, actuaries were sharing the same pain in actuarial discussions. Shane Leib, FSA,


[wrote in the May SOA Actuarial Intelligence Bulletin](https://www.soa.org/globalassets/assets/files/resources/research-report/2026/2026-05-ait170-ai-bulletin.pdf) , that the slowdown would be worse for actuaries than for developers. The reason it would be worse, in Leib’s framing, is that actuarial work creates two debts that software does not pay in the same way. (1) Verification debt, the work of proving AI output is right, and (2) defensibility debt, the work of explaining why it is right, both accumulate new tasks not previously burdensome.


Exacerbating this is that the consequences are higher for actuaries. In traditional coding, a wrong line of code announces itself by breaking something that can be debugged later. In actuarial science, bugs produce numbers that look right until surfacing as nonsensical rates or indefensible analyses. Actuaries face scrutiny by the presidents of their companies, regulators, and ultimately the insureds. AI does not document its work by default, so the decisions leading up to regulatory penalties or return premiums would go unexplained. Plausible, incorrect answers for actuaries produce consequential, expensive errors.


Leib was justified to flag this shortcoming at the time, but twelve months in the post-LLM world feels like an entire generation. The plot shift occurred this last February when


[the same METR researchers ran their study again](https://metr.org/blog/2026-02-24-uplift-update/) with newer tools and remarkably, the slowdown disappeared. METR followed the same methodology again with a larger pool of developers, and newly available (late-2025) tools, which in practice meant Claude Code with Opus 4.5 and background agents rather than the chat-with-Sonnet setup in the original. The nineteen percent slowdown transformed itself into a “happy”


*eighteen percent speedup* :


Source: METR 2026 Update to 2025 Study: https://metr.org/blog/2026-02-24-uplift-update/


The confidence interval was wide, so no one is popping champagne, but the results were real and the developers knew it. METR even flagged a meaningful selection problem: Several developers refused to participate in the no-AI arm because they no longer wanted to work that way. Anecdotally, the author, who spent two decades producing actuarial outputs, now feels the same when working with actuarially trained agentic AI.


So what changed? One might think better models yield better results. This is partially true, but it does not explain a


*thirty-eight* percentage point swing. That improvement is not credited to the model, but everything


*surrounding* it. In the original study, a developer copy-pasting into a chat window was paying verification debt by hand, sentence by sentence, because the model could not see the codebase, run tests in the background, or check its own work. In the follow-up, the developer had an agent doing that work in the background, reading the repository, running the tests, surfacing the failures, and bringing back code already checked against the actual state of the system. The debt did not get smaller, it was paid by “someone” else, a horde of behind-the-scenes context-aware agents.


In practice, this debt makes true AI work for actuarial purposes a non-starter. Take, for instance, an LDF selection on a triangle that runs through a mix-shift year. Verification debt and defensibility debt will be at their highest. While a chatbot might recognize the effect, (whether or not it catches the right effect), it writes a confident paragraph with no audit trail beyond the ephemeral chat log. The actuary pays both debts by hand, and at the end of the afternoon has done the work twice: once with the model and once without trusting it.


**“** We believe scaffolding benefits for actuarial agents will


*exceed* similar benefits realized in coding.


**”**


Can AI remedy this actuarial double debt? Again, we take a contrarian position, an emerging theme in this nascent periodical. Not only will it be remedied, but we believe scaffolding benefits for actuarial agents will


*exceed* similar benefits realized in coding. There are two main reasons for this. First, learnings by actuarial agents are more portable to other actuarial problems, whereas for coding, different syntaxes and applications may present less comparable solutions. Second, the structural benefits of adding certain features, reproducibility, auditability, documentation, facilitate actuarial work more so than similar contexts in coding.


The labs are unlikely to build this.


[As we argued in our piece on jagged intelligence](https://tesoraai.substack.com/p/jagged-intelligence-for-actuarial) , the path from a general-purpose model to actuary-ready tools requires scaffolding the labs have little commercial reason to build. The work falls to actuaries working with AI and chief actuaries approving projects. This scaffolding is the necessary progression to AI adoption for actuaries. This year, the tools are being quietly built either in-house or by companies like Tesora. Without this scaffolding, Leib’s implied prophecy that AI is not fit for actuaries will turn out correct, not because AI is incapable of actuarial work, but because nobody built the version that did.


[Share](https://tesoraai.substack.com/p/without-the-scaffolding-even-the?utm_source=substack&utm_medium=email&utm_content=share&action=share)


A guest post by


[Philo Bishay](https://substack.com/@philobishay?utm_campaign=guest_post_bio&utm_medium=web)


Head of Product, former chief actuary, MBA, lover of learning


[Subscribe to Philo](https://philobishay.substack.com/subscribe?)
