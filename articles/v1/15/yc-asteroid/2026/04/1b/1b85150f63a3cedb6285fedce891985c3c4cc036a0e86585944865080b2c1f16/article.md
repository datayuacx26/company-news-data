---
schema_version: "1.0.0"
document_id: "1b85150f63a3cedb6285fedce891985c3c4cc036a0e86585944865080b2c1f16"
company_key: "yc-asteroid"
company: "Asteroid"
source_id: "yc-asteroid-news-import-1b74f0bf6416"
canonical_url: "https://asteroid.ai/blog/browser-employees/"
published_at: "2026-04-15T00:00:00+00:00"
first_seen_at: "2026-07-24T08:09:50.096820+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:edb04860a6092cadb1a7c7843a33536442f36d3be71aaa943a6f1bf6db1e2e80"
---

# Asteroid 2.0: From a browser tool to a browser employee

The first generation of Asteroid agents have had a good run; over the last few months they’ve quietly automated 22,000 hours of human work. Operating deep inside hospitals, call centers and insurance back offices, we’ve helped companies running critical infrastructure unshackle from the scalability, reliability and cost issues of outsourced teams and legacy robotic process automation (RPA).


But as our customers have scaled and expanded their operations, they’ve demanded more. They need faster, more intelligent agents that can tackle the most complicated workflows underpinning their business. They need agents that learn over time and remember key details, and above all they want agents that run reliably every time.


Asteroid’s 2nd Generation of agents are our answer to that demand. Where traditional robotic process automation in healthcare breaks down — brittle scripts, constant maintenance, and no ability to adapt — Gen 2 agents self-heal, rewrite their own code, and improve each time they run.


## A new era of computer agents


4 months ago, we had a fundamental realization; agents don’t belong inside our software. Agents belong on their own computers.


Our agents were bottlenecked on what they could achieve, not because of their intelligence but because of their environment. What followed was a dramatic rearchitecture of Asteroid. The result of that effort is that today, every Gen 2 Asteroid agent gets its own Linux machine, just like a software developer.


That means that every time your agent is automating one of your workflows, it has access to a real file system, a real browser, and a real desktop. Powered by Daytona, Gen 2 agents now read and write code, handle file management, and use a terminal. All of this is native to the agent, built in from the ground up.


Writing down learnings and code for the next run is now a default behavior. Your agent improves over time without any extra work on your side.


## Any screen, any environment


Our customers operate on a huge surface area of legacy platforms. Our agents need to be able to meet customers where they get work done.


For example:


- If a hospital needs to automate medical billing — pulling remittance information from a payer portal built in ASP.NET WebForms, running inside a frameset, served over Citrix — we have to be able to handle it.
- If a prior authorization automation workflow sits behind OAuth and 2FA, our agent needs to be able to retrieve the 6-digit code from a mobile app and authenticate securely.
- If a clinical workflow relies on a portal that spits out result PDFs that are actually image-based scans of fax printouts (yes, really), Asteroid agents need to download the file, run OCR, validate against expected reference ranges, flag out-of-range values, and return a CSV.


Gen 2’s ability to operate seamlessly across desktop and the web browser, on internal networks and authenticated portals, gives us flexibility at the most fundamental level.


## Agents get smarter over time


The biggest request we had from customers was for their agents to learn automatically from their feedback and encode that knowledge.


On the first run, the agent works fully agentically. It uses frontier models to navigate the task, takes notes on what it finds, and writes code it can reuse later. Over time, each failure or unforeseen edge case becomes a learning opportunity, encoded in scripts and written notes stored alongside the agent. As those scripts mature, the agent stops relying on inference for steps it already knows how to do. It gets faster and more reliable at the same time.


When tasked with downloading files from Google Drive, the agent learns that it’s much more efficient and reliable to intercept Google Drive network requests and run` curl` , so it encodes this information in a script for later reuse.


## Browser agents who code: lightning fast & resilient automation


Asteroid runs agents at scale in critical industries, so predictability matters. Gen 2 introduces code writing and execution to Asteroid agents. This allows us to offer what we call ‘determinism mode’: after a few agentic runs, the agent switches to executing its learned scripts directly. No inference or improvisation, just the script running as written.


Not only does this increase reliability over thousands of executions, it also dramatically decreases execution time.


When a script fails, you choose what happens next. Cancel the run and alert, so nothing proceeds on bad data. Or let the agent recover, fix its own script, and carry on. You configure this per step, so high-stakes nodes can be strict while others stay flexible.


Script execution modes


Agentic


The agent reasons through each step using a frontier model, taking notes and writing scripts as it goes.


On failure


AI retries and adapts


Scripted


Runs the learned script first. If it succeeds and the path is clear, the LLM is skipped entirely.


On failure


AI recovers and updates the script


Frozen


Script is locked in production. No AI guesswork — the script runs exactly as written, every time.


On failure


Execution cancels immediately


## The economics of computer-use agents at scale


AI models are increasingly expensive. Users are constantly trying to automate harder and harder computer-based tasks that are critical to their business. These two facts mean that the cost per execution is going up, and the prices can sometimes be prohibitive.


We have a simple trick that lets us work with both of these facts: use the best models money can buy when running agentically, then have the agent write code to automate itself out of a job. If the code ever fails, the website changes, or something new happens, fall back to the AI and have it fix its own scripts.


This lets users tackle complex tasks using the world’s most powerful models, while staying cheap enough to run thousands of times per day and fast enough to power actual work in the hot-path of your company.


One of our customers sends patient information via fax thousands of times per day. They saw their execution time fall from six minutes to sixty-seven seconds as their agent’s scripts hardened. At this volume and risk level, both speed and trust are not optional.


## HIPAA-compliant AI security your IT team can trust


Valuable automation does not happen on the clear web. It happens inside internal systems that carry sensitive data. That’s why we built Generation 2 with zero-trust in mind.


Every execution runs in a fully isolated environment. Credentials are decrypted in memory, injected at the point of use, and never written to disk. Each agent gets permission to read and write exactly what it needs to and nothing else.


Asteroid is a HIPAA-compliant AI agent platform, SOC II Type 2 certified, and trusted by some of the largest healthcare companies in the US. We’re able to sign Business Associate Agreements, maintain full audit trails of agent actions, and handle PHI securely. For healthcare IT teams, that means you can deploy Gen 2 agents into your most sensitive workflows without the lengthy compliance review.


If you have back office browser and desktop work that’s blocking your growth, we’d love to show you what Gen 2 can do.[Book a demo.](https://cal.com/asteroidai/15min)
