---
schema_version: "1.0.0"
document_id: "7de83954ccc58212cd036c3eb30e8e8bcdec2e407d6ee9fccbd8488948f0f63d"
company_key: "yc-tesora"
company: "Tesora"
source_id: "yc-tesora-rss-4d7d570f276c"
canonical_url: "https://tesoraai.substack.com/p/companies-have-claimed-they-can-read"
published_at: "2026-06-05T18:09:04+00:00"
first_seen_at: "2026-08-10T03:58:35.856576+00:00"
fetched_at: "2026-08-10T03:58:36.939902+00:00"
content_hash: "sha256:9936dde06efa3b0df88ccae8d20167a3b1d9014eeab5cb6525402f4bbdd15f46"
---

# Companies have claimed they can read documents for years, but has the problem been solved?

# Companies have claimed they can read documents for years, but has the problem been solved?


### Why context and audit agents will be the right solutions to reading documents


[AI for Actuaries @Tesora.ai](https://substack.com/@aiforactuariestesoraai)


,[Philo Bishay](https://substack.com/@philobishay)


, and[Vivek](https://substack.com/@tesora3)


Jun 05, 2026


First, it was called OCR. Then data capture, intelligent document processing, extraction, and now companies call it ingestion (with ETL still penciled into a deck somewhere). No problem in software has been renamed this many times, and with each iteration arrives with a fresh wave of companies promising it is solved. Our contrarian thesis is that, as of this year, it finally is. YC-backed providers like Reducto and Extend, the frontier models underneath them (Gemini, OpenAI, Anthropic), and vertical players like ProSights now read a clean page as well as a careful person, and a thousand pages costs about a coffee.


*A Luckin coffee, not Starbucks* : Gemini Flash reads six thousand pages for a dollar.


*On clean text, the engines passed 99% years ago. On OmniDocBench’s tables the same models drop to the mid-eighties, which is the number a loss run actually tests: Insurance documents are rarely clean.*


Which is why it caught us off guard that the better these tools got, the less comfortable insurance has been at putting one in front of


*their own* loss runs. This explains a habit we see in every customer. Before a chief actuary asks how our agent reasons or what it does with a rate filing, they ask whether it can read their submissions accurately, the way a surgeon might open with a question about the scalpel. They have been burned before, and have learned that ingestion is the one question that predicts whether anything built on top of it can be trusted (


*garbage in, garbage out…)*


**Ninety-seven percent is the wrong number to celebrate**


Insurance does not get to use the accuracy that satisfies everyone else. Ninety-seven percent is about where the best extractors land on messy documents, and for a consumer app misreading three receipts in a hundred that is a strong grade. The same figure is also a three percent failure rate, and in insurance those three errors are not spread evenly across fields you can afford to lose.


Each square is one extracted value. A 97% benchmark score leaves 3 wrong in 100. In insurance the errors are not random typos, they are the development factor, the netgross flag, the valuation date. Any one is the loss pick. Illustrative.


On a loss run, which often arrives as a scanned fax of a printout, the value the model fumbles is as likely to be the development factor, the net-versus-gross flag, or the valuation date as anything harmless, and any one of those is the number the indication turns on. A wrong word in a chatbot is a sentence you laugh off. But a wrong number in a rate filing is a meeting with a regulator three years later, in front of a file note that reads “rolled forward from prior.”


They are expensive because they are hardly reading errors at all. “Incurred loss” can mean paid losses plus case reserves as of a valuation date, or it can mean ultimate losses carrying a provision for claims not yet reported, and on an immature accident year those two numbers live in different area codes. A general extractor transcribes both columns flawlessly and cannot tell that it has blended them, because the carrier labeled them with the same word. Nor is the trouble confined to paper: Philo recently sent over a voice memo of edits, we ran it through a leading transcription model, and it turned his actuarial shorthand into confident gibberish. Telling it to think like an actuary only produced a second answer, equally confident and equally wrong.


**The better the model reads, the better it hides its mistakes**


Start with autocorrect. It is right so often that you stop proofreading, and then one day it changes “festival” to “funeral” and the message goes out anyway. The tool did not get worse. You got comfortable, and it saved its one mistake for the moment you had stopped looking.


Document models do the same thing, in two ways at once. As they get more accurate they clear the easy errors first, so the mistakes left over are the hard, judgment-heavy ones, which in insurance are also the costly ones. You get fewer errors, but a bigger share of the ones that remain land on a number that matters. At the same time, the better the model reads, the more people trust it and the less they check. Not even actuaries are immune: a team that will re-run a triangle which is already balanced will, after a tool has been right all quarter, quietly stop re-running anything.


Put simply, a better model fails less often, but when it does fail the mistake is both more important and less likely to be caught. That is why the benchmark score and the real risk pull apart. The leaderboard number keeps climbing, while the thing you actually care about, an expensive error slipping into a signed filing, can sit still or quietly get worse.


**The fix is introducing doubt, not assuming accuracy in the first go**


The answer is not a sharper set of eyes but a system that refuses to take a confident reading at face value. It checks each value against the structure of the document and the carrier’s history, so an ambiguous “incurred” column is resolved against what the rest of the exhibit implies rather than guessed. That is grounding rather than roleplay, the thing no instruction to think like an actuary can stand in for. We call these “audit” agents within a harness.


*Text-capture and table-structure bars are directionally consistent with public OCR/extraction benchmarks. “Actuarial field meaning” (correct reading of incurred vs paid, valuation date, ALAE bundling) is Tesora internal evaluation, shown to illustrate the argument. Tesora pairs Claude Opus 4.8 with a harness trained for actuarial work.*


The behavior that earns the signature, though, is the one no benchmark rewards and every careful actuary already practices: knowing when to stop. When a value falls outside the range the system has earned the right to be sure about, it abstains and hands that value to a person rather than emit a number as certain-looking as the rest. That reverses the complacency instead of feeding it, and tells the reviewer which cells to distrust. Higher accuracy improves the average. Calibrated doubt surfaces the expensive mistake, and in a regulated workflow that is the entire game.


The labs are unlikely to build this, because the data that would teach these distinctions sits inside carriers who have every reason to keep it there. From the outside, ingestion will keep looking finished. Inside insurance, the work that is left is not teaching the machine to read more of the page correctly. It is teaching it to admit which parts it might have read wrong, and to say so before an actuary signs underneath.


*Tesora builds AI agents and workflows that understand actuaries. If an ~extracted~ loss run has ever lied to you, we would like to hear from you.*


[Share](https://tesoraai.substack.com/p/companies-have-claimed-they-can-read?utm_source=substack&utm_medium=email&utm_content=share&action=share)


A guest post by


[Philo Bishay](https://substack.com/@philobishay?utm_campaign=guest_post_bio&utm_medium=web)


Head of Product, former chief actuary, MBA, lover of learning


[Subscribe to Philo](https://philobishay.substack.com/subscribe?)


A guest post by


[Vivek](https://substack.com/@tesora3?utm_campaign=guest_post_bio&utm_medium=web)


Working on an AI Workbench for Actuaries :) Formerly UChicago and a PE Investor. Avid Traveler and Fitness Enthusiast


[Subscribe to Vivek](https://vivek885.substack.com/subscribe?)
