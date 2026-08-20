---
schema_version: "1.0.0"
document_id: "2226a079a7305d77b0d0b7eb0eda5ec527ccdc534fcbaa4ed08a583005c81550"
company_key: "yc-freestyle"
company: "Freestyle"
source_id: "yc-freestyle-news-import-21b14ff45bb6"
canonical_url: "https://www.freestyle.sh/blog/engineering/browser-agents-need-stateful-vms"
published_at: "2026-06-03T00:00:00+00:00"
first_seen_at: "2026-07-25T13:26:33.334256+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:39383fe030e29beff2c112a84fa1ce7f975c9071588990cf4417f66e825a21fb"
---

# Browser Agents Need Stateful VMs

A browser agent looks simple in the demo. Give the model a URL, let it click around, return an answer. Under the hood, the useful version is not simple at all.


The agent needs Chromium or Firefox, a display stack, a browser profile, downloads, cookies, screenshots, accessibility trees, network logs, local files, credentials, a test runner, and often the app it is testing running on the same machine. It needs to survive flaky pages, wrong clicks, modal dialogs, slow installs, and user pauses. If it is building or fixing a web app, it also needs the repo, the dev server, the database, and a way to review the code it changed.


That is not just "browser automation." That is a computer with a browser attached.


This post is about building browser agents on[Freestyle VMs](https://www.freestyle.sh/products/vms) : full Linux machines that can run real browsers, keep state between turns, fork before risky paths, and pause when nobody is watching.


## What browser agents actually do


There are two common browser-agent workloads.


The first is web operation: log into a site, inspect data, fill a form, download a file, compare prices, scrape a dashboard, or complete a workflow on behalf of a user. The browser is the product surface. The agent is using the web the way a person would, except it can also read DOM state, inspect network responses, and run scripts.


The second is web development: run a local app, open it in a browser, reproduce a bug, inspect the DOM, patch the code, restart the server, and verify the fix with screenshots or Playwright tests. The browser is not the final destination. It is the instrument panel for the code the agent is changing.


Both workloads need the same primitives:


- a real browser with a durable profile
- a filesystem for downloads, screenshots, traces, and scripts
- long-running processes for dev servers and test runners
- package managers and system dependencies
- a way to snapshot and recover from bad actions
- isolation from other users and from your backend
- an escape hatch for human debugging


Those are VM-shaped requirements. A narrow browser API can click and screenshot. A VM can host the whole workflow.


## Why a browser session needs a machine


Most browser automation products start from the browser process. That is understandable, but it puts the wrong thing at the center.


The browser is only one process in the session. A realistic agent might run` pnpm dev` , PostgreSQL, a mock API, Playwright, a screenshot diff tool, an image optimizer, and a small script that summarizes console errors. It may need fonts installed so screenshots match production. It may need` ffmpeg` to inspect uploaded media. It may need a persistent downloads directory because the task spans several turns.


When the agent is debugging a web app, the browser and the app must be neighbors. A local server on` localhost:3000` should be reachable by the browser without a tunnel. Logs should sit on disk. Test artifacts should remain available after the model asks a follow-up. If the user disappears for an hour, the whole session should pause instead of being rebuilt from scratch.


Freestyle VMs give the browser agent that machine model. Each VM is a full Linux environment with root, SSH, systemd, users and groups, real networking, and enough control to run the browser stack and the application stack together. VMs start quickly enough to create per task, can pause and resume with state intact, and can be live-forked when the agent needs to try multiple paths.


## The Freestyle browser-agent architecture


A practical browser-agent session has four layers.


The VM is the isolation boundary. Create one VM per user task or per active workspace. Do not share browser profiles between tenants. Put all untrusted page behavior, downloaded files, generated scripts, and app code inside the VM.


The browser controller is the agent's immediate tool surface. For most teams that means Playwright or Puppeteer running inside the VM. The model should not receive a raw remote-control channel with no structure. Wrap the controller in tools like` open_url` ,` click` ,` type` ,` screenshot` ,` get_accessibility_tree` ,` read_console` , and` download_file` .


The workspace is the durable scratch area. Store screenshots, traces, HAR files, downloaded PDFs, generated scripts, and notes under a predictable directory such as` /workspace/browser` . The agent should be able to refer to those artifacts later without asking the model to remember them.


The orchestrator is your product code. It creates the VM, starts the browser service, sends tool calls, streams screenshots or logs to the UI, suspends idle sessions, and terminates VMs when the task is done.


In TypeScript, the shape is intentionally boring:


```text
import { freestyle } from "freestyle";
import { VmNodeJs } from "@freestyle-sh/with-nodejs";


const { vm, vmId } = await freestyle.vms.create({
name: "browser-agent",
with: { js: new VmNodeJs() },
workdir: "/workspace",
idleTimeoutSeconds: 600,
});


```


The important part is not the exact wrapper. It is that the browser runs inside the same Linux environment as the rest of the task.


## Keep browser state between turns


Browser agents are stateful. The model signs in, accepts a cookie banner, opens the relevant page, filters a table, downloads a CSV, and then the user asks a follow-up. If the next turn starts from a blank profile, the product feels broken.


Persist the browser profile on the VM disk. Keep downloads and screenshots in the workspace. Run the browser controller as a long-lived process rather than launching a fresh browser for every click. When the session goes idle, suspend the VM instead of destroying it.


That gives you continuity without keeping compute hot forever. The user can come back later and the agent resumes with the same profile, files, processes, and partially completed work. For internal tooling and authenticated workflows, that difference is the line between a toy demo and a usable product.


Use explicit cleanup policies. Some browser sessions should be ephemeral and deleted after the task completes. Others should be persistent because the user expects the same workspace next week. The infrastructure should support both; your product decides which one matches the task.


## Fork before risky browser actions


Browsers are full of irreversible actions. Submit the form, delete the row, purchase the item, publish the page, mark the ticket closed. A careful agent should not discover the consequence after it clicks.


Freestyle's live fork primitive lets you branch the whole running browser session before risky steps. The parent VM stays parked at the decision point. One fork clicks through the path. Another fork inspects the confirmation page. A third fork may try the safer route. Each copy starts with the same cookies, DOM state, files, and running processes, then diverges independently.


That pattern is useful even when nothing is dangerous. Browser tasks are often ambiguous. The agent can fork from the search results page and explore several candidate pages in parallel. A web-development agent can fork after reproducing a bug and try separate fixes without reinstalling dependencies or restarting from a clean repo.


The product result is better than a single linear browsing loop: more exploration, less repeated setup, and cleaner recovery when one path fails.


## Run the app and the browser together


For coding agents and app builders, browser automation should not be separated from the development environment. The agent needs to edit files, run the dev server, inspect the app in a browser, read logs, and repeat.


Put the repo, dependencies, services, and browser in the same VM. Let` systemd` keep the dev server alive. Let Playwright hit` localhost` . Store screenshots next to test traces. When the agent changes CSS, it can reload the page immediately. When a test fails only after a database migration, the database is right there.


This is also where[Freestyle Git](https://www.freestyle.sh/products/git) belongs. The VM is the workbench; Git is the reviewable source of truth. The agent can work in normal files inside the VM, commit changes to a branch, push screenshots or notes as artifacts where appropriate, and let your product show diffs for human review.


Do not make the VM disk your only record of project progress. Use it for hot state: browser profiles, installed packages, build caches, logs, traces, and running processes. Use Git for the work product that should be reviewed, rolled back, synchronized, or deployed.


## Operational checklist


A browser-agent platform should make these choices explicit:


- create one isolated VM per task, user workspace, or branch
- run the browser controller inside the VM, not as a disconnected external service
- persist browser profiles and downloads only as long as the product requires
- capture screenshots, console logs, network traces, and accessibility trees as files
- suspend idle sessions so memory state survives without burning compute
- live-fork before destructive or ambiguous browser paths
- keep app code in Git and treat the VM as the active workbench
- expose SSH or terminal access for support and debugging
- terminate VMs when the session is truly over


The common mistake is treating "browser agent" as one capability. It is not. It is a bundle of capabilities around a browser: state, files, processes, networking, credentials, debugging, recovery, and review.


That bundle wants a real machine.


## The bottom line


The browser is the agent's window into the web, but the machine is the agent's workspace. If you only provide a headless browser API, the first impressive demo will work and the first serious product workflow will start asking for everything around it.


[Freestyle VMs](https://www.freestyle.sh/products/vms) put the whole browser-agent session in one isolated Linux environment: browser, profile, repo, dev server, logs, screenshots, downloads, and long-running processes. Pause keeps it affordable while idle. Forking lets the agent explore. SSH makes it debuggable. Git makes the output reviewable.


That is the infrastructure shape browser agents need: not just a browser, but a stateful computer that can run one.
