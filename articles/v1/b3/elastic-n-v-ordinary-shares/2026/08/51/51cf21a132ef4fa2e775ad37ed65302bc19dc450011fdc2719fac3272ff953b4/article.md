---
schema_version: "1.0.0"
document_id: "51cf21a132ef4fa2e775ad37ed65302bc19dc450011fdc2719fac3272ff953b4"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/npm-cooldown-removal-detection-elastic-agent"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-08-07T14:52:03.108937+00:00"
fetched_at: "2026-08-07T14:52:04.996750+00:00"
content_hash: "sha256:7c442b2df285199d8ef64badc3a262d44885fb21a983c0112c452e01c05f1351"
---

# The security signal log tailing can't see: tracking npm cooldown removals with Elastic Agent

7 August 2026 •


[Wieger van der Meulen](https://www.elastic.co/security-labs/author/wieger-van-der-meulen)


# The security signal log tailing can't see: tracking npm cooldown removals with Elastic Agent


A 40-line CEL integration snapshots .npmrc files every 6 hours to catch cooldown removals. This post walks through the three ways we broke filestream before landing on snapshot semantics.


12 min read


[Enablement](https://www.elastic.co/security-labs/category/enablement) ,


[Detection Engineering](https://www.elastic.co/security-labs/category/detection-engineering)


npm's` min-release-age` setting tells npm to ignore any package version published less than a set number of days ago, keeping freshly compromised releases out of` npm install` during the window when they do the most damage. Getting the setting onto developer workstations is straightforward. Knowing when someone quietly deletes it is a different problem entirely, and log-tailing inputs are no help because they only fire when lines are appended to a file. We built a ~40-line Common Expression Language (CEL) integration in[Elastic Agent](https://www.elastic.co/elastic-agent) that snapshots every` .npmrc` on a 6-hour heartbeat. When` min-release-age` disappears from the next snapshot, the pipeline marks it` cooldown.absent = true` . This post walks through that pipeline, the filestream approach we tried first, and what we learned from three iterations before landing on the final design.


The full path from config file to dashboard looks like this:


```text
flowchart LR
subgraph W["Developer workstation"]
A[".npmrc files<br/>user + global"] --> B["CEL integration<br/>6-hour snapshot"]
B --> C["JavaScript processor<br/>drops auth-token lines"]
end
C --> D["Ingest pipeline<br/>key, value"]
D --> E[("Cooldown dataset")]
E --> F["Adoption dashboard"]
classDef input fill:#e8eef9,stroke:#5a7db8
classDef proc fill:#fdf3dc,stroke:#c9a227
classDef out fill:#e7f2e9,stroke:#4c9a63
class A input
class B,C,D proc
class E,F out
```


##


Why developer workstations are the npm supply-chain gap


CI/CD pipelines have their own supply-chain guardrails that can be managed at the enterprise level. Workstations are where the gap usually sits. A developer running` npm install` on a freshly compromised package is a different surface from the build pipeline, and the package manager itself is the right place to apply a delay before a fresh release becomes installable. npm's[min-release-age](https://docs.npmjs.com/cli/v11/using-npm/config) setting, available since npm 11.10, does exactly that: the value is a number of days, and npm excludes any version published more recently from resolution. In MITRE ATT&CK terms, the technique it blunts is[Compromise Software Dependencies and Development Tools (T1195.001)](https://attack.mitre.org/techniques/T1195/001/) .


Most other package managers have an equivalent flag;[cooldowns.dev](https://cooldowns.dev/) tracks which ones, what each flag is called, and recommended values. This post focuses on npm, and recent attacks have happened, but the technique applies to any of them.


We enforce the setting with Jamf: a script writes` min-release-age=7` to each user's` .npmrc` and the machine-global npmrc, and re-applies once a day. A removal therefore self-heals within 24 hours, and the telemetry is what makes the removal visible at all: the 6-hour heartbeat catches the gap window before Jamf closes it, and shows us which hosts keep removing the setting.


##


What does an npm cooldown config file look like?


The target is small: npm writes the setting to a user-level` .npmrc` , typically 10 to 50 bytes.


```text
~/.npmrc    →    min-release-age=7        # days
```


Machine-global settings live in a handful of well-known paths (` /opt/homebrew/etc/npmrc` and` /usr/local/etc/npmrc` on macOS,` /etc/npmrc` and` /usr/lib/node_modules/npm/.npmrc` on Linux), so a complete inventory has to include those paths too. While we haven’t deployed a Windows version yet, we did include all the details to make this work in the Linux and Windows variants section. These can be found in the section Linux and Windows variants.


Two ingredients made this look easy. First, as InfoSec is Customer Zero at Elastic, the Elastic Agent is already rolled out to every endpoint and runs with the privileges to read these files. Second, the schema is essentially flat: one key, one value, one config file. So in theory, you configure a file input, tail the cooldown lines, and ship them to[Elasticsearch](https://www.elastic.co/elasticsearch) .


The complication is that auth tokens for private registries (` //registry.npmjs.org/:_authToken=...` ) live in the same` .npmrc` file. None of that can leave the host. Any approach has to filter the file body before it transits anywhere.


##


First approach: Monitoring .npmrc with the Custom Logs Filestream integration


The[Custom Logs Filestream integration](https://www.elastic.co/docs/reference/integrations/filestream) in[Fleet](https://www.elastic.co/docs/reference/fleet) wraps Filebeat's modern` filestream` input and lets you specify path globs and an ingest pipeline. Note: this is a distinct Fleet integration from the older "Custom Logs" integration, which wraps the legacy` log` input. The two have different configuration surfaces and file-identity models, so if you're migrating between them the reference linked above is worth reading before either direction.


We configured the integration across all users on macOS, plus the system-global npm paths:


```text
/Users/*/.npmrc
/opt/homebrew/etc/npmrc
/usr/local/etc/npmrc
/etc/npmrc
```


The ingest pipeline did four things:


1. [Grok](https://www.elastic.co/docs/reference/enrich-processor/grok-processor) the` message` field to pull out the` min-release-age=value` pair.
2. Drop any line whose key wasn't` min-release-age` , so registry auth tokens never made it into Elasticsearch.
3. Convert the value to a` long` .
4. Remove the raw` message` and` event.original` fields before indexing.


It worked, with three caveats that changed our minds about whether it was the right integration for this job.


###


Three filestream behaviours that don't fit config file monitoring


####


Native file identity beats fingerprint for small files


Filebeat's default file identity strategy is "fingerprint", which hashes the first N bytes of a file to give it a stable ID across rotations. The fingerprint length defaults to 1024 bytes, and a file shorter than that minimum is silently held back from ingestion until it grows. A 22-byte` .npmrc` will never grow, so the data never moves. This is in line with the description of the Filestream integration and also apparent in the agent logs:


```text
"ingestion from some files will be delayed, files need to be at least
1024 in size for ingestion to start"
```


The fix is to switch the integration's advanced options to use native file identity, which uses inode plus device and has no size floor. In our deployment on Elastic Agent 9.x, disabling fingerprint alone did not fall back to native; we had to flip both toggles explicitly.


####


clean_inactive and ignore_older are coupled


We wanted periodic re-emission of each config file so a stale event could be distinguished from a current one. filestream's` clean_inactive` is the right knob for that, but it requires` ignore_older` to also be set. Together they cleanly trim files that haven't moved recently, which is useful for rotating log files and not useful for static config files that may sit unchanged for months. We disabled` clean_inactive` .


####


Filestream emits on append, not on removal


This third learning is structural rather than a config gotcha. filestream is designed for log tailing: when bytes arrive at the end of a file, an event is emitted. When content disappears (because npm rewrites` .npmrc` without the cooldown line, or because the file is deleted), filestream sees a modification, but every resulting line is dropped by our allowlist filter, and nothing reaches Elasticsearch. The last "set" event for that host stays in the index indefinitely.


For application logs that's correct behavior, since log lines aren't usually retracted. For config file state monitoring, removal is the signal the adoption campaign needs most, and it never arrives. That's why we switched to CEL.


###


Why nvm inflates npm version counts for cooldown adoption


Machines routinely report more than one npm version, because every Node.js version installed through nvm (Node Version Manager) ships its own npm binary. The distortion stood out as soon as we checked test pulls from our[osquery manager integration](https://www.elastic.co/docs/reference/integrations/osquery_manager) package-version inventory: one machine reported 34 distinct npm versions.


The config side of this is forgiving.` npm config set min-release-age 7` writes to the user-level` ~/.npmrc` , and every npm installation on the machine reads that file, including the per-Node copies nvm installs. Enforcement is the strict side: versions older than 11.10 ignore the key. A machine carrying npm 11.12 in one nvm environment and npm 10.9 in another is only protected when the newer npm is the one running` npm install` . A count of machines with any capable npm overstates who is protected, which is one more reason to track the config file directly rather than infer adoption from version counts.


##


Second approach: Monitoring .npmrc with CEL snapshot semantics


The[CEL input](https://www.elastic.co/docs/reference/beats/filebeat/filebeat-input-cel) in Elastic Agent was designed for HTTP API polling, but it also exposes a` file()` function and a` dir()` function for filesystem access. That enables a different model from filestream. filestream tails lines as they arrive; CEL takes a snapshot of the whole file each interval. For state monitoring, the snapshot is what we want: the unit of interest is the current contents of` .npmrc` as a whole.


Here is the complete script we run, trimmed to npm (our production version watches the other package managers' config files with the same pattern, and carries an extra guard for non-UTF-8 file content):


```text
(
(
try(dir("/Users")).as(entries, type(entries) != type("") ?
entries.filter(u,
u.is_dir
&& !string(u.name).startsWith(".")
&& string(u.name) != "Shared"
&& string(u.name) != "Guest"
).map(u, "/Users/" + string(u.name) + "/.npmrc")
: [])
) + (
try(dir("/home")).as(entries, type(entries) != type("") ?
entries.filter(u,
u.is_dir
&& !string(u.name).startsWith(".")
).map(u, "/home/" + string(u.name) + "/.npmrc")
: [])
) + (has(state.files) ? state.files : [])
).map(f,
try(file(f)).as(content,
type(content) == type("") ?
{"file": f, "exists": false}
:
{"file": f, "body": string(content),
"hash": content.sha256().hex(), "exists": true}
)
).as(file_data, {
"events": file_data.filter(fd, fd.exists).map(fd, {
"message": fd.body,
"file": {"path": fd.file, "hash": {"sha256": fd.hash}},
}),
"cursor": {"hashes": file_data.filter(fd, fd.exists)
.map(fd, {"file": fd.file, "hash": fd.hash})},
"url": state.url,
"files": has(state.files) ? state.files : [],
})
```


The first block enumerates user home directories at runtime, with filtering that path globs can't express, and appends the machine-global paths from` state.files` .` try(dir(...))` and` try(file(...))` return an error string when the path doesn't exist, so each block checks the type of the result and collapses a missing directory or file to an empty result. A macOS host has no` /home` , a Linux host has no` /Users` , and the same script runs on both.


The initial state supplies the global paths and the poll interval is 6 hours:


```text
files:
- /opt/homebrew/etc/npmrc
- /usr/local/etc/npmrc
- /etc/npmrc
- /usr/lib/node_modules/npm/.npmrc
```


**An aside on Fleet:** the integration to add in the UI is called["Custom API using Common Expression Language"](https://www.elastic.co/docs/reference/integrations/cel) , not "CEL". It exposes the full CEL runtime including file system access. Set` resource.url: file:///dev/null` to satisfy the required URL field without making an HTTP request, and put the initial state in the "Custom request cursor" YAML at the bottom of the form.


###


How the CEL integration evolved: emit-on-change, tombstones and heartbeat


The CEL snapshot integration shown above is the third version. The path there is the useful part.


**Version one emitted only on change.** Each heartbeat hashed every file and emitted an event only when the hash differs from the cursor, the state the input persists between runs. Efficient, and it produced the removal signal cleanly: npm rewrites` .npmrc` in place when a setting changes, the hash moves, the new snapshot carries no` min-release-age` line, and the ingest pipeline marks the event` cooldown.absent = true` .


**Version two added a deletion tombstone.** Hash comparison can't see a file that stopped existing (` npm config delete min-release-age` removes` .npmrc` entirely when it's the only setting), because version one filtered out non-existent files before emitting. A one-block extension emitted a stub event, once, for any file that was in the cursor but no longer on disk.


**Version three replaced both with a heartbeat, because the dashboard demanded it.** The adoption dashboard counts hosts whose cooldown state falls inside the selected time window. Under emit-on-change, a host that sets a cooldown emits exactly one event and then goes silent. Once that single event ages past the dashboard's window (say,` now-7d` ), the host disappears from the "adopted" count even though the cooldown is still in place. We watched adoption climb during rollout and then start to erode a few days later, purely as an artifact of one-shot events aging out of the window. The telemetry was correct; the time-windowed view of it was not.


So the final integration re-emits each existing file's current state on every 6-hour heartbeat. Every host re-reports its cooldown posture four times a day, the windowed dashboard reflects live state, and a host that stops appearing is genuinely offline rather than merely quiet. Removal of the cooldown line still surfaces as` cooldown.absent = true` in the next snapshot, and a deleted file drops out of subsequent snapshots, so the tombstone becomes unnecessary.


The cost is volume: a snapshot per file per host per interval instead of one event per change. At a 6-hour cadence that is a few thousand small documents a day across the fleet, which is immaterial. At a 60-second interval it stops being immaterial: always-emitting at 60s ships the same unchanged state every minute, which we measured at roughly a thousand-fold more documents for zero added signal. If you adopt the heartbeat, set the interval in hours.


The detection latency for a removal is the heartbeat interval, 6 hours in our case. For an adoption campaign measured over days and weeks against a 7-day cooldown, that is not a meaningful constraint. If you need lower-latency, discrete removal events for alerting, run the emit-on-change variant instead; the snapshot model supports both.


###


How to filter .npmrc auth tokens before they leave the host


A snapshot-based integration sends the whole file body across the wire to the ingest pipeline. That's fine for config keys; it is not fine for` .npmrc` registry auth tokens. Filtering at the ingest pipeline is too late, because by then the tokens have already transited the network.


The fix is an agent-side[script processor](https://www.elastic.co/docs/reference/beats/filebeat/processor-script) in the integration's Advanced options → Processors field. It keeps only` min-release-age` lines and drops everything else before the event leaves the workstation:


```text
- script:
lang: javascript
source: >
function process(event) {
var msg = event.Get("message");
if (msg == null) return;
var filtered = [];
var lines = msg.split('\n');
for (var i = 0; i < lines.length; i++) {
var line = lines[i].trim();
if (line.indexOf('min-release-age') === 0) {
filtered.push(line);
}
}
event.Put("message", filtered.join('\n'));
}
```


Tokens never reach the wire. We verified by adding a test token to a` .npmrc` on a managed host, running a cycle, and confirming zero search hits for the token string in Elasticsearch.


###


The npm cooldown ingest pipeline


The pipeline below is the npm-only variant of the one we run in production, and it is short enough to show whole. Grok extracts the key and value, a Set processor turns "no key found" into the explicit removal signal, and the raw message is removed before indexing:


```text
PUT _ingest/pipeline/npm-cooldown-workstation
{
"description": "Parses min-release-age from .npmrc snapshots",
"processors": [
{
"grok": {
"field": "message",
"patterns": [
"(?<cooldown.key>min-release-age)\\s*=\\s*(?<cooldown.value>[^\\n\\r]*)"
],
"ignore_missing": true,
"ignore_failure": true
}
},
{
"set": {
"if": "ctx.cooldown?.key == null",
"field": "cooldown.absent",
"value": true
}
},
{
"set": {
"if": "ctx.cooldown?.key != null",
"field": "cooldown.unit",
"value": "days"
}
},
{
"convert": {
"field": "cooldown.value",
"type": "long",
"ignore_missing": true,
"ignore_failure": true
}
},
{
"remove": {
"field": ["message", "event.original"],
"ignore_missing": true
}
}
]
}
```


Two details earned their place the hard way. The value capture is` \[^\\n\\r\]*` , which stops at both LF and CRLF line endings; an earlier revision used` \[^ \\r\]*` , which truncated any value containing a space and could run across newlines. And` cooldown.unit` is set explicitly even though npm's unit is always days, because dashboard rows that read` 7 days` stay unambiguous when the telemetry later grows beyond npm.


The` cooldown.absent = true` event is the payoff. The host was in the index six hours ago with` cooldown.key = min-release-age` ; the next snapshot has no key; the dashboard shows the transition. That is the removal signal filestream could not produce.


##


CEL vs. filestream for config file monitoring


We ran both in parallel for a few weeks and then made a call.


The core reason is the shape of the data. filestream is built for append-only logs, where new lines arrive at the end of a file and the job is to harvest them.` .npmrc` is a state file: npm rewrites it in place when a setting changes and deletes it when the last setting is removed. The question the telemetry needs to answer, "what is the cooldown state of this host right now, and when did it change," is a question about the current contents of a file. Tail offsets can't answer it.


CEL Filestream


Designed for State files (snapshot + hash) Append-only logs (tail)


Cooldown-line removal detection Yes (next snapshot marks` cooldown.absent` ) No


File deletion detection Yes (host drops out of snapshots) No


Snapshot semantics Whole file Per line


Auth-token posture Filtered agent-side before transit Same (agent-side allowlist)


Fleet integration Custom API using Common Expression Language Custom Logs Filestream


Config complexity ~40-line CEL integration Declarative path globs


State refresh / latency 6h heartbeat ~10s (tail)


Running both meant every config change produced two events into two separate datasets, and we maintained two ingest pipelines with dashboards split across indices. The complexity cost outweighs any redundancy benefit, and the only thing filestream gives that CEL doesn't is faster detection latency, which is not a meaningful constraint for adoption tracking on a 7-day cooldown.


We also considered osquery and auditd before settling on CEL. osquery can read config file contents on a schedule through its` file` and` file_lines` tables, and auditd can fire on file writes, but neither produces a clean current-state signal across ingestion, and both would mean standing up a second collection path next to the agent already deployed for endpoint telemetry.


CEL won. End-to-end validation on a single macOS workstation:


Scenario Expected Result


` npm config set min-release-age 5`` cooldown.key = min-release-age` ,` cooldown.value = 5` Pass


` .npmrc` exists with no cooldown key` cooldown.absent = true` Pass


` npm config delete min-release-age` (deletes the file) path absent from subsequent snapshots Pass


Auth token line in` .npmrc` No token in Elasticsearch Pass


##


npm cooldown monitoring on Linux and Windows


macOS was the primary platform; the design generalizes cleanly.


Linux needed no integration change at all. The` /home` block in the integration above already covers it: on each platform, the directory that doesn't exist fails the type check and collapses to an empty list, and the other side fills in. The Linux variant runs the same integration through the same ingest pipeline into the same dataset, with` /etc/npmrc` and` /usr/lib/node_modules/npm/.npmrc` in the global watch list.


Windows is a separate integration on the same dataset, and it's the one variant we designed but haven't deployed: our own fleet has too few Windows machines to justify the rollout yet. We're documenting it anyway because Windows is the most common OS in corporate fleets, so for many readers this variant is the one that matters most. The integration enumerates` C:/Users/*` (forward slashes work on Windows under Go/CEL), excludes` Public` ,` Default` , and` Default User` , and watches each user's` .npmrc` plus the system globals under` C:/ProgramData` . The Grok pattern's value capture,` \[^\\n\\r\]*` , stops at both LF and CRLF line endings, so it works unchanged on Windows without capturing the trailing` \\r` in the value. The agent-side script processor is also unchanged.


##


Rollout status and extending beyond npm


The pipeline is now reporting from several hundred macOS, each re-emitting its current npm cooldown state on the 6-hour heartbeat. We're sharing the technique at this stage so other security engineering teams can build on it; once the rollout reaches the full fleet we'll follow up with an org-wide adoption-rate post. The Windows variant stays on the shelf until our Windows population justifies the rollout; if your fleet is Windows-heavy, the design in this post is ready to adapt.


In production the same integration already watches the config files for pip, uv, pnpm, yarn Berry, and bun. That expansion deserves its own post, because cooldown values are not unit-comparable across package managers, and one manager's documentation and implementation disagree about the unit by three orders of magnitude. If you're extending this design beyond npm, check[cooldowns.dev](https://cooldowns.dev/) for each flag's unit before you compare values across tools.


##


What we learned about npm cooldown monitoring with Elastic Agent


- npm cooldown adoption telemetry is state monitoring. The signal that matters most is a host removing` min-release-age` , and append-driven log tailing cannot see that happen.
- A snapshot-based CEL integration produces the removal signal directly: re-emit each` .npmrc` 's current state on a 6-hour heartbeat, and a removal shows up as the key's absence in the next snapshot.
- A time-windowed adoption dashboard needs the heartbeat, not one-shot change events, which age out of the window and make adoption look like it's eroding when it isn't.
- Small config files (under 1024 bytes) need native file identity, not fingerprint, regardless of which integration you use.
- An agent-side script processor keeps` .npmrc` registry auth tokens on the workstation: tokens never reach the wire.
- nvm inflates capability counts: each Node.js version ships its own npm, all of them read the shared` ~/.npmrc` , but only npm 11.10+ enforces the key. Track the config file, count enforcement-capable versions separately.


#### Jump to section


- [Why developer workstations are the npm supply-chain gap](https://www.elastic.co/security-labs/npm-cooldown-removal-detection-elastic-agent#why-developer-workstations-are-the-npm-supply-chain--gap)
- [What does an npm cooldown config file look like?](https://www.elastic.co/security-labs/npm-cooldown-removal-detection-elastic-agent#what-does-an-npm-cooldown-config-file-look-like)
- [First approach: Monitoring .npmrc with the Custom Logs Filestream integration](https://www.elastic.co/security-labs/npm-cooldown-removal-detection-elastic-agent#first-approach-monitoring-npmrc-with-the-custom-logs-filestream-integration)
- [Three filestream behaviours that don't fit config file monitoring](https://www.elastic.co/security-labs/npm-cooldown-removal-detection-elastic-agent#three-filestream-behaviours-that-dont-fit-config-file-monitoring)
- [Native file identity beats fingerprint for small files](https://www.elastic.co/security-labs/npm-cooldown-removal-detection-elastic-agent#native-file-identity-beats-fingerprint-for-small-files)
- [clean\\_inactive and ignore\\_older are coupled](https://www.elastic.co/security-labs/npm-cooldown-removal-detection-elastic-agent#clean_inactive-and-ignore_older-are-coupled)
- [Filestream emits on append, not on removal](https://www.elastic.co/security-labs/npm-cooldown-removal-detection-elastic-agent#filestream-emits-on-append-not-on-removal)
- [Why nvm inflates npm version counts for cooldown adoption](https://www.elastic.co/security-labs/npm-cooldown-removal-detection-elastic-agent#why-nvm-inflates-npm-version-counts-for-cooldown-adoption)
- [Second approach: Monitoring .npmrc with CEL snapshot semantics](https://www.elastic.co/security-labs/npm-cooldown-removal-detection-elastic-agent#second-approach-monitoring-npmrc-with-cel-snapshot-semantics)
- [How the CEL integration evolved: emit-on-change, tombstones and heartbeat](https://www.elastic.co/security-labs/npm-cooldown-removal-detection-elastic-agent#how-the-cel-integration-evolved-emit-on-change-tombstones-and-heartbeat)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=The%20security%20signal%20log%20tailing%20can%27t%20see:%20tracking%20npm%20cooldown%20removals%20with%20Elastic%20Agent&url=https://www.elastic.co/security-labs/npm-cooldown-removal-detection-elastic-agent)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/npm-cooldown-removal-detection-elastic-agent)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/npm-cooldown-removal-detection-elastic-agent&title=The%20security%20signal%20log%20tailing%20can%27t%20see:%20tracking%20npm%20cooldown%20removals%20with%20Elastic%20Agent)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/npm-cooldown-removal-detection-elastic-agent&title=The%20security%20signal%20log%20tailing%20can%27t%20see:%20tracking%20npm%20cooldown%20removals%20with%20Elastic%20Agent)
