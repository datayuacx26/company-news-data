---
schema_version: "1.0.0"
document_id: "b3448e4a968db54bcc8ebeb96e0c1740cfa6e3d9f36692ff1050bceebb52a18a"
company_key: "yc-momentic"
company: "Momentic"
source_id: "yc-momentic-news-import-348aec23cbaf"
canonical_url: "https://momentic.ai/blog/where-qa-went-wrong"
published_at: "2025-10-08T00:00:00+00:00"
first_seen_at: "2026-07-22T04:45:46.439360+00:00"
fetched_at: "2026-07-23T16:54:58.821228+00:00"
content_hash: "sha256:47fdf7262eda91fa97c37b0bf07a575ab9062ebba7bd69cd2b2c6cda7dc0d84d"
---

# Where QA Went Wrong

Oh, QA. You had such hopes and dreams. The hope was to ship quality software with minimal fuss and catch bugs before users did. The dream was to become the central actor within the software org and a trusted advisor who ensured every release was solid gold.


But what has happened to the field is closer to a night terror than a reverie. Like Chris, everyone hates QA. And it’s mostly QA's fault. Instead of becoming the trusted authority in testing and quality within an org, the field made several fatal mistakes that have led to QA simply no longer being a core part of quality. QA wants to blame AI, but the truth is, these problems were there way before we started chatting to machines.


So, how did we get here?


## 1. We commoditized the craft


*The choice: Treat QA primarily as a cost center and move large portions of testing to low-cost, offshore labour markets.*


When companies moved QA offshore to save costs, they fundamentally changed how the organization viewed testing. What was once seen as a skilled discipline became just another line item to minimize.


Offshore teams, often working in different time zones with high turnover rates, couldn't build the deep product knowledge that comes from sitting with developers and product managers. They couldn't push back on bad designs or suggest better approaches because they lacked context and authority. DevOps and MLOps rely on rapid iteration and tight feedback loops. When your QA team is 12 hours away and working through intermediaries, those loops break.


Testing became about following scripts rather than thinking critically about user experience and edge cases. The physical and cultural distance meant QA lost its seat at the table during design discussions and architecture decisions. When bugs inevitably slipped through, it reinforced the perception that QA wasn't adding value, creating a cycle where companies invested even less in quality talent and tooling.


## 2. We undervalued automation, then automated the wrong things


*The Choice: Adopt test automation late, tie it to GUI-heavy scripts, and measure progress in "percentage of test cases automated."*


When QA finally embraced automation, it was already playing catch-up. While developers had CI/CD pipelines and infrastructure as code, QA was still manually clicking through test cases. The rush to automate led to poor choices:


- Teams automated UI tests that broke with every CSS change instead of building robust API and integration tests
- They measured success by counting automated test cases rather than asking if those tests caught real bugs or provided fast feedback
- They chose record-and-playback frameworks over learning programming languages and modern testing tools


Meanwhile, developers were writing unit tests and using modern testing frameworks that QA teams weren't familiar with. This created two separate testing worlds: developers owned the fast, reliable tests while QA owned the slow, flaky ones.


The[2024 World Quality Report](https://www.capgemini.com/us-en/news/press-releases/world-quality-report-2024-shows-68-of-organizations-now-utilizing-gen-ai-to-advance-quality-engineering/) lists “ *lack of a comprehensive automation strategy* and *legacy test frameworks”* as the top two barriers blocking quality-engineering modernisation. When automation became synonymous with overnight regression suites that took 8 hours to run and failed for mysterious reasons, the organization learned to ignore QA automation entirely. By the time QA realized they needed to learn to code and think like developers, the damage was done, and testing had already moved left without them.


## 3. We kept QA organizationally siloed


*The Choice: Preserve a separate QA phase/team instead of shifting quality left (into design) and right (into production monitoring).*


While the industry moved toward "you build it, you run it" models, QA remained stuck in a handoff mentality. Developers would throw code over the wall to QA, who would throw bugs back, creating adversarial relationships instead of shared ownership. QA became the final gate before production rather than a practice embedded throughout the development lifecycle.


This siloed structure couldn't adapt to modern development speeds. AI-native teams collapse testing into continuous experimentation; a handoff model cannot keep pace with daily or hourly release trains. When features go from idea to production in hours, not weeks, a separate QA phase becomes impossible.[Recent DevOps and DORA studies](https://cloud.google.com/devops/state-of-devops) find that every 25% jump in AI-tool adoption correlates with faster flow of work and higher documentation quality—benefits that rely on tightly integrated teams, not back-office test factories.


The organizational separation also meant QA missed critical context:


- They weren't in design reviews to prevent testability issues.
- They weren't monitoring production to understand real user behavior.
- They weren't pairing with developers to build quality in from the start.


While engineering teams adopted SRE practices and learned from production incidents, QA stayed focused on pre-production testing, missing the rich feedback loops that come from observing systems in the wild. By maintaining these walls, companies ensured QA would always be reactive, always behind, and always seen as the department that slows things down.


## 4. We managed to volume, not value


*The Choice: Optimise for test-case counts, pass/fail percentages, and defect discovery rates instead of user experience, resilience, and business outcomes.*


QA became obsessed with vanity metrics that made impressive dashboards but meant nothing to the business. Teams would proudly report running 10,000 test cases with 98% pass rates, while customers complained about basic workflows being broken. Managers rewarded testers for finding more bugs, creating perverse incentives to nitpick minor issues while missing fundamental problems. Test coverage percentages became a goal in themselves, leading teams to write redundant tests for trivial code while leaving critical payment flows untested.


This volume-based approach missed what mattered. While QA counted test cases, the business cared about conversion rates, customer retention, and revenue impact. A single bug in the checkout flow mattered more than 100 cosmetic defects, but the metrics couldn't capture this distinction.[The 2024 World Quality Report](https://www.capgemini.com/us-en/news/press-releases/world-quality-report-2024-shows-68-of-organizations-now-utilizing-gen-ai-to-advance-quality-engineering/) warns that QA metrics disconnected from business KPIs remain a blocker to board-level investment in quality. When executives saw QA reports full of abstract numbers with no tie to business outcomes, they naturally questioned the value.


## 5. We failed to upskill for an AI world


*The Choice: Assume that domain knowledge plus basic scripting would remain sufficient for career-long relevance.*


The skills gap became a chasm. Today's AI-driven tools generate tests, predict defects, and triage failures autonomously. The new QA engineer needs to curate data, tune models, and interpret statistical risk. But most QA teams are still writing Selenium scripts.


This created painful disconnects everywhere:


- QA needed to understand probability to work with AI confidence scores, but many couldn't read a confusion matrix
- They needed to design test data strategies for model training, but were still creating datasets in Excel
- They needed to validate AI behavior and edge cases, but lacked the ML fundamentals to know what to look for
- They needed to shift from deterministic to probabilistic thinking, but kept expecting binary pass/fail results


Skills, knowledge gaps, and cultural resistance to change are the leading reasons AI testing pilots stall. This failure to upskill created a vicious cycle: the most talented people left for development or DevOps roles while those who remained often viewed AI as a threat rather than a tool. The irony was painful—the very tools that could have elevated QA's strategic value were rejected by teams too undertrained to use them.


## 6. We mistook "AI in testing" for "testing AI"


*The Choice: Deploy AI as another automation library rather than a fundamental rethink of how quality is engineered.*


When AI arrived, QA treated it like just another tool in the toolbox. Teams excitedly added AI-powered features to their existing Selenium suites, expecting magic. They bolted AI heuristics onto old scripts without ensuring clean, labeled data. The predictable result? Flaky predictions and loss of trust. AI became synonymous with unreliable test results and false positives.


The fundamental misunderstanding was treating AI as an enhancement to traditional testing rather than recognizing that it required a complete paradigm shift. Modern platforms treat testing as a data problem, not a scripting problem. But QA teams kept trying to sprinkle AI on top of their existing processes like fairy dust.


This shallow adoption meant missing AI's real potential. Instead of using AI to:


- Mine production logs to identify untested user paths
- Predict which code changes were likely to cause failures
- Automatically generate test data that matched production patterns
- Learn from past failures to prevent similar issues
- Automate all testing at scale


...teams used it to make their flaky UI tests slightly less flaky. They celebrated when AI could self-heal a broken locator, missing that the real opportunity was eliminating the need for those brittle tests entirely. By treating AI as just another automation library, QA ensured they would always be playing catch-up with teams that understood testing had fundamentally changed.


## What QA Leaders Can Do Next


Is there hope? A sliver. QA can switch to become AI-centric.


The problem is, this will be a huge jump:


QA-Centric Teams AI-Centric Teams


Releasefrequency Weekly → quarterly Multiple per day


Defect triage Manual, ticket queues LLM-generated root-cause clusters


Testmaintenance Human refactor Self-healing scripts & model-based regeneration


Quality KPIs Pass/fail %, defect density User journey health, reliability SLOs


Skill profile Manual tester, test automator SDET, data engineer, prompt & model tuner


The reality is that AI-centric teams are focused entirely within the dev team. When testing can be an automated process within development, where does that leave QA?


The genuine answer is nobody knows. We’re so nascent in the AI-testing journey that we don’t yet understand how it will entirely impact development and QA teams. But, like we said, this isn’t AI’s fault. The rot had already set in when QA chose to optimize for short-term cost and legacy metrics just as software, and now AI, shifted toward speed, data-driven learning, and integrated delivery.


If QA wants a future, it needs to start anticipating what quality means in an AI-first world rather than scrambling to catch up with where AI is today.
