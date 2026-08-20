---
schema_version: "1.0.0"
document_id: "de07fa2438105079968850baf25bed404427fbd2c0f51822cec891d39ad0e87a"
company_key: "yc-zepto"
company: "Zepto"
source_id: "yc-zepto-rss-dc680377f8f2"
canonical_url: "https://blog.zepto.com/the-developer-pipeline-my-zepto-engineering-internship-c8096c07d6e6"
published_at: "2026-07-16T07:13:30+00:00"
first_seen_at: "2026-08-10T05:06:20.120332+00:00"
fetched_at: "2026-08-20T03:19:45.384495+00:00"
content_hash: "sha256:e2616cc2c46f7b6abcfe2f9dfd2dab469c8bd3b45890a8488b19c0ba70e08900"
---

# The Developer Pipeline: My Zepto Engineering Internship

git init : **Starting my Journey**


I was thrilled to land an internship offer from Zepto through our on-campus placements. Naturally, I spent the weeks leading up to the start date buzzing with excitement over the kinds of projects I would be tackling.


And before long, the joining date arrived. I felt an incredible surge of curiosity about what those two months would hold, and I remember being so genuinely eager to dive into the unknown and start learning.


git clone zepto : **Onboarding at Zepto**


On the first day itself, we were given the opportunity to interact directly with Zepto’s senior leadership. The session with **co-founder** **Kaivalya Vohra (KV)** was particularly eye-opening. He shared insights into his personal journey and walked us through exactly how Zepto was built from the ground up. What stood out to me the most wasn’t just the scaling strategy, but his sheer conviction in the vision of Zepto and the passion driving it forward.


Session with KV


Next, Zepto’s leadership mapped out our internship roadmap and high-stakes ecosystem. **CTO Nikhil Mittal** emphasized how even seconds of engineering downtime cause massive losses, while **Koushik Kottamasu (Sr. Director, Eng.)** and **CHRO Sneha Arora** covered AI and career growth.


Following the leadership sessions, we stepped away from our desks to visit the actual backbone of Zepto: the **dark stores** and **super stores** . Walking in, the contrast to a traditional supermarket was immediately striking. Instead of finding similar items grouped together, a bar of soap sat right next to a bottle of moisturizer. While seemingly random, this calculated layout optimized every square inch to minimize picker movement and shave crucial seconds off delivery times.


Dark Store Visit


After the visit, I was introduced to my team and given all the required access for my work as an intern.


git checkout -b monetization : **Joining the Monetization Team**


During my time at Zepto, I had the opportunity to work with the Monetization team, the team responsible for all the advertisements you currently see on the platform. Knowing that ad revenue is poised to be one of Zepto’s major growth drivers moving forward, it was fascinating to peek behind the curtain and learn how the business logic of ad serving actually works at scale.


What immediately stood out to me was how well-organized the team was. We had daily standups and frequent discussions about both current and upcoming projects. I wasn’t just there to listen, either. The team actively expected me to be inquisitive, ask the tough questions, and bring my own ideas to the table during these discussions.


But the most amazing part of the entire experience? **The absolute ownership I was given over my project.** It is an open secret in the tech industry that a lot of companies are hesitant to let interns touch production. Zepto took the exact opposite approach. Instead of handing my code off to a senior engineer, I was expected to handle the deployments myself. I was the one pushing the code, actively monitoring the live traffic, and keeping a close eye on the logs during rollouts. That level of trust and hands-on responsibility taught me more about real-world software engineering than any classroom ever could.


The constant support I received from **Shivam Singhal (Engineering Manager), Gaurav Gupta (Software Engineer III),** and the entire **Monetization team** played an absolutely vital role during my time there.


What I appreciated most was that their mentorship extended far beyond just helping me ship code and hit my internship milestones. They genuinely invested in my growth as a professional, teaching me lifelong skills that I will carry forward in my career: how to structure my thoughts, frame my ideas effectively, and articulate complex technical approaches in a way that actually makes sense to others.


git push origin master: **What I built**


The core problem was a major operational bottleneck: developers were constantly interrupted to investigate why specific brand advertisements were not showing up. While some causes were simple, like an exhausted budget, others involved complex backend ranking algorithms. Because diagnosing these required manual log inspection and deep tribal knowledge, non-technical teams had to escalate almost every query, delaying responses and draining critical engineering bandwidth.


To eliminate this dependency, I built a self-serve diagnostic system using a **Model Context Protocol (MCP)** connected to an LLM. Instead of forcing teams to parse raw server data, the MCP autonomously mimics a live ad-request path across our downstream microservices to fetch the underlying debug data. The connected LLM then analyzes this data to extract the relevant diagnostic signals, pinpointing exactly where and why an ad dropped out of the auction without exposing sensitive payloads or impacting production traffic.


I integrated this architecture directly into the team’s daily workflow via our internal Slack bot. Now, when a visibility query is raised, an AI agent orchestrates the MCP’s diagnostic trace and **uses the LLM to translate those extracted signals into a clear, plain-English explanation.** This empowers operations teams to resolve most blockers instantly on their own, reserving developers only for genuinely complex escalations.


time.sleep() : **Life Outside the Code**


Compiling…☕️


Of course, my time at Zepto wasn’t strictly about architectures and deployments; the Monetization team knew exactly how to unwind. On a day-to-day basis, our evenings were punctuated by casual coffee breaks and some highly competitive, friendly banter around the office foosball table.


We also went on an incredible team outing that served as the perfect, refreshing break from our fast-paced environment. From playing multiple games and jumping into the pool, to sharing late-night conversations over a ton of amazing food, we had an absolute blast. It was these everyday office moments and offsite adventures away from the laptops that truly helped us bond and made the overall internship experience so memorable.


Some takeaways that will stay with me:


- **Scale demands resilience** — in a hyper-growth environment, even a few seconds of downtime have massive business consequences.
- **Code needs business context** — understanding the *why* behind the product is what makes engineering truly impactful.
- **Build to empower others** — using smart architectures like AI to solve operational bottlenecks scales the whole team, not just the codebase.
- **Framing ideas is a technical skill** — writing the code is only half the battle; explaining your approach clearly to others is the rest.


Less debugging, more spiking


Looking back, my time at Zepto was exactly what I hoped it would be: fast, challenging, and incredibly rewarding. It taught me how to think like a product engineer and build systems that actually matter. No matter where my career takes me next, the lessons I picked up during these months will be the foundation I continue to build on.


---


[The Developer Pipeline: My Zepto Engineering Internship](https://blog.zepto.com/the-developer-pipeline-my-zepto-engineering-internship-c8096c07d6e6) was originally published in[Zepto TechXPress](https://blog.zepto.com/) on Medium, where people are continuing the conversation by highlighting and responding to this story.
