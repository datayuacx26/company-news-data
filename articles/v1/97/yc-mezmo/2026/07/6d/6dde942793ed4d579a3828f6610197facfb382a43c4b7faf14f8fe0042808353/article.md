---
schema_version: "1.0.0"
document_id: "6dde942793ed4d579a3828f6610197facfb382a43c4b7faf14f8fe0042808353"
company_key: "yc-mezmo"
company: "Mezmo"
source_id: "yc-mezmo-news-import-3b2f958954ed"
canonical_url: "https://www.mezmo.com/blog/builder-in-the-loop-eric-lake-on-on-making-aura-smarter-after-every-incident"
published_at: null
first_seen_at: "2026-07-22T04:13:38.440386+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:d9cad6ea30831c40f306a84c5a6e50387df2130b3609096b675cc2feed917c8b"
---

# Builder in the loop: Eric Lake on making AURA smarter after every incident

Builder in the Loop is a Mezmo interview series focused on the engineers, product leaders, and operators shaping AURA, an open-source, MCP-native agent harness for production operations.


The goal is to get past the polished product layer and talk through the decisions that matter when AI starts interacting with real systems. Key questions include: What should agents be allowed to do? How do they get better over time? Where should humans stay in the loop? What does it take to make agentic workflows reliable enough for SRE and platform engineering teams to trust?


This installment features Eric Lake, an SRE at Mezmo,[discussing runbook automation](https://www.mezmo.com/blog/the-runbook-problem-how-aura-documents-what-teams-dont-have-time-to-write) , investigation memory, and why the teams that will get the most out of AURA are the ones who have been woken up at 2 AM too many times.


## **The short version**


Eric's perspective: agents are only as useful as the context they can reach. An agent that rediscovers the same ground every incident is not actually learning anything.


To make[AURA](https://www.mezmo.com/aura) useful for on-call SREs, the work has focused on three things:


- **Runbook access:** AURA connects to GitHub-hosted runbooks via MCP, giving investigations a documented starting point
- **Runbook generation:** when a runbook doesn't exist, AURA creates one from what it finds, with a human reviewing before it merges
- **Investigation memory:** AURA stores what it learns from each investigation, so the next incident starts with context, not a blank slate


## **Icebreaker: The tool Eric cannot live without**


To kick things off, we asked Eric to share the one tool he can't live without. His answer: NeoVim. He's customized his setup with support for the languages and file formats he works with daily, everything from Terraform to Markdown and occasionally drops into a terminal right inside the editor when he doesn't want to break his flow. It's less "text editor" and more command-line home base.


It's a fitting answer from someone whose workflow is also about reducing context-switching: alert fires, AURA investigates, resolution happens, without bouncing between tools.


## **The problem: Every investigation starts from zero**


When Mezmo first wired up a Slack bot to surface AURA as an on-call interface, the flow looked clean on paper. A PagerDuty alert fires, the Slack bot sends the alert body to AURA, and AURA starts investigating.


The problem was what AURA could not reach. Alert payloads typically include a runbook link, but AURA had no access to that document. No MCP connection, no way to read it.


"Every single time that AURA needed to do an investigation, it was having to rediscover everything brand new."


For an SRE team, that's the equivalent of hiring someone who has handled the same incident dozens of times but starts each shift with no memory of any of them. The agent was capable. It was just working blind.


## **The build: GitHub MCP, auto-generated runbooks, and stored memory**


Eric and the team attacked the problem in layers.


First, they migrated runbooks to GitHub and connected the GitHub MCP server to AURA. Now when an alert fires with a runbook link, AURA checks whether that document exists. If it does, it pulls the content and uses it to guide the investigation. If something new is discovered during the investigation then AURA can generate a PR for updates to the runbook. If the runbook is missing or the link is to the base repo instead of a document, AURA completes the investigation anyway and generates a PR to create a new runbook based on what it found. A human reviews and merges.


The second layer was memory. Each time AURA runs an investigation, it stores what it learned. The next time a similar alert comes in, the prompt directs AURA to check its memory before starting fresh.


What AURA learns in one incident shows up in the next. Not through retraining, but by accumulating the institutional knowledge that SRE teams have always relied on people to carry.


The third layer is accuracy, and Eric has a specific way of framing it.


"The AURA SRE agent is a tool. Like a knife in a chef's kitchen, you have to hone it and keep it sharp. Having runbooks and memories that are accurate and to the point is like sharpening the tool so it can do the best job possible for you."


That shapes how the team thinks about the review step. The PR gate before any runbook merges is not just quality control. It is how trust gets built. Giving an agent more autonomy requires confidence that it will do the right thing, and that confidence has to be earned through a track record. The operator sees what AURA found, validates what it proposes, and decides whether to merge. Over time, that record is what makes it reasonable to let AURA handle more.


## **Daily usage: On-call from anywhere**


Eric's most common workflow with AURA runs through the same Slack bot. The bot is, as he puts it, "just the wrapper and the interface for getting my questions to AURA." Behind the scenes it uses MCP tooling to pull whatever data AURA needs.


What that unlocks is something SREs have wanted for years: a real investigation without being tethered to a laptop.


"I could be sitting on my couch, and in Slack, I can say on my phone, hey, look into this thing for me, and let the agent go do the heavy lifting."


No terminal. No Kubernetes context switch. AURA digs in while the SRE stays in Slack, on whatever device they have in hand.


## **The vision: AURA as first-line on-call defense**


Eric knows where AURA sits today. The immediate value is investigation support: validating whether an issue is still active, digging into root causes, surfacing context faster than a human could gather it manually.


The longer-term picture is more ambitious.


"Once AURA is able to remediate some of the low-hanging fruit and not wake up somebody at 2 AM, then I think that that's gonna really make people's lives a little better."


That means AURA receiving a page, acknowledging it, working through the runbook, restarting a pod, and only escalating when it genuinely cannot resolve the issue. Better work-life balance for anyone who has had their sleep interrupted by an alert that turned out to be nothing.


## **Wait, you can do that?**


Not every AURA interaction is a production incident.


Eric's manager once opened a Slack thread with the bot and asked it, with no particular purpose, to draw a dinosaur. AURA asked whether he wanted it as a PNG or ASCII. He picked ASCII. The result was, by Eric's account, actually pretty good.


Prompt: **@SRE Bot - Prod** make me an image of a dinosaur


‍


"I would never have thought to ask our SRE agent to generate an ASCII chart. But you would think you could. It can do mermaid charts and diagrams and flows of how your data is going through the system."


An agent harness with broad MCP connectivity will do more than the use cases you planned for. The only way to find out where the edges are is to push on them.


## **What’s next**


[AURA](https://github.com/mezmo/aura) has evolved from a single-agent setup to a multi-agent architecture. The work of making it reliable enough to trust with more autonomy is ongoing.


For SREs and platform engineers thinking about where to start: the goal is not to hand off the incident. It is to stop starting from zero every time.


AURA is Apache 2.0. Repository:[github.com/mezmo/aura](http://github.com/mezmo/aura)


New to AURA: The Quick Start Guide is the fastest path to your first investigation.


‍


## **About the Interviewee**


[Eric Lake](https://www.linkedin.com/in/ericlake/) is an SRE at Mezmo with over two decades of on-call experience across multiple companies. His work on AURA focuses on runbook automation and investigation memory, with the goal of making AURA a reliable first line of defense before incidents reach a human.


‍
