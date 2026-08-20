---
schema_version: "1.0.0"
document_id: "f205cb66f587bd7fdceee6b7dca39d350215eda8e67af650ce3790f2dd4cf0ed"
company_key: "yc-codebuff"
company: "Codebuff"
source_id: "yc-codebuff-rss-bef55ad83d13"
canonical_url: "https://news.codebuff.com/p/codebuff-launch-week-day-4-init"
published_at: "2025-04-25T00:19:05+00:00"
first_seen_at: "2026-07-27T08:33:23.704221+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:70aca5686c0f5751569a5d604920933ad61202c25dc237d28ea0b24c2f7d291d"
---

# Codebuff Launch Week — Day 4: Init

# Codebuff Launch Week — Day 4: Init


### Type "init" inside codebuff to generate a knowledge file and (new!) a codebuff.json to manage dev processes


[Codebuff](https://substack.com/@codebuff)


Apr 25, 2025


Hey y’all! Today I’m pumped to share a really useful new command to set up your future projects with Codebuff.


Open codebuff, type “init”, and hit enter!


(You can also run


**codebuff --init** for the same effect!)


This command will send Codebuff off to examine your project and generate two files:


-


**[knowledge.md](https://www.codebuff.com/docs/tips#knowledge-files)** — the classic file to store project knowledge, preferences, and common commands in an unstructured format


-


**[codebuff.json](https://www.codebuff.com/docs/advanced#configuration)** — codebuff’s new config file! This can contain background processes that codebuff should manage


It’s super useful to have Codebuff create these automatically. It works even if you already have a knowledge.md file.


Over time, we can tweak this generation to incorporate best practices for Codebuff!


#### Knowledge files


For those new to knowledge files


[(check out our docs!)](https://www.codebuff.com/docs/tips#knowledge-files) , this is a great place to note down any weird bits about your project so codebuff doesn’t get confused. These files will always be added to context (if in the root level of your project).


Additionally, Codebuff will automatically update this file when you correct it with some info or preference it deems important enough to save.


#### Codebuff.json (new!)


This is the most exciting new piece in today’s release!


You can use this file to specify startup processes, which codebuff will manage for you.


Codebuff will automatically:


-


Start these processes & kill them when you exit


-


Receive new logs from these processes, which can help debug errors!


Instead of opening multiple terminal windows and switching back and forth, now you can just have codebuff run them all.


And, it’s an absolute superpower for the coding agent to be able to monitor the output of all these processes and incorporate any insights from them to help you solve problems.


I’m incredibly excited by this release, which takes a step toward our vision of coding agents managing local development for you.


Try it out for yourself, and let us know how it goes!


---


That’s all for today. But do check back tomorrow!


We’ve saved a real doozy for the 5th and final day of Codebuff Launch Week.


Cheers,


James
