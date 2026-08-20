---
schema_version: "1.0.0"
document_id: "3c30b93694b10c5dee42bcaa4b7d4fbb7568a77498a48125913f0f639c5d6369"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-fc8f7e880d65"
canonical_url: "https://emergent.sh/blog/why-we-rebuilt-our-deployment-engine"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-24T18:25:54.732723+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:0a8160abe5a48e82375edffb87ef1488d89724e63d3547dbd15918fd612045ec"
---

# From Prompt to Production: Why We Rebuilt Our Deployment Engine that wasn’t on fire

In one GCP outage, GCS uploads failed for over four hours. Every deployment on our platform that needed an upload failed with them. We found out from support tickets. Our monitoring saw it, technically: we had an alert on overall deployment failure rate. But deployments fail for many reasons, most of them bad user code, so that alert fired often enough that it had become noise. Some vigilant on-call engineer would check it, conclude everything was fine, and move on. When the alert finally meant something, nobody could tell.


That outage is why we rebuilt our deployment engine, in miniature. Not one dramatic incident that forced our hand, but a slow accumulation of papercuts: alerts we couldn't trust, an in-place deploy strategy that let bad code take down healthy apps, hand-rolled state management that couldn't survive a pod eviction, and a codebase where small fixes were difficult and big projects were a nightmare.


We used to receive 5 to 10 deployment support tickets a day. Since the rebuild, that number is zero.


A deploy on Emergent is a seven-step pipeline touching several external systems: fetch source, build the image, prepare the database, deliver secrets, apply Kubernetes resources, health-check, switch traffic. At tens of thousands of apps deploying continuously, the engine coordinating it is one of the most reliability-critical systems we run. V1 of that engine grew organically from our earliest days and served us through our fastest-growing months. It also had three structural problems that no amount of patching was going to fix.


‍


WHAT WAS BROKEN


## Three problems, one root cause


**Bad code could break a healthy app.** V1 deployed updates in place: new pods came up under a new replica set via rolling update, under the same service that was serving live traffic. Combined with gaps in startup and readiness probes, this meant a bad deploy regularly damaged the healthy, traffic-serving version it was replacing. A broken update taking down a working app was structurally possible on every deploy.


**A dead worker meant a dead deploy.** V1 ran as a pubsub-to-worker pipeline: one huge block of code executing in a worker, with nothing orchestrating it from outside. State lived in the database, maintained by hand, and the retry flow was complicated, unreliable code that read that state back to figure out where it was. A clean failure was somewhat handled. A force termination, a pod eviction, or hardware crash mid-deploy, was unrecoverable. The deployment just hung until something reaped it.


**We were blind to systemic failures.** The GCS story above. Our only platform-level signal was aggregate failure rate, and aggregate failure rate mostly measures user code quality, not platform health. Real outages hid inside the noise for hours.


Underneath all three sat the same root cause: the data model. V1 had two entities, apps and deployments, and neither actually represented an app or a deployment. status = deleted on either didn't reliably mean the thing wasn't live. A chain of fork-and-replace deployments was impossible to trace: we knew a job was deployed as a replacement, but not who replaced whom, and there was no way to follow a single deployment's lifecycle end to end. When your core entities don't mean anything precise, every feature built on top of them inherits the ambiguity.


There was a V2 along the way, an interim step. V3 is the full rewrite, and it rests on two modeling decisions.


‍


THE MODEL


## A deploy is an immutable record, and going live is a pointer flip


In V3, every deploy is a run: a permanent, read-only record of one attempt, carrying a snapshot of exactly what was meant to be deployed. A run is never edited and never re-run. A retry is a new run linked to the previous one. So is a rollback. So is an automatic fix. The history of an app is every attempt, in order, nothing overwritten. The fork-and-replace archaeology problem disappears because the lineage is the data model.


Every run also creates its own fully isolated Kubernetes resources: its own pods, its own service, nothing shared with the version serving traffic. The new version comes up beside the old one, with proper startup and readiness probes, and gets health-checked in isolation. Only when it is completely healthy does the engine flip a single guarded, atomic pointer that switches traffic over; then the old resources are cleaned up. If the new version is bad, the flip never fires and the live app never notices. Blue-green deployment, but not as a pattern we implement carefully on each deploy. It's the only path that exists.


Rollback falls out for free: the previous version already exists as a run with a known-good image, so reverting is another pointer flip, no rebuild.


‍


ORCHESTRATION


## Why Temporal


The model fixed what a deploy is. The pipeline still had to survive the thing V1 couldn't: a worker dying mid-flight.


We moved orchestration onto Temporal. Each pipeline step became a small activity with its own retry policy and timeout; Temporal records every completed activity in a durable event history, and if the server running a deploy dies, the workflow replays elsewhere and resumes at the exact activity that was in flight. The force-termination case that was unrecoverable in V1 is handled out of the box, the hand-rolled DB state management is gone, and the step recording a deploy's final outcome runs detached, so a run can never be left stuck "running" forever.


We didn't run a bake-off. Temporal was already battle-tested inside Emergent, running Cortex and our main agent workflow, by the time the deployment rebuild started. Plenty of other workflow executors exist, but two things settled it. Familiarity was one. The other was Temporal's architecture: the Temporal server only manages metadata and workflow state, while the actual worker code executes on our own infrastructure. Our control-plane logic never leaves our infra, which means we could use a managed Temporal service without exposing intellectual property and without taking on the maintenance cost of self-hosting the orchestrator.


The activity-level granularity also fixed the observability problem, almost as a side effect. We no longer watch aggregate failure rate and guess. A single failure of a specific type, say a GCS upload failing after three retries or a Mongo user creation failing once, alerts the team immediately, because that shape of failure is a platform signal, not a user-code signal. The four-hour blind spot that opened this post is structurally closed: today that outage would page us on the first failed upload.


‍


RESULTS


## What actually changed


The deployment failure rate did not change much, and V3 was never going to be a magic wand for it. Most failures are bad user code, and no orchestrator fixes that. If you rebuild an engine expecting the top-line failure number to collapse, you will be disappointed.


What changed is everything around that number. Deployment support tickets went from 5 to 10 a day to zero. Systemic failures surface in minutes instead of via the support queue. Bad deploys can no longer damage healthy apps. And the velocity cost of the old codebase, where small fixes were difficult and bigger projects were a nightmare, is gone: since V3 landed, our entire agentic deployment flow shipped on it, and we shipped multi-region support for GDPR compliance, letting customers pick where their apps are deployed. Multi-cloud and multi-tenancy are in progress on the same foundation. None of those would have been sane to attempt on V1.


That last part is the real justification for the rewrite. The case for V3 was forward-looking more than anything else: not "the system is on fire" but "every future thing we want to build costs too much on this foundation." The lesson we keep relearning across Emergent's infrastructure is that the data model does most of the work. Once a deploy became an immutable record, going live became a guarded pointer flip, and the pipeline ran on durable, replayable rails, then safe updates, fast rollback, precise alerting, and a shippable roadmap stopped being features to build. They were consequences of the design.
