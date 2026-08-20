---
schema_version: "1.0.0"
document_id: "393bc84cbe8a290452b9a0908869f4cc5cea50a0c2fc7cf15553992a923dd088"
company_key: "yc-metorial"
company: "Metorial"
source_id: "yc-metorial-news-import-9c457a3d81d4"
canonical_url: "https://metorial.com/blog/what-i-actually-handed-the-agent"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-24T04:14:45.918392+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:b24d5ab20a8a2f009f9c3d55a32faa28b67707ac03c52d3984107f9d51eef9c2"
---

# What I actually handed the agent

# What I actually handed the agent


In mid 2025, before I joined Metorial, I built an internal agent to help with CRM cleanup. Deduping contacts, filling in missing fields, the kind of tedious work that is perfect for a machine. It was great. It saved me hours. I was, briefly, very pleased with myself.


Then a friend who does security asked me a simple question over coffee. "What can it actually do in there?"


I started to answer, opened my mouth, and realized I did not fully know. So I went and looked. This is what I found, written down plainly, because the moment I wrote it down was the moment it stopped feeling clever and started feeling like a problem.


## The connection


The agent was connected to our CRM. It was authorized as an admin. The token had no expiry.


Read that back slowly, because I did not, at the time. An autonomous process, running on a schedule, making decisions from model output, was holding an admin credential that never expired. I had given a script the same access I would give a senior operations hire, except the script does not get an onboarding, a manager, or a performance review.


I did not do this because I was reckless. I did it because admin was the path of least resistance. The CRM's auth screen offered me admin, the integration worked immediately with admin, and the deadline was that afternoon. Every shortcut I took was the shortcut the tooling made easiest. That is the part that should worry all of us. The insecure default was also the convenient default.


## What it was granted


When I actually listed the permissions, here is what the agent had.


**Read every contact.** Every name, every email, every private deal note our sales team had ever written. Not the subset it needed for dedup. All of it.


**Write every field.** It could overwrite anything. There was no notion of "this agent may correct a phone number but may not touch the deal stage." Write was write. The blast radius was the entire object.


**Send email on our behalf.** This one I had genuinely forgotten was in the scope bundle. The agent could send mail as us, with no human in the loop, because the integration granted messaging and contacts together and I accepted the whole bundle.


Three grants. Each one wider than the job required. Together, basically the keys.


## What was missing


The grants were the half that existed. The scarier half was the things that did not exist at all.


**Scoping.** There was no way to say what the agent could and could not touch. It had reach over everything, and "everything" is not a permission you can reason about. It is just a surface.


**An audit log.** There was no record of what it did. None. If it had quietly corrupted a thousand records at 3am, I would have found out from a confused sales rep, not from a log.


**An answer to "what did it change?"** The only way to reconstruct the agent's actions was to diff the database by hand against a backup, assuming I had a clean backup from the right moment, which I did not, because why would I, it was a cleanup script.


So the honest summary of my clever little agent was this. It could reach everything, and I could prove nothing.


## This is not a me problem. It is a default problem.


I want to be clear that I am not writing this as a confession of unusual carelessness. I am writing it because the more people I talk to, the more I realize this is the normal state of internal agents at companies that are otherwise very serious about security.


The reason is structural. When you connect an agent to a system today, you are usually handed a binary choice. Connect with broad access and ship now, or build your own permission and audit layer and ship in three weeks. Under any real deadline, broad-access-now wins every time. The tooling rewards the wrong thing.


And agents make this worse than the old script-with-an-API-key situation, for two reasons. First, they act on model output, which means their behavior is not fully predictable from the code, which means "I read the script so I know what it does" is no longer true. Second, they are getting more capable and more autonomous every quarter, so the gap between what they are allowed to do and what you can observe them doing is widening, not closing.


A standing admin token with no audit log was an acceptable risk for a cron job that ran one known query. It is a genuinely bad idea for a thing that decides, at runtime, what to do next.


## What good actually looks like


After the coffee, I rebuilt the thing, and I had a much clearer spec for what I wanted. It was not complicated. It was just everything the easy path had skipped.


Scoped access, so the agent can read the fields it needs for dedup and literally cannot see private deal notes. Narrow writes, so it can fix a malformed phone number and cannot move a deal to closed-won. No standing admin and no immortal tokens, so access is short-lived and least-privilege by default. And a real audit log, so that the answer to "what did it change" is a query, not an archaeology project.


None of that is exotic. It is the same access-control hygiene we already apply to humans and to services. The only reason agents skipped it is that the integration layer for agents is young, and young infrastructure ships the convenient default before it ships the safe one.


Honestly, that experience is a big part of why I ended up at Metorial. The connection between an agent and a real system should come with scoping, expiry, and audit as the default, not as the three-week project you do after a security review scares you. The right amount of access, provable after the fact, should be the easy path. Back then it was the hard one, so I picked the easy path, because people always pick the easy path. So the easy path has to be the safe one. That is the thing we are trying to make true.


## The question I would ask you


You do not have to take my word for any of this. Just go run the same audit I did.


Pick your most useful internal agent. Write down, in plain words, three things. What is it authorized as. What can it touch. And how would you prove what it did last Tuesday.


If you can answer all three quickly, you are in good shape and I am a little jealous. If you cannot, you are not careless. You just took the path the tooling made easy, same as I did. The fix starts the same way mine did, with someone asking a simple question, and you being honest enough to actually go look.
