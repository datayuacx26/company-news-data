---
schema_version: "1.0.0"
document_id: "dee3c1a69a727d2fc2f169ccd84562c4645c4525e6b55c0674452a68b2dd3abd"
company_key: "yc-grain"
company: "Grain"
source_id: "yc-grain-news-import-81739ddbe75a"
canonical_url: "https://grain.com/blog/ai-note-taker-privacy-security"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-08-09T22:47:29.145404+00:00"
fetched_at: "2026-08-09T22:47:30.801934+00:00"
content_hash: "sha256:01c6ff0fd6db646096c23df8fe360fdee3a9ef27b8df16e148f446fb118a0b2a"
---

# What "Secure" Really Means (and How Grain Handles It)

An AI note-taker now joins most meetings by default. It listens, transcribes, writes the recap, and drops it in a channel before anyone has left the call. For a while the only question people asked was whether the notes were any good. That has changed. In August 2025, a[class-action lawsuit accused Otter.ai](https://www.npr.org/2025/08/15/g-s1-83087/otter-ai-transcription-class-action-lawsuit) of recording private conversations and using them to train its models without proper consent. The story traveled fast, and "wait, is this even safe?" became a front-page question instead of an IT footnote. AI note-taker privacy and security is now something buyers research before they sign up.


Here is the problem with that question. When someone asks whether an AI note-taker is secure, they are usually asking about one of four very different things, and most buying guides answer the wrong one. Worse, the common reaction to all this anxiety is to lock everything down, which quietly destroys the reason you adopted the tool in the first place. So let's separate the four worries, look at why the lockdown instinct backfires, and lay out how to evaluate any note-taker without trading away the value. Grain included.


## **The four things people mean by "secure" in an AI note-taker**


### **1. Will my data train someone else's AI?**


This is the loudest worry, and the one the Otter case put under a spotlight. The fear is not really about hackers. It is that your confidential discussions get processed on a vendor's servers and then fed into the models that improve the product, or worse; sold to a competitor. Think deal strategy, customer details, a sensitive HR conversation. Large language models can sometimes surface patterns from their training data, so "we use your meetings to make the AI better" reads, to a lot of buyers, as "your secrets might leak into someone else's output."


The reassuring part is that this is now a settled expectation, not a fringe demand. Granola, Read AI, MeetGeek, and Fellow all publish explicit "we do not train on your data" statements, and several enforce it contractually for enterprise accounts. If a vendor cannot give you a clear answer here, that silence is the answer.


### **2. Did everyone actually agree to be recorded?**


This is the consent worry, and it is[part legal and part social.](https://www.littler.com/news-analysis/asap/ai-transcription-and-note-taking-technologies-seven-points-employers-consider) The complaints that fueled the Otter lawsuit were not about encryption. They were about a bot appearing in a meeting nobody invited it to, recording before anyone agreed, and emailing a transcript to people who never showed up. In many states and countries, recording a conversation requires consent from everyone on the call. Two-party consent and wiretap laws turn a sloppy default into a real liability.


So when a nervous participant asks if a tool is "safe," they often mean something closer to "will this thing record me without telling me, and who ends up reading it?" Notification, consent prompts, and control over who receives the transcript matter more to this person than any certification badge.


### **3. Will it pass our security review?**


This is "secure" in the formal, procurement sense. The person asking is usually an IT admin or a security reviewer, and they are not scared. They are qualifying you. They want SOC 2 Type II, ISO 27001, GDPR alignment, plus encryption in transit and at rest. SOC 2 Type II in particular signals that an independent auditor checked that the controls actually work over time, not just on paper.


This crowd is smaller than the first two, but they sit at the bottom of the funnel with a purchase order in hand. A clear, current trust page answers most of their questions before they have to email you.


### **4. Can the app itself be breached?**


This is the literal reading: can the software be hacked, has it suffered a breach, is my account safe. It is a real concern, but it is the smallest of the four, and most people who think they are asking it are actually asking about data training from worry number one. The exception is the privacy-purist segment that wants a fully private AI note-taker that runs local, offline, or self-hosted, where audio never touches the cloud at all.


## **Why locking everything down is the wrong fix**


Once a team gets spooked, the reflex is to clamp down. Stop recording internal meetings. Wall off every transcript by person and team. Restrict access so tightly that finding last week's call takes three approvals. It feels responsible. It is mostly counterproductive.


Here is the trade-off nobody mentions. AI gets more useful as it gets more context. A model that can see the full arc of a deal across six calls writes a sharper follow-up than one staring at a single transcript. An assistant with access to the last three planning meetings can draft a decision document that actually weighs the trade-offs your team already discussed. Cut off that context and you do not just lose convenience. You lose the recall, the cross-meeting connections, and the post-meeting work that justified buying an AI tool instead of a basic recorder.


The lockdown instinct treats privacy and value as opposites. They are not, as long as access control is granular. The goal is not to record less. It is to control access well enough that you can safely record more.


## **How Grain approaches it**


Grain's model starts from a different premise than most note-takers. Instead of capturing only external sales calls and siloing everything else, Grain is built to capture all of your meetings, internal and external, and treat them as connected. Each meeting is enriched with who said what. Meetings link to each other so the system can build a working knowledge graph, which means the AI understands references to earlier conversations instead of reading every call with zero context.


Capturing everything is only safe if you decide who sees what. Access is role and permission based, so you choose who can view a given meeting and what gets shared more widely. Enterprise workspaces can require single sign-on, which gives admins tighter control over who gets in. Most of your meetings are not sensitive, and sharing those broadly is what makes the AI smart. The handful that are sensitive stay locked to the right people. You get the context advantage without turning every internal call into an open file.


Take the four worries in order.


On training, Grain does not use your meeting data to train AI models. Grain generates summaries, key points, and next steps by sending your transcript to a large language model through AI providers like OpenAI and Anthropic. You can learn more about our model inference partners at[grain.com/grain-subprocessors](http://grain.com/grain-subprocessors) . The connection runs over TLS. You can also delete any meeting yourself, which removes the transcript, clips, and summaries, with a 30-day window to restore it before it is gone for good.


On consent, Grain's botless capture sidesteps the "uninvited bot" problem entirely. It records and transcribes without sending a bot into the call, so there is no separate participant showing up in the attendee list. That matters for the exact scenario behind the recent headlines, where a bot joins a meeting nobody invited it to.


On compliance, Grain is SOC 2 Type II certified with a clean report, and complies with GDPR and SOC 2 where applicable. Customers can request the SOC 2 report and other documentation through Grain's Trust Center.


On app security, all customer data, including recordings and transcripts, is encrypted in transit and at rest. Connections use SSL, any HTTP request is redirected to HTTPS, and Grain holds an A+ grade from Qualys SSL Labs. The application is hosted on AWS in the United States inside a virtual private cloud with restricted access, and Grain runs third-party penetration testing at least once a year on top of its own internal review.


Read more at[grain.com/security.](http://grain.com/security)


The point is the shape of the approach. Privacy at Grain is not a wall bolted on after the fact. It is the access layer that makes capturing everything reasonable in the first place.


## **A checklist for evaluating any note-taker**


You do not have to take any vendor's word for it, Grain included.[Run every tool you consider through the same questions](https://meetingnotes.com/blog/gdpr-compliant-ai-meeting-assistants) :


- **Training:** Do you use my meetings to train your models? Can I opt out, and is that the default? Is it enforced contractually for my plan?
- **Consent:** How does a participant know recording is happening? Can I control who receives the transcript?
- **Certifications:** Are you SOC 2 Type II? GDPR aligned?
- **Encryption:** Is data encrypted at all times, and to what standard?
- **Retention and residency:** How long do you keep my data, where is it stored, and can I delete it on demand?
- **Default access:** Who can see a recording the moment it is created? Is the default private, team-wide, or company-wide?


A confident vendor answers all six without flinching. Vague or missing answers tell you where the risk lives.


## **The takeaway**


The anxiety around AI note-takers is reasonable, but the framing is usually off. "Is it secure" is really four questions, and only one of them is about the app getting hacked. The bigger concerns are whether your data trains someone's model, whether people consented to being recorded, and whether the tool clears your compliance bar.


If the product you are reviewing answers those well, you do not have to retreat into recording nothing. Privacy done right expands what AI can do for you rather than shrinking it. The teams getting the most out of this technology are not the ones recording the least. They are the ones who control access well enough to safely record more.


## **FAQ**


**Does Grain train its AI on my meeting data?** No. Grain does not use your meeting data to train AI models. Summaries and other AI content are generated by sending your transcript to a large language model provided by Claude, Gemini, and ChatGPT. Learn more about which models we use at[grain.com/grain-subprocessors](http://grain.com/grain-subprocessors) .


**Is it legal to record meetings with an AI note-taker?** It depends on where the participants are. Many states and countries require consent from everyone on the call. Grain's botless capture records without sending a bot into the meeting, but you are still responsible for getting consent where the law requires it.


**Is Grain SOC 2 or GDPRcompliant?** Grain is SOC 2 Type II certified and complies with GDPR and SOC 2 where applicable. A BAA is available on enterprise plans. You can request the SOC 2 report through Grain's Trust Center at[trust.grain.com](http://trust.grain.com/) .


**Is my data encrypted?** Yes. All customer data, including recordings and transcripts, is encrypted in transit and at rest. Grain holds an A+ grade from Qualys SSL Labs and hosts its application on AWS in the United States.


**Can I control who sees a particular meeting?** Yes. Access is role and permission based, so you set who can view a given recording and what gets shared more broadly. Enterprise workspaces can also require single sign-on for tighter admin control.


**What is the difference between how Grain handles internal versus external meetings?** Grain is designed to capture both and connect them, rather than siloing internal calls. Access controls let you decide what stays private regardless of meeting type.


**Are my AI meeting transcripts confidential?** Only if the tool defaults to private access and lets you control sharing. Some note-takers make notes shareable by link by default, so anyone with the URL can read them. In Grain, access is role and permission based, so you decide who can view each recording.


**Can my employer use an AI note taker to record me?** In most workplaces, yes, as long as recording laws are followed. Where you are may require everyone on the call to consent first. A responsible employer tells people a meeting is being recorded and limits who can see the notes afterward.


**How do I stop an AI note taker from joining my meeting?** It depends on the tool. Most let you turn off calendar auto-join, remove the bot mid-meeting, or revoke its calendar access. With Grain's botless capture there is no bot to remove, since nothing joins the call as a separate participant.


‍
