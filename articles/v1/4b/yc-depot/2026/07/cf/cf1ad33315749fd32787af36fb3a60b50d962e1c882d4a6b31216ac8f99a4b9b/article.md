---
schema_version: "1.0.0"
document_id: "cf1ad33315749fd32787af36fb3a60b50d962e1c882d4a6b31216ac8f99a4b9b"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/why-your-team-processes-suck"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:4dba7d117e78ea2cb77d2e1c4292ff38b9be767f46a528d4496c0883f10f00c0"
---

# Why your team processes suck

I recently took an engineering management role, which means I guess this is who I am now: a person with opinions about meetings. My ancestors would be proud.


Engineers are funny about this kind of stuff. In technical work, we tend to care deeply about what things do and why they work. But the second *process* comes up, everyone suddenly gets very comfortable saying, “Well, this is what we did at my last company.”


Or worse: “This is just how we do things.”


These are terrible answers.


It makes sense why this happens. Computer systems are easy in one specific way: they are mostly deterministic and mostly under our direct control. Human systems are much messier. There is no USB-C port I can plug into a coworker's brain and upload` stop-being-a-dick.exe` .


Tragic, honestly.


People systems are stochastic (at best, we have *influence* , not control). A lot of engineers write them off as the mushy, manager-brained stuff that gets in the way of the "real" work. The irony is that ignoring the people system is a great way to make sure the real work doesn't get done. Or worse, that the wrong work gets done extremely efficiently.


Process isn't inherently shitty. Teams need it to move context, make decisions, and notice friction. But process becomes theater when we confuse the **format** with the **principle** .


## The standup test


The *format* of a process is the visible part: a daily meeting, a retro, a Slack post every Friday. The *principle* is the reason the thing exists in the first place.


Take standup. The classic format is: *what did I do yesterday, what am I doing today, any blockers.* If the process stops there people eventually mentally check out.


*"Yesterday I worked on making the logs page faster."*


Cool. What does that mean? Is it going well? How is it impacting what others are working on? Is it about to burst into flames?


The shape of standup survived, but the reason for it did not. I've seen the exact same format produce completely different outcomes depending on the principle underneath it. If a manager's principle is *control* , a standup turns into a daily surveillance ritual to prove everyone is earning their paycheck. The room feels like a tribunal.


If the principle is *connection* , it becomes the one moment in a distributed day where everyone is actually together. We pick up on the weird little human signals that don't survive the trip into Slack. We notice when someone is circling the same problem for the third day in a row.


Same format. Different principles. Completely different outcomes.


## Scaling process and culture


When the principle is useful and understood, the format can change without losing utility. When the principle is missing, all that's left is a ritual. And rituals are sticky.


We hit this wall recently with project updates. Our leadership was staying informed through Linear project updates. When there were three projects, that was fine. When it grew to fifteen, it broke. The updates were scattered across different days, with completely different altitudes of context.


A template could only fix so much. The deeper issue was the principle: *visibility at the right altitude* .


Project-level updates were too granular for leadership and too scattered for the ICs to see how the work fit together. So, we killed the ritual and changed the process. We introduced **initiatives** : a collection of related projects tied to a larger business objective, managed by one owner in one channel.


Upward communication got better because leadership had one place to look at the right altitude. Lateral communication got better because ICs finally had a forcing function to see how their work connected to the broader system.


The old process wasn't stupid. It worked until it didn't.


## Keep the why alive


I don't want process for the sake of process. I want exactly enough of it to make the team work. Not so much that everyone spends their lives feeding status machines, and not so little that context disappears into DMs and engineers become isolated code islands.


That balance changes as a company scales.


If I don't know why a process exists, I don't know what to keep, what to change, or what to kill. I am just copying shapes. Process doesn't suck. Process without principles really does.


## FAQ


What's the difference between a process's format and its principle?


The format is the visible part: the daily standup, the Friday Slack post, the retro on the calendar. The principle is the reason that thing exists in the first place. The same format can produce wildly different outcomes depending on the principle underneath it. A standup run for control feels like a tribunal. The same standup run for connection becomes the one moment your distributed team is actually together.


Why do team processes stop working as a company grows?


Usually because the format was tuned for a smaller scale and nobody revisited the principle when the scale changed. Our Linear project updates worked great at three projects and fell apart at fifteen, not because the template was wrong but because "visibility at the right altitude" stopped being possible when updates were scattered across fifteen projects on different days. The principle was still valid but the format had outgrown it.


How do I figure out the principle behind a process I inherited?


Ask what actually breaks if you delete it. If the answer is "nothing, but people would be annoyed," you're probably looking at a ritual with no principle left. If the answer is something concrete like "leadership loses visibility" or "ICs stop seeing how their work connects," that's your principle, and now you can ask whether the current format is still the best way to serve it.


When should I kill a process instead of fixing it?


Kill it when the principle it was serving is either gone or better served another way. We didn't patch the project-update ritual with a stricter template. We replaced it with initiatives because the underlying need, visibility at the right altitude, called for a different shape entirely. If you're only tweaking the format to make a dead principle feel less annoying, that's a sign to kill it.


## Related posts


- [The 3 fears that make engineering suck](https://depot.dev/blog/3-fears-that-make-work-suck)
- [We hire people who care about their work](https://depot.dev/blog/we-hire-people-who-care-about-their-work)
- [How I make support suck less](https://depot.dev/blog/how-i-make-support-suck-less)


Andrew "Watts" Watkins


Engineering Manager at Depot
