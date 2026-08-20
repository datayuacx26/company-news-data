---
schema_version: "1.0.0"
document_id: "4812efeb0c6b07cd7958eb9c716f915cabf9953e224f82ff6055eff73319255a"
company_key: "yc-gusto"
company: "Gusto"
source_id: "yc-gusto-engineering-rss"
canonical_url: "https://engineering.gusto.com/storage-is-the-missing-piece-for-agent-built-software-ede96ac7ac27"
published_at: "2026-06-16T15:59:27+00:00"
first_seen_at: "2026-07-19T22:15:27.842622+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:ca3ce141bfc0eef8fbf2fe585b21ca900823846c913cc7c80538d3e96c98811e"
---

# Storage Is the Missing Piece for Agent-Built Software

# Storage Is the Missing Piece for Agent-Built Software


[Eric Baer](https://medium.com/@baerbaerbaer?source=post_page---byline--ede96ac7ac27---------------------------------------)


6 min read


·


Jun 16, 2026


--


Press enter or click to view image in full size


Gusto, like most companies building with LLMs, is amassing a[LOT of software](https://x.com/kdaigle/status/2040164759836778878?lang=en) : one-off projects, Claude Artifacts, HTML visualizations, dashboards, and my own growing fleet of personal tools.


The agent loop made building 100 times easier, but deployment and operations still assume the old world. Your choices are weirdly binary:


1. Static sites with data *in the HTML*
2. A whole app with Dockerfile, Postgres, secrets, CVE mitigation, deploy pipeline, PagerDuty, and sharp edges.


When software was hard, most things were #2. Now, most things sit in the middle: stuff that’s real enough to need a database, but not real enough to deserve an app stack.


Neither fits, and this isn’t about engineers vs. non-engineers. Everyone’s a builder now, and everyone from marketers to top engineering teams is[using the same tools](https://openai.com/index/harness-engineering/) . The agent loop is producing heaps of legitimate software, and IMO, if your platform can’t survive this authoring mode, it’s the wrong thing for the moment we are in.


We need things that make “casual software” easy to create *and* easy to operate.


## Saying “yes” to builders, without compromising engineering


There is an AI catechism: automate supply, summon infinite demand. And, as our platform and security teams’ request queues can attest, this seems to be true.


We use Claude heavily in all roles, both inside and outside our engineering org, and our platform help channels were filling up with folks asking essentially: “Where do I put this thing?” So we built a simple static-site hosting tool, and for a minute, everyone was happy. They went off analyzing customer engagement data, code quality metrics, and the 100 other things our MCP tools have access to, and would share this all over Slack.


Inevitably, some of these sites became load-bearing, which exposed the harder question: “If I need to update the data regularly, how can I store it?” For most of these projects, neither an HTML page nor “Congratulations, you own/operate a service” is a good answer.


There were two problems to solve:


- How do we make it easy for a non-engineer builder to own/operate?
- How do we support this in our infrastructure?


**On Supporting Non-Engineer Builders**


The agent loop works for Engineers because they quietly absorb its failures. But a workflow that depends on taste, scar tissue, and vigilance does not scale to[everyone else](https://github.blog/news-insights/octoverse/octoverse-a-new-developer-joins-github-every-second-as-ai-leads-typescript-to-1/) .


When the operator may not be technical enough to understand a crash, tools should make it easy to claw their way out with minimal fanfare. The operational burden can’t ask them to look in Buildkite logs to understand that there is an issue with secrets rotation.


**Platform Problems**


Second, we should be seriously considering fleet economics. For “serious software,” a little extra machinery is fine. For a large number of agent-built internal tools, that machinery becomes really important. Every idle database and forgotten secret is operational overhead someone eventually pays.


Our platform teams are already busy supporting our load-bearing[apps](https://gusto.com/company-news/gusto-reaches-1b-in-revenue) . They don’t need a hundred semi-serious tools that keep secrets, generate alerts, appear in service catalogs, and page people after the original author has moved on, if there even was a formal owner in the first place.


## Safety = Guardrails + AI-native Building Blocks


There are two answers: Guardrails to reduce the blast radius, and building blocks that make the safe path the easy path.


Guardrails matter, and we’re building them: quarantined runtimes, safer agent execution, separate AWS accounts, review bots, and other blast-radius reducers. But guardrails only make dangerous paths less dangerous. They do not create a better default than “HTML blob” or “real service.”


## Get Eric Baer’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


**What about the building blocks?**


The standard engineering toolkit has a lot of sharp edges because it’s fundamentally built to handle really big workloads. For semi-serious apps, here’s my wish list for tools


- **Zero Idle Burden.** It should not accumulate rent for existing, and regardless of who is operating it, fleets of small software can’t tolerate CVE mitigation, patching, kernel panics, inactivity emails, or a $5 monthly floor.
- **Success has an exit.** If the app becomes important, graduation should be mechanical, not a deep technical migration
- **Agent-shaped API.** The entire surface needs to be small enough to fit in context, typed enough to reject hallucinated methods, and low-ceremony enough that the model is not asked to invent and preserve a schema before it has earned one. This is not cosmetic.


In 2026, you need three basic parts to build an app: compute, storage, and tokens. Compute and tokens both have answers that fit the bill. Compute has scale-to-zero primitives like FaaS that are[hard to get wrong](https://arxiv.org/abs/2307.03958) . Tokens are just API calls.


But what about storage? I am a card-carrying member of the[Boring Technology Club](https://boringtechnology.club/) , and Postgres is the[right answer](https://www.amazingcto.com/postgres-for-everything/) to many questions, but it’s wrong here for two reasons: Operations and LLM Usability.


Real databases entail real obligations, and none of that becomes free because the app has four users. Provisioning, secrets, backups, VULNs, migrations, alarms when the disk fills, alarms when the pool is exhausted, etc.


Worse, “Real” tools invite the agent to generate a real-service ceremony: migrations, schemas, RLS, seed files, whatever. Complexity creep degrades the Agent’s loop speed and reliability as the app grows and can leave the operator unsure why.


## Gusto’s New AI Building Block


We’ve started trying out something new. It’s called[baerly-storage](https://github.com/Gusto/baerly-storage) , and we’ve open-sourced an early version of it today.


It’s an S3-backed datastore with **no runtime** . None. Everything runs to completion inside the HTTP request that triggered it, and the whole public API fits in a single markdown file small enough that an LLM can hold it in context.


**For Platform Teams: It’s just a bucket**


You know what is even more boring than Postgres? Object storage like S3. It’s the only modern infrastructure primitive that is pre-cleared by security, politically boring, and where every major cloud has a compatible endpoint. It’s safe and portable by definition. Your bytes in your bucket with your security.


And it turns out that with a little work, you can turn an S3-compatible bucket into a document database with a 100 KB TypeScript library. Yes, library. A database without a runtime. No daemon, no service bill, no on-call. The only persistent component is your bucket. The entire thing runs inside the HTTP request that triggered it.


The trick is not to make S3 behave like a traditional database. It is to use it like other modern storage systems: write immutable data, then publish a new version with a small, atomic metadata update. S3 now[gives us the consistency](https://aws.amazon.com/blogs/aws/amazon-s3-update-strong-read-after-write-consistency/) and conditional writes we need for that final “publish” step, so the rest is mostly borrowing from a well-known storage playbook. Iceberg, Delta Lake, Turbopuffer, and Litestream all landed on this shape.


And you can export the change log as a Debezium-style JSON change event, so upgrading to Postgres is easy.


**For Builders: This is LLM-native storage**


Most APIs are built for humans, then adapted for LLMs. We wanted the opposite: an API where the model is a first-class user.


The entire public surface is a 12k token doc file, which is tight enough that even small models can include it as context. And, the whole thing is constrained by a type system. The API boundary is small and closed, and if the LLM hallucinates, it doesn’t even compile.


We don’t want to have to say “you should have used better prompts,” we need APIs whose affordances are small, clear, and typed enough for the model to use natively.


## How its Goin’


We’ve rolled this out across a few internal applications, partnered with a couple of non-engineer builders to make sure it’s as easy as we think it is, and started building an internal SaaS for lightweight apps. In short, we’re testing this in a few constrained ways before making it more widely available. But, so far, so good!


We are all increasingly authoring software through the agent loop, and downstream of that are many small, semi-serious software artifacts, for which compute and tokens have a primitive, and storage does not.


So we made one shape of an answer and started trying it. Not because every prototype should live forever on object storage, but because the success path should be graduation, not regret. The alternative for many of these tools is not “use a better database.” It is that the experiment never happens, or quietly becomes another spreadsheet or shadow operational burden.


Give[baerly-storage](https://github.com/Gusto/baerly-storage) , a try. Maybe it will help you build fast, too. Or, join us! Gusto is hiring. Check out our careers page at[gusto.com/careers](https://gusto.com/careers) .
