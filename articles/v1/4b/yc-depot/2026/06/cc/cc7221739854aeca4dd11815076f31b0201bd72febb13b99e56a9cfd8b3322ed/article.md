---
schema_version: "1.0.0"
document_id: "cc7221739854aeca4dd11815076f31b0201bd72febb13b99e56a9cfd8b3322ed"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/now-available-the-depot-sandbox-sdk"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:56f4a59d7c49df5a4e3607a70cd18e6ea4efafba12e9da298c27d957395db62b"
---

# Now available: Depot Sandbox SDK

Whether you need to run code you don't trust, give every PR a fresh environment, or hand every user their own isolated sandbox, you need the same thing underneath: a real machine you can create on demand, run commands in, and tear down cleanly.


That's why today we're handing you direct access to the compute layer that underpins Depot CI. The **Depot Sandbox SDK** lets you manage sandboxes, execute commands, and directly modify filesystems; programmatically, from your own code.


Beta


The Sandbox SDK is in private beta. Method names and options might change before it's generally available.[Contact us](https://depot.dev/help) to request access for your organization.


The SDK is available as a TypeScript package:[@depot/sandbox](https://www.npmjs.com/package/@depot/sandbox) . You can read the full[Sandbox SDK reference](https://depot.dev/docs/api/sandbox-sdk-reference) in our docs, or browse the source on[GitHub](https://github.com/depot/sandbox-sdk) .


## Why we built it


At Depot, we've always worked to deliver performant building blocks, helping engineers focus on shipping amazing things rather than babysitting their infrastructure. That started with Docker builds, then continued with GitHub Actions runners and Depot CI. As we built these products, we kept wishing we could just call a function and get a fully-featured, blazing fast compute sandbox, and our customers wanted that too. Depot CI is itself, under the hood, a fleet of exactly these sandboxes, and we wanted to give that fleet a clean API so the rest of our products, and yours, could build on it.


We built a prototype of sandboxes using containers instead of microVMs, but whenever we tried using it, we'd always bump into limitations. We realized there was no replacement for an actual machine: a machine that can run Docker or whatever other container daemon you like, a machine that supports the full Linux syscall surface, a machine that gives you pinned CPU cores and a dedicated block of RAM, a machine that comes up in seconds and disappears cleanly. It's what our customers already get with Depot CI. The SDK is exactly that same compute, exposed as a primitive anyone can build on top of.


## What the SDK gives you


The SDK wraps our` depot.sandbox.v1` API in a small set of ergonomic classes that might remind you of the Node API and other familiar interfaces. Here's what we have so far:


- **Real microVMs, not containers.** Each sandbox is a genuine VM with full syscall compatibility, with nothing stopping you from running Docker, building images, starting nested virtual machines...
- **Boot in seconds.** Sandboxes start from Depot's pre-cached base image, so you go from` create()` to running commands within a few seconds.
- **A` node:fs/promises` -shaped filesystem.**` sandbox.fs()` gives you` readFile` ,` writeFile` ,` mkdir` ,` readdir` ,` stat` , and the rest; the same stuff you (and your agent) know super well.
- **Streaming command execution.**` sandbox.runCommand()` returns as soon as the command starts, so you can stream` logs()` live, or` await` the final` output()` and` exitCode` .
- **Lifecycle you control.** Create, get, list, stop, and kill sandboxes, set per-sandbox resources, and extend timeouts to keep long-running work alive.


Here's a quick sample of what it might look like:


```text
import   {createClient, Sandbox}   from   '@depot/sandbox'


const   client   =   createClient  ({token: process.env.  DEPOT_TOKEN  !  })


const   sandbox   =   await   Sandbox.  create  (client)


const   command   =   await   sandbox.  runCommand  ({cmd:   '/bin/sh'  , args: [  '-c'  ,   'echo hello from depot'  ]})
const   finished   =   await   command.  wait  ()
console.  log  (finished.exitCode)   // 0
console.  log  (  await   command.  stdout  ())   // "hello from depot\n"


const   fs   =   sandbox.  fs  ()
await   fs.  writeFile  (  '/tmp/message.txt'  ,   'hello'  )
console.  log  (  await   fs.  readFile  (  '/tmp/message.txt'  , {encoding:   'utf8'  }))   // "hello"


await   sandbox.  stop  ({blocking:   true  })
```


## What I built with it: VS Code in a box


The first thing I did with the SDK was build myself a remote dev environment. It's[one TypeScript file](https://github.com/depot/examples/tree/main/sandbox-vscode) : it boots a sandbox, clones a set of my repos into it, opens them as a multi-root VS Code workspace, and hands me back a URL I can open in any browser, all in twenty seconds. No prebuilt image, no machine to keep warm, just super-fast productivity for exactly as long as I need it.


The SDK makes this a three-step process. First, we write a repo list and my GitHub token so the box can clone my private repos, using the filesystem API:


```text
const   fs   =   sandbox.  fs  ()
await   fs.  writeFile  (  `${  HOME  }/repos.txt`  , repos.  join  (  '\n'  ))


// drop a GitHub token where gh expects it, so there's no login prompt
await   fs.  mkdir  (  `${  HOME  }/.config/gh`  , {recursive:   true  })
await   fs.  writeFile  (  `${  HOME  }/.config/gh/hosts.yml`  ,   `github.com:\n    oauth_token: ${  token  }\n`  )
```


` runCommand` clones everything and installs the editor, streaming progress back to me as it goes:


```text
const   clone   =   await   sandbox.  runCommand  ({
cmd:   '/bin/bash'  ,
args: [
'-c'  ,
`mkdir -p ${  HOME  }/ws && cd ${  HOME  }/ws && xargs -P8 -I{} git clone --depth 1 https://github.com/{}.git < ${  HOME  }/repos.txt`  ,
],
})
for   await   (  const   chunk   of   clone.  logs  ()) process.stdout.  write  (chunk.data)
await   clone.  wait  ()
```


And` detached: true` keeps the editor running after the script exits:


```text
await   sandbox.  runCommand  ({
cmd:   '/bin/bash'  ,
args: [  '-c'  ,   `code-server --bind-addr 127.0.0.1:8080 --auth password ${  HOME  }/ws/workspace.code-workspace`  ],
env: {PASSWORD},
detached:   true  ,
})
// consider trying detached mode for running a tunnel, background `npm install`, unshallowing repos...
```


Once the built-in timeout runs out (by default, two hours), the sandbox disappears immediately. This is the kind of thing the SDK is for: take a workflow that used to need standing infrastructure, and make it a script that pulls a real machine out of thin air on demand and makes it disappear just as quickly.


## Get started


You'll need a[Depot account](https://depot.dev/sign-up) with beta access —[reach out](https://depot.dev/help) and we'll enable it for your organization. Then:


```text
pnpm   add   @depot/sandbox
```


Set` DEPOT_TOKEN` in your environment (if you don't have one on hand, try` depot login token` ), create a client, and you're off:


```text
import   {createClient, Sandbox}   from   '@depot/sandbox'


const   client   =   createClient  ({token: process.env.  DEPOT_TOKEN  !  })
const   sandbox   =   await   Sandbox.  create  (client)
```


For more, see the[Sandbox SDK reference](https://depot.dev/docs/api/sandbox-sdk-reference) .


## Pricing


Sandboxes run on the same compute that powers Depot CI and are billed at the same compute rate, metered by the resources and time you use. See the Depot CI section on our[pricing page](https://depot.dev/pricing) for details.


## What's next


We're just getting started! Snapshots, secrets, and persistent disk support are all on the way, and we have a long list of Depot CI features we want to bring to the SDK.


We'd love your input on what comes first.[Request access](https://depot.dev/help) , build something, and tell us what would make the experience better. Your feedback shapes what we ship next.


## Related posts


- [Now available: Depot CI API and CLI](https://depot.dev/blog/now-available-depot-ci-api)
- [Context isolation in coding agent loops](https://depot.dev/blog/context-isolation-in-coding-agent-loops)
- [Optimizing microVM boot times](https://depot.dev/blog/optimizing-microvm-boot-times)


Rob Stolarz


Staff Software Engineer at Depot
