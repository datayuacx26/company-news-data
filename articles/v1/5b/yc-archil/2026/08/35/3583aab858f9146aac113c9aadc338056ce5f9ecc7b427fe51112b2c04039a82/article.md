---
schema_version: "1.0.0"
document_id: "3583aab858f9146aac113c9aadc338056ce5f9ecc7b427fe51112b2c04039a82"
company_key: "yc-archil"
company: "Archil"
source_id: "yc-archil-news-import-fbbb60f1501e"
canonical_url: "https://archil.com/post/serverless-execution"
published_at: null
first_seen_at: "2026-08-09T19:14:48.524951+00:00"
fetched_at: "2026-08-09T19:14:50.194785+00:00"
content_hash: "sha256:e04d802e573a6b7a2445a63954eb5af77bad2b88d9ebba42a30651b2fdc131f1"
---

# Bash is the SQL for file systems

**TL;DR:** Archil file systems now run your code. Call` disk.exec("grep ERROR logs")` and we spin up a container with your file system already mounted, run the command to completion, and return stdout, stderr, and an exit code. No sandbox to provision, no data to move. Bundled free with your storage


**30 minutes of active execution per $1 of monthly storage spend, reset each month.** A 50 GB disk ($10/mo) includes ~5 hrs/mo free; a 1 TB disk includes ~100 hrs. Past the free tier, $0.18/hr active, billed in 1ms increments with a 100ms minimum per call.


for most workloads. Live today for all file systems in AWS regions.


---


(a) database


select … where


client


—


select statement


small data


database


cpu


data


(b) archil


disk.exec('grep …')


client


—


bash command


small data


archil


cpu


data


Same shape. In both cases the compute lives next to the data — the client sends an instruction, the storage runs it, and only the answer comes back. A statement in, a result out, whether the data lives in a table or on a filesystem.


When onboarding, almost every customer asks me about egress fees from the clouds. This makes sense — any public service (not running in your cloud account) gets charged full egress, even when the actual service lives in the same region. We support private networking for our largest customers to avoid this, but the question comes up a lot.


A couple of months ago I was talking with Nikita Shamgunov — founder of[Neon](https://neon.com/) , which Databricks acquired last year — and I asked him how he dealt with the fact that his customers must be angry about egress too. He told me *"it's never come up,"* which struck me. Why is it that the largest serverless Postgres provider doesn't have customers complaining about egress? I wrote it off as a fluke. Databases are IOPS-heavy but not throughput-heavy.


A few weeks later I was talking with Richard Artoul, who built[WarpStream](https://www.warpstream.com/) , about Archil. Unprompted, he said: *"You guys have the same problem as WarpStream, huh? Everyone is worried about egress because customers have to read 1:1 every byte they put into the service. How do you solve it?"*


Yes. The truth sat exactly in what he said.


**"Customers have to read 1:1 every byte they put into the service."** At that moment I became enlightened.


## The file system vs. the database


Take a super-simple case in data storage: searching through some fields. On a database, you'd write` select * from table where x = y` . On a file system, you'd use` grep` .


There's a huge difference between these two things. When you` grep` , your machine literally downloads every file you're searching through and (in its memory) pokes around to find the ones that match. It's also single-threaded, which is just great.


When you execute a` select ... where` on a database, your client sends the statement to the database. The database embeds compute inside of it that does a similar thing, but smarter: identifies what indexes are relevant, tries to build the most optimal version of the query, and — importantly — only sends back the specific data you asked for.


Not only is that faster, it's also significantly less network traffic between the compute and the storage system. What might be 100 GB of egress on the file system could be less than 2 KB on a database.


(a) database


select … where


client


—


select statement


small data


database


cpu


data


(b) file system


grep a file system


client


cpu


read X, read Y, …


everything


file system


data


download + grep locally


Same question, two architectures. A database runs the query next to the data and ships back the rows. A file system can’t — it has no compute, so the client reads every file and runs` grep` locally. Same question, everything flows.


The database isn't transferring the data. **It's transferring instructions for how to interact with the data.**


This is the reason we work so hard to keep latencies low on our service, and only recommend Archil when your compute has a network round trip in the hundreds of microseconds. But it's worth asking: why is the file system crippled like this? Is there any way to send instructions to the file system itself, instead of transferring the data raw?


There is. We already have a lingua franca for interacting with file systems — a Linux computer with a bash interface.


## ` disk.exec`


We've added an` .exec` method to our TypeScript client. It sends a bash command to the Archil cloud, runs it inside a container with your file system already mounted as the working directory, and returns stdout, stderr, and an exit code. Full details in the[serverless execution docs](https://docs.archil.com/concepts/serverless-execution) .


```text
import   { Archil }   from   '@archildata/client/api'


// A disk, backed by your S3 bucket
const   {   disk   }   =   await   new   Archil  ().  disks  .create  ({
name  :   'agent-workspace'  ,
mounts  :   [{ type  :   's3'  ,   bucketName  :   'agent-bucket'   }]  ,
})


const   {   stdout  ,   stderr  ,   exitCode  ,   timing   }   =   await   disk  .exec  (
'grep -r "ERROR" logs'
)
```


Each` exec` gets its own container with dedicated CPU and memory. Because the file system is multi-attach, you can fan out as many parallel execs as you want, each against the same data:


```text
// Full map-reduce across the bucket, no grep, no ripgrep
const   files   =   (  await   disk  .exec  (  'ls logs'  )).  stdout  .split  (  '\n'  )  .filter  (Boolean)


const   results   =   await   Promise  .all  (
files  .map  ((f)   =>   disk  .exec  (  `grep ERROR logs/  ${  f  }  `  ))
)
```


You're only billed for the time your command is running. Idle sandboxes don't exist — there's nothing to size, nothing to keep warm, nothing to shut down.


## Give your agents a portable bash tool


A bash tool is the most generic capability you can give an LLM. You expose a single function — "run a shell command" — and the model can inspect files, run scripts, transform data with` sed` /` awk` /` jq` , call APIs with` curl` , and glue together anything else it has access to. One tool definition collapses dozens of narrow ones. Every serious coding or data agent ends up with one.


Your agent doesn't want random compute — it wants to *do something* on data it already has access to. It wants to exec on a disk. The standard answer today flips this: spin up a sandbox, wire data into it, manage its lifecycle, figure out how state survives between calls. That's compute plumbing — incidental to what the agent is trying to do, but it dominates the engineering.


` disk.exec` is exactly that, with none of the wiring. One SDK call. The file system is already mounted. The container spins up on demand. State persists automatically between calls, because it's just a file system — the agent writes a file and the next call sees it.


```text
const   bash   =   tool  ({
description  :   'Run a shell command inside the workdir.'  ,
inputSchema  :   z  .object  ({
command  :   z  .string  ()  ,
})  ,
execute  :   async   ({ command })   =>   {
const   {   stdout  ,   stderr  ,   exitCode   }   =   await   disk  .exec  (command)
return   `exit   ${  exitCode  }  \n  ${  stdout  }${  stderr   ?   `\n  ${  stderr  }  `   :   ''  }  `
}  ,
})
```


No sandbox provider. No Dockerfile. No machine lifecycle. The file system is always there; compute runs when the agent needs it.


**And the tool is portable.** Because the disk is multi-attach, the same bash tool can back any number of agents simultaneously — each getting its own container, all sharing the same file system. A planner hands work off to an executor agent and the executor picks up with full state. Subagents fan out in parallel, each seeing what the others wrote. Mount the disk on your laptop and watch what they're doing. The file system is the handoff primitive; the compute is ephemeral.


## Free with your storage


Here's the part we're most excited about. **Every $1 of monthly storage spend includes 30 minutes of execution free.** A 50 GB disk ($10/mo) gets ~5 hours of active execution a month — that's around 600 agent bash-tool sessions before you pay a cent for compute. A 1 TB disk gets ~100 hours.


We sell storage, not compute. Running code on your data is how you actually *use* the product. Charging for it line-by-line would be like a database charging per` SELECT` . You already paid to store the data; running bash on it comes with the territory.


If you run Archil at any real scale, exec is just included.


## Limits


- **HTTP response returns after 5 minutes.** The container keeps running; you just stop getting output. For longer jobs, break work up and communicate through files on the disk — we're working on a long-running sessions API with streaming output that'll land soon.
- **Stdout and stderr are each capped at 128 KiB per call.** Truncated output gets a trailer noting how many bytes were dropped. Pipe large outputs to a file on the disk.
- **Single container shape today: 2 vCPU / 4 GiB.** Larger shapes are coming. No GPU sandboxes yet.
- **AWS regions only at launch.**` aws-us-east-1` ,` aws-us-west-2` ,` aws-eu-west-1` . GCP and private-networked deployments soon.
- **Consistency:** Execs are regular Archil clients — the same[read-after-write consistency and delegation rules](https://docs.archil.com/concepts/shared-disks) apply as for mounted clients. A crashed exec leaves behind whatever it wrote before crashing, same as a crashed process on a local machine.


Past the bundled free tier, pricing is $0.18/hr active, billed in 1ms increments with a 100ms minimum per call. Full per-region rates and a calculator are on the[pricing page](https://archil.com/pricing) .


## What's next


The goal from here is to make it easier to *use* your storage, not just store bytes. That means built-ins for common operations like full-text search and vector indexing, harnesses for common agent workflows, and — eventually — a query planner that can rewrite and parallelize the bash you send us across our internal compute. The SQL analogy gets a little closer to literal with every step.


Serverless execution is live today.[Create a file system](https://console.archil.com/) and try` disk.exec` —[the docs are here](https://docs.archil.com/concepts/serverless-execution) .
