---
schema_version: "1.0.0"
document_id: "99ea4194463f828873a005bfb711d4095d5a729ff02af0690e405abc89369a76"
company_key: "yc-miggos"
company: "Miggos"
source_id: "yc-miggos-news-import-47b875555782"
canonical_url: "https://www.miggo.io/post/an-autonomous-agent-ran-17-600-actions-against-hugging-face-how-to-block-attacks-like-it"
published_at: "2026-08-04T16:06:01.355+00:00"
first_seen_at: "2026-08-10T00:40:00.987292+00:00"
fetched_at: "2026-08-10T00:40:01.984910+00:00"
content_hash: "sha256:24ea7c58602ffee1d46983113613a670593cfd9c741f71164d17eb8045df7e34"
---

# An Autonomous Agent Ran 17,600 Actions Against Hugging Face. How to Block Attacks Like It.

###### TL;DR


##### ‍
***Autonomous agents are now finding and exploiting zero days without a human directing them. They do not use new techniques. They use the same ones, thousands of times, and the successful exploitation disappears into the noise of everything that failed. Attacks in this class turn on application-level code paths that process telemetry can only infer, and that is a layer Miggo is built to see: the code path behind the event, and enforcement at the critical path that does not wait for anyone to work a queue.***


## An Unsupervised Agent Ran a Four-Day Intrusion


On July 27, Hugging Face published a[technical timeline](https://huggingface.co/blog/agent-intrusion-technical-timeline) of an intrusion into their production infrastructure. It is the most detailed account of an agent-driven campaign anyone has released, and they named their own misconfigurations in it. Read it.


This post walks through what the agent behind the Hugging Face intrusion actually did, why conventional detection caught it and still lost, and then shows Miggo stopping the same attack: we rebuilt the pivotal step of that chain in our own environment, and with Miggo enforcement on, the exploit does not execute.


## The interesting part is the execution


There is a temptation to look for something exotic in a campaign like this - an agent autonomously infiltrating a large organization feels exotic. While it is a novel and interesting case, when we look at the details, they look all-too-familiar


The agent exploited a zero-day in a package registry cache proxy to escape its own sandbox. It reached the public internet, rooted a third-party code-execution harness, and used it as a launchpad against Hugging Face for four and a half days. It found an unknown injection flaw in a production data pipeline, reached code execution inside a Kubernetes worker pod, escalated to root on the node, and read a secret object holding secret keys. It built a self-respawning pod fleet across multiple Kubernetes nodes, enrolled these as devices in the corporate mesh network, and minted its own source-control tokens.


There are all widely known techniques and primitives that every pentester has in their toolbox. The agent did not create a novel attack vector to present in Blackhat.


The part that should alarm security teams is the execution - specifically, the volume and speed. The reconstruction recovered roughly 17,600 actions across four and a half days, of which 2,911 were direct command execution - that comes up to 4,400 a day.. A human operator prunes hypotheses before testing them, because their time is expensive. They’ll create scripts to automate their requests (who doesn’t love blind sql injections and dirbusting?). An agent does not prune. It tests, and when something fails it tries the next variant immediately. An agent can test multiple hypotheses at once, over and over and over.


Look at how it moved through the data loader. The first attempt was SSRF at the metadata endpoint, which the library's URL allowlist rejected outright. So it stopped asking the worker to fetch remote resources and started making it act on local ones instead: read a local file, then execute local code. Neither is a URL fetch, so the allowlist never evaluated them. Later, when its exfiltration channel closed, it rewrote DNS, pinned hosts in /etc/hosts, monkey-patched the in-process resolver, and fell back to pre-staged binaries. An experienced attacker would have done all this - they would have taken more time to look things up, or to deliberate, or go to lunch. The agent doesn’t need those.


## Existing tools buckle under the load


Hugging Face's own stack discovered this attack. Their AI security tooling correlated the ambiguous signals into a coherent attack signal. The detection logic worked. What then happened is that the alert's criticality was not set high enough to page the on-call team, and the response lost time it did not have. The correlation was right but the prioritization failed them.


It was difficult to prioritize because this is a problem of precision and effort.When a campaign produces thousands of low-confidence events per day and the successful path is buried inside the noise thrown off by everything that failed, the constraint stops being "did we see it" and becomes "could anyone tell which one mattered, fast enough."


Agents attack that constraint directly. Not by hiding, but by generating more than a queue can absorb.


The same pattern showed up in the forensics. Payloads were chunked, XORed and compressed with a per-campaign key, so the secrets never appeared as recognizable strings. Hugging Face’s first automated scan of the raw capture found very few. Replicating the agent’s own decoding recovered roughly four times as much, mostly JWTs and platform tokens hidden inside encoded blobs. The scanner was not misconfigured. It was reading bytes that had been deliberately made unreadable, and pattern matching had nothing to match.


## The noise can’t be tuned away


Let’s look at the SSTI, the jinja code injection. An attacker reaches code execution in a Python worker and runs a shell.


An EDR sees Python running a shell. It evaluates the binary, the path it launched from, the command line, and the parent process.A Python process launching` /bin/sh` with an unusual argument string is exactly the kind of pattern that scores.


But every one of those signals describes the process. None of them describes what caused it. The verdict has to be reached heuristically: given what this process looks like, how likely is it to be malicious?


For this pattern, the answer is "possibly." Worker pods spawn shells all the time for legitimate reasons. Data pipelines shell out to converters, entrypoints run helper scripts, jobs call system utilities. The artifacts of a malicious shell and a benign one overlap heavily, so the output is a suspicion with a confidence attached. A suspicion with a confidence attached is a queue item.


That is where tuning starts. Alert on every shell from every worker and the noise overcomes the signal. Tune it down and the real one goes unreported. Most teams settle somewhere in between, where they don’t drown in alert fatigue, and the gap that leaves is not a defect in the product..


Now multiply by an agent's throughput. The volume does not create the ambiguity. It floods a queue that was already absorbing it.


## Application context as signal


A better heuristic doesn’t solve our problem because we’re looking at the problem wrong. We need to know *why* the shell was spawned, *who* caused it to spawn. That’s inside the application, where the EDR can’t see.


Take the RCE step from the Hugging Face chain. A dataset config used an fsspec` reference://` spec whose numeric offset field held a Jinja2 template instead of a number. The renderer evaluated it. The agent walked the standard Python object graph to reach` exec` :


```text
{{ cycler.__init__.__globals__.__builtins__.exec(
"import gzip,base64; exec(gzip.decompress(base64.b64decode('<payload>')))"
) }}
```


A field that should have held plain data was passed to a template renderer. That is the entire bug, and it is what turned a config upload into running code inside a production pod. Everything the agent did inside that perimeter descended from it.


[We rebuilt the vector in a minimal Flask service](https://gist.github.com/arad-g/64293508d2d913d9fa779d2f726368a4) , and the code is simple:


```text
result = Template(template_input).render(config=app.config, app=app)
```


```text
curl -v -G 'http://localhost:4000/render' \
--data-urlencode "template={{cycler.__init__.__globals__.__builtins__.exec(\"import gzip,base64;exec(gzip.decompress(base64.b64decode('$PAYLOAD')))\")}}"


```


Nothing in that request looks like an attack to a control that inspects requests. It is a string in a parameter.


**Image 1:** Miggo DeepTrace stack trace showing Jinja2 Template.render as the parent frame of a spawned process.


` Template.render` sits directly below the process spawn in the stack.


Seeing this stack trace in context is what turns a simple heuristic into a statement of fact. The finding is not "this process looks malicious." It is "template rendering in this service creates a process." No healthy application does that - this is the very definition of SSTI.


Miggo ADR already covered this pattern within its rule set. Because this detection model targets general attacker techniques rather than specific exploits, no custom adjustments were necessary to identify this threat. As a result, attacks are detectable even before the underlying vulnerability is known.


This is the difference between inferring from one signal and proving from the flow. An EDR reason about the process it can see. Miggo has the application runtime as well: the request, the code path, the render, and the call stack at the moment of execution. The malicious part was never the shell. It was the edge between two things that should never touch, and you can only see an edge if you can see both ends.


Precision at that level does not degrade under volume. Ten thousand failed attempts produce ten thousand events that do not match. The one that succeeds produces a finding that stands on its own.


## Miggo Blocks It at Execution, Removing the Triage Dependency Entirely


When we turn on enforcement, the execution never happens. The syscall returns` EPERM` , the injected command dies, and the application process keeps serving traffic.


Image 2: Miggo enforcement blocking the injected exec call, EPERM returned, application process still running


The command is denied at the moment it tries to run, whether it is attempt one or attempt eleven thousand, and whether or not anyone is watching a console. Against an attacker that generates more events than a team can process, that independence is the point.


#### **Mitigation, Mitigation, Mitigation**


Nothing about this was patchable in advance. The flaw in that data pipeline had no advisory, no CVE, and no known-vulnerable version to upgrade past. It became known because an agent found it and used it. Patching is an outcome that arrives later, after the code is written, reviewed, and rolled out everywhere it needs to go. The only thing that changes what happens in the meantime (at runtime) is something that makes the exploit fail while the flaw is still there.


That is what[Miggo's Defense-in-Depth Mitigation](https://www.miggo.io/defense-in-depth-book-a-demo) is built for: holding the line on flaws that are real, reachable, and not yet fixed.


The right mitigation can do that from inside the application, at the function level, in addition to at the edge when needed. Mitigation is the one control that runs on the attacker's clock instead of the team's, and it holds until the patch can take its normal organizational process.


See live how Miggo protects your organization at runtime[here](https://www.miggo.io/defense-in-depth-book-a-demo)


```text
<script src=  "https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/gsap.min.js"  ></script>
<  script     src  =  "https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/Flip.min.js"  >  </  script  >


<  script  >
document  .addEventListener(  "DOMContentLoaded"  ,   (  event  ) =>   {
gsap.registerPlugin(Flip);
const   state = Flip.getState(  ""  );
const   element =   document  .querySelector(  ""  );
element.classList.toggle(  ""  );
Flip.from(state, {
duration  :   0  ,
ease  :   "none"  ,
absolute  :   true  ,
});
});
</  script  >
```


```text


<  script  >
document  .addEventListener(  "DOMContentLoaded"  ,   (  event  ) =>   {
gsap.registerPlugin(Flip);
const   state = Flip.getState(  ""  );
const   element =   document  .querySelector(  ""  );
element.classList.toggle(  ""  );
Flip.from(state, {
duration  :   0  ,
ease  :   "none"  ,
absolute  :   true  ,
});
});
</  script  >
```
