---
schema_version: "1.0.0"
document_id: "87812d332a162248c0ffa7956d97efd9a803a10b1166ac380b36265d3c1621eb"
company_key: "yc-superagent"
company: "Superagent"
source_id: "yc-superagent-news-import-3c33ed06ece7"
canonical_url: "https://www.superagent.sh/blog/a-bad-patch-is-worse-than-no-patch"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-24T02:47:34.464363+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:24533df9d20fcdb84134a140cc21d702cd16633180bf1c65a0464f6c0594ddf6"
---

# A bad patch is worse than no patch.

That breaks most people's intuition about software. More fixes should mean fewer bugs. Acting should beat sitting still. But security doesn't work that way. A fix that bumps a dependency and breaks the build, or a critical CVE that turns out not to apply to how you actually use the package, costs you more than the quiet bug you never touched. Doing nothing has a floor. Doing the wrong thing does not.


Every security team knows this in their bones. It's why the dashboard with four thousand open findings gets ignored. The tool cried wolf enough times that the humans stopped listening. We call it alert fatigue, which is a polite way of saying detection was never the hard part.


I keep coming back to this, because the model labs are about to make finding vulnerabilities free.


Everyone reads that and concludes security is about to get cheap. It isn't. The cost didn't vanish. It moved.


Finding a flaw is becoming free. Closing one is not. Closing a vulnerability means proving it's real, reproducing it safely, writing a fix that doesn't break the build, and getting a human to merge it into main. That work doesn't get cheaper because detection did. We only stopped noticing it because finding things used to be the hard part.


An analogy: raw model is a metal detector that beeps on everything. The beep is free. The digging is not. A detector that sends you digging up bottle caps all afternoon doesn't make you faster. It makes you slower, and a little angry.


Every founder building on top of AI gets the same question eventually. Investors just ask me the pointed version. Won't a model lab build this?


Because security was never a model problem. A lab sells beeps. More inference, more tokens out the door. Closing a vulnerability isn't a smarter model, it's a pipeline. A triage step that validates before anything reaches a person, and a pull request a human reviews and merges. The merge is the enforcement. Nothing lands in your repo because an agent felt confident.


Which is why we don't care what model sits underneath. Best one this month, a different one next month, maybe one on your own hardware. The labs are racing to commoditize the beep.


The scanners owned the other half. They detect and alert, hand you a report, and walk off. A report is a finding that never closed.


So here's what I'd flip around. People think AI is about to commoditize security. It's commoditizing the cheap half and quietly handing the whole bill to the half nobody priced. The moat was never the model. When the finding is free, the only thing left worth owning is the close.
