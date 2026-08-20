---
schema_version: "1.0.0"
document_id: "2e7a8d7abf8d27cbfe3418d824f75778e323df5b29e4a115eda274c11691d388"
company_key: "yc-speak"
company: "Speak"
source_id: "yc-speak-news-import-4988501dda0c"
canonical_url: "https://www.speak.com/blog/using-agentic-coding-to-solve-influencer-ops"
published_at: "2026-05-21T00:00:00+00:00"
first_seen_at: "2026-07-22T14:26:08.696382+00:00"
fetched_at: "2026-07-28T22:12:59.555603+00:00"
content_hash: "sha256:da67eb0ae144ee016af199d70706ca7065bfbbea7c498d1dbedbc7e3e5a1b467"
---

# Using Agentic Coding to Solve Influencer Ops

Influencer marketing is famously resource-intensive. Unlike paid social, you can't turn a dial on a dashboard and scale. Growth means more humans doing more work: finding creators, managing endless email threads, chasing down drafts.


That kind of breakdown isn't a discovery problem or a quality problem. It's a communication problem. And in influencer marketing, communication is where ROI quietly dies.


Most teams split this work into three buckets: finding creators, managing the campaign, and reviewing the content. Discovery and review directly shape the final output, and they deserve human brainpower. The middle bucket is overhead. It exists so the project doesn't fall apart, not because it makes the work better.


Human brainpower is the most expensive compute we have. Spending it on chasing down draft submissions and re-sending trial credentials is a misallocation.


**Why generic tools didn't fix it**


We tried Google Forms & Zapier first. The pitch of generalized SaaS is that it solves everyone's problem well enough, and that's the problem. "Well enough" meant influencers skipped the drafting step, missed submission dates, and lost their unique trial account info inside email threads we couldn't search.


I'm a marketer, not an engineer. But the gap between what we needed and what off-the-shelf tools gave us was wide enough that I decided to build something specific instead.


**What we built: Musubime** ‍


In Japanese, Musubime means “knot.” Just as a knot binds two strands into one, Musubime ties our team and creators together in a single shared workflow. Our influencer partners log in via a unique login link that’s connected to our database. Once in, they can see every active partnership at a glance: what's due next, days until the upcoming deadline, the full submission timeline.


It does two things well.


**The influencer side shows one thing at a time.** When a creator logs in, the top of the screen is a "Next Step" panel. If they haven't signed the contract, it asks for their legal info and fires the DocuSign API to send a pre-filled contract once they submit. If they owe us a draft, the panel asks for the draft. Below the action is a progress bar from Onboarding to Payment so they know where they stand. No hunting through email for the latest instruction.


**The admin side is a queue, not a database.** Our team doesn't open Musubime to browse, we open it to act. The "Actions" tab only surfaces items that need a human decision. A draft came in? It's in the queue. Approve or send feedback inline. The system handles the rest, including the email back to the creator. We didn't try to replace email. We’ve found that email is still where influencers actually read things. We just took the sending responsibility out of our hands.


As soon as we implemented Musubime, we saw a 70% drop in communication volume, saving us roughly an hour per creator. This allowed us to spend more time reading their drafts for feedback which improved both the content quality and performance. We were initially worried influencers wouldn’t take action in Musubime, but to our surprise, 95% took the right actions and successfully delivered.


**How we built it**


Reminder: I’m a marketer with no technical background. I built the MVP in a week using Antigravity, an agentic coding IDE. We used Antigravity because it was fast, and was offering a credit bonus after their launch. I now maintain it with Codex and Claude Code.


The stack:


- **Database:** Supabase
- **Hosting:** Vercel
- **Automation:** GitHub Actions for daily reminders and backups
- **Glue:** Zapier webhooks for backend logic where waiting on full API access would have slowed me down


Yes, Zapier is in here! The same Zapier that didn't work as a product layer works fine as plumbing.


I didn't use clever prompt engineering. I described what the app needed to do and let the agent build. The one thing I'd redo: spend more time on the database schema and directory structure before writing the first line of code. Cleaning up that debt later costs more than thinking it through upfront.


Building it was simple, and I could see my vision, in plain language, being built quickly in front of me. Sometimes errors would pop up on my screen, but I would take a screenshot and share it with Antigravity, which usually fixed the issue quickly. In hindsight, though, I realized it was also creating “spaghetti code” behind the scenes. Agentic LLMs are still not very good at cleaning up code as they build. Whenever I request a new feature or a bug fix, it tends to add something into the existing code rather than structuring it cleanly. Instead of organizing folders or reusing existing code, it often increases the line count and adds complexity that will hurt in the future.


I can imagine many non-technical people would build things without noticing this happening behind the scenes, which might be okay for a simple product used by a few people. However, when it comes to an end-to-end platform that’s customer (in this case influencer) facing, I’d highly recommend thinking through the architecture first, either yourself or with the help of technical people, and making sure you are satisfied with that system design rather than simply telling AI your vision.


**The takeaway**


If you have a codebase and a database, you don't have to wait for a SaaS vendor to solve your specific problem on their roadmap. Plan the schema. Describe the workflow. Build what you need.


The cost of building a tool that fits your team exactly has dropped low enough that "build it" is now a reasonable answer to problems where the only previous answer was "live with the generic tool."


**What’s next for Musubime?**


We’re evolving the platform to be agentic. Soon, Musubime will work 24/7: reading emails via the Gmail API, drafting contextual replies, updating the database status, and sending dynamic follow-ups (currently, it only handles deterministic follow-ups).


When the team accesses Musubime, they will simply see a log of what the agent handled autonomously, alongside a curated list of action items requiring human judgment (like nuanced emails or final draft approvals).


This next phase of automation will give us even more time back to focus purely on work that requires human brainpower: strategy, quality assurance, and brand alignment to grow the influencer channel further.


The bar for 'just build it yourself' has moved. If you can describe the workflow, you can ship the tool.


‍
